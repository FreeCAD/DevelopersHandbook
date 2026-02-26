---
layout: default
---

# Signing the FreeCAD executables and installers

## Mac OS

### Prerequisites

You must be a member of Apple's Developer program to generate and use signing keys. Information can be found at https://developer.apple.com

To sign an app you must have a "Developer ID Application" certificate. These can be created on the
Certificates, Identifiers & Profiles page of your Apple Developer account. Once it is created, download the certificate onto your computer
and install it into one of your Keychains (double-click on the certificate to open and install it).

The easiest way to upload an app for notarization is to create an App Store Connect API Key at https://appstoreconnect.apple.com . This is
a file that resides on your computer and can be referenced by the notarization upload tool. Run `xcrun notarytool store-credentials` to store
the credentials and allow the signing script to access them when running the notary tool.

Note that the Intel bundles must be signed on an Intel machine, and the ARM bundles must be signed on an ARM machine.

### Local Signing Process

Once the prerequisites above are met, actually signing the FreeCAD.app bundle is straightforward:
1. Create a folder that represents the final disk image. In general it should have the FreeCAD.app bundle in it, and an alias to /Applications.
2. Copy the file *src/Tools/macos_sign_and_notarize.sh* to the location that contains that folder (not to within the folder itself).
3. Edit the script to set the name of the folder as well as the release number information and architecture of your machine.
4. Set an environment variable called `FREECAD_SIGNING_KEY_ID` to the ID of your "Developer ID Application" certificate. You can find the
ID by running `security find-identity -p basic -v`.
5. Run the script. This will take several minutes, as it scans the bundle for all shared library and executable objects and signs them with
the specified certificate. It will automatically create DMG, submit it for notarization, and staple the final authorization from Apple to
the disk image.

Once complete, the disk image is ready for distribution.

### GitHub Actions Signing Process

The GitHub actions signing process is a bit more involved because of the additional security requirements inherent in a remote build. They also need to be repeated periodically when certificates or agreements expire. Sometimes only the certificate generation needs to be renewed, but sometimes the entire sequence. To perform this sequence you must have a valid Apple Developer account. If you are trying to sign with the FreeCAD project association's certificate you must be a member of that organization on Apple's account system. Talk to an FPA administrator to get such access if you require it. If you are only trying to sign an executable as yourself, using your own Apple account, there is no need, and the steps are the same.

The complete process is:
1. Log into your Apple Developer account at https://developer.apple.com
2. Go to your certificates list at https://developer.apple.com/account/resources/certificates/list
3. If you are using the FPA's certificate, you should see a key for `The FreeCAD project association, Developer ID Application, macOS, Yorik van Havre, 2027/02/01`. If you don't have access to that certificate you can generate your own to sign the code as yourself instead. Follow Apple's steps to create a macOS Developer ID Application certificate.
4. Click on that certificate and download it. In this example that will be referred to as `FPA_Cert.cer` but you can call it whatever you want.
5. On your Mac, double-click on that file and it will load in the `Keychain Access` app.
6. Select it in that app and use the Export command from the File menu to export a `*.p12` file. This example will use `FPA_Cert.p12` but again, you can call it anything you want. When exporting you will be prompted to create a password for that exported file: create a new, unique, high-quality password for it and record it, you will need it again when setting up the GitHub actions environment.
7. Base64-encode the contents of that file. This is easiest done in a terminal window: `base64 -i ~/Downloads/FPA_Cert.p12 -o ~/Downloads/FPA_Cert.p12.base64_encoded` -- this is the completed certificate. In some cases this will be the only thing that needs to be updated in the GitHub actions environment and you can skip to step N.
8. Now go to https://developer.apple.com/account/resources/identifiers/list to see your identifiers
9. If an identifier already exists that you want to use, skip this step. Otherwise create a new unique identifier for the app you are building. This is not typically visible to users so its exact name does not make a huge difference. It does not need any additional capabilities, etc.
10. Once you have an identifier for the app, go to https://developer.apple.com/account/resources/profiles/list
11. If no provisioning profile exists for the app you are signing, create a new one. Make a Developer ID profile, associate it with the app you are signing, and then either the FPA's Apple account or your own, depending on circumstances. When you get to the final screen, **download the profile** -- this will be your only opportunity to do so. This example will call it `FPA.provisionprofile`.
12. Base64-encode that profile: `base64 -i ~/Downloads/FPA.provisionprofile -o ~/Downloads/FPA.provisionprofile.base64_encoded`
13. At this point it's easiest to log into your GitHub account in another browser tab, and navigate to your project's Actions environment setup page. For FreeCAD this is https://github.com/FreeCAD/FreeCAD/settings/environments/6267580426/edit
14. In another tab, log into your Apple account and create a new "app-specific password" at https://account.apple.com/account/manage -- call it whatever you want, and either store this value someplace accessible, or leave this browser tab open and switch back to GitHub.
15. You will need to create the following environment secrets:
    * `APP_SPECIFIC_PASSWORD` -- you created this in step 14, just paste it in
    * `APPLE_ID` -- *your* Apple ID, the one you used to create the app-specific password
    * `BUILD_CERTIFICATE_BASE64` -- the contents of `FPA_Cert.p12.base64_encoded` (make sure you don't include the final newline if copying it off the command line)
    * `BUILD_PROVISION_PROFILE_BASE64` -- same as the certificate, but for `FPA.provisionprofile.base64_encoded`
    * `DEVELOPER_TEAM_ID` -- Either the FPA's team ID (visible in your Apple developer account), or your own ID team ID from Apple
    * `KEYCHAIN_PASSWORD` -- An arbitrary password, used on the CI system as the password for the keychain. Anything you want.
    * `P12_PASSWORD` -- The password you set when exporting the certificate to the `*.p12` file.
    * `SIGNING_KEY_ID` -- The ID of the certificate associated with `BUILD_PROVISION_PROFILE_BASE64`. Obtained by running `security find-identity -v -p codesigning` on a machine with the key installed. It is the SHA hash of the selected key in the displayed list.
16. Run the "Weekly Build" GitHub action to generate the signed macOS weeklies based on these settings.

## Windows

FreeCAD is currently signed using an Azure-based system sponsored by the FreeCAD Project Association (FPA). Signing during the GitHub Actions-based build is done using a combination of the `azure/login` Action provided by the Azure team and the .NET `sign` executable that is part of Microsoft's .NET family of tools. To examine the actual signing commands, see [`create_bundle.sh`](https://github.com/FreeCAD/FreeCAD/blob/main/package/rattler-build/windows/create_bundle.sh).

To keep the system flexible and make it easier to change to a different signing certificate later on (for example, if a fork wants to set up signing), all Azure signing parameters are stored as either Repository secrets (for sensitive information) or Repository variables (for non-sensitive data). The variables are:
* `AZURE_TRUSTED_SIGNING_ACCOUNT`: "FreeCAD-Code-Signing" if using the FPA's certificate
* `AZURE_TRUSTED_SIGNING_CERTIFICATE_PROFILE`: "FreeCAD" (defined when the FPA's certificate was created)
* `AZURE_TRUSTED_SIGNING_ENDPOINT`: "https://neu.codesigning.azure.net" (Northern Europe for the FPA's certificate)

Project administrators configure four GitHub Actions Repository secrets (in FreeCAD's case using credentials controlled by the FPA):
* `AZURE_CLIENT_ID`
* `AZURE_CLIENT_SECRET`
* `AZURE_SUBSCRIPTION_ID`
* `AZURE_TENANT_ID`

### Generating the FPA's signing credentials

The values for those variables come from the output of the Azure RBAC creation command (run on a local Windows machine, not a CI runner):
```
az ad sp create-for-rbac --name "FreeCADGitHubActions" \
  --role "Artifact Signing Certificate Profile Signer" \
  --scopes /subscriptions/OUR_SUBSCRIPTION_ID/resourceGroups/FreeCAD_Code_Signing/providers/Microsoft.CodeSigning/codeSigningAccounts/FreeCAD-Code-Signing \
  --json-auth
```
Here, "FreeCADGitHubActions" is an arbitrary account name being created by the command, and the FPA's subscription ID is "1e441f5e-9283-4984-9b1b-e42f96a7db0f". Using that command requires first logging into the appropriate tenant:
```
az login --tenant OUR_TENANT_ID --use-device-code --scope "https://codesigning.azure.net/.default"
```
where our tenant ID is "fa7cd937-4c12-49bd-b500-075147b38022". FPA administrators have login access to this account and can authenticate by following the prompts from the login command. Note that `--use-device-code` is not the only login option, so administrators are free to use alternative login mechanisms as necessary.

### Creating your own signing system

If you would like to configure a build of FreeCAD to sign with your own certificate, rather than the FPA's, you can follow the instructions in [this blog post](https://www.hanselman.com/blog/automatically-signing-a-windows-exe-with-azure-trusted-signing-dotnet-sign-and-github-actions) (note that there have been some minor terminology changes since it was written -- in particular, the necessary role is now called "Artifact Signing Certificate Profile Signer", not "Trusted Signing Certificate Profile Signer"). The sequence is:

1. Create a paid Azure account (the lowest price option is almost certainly adequate if all you are doing is code signing).
2. Install the [Azure CLI](https://aka.ms/installazurecliwindows)
3. Go to [Azure Portal](https://portal.azure.com) and search for "Artifact Signing Account"
4. Create a new account there:
5. For the required parameters you can use basically anything you want. If you aren't going to do anything else with Azure it doesn't really matter. Here's what we used for the FPA's account:
    * Resource Group: FreeCAD_Code_Signing
    * Account Name: FreeCAD-Code-Signing
    * Region: Northern Europe (e.g. the `neu.codesigning.azure.net` endpoint)
6. Starting at the Azure portal's "Artifact Signing Accounts" page, go through the identity verification process. The exact steps will depend on where you live and whether you are signing as an indvidual developer or as an organization. For the FPA this required providing our DUNS number and address information, and correlating that to one of the FPA board members' personal identities verified via a government-issued identification card. The process took a few hours to complete.
7. In the page for the Artifact Signing Account you just verified, create a certificate profile. Again, you can be somewhat arbitrary with the values here. The FPA used:
    * Profile name: FreeCAD
    * Profile type: Public Trust (required to prevent SmartScreen)
    * Identity validation: Select your approved identity
8. Record the following pieces of information for later use (The FPA's information is shown below, you should record whatever you actually used):
    * Account Name: FreeCAD-Code-Signing
    * Certificate Profile Name: FreeCAD
    * Endpoint URL: https://neu.codesigning.azure.net/
    * Subscription ID: Found in Azure Portal
    * Resource Group: FreeCAD_Code_Signing
9. From the Artifact Signing Account's page, choose the Access Control settings from the left sidebar, add a role assignment, and search for "Artifact Signing Certificate Profile Signer". Select that role, click Next (hiding at the bottom of the screen), and assign that role to your account.
10. Log out of your current scope, and into the code-signing scope using the tenant ID found in the portal at Microsoft Entra ID → Overview → Tenant ID
    ```
    az login --tenant YOUR_TENANT_ID --use-device-code --scope "https://codesigning.azure.net/.default" 
    ```
    Follow the prompts to log in using your browser.

At this point you are ready to sign code locally, which is a good test to do even if you eventually plan to migrate to using some Ci system.

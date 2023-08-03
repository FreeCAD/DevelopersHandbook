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
a file that resides on your computer and can be referenced by the notarization upload tool.

Note that the Intel bundles must be signed on an Intel machine, and the ARM bundles must be signed on an ARM machine.

### Signing process

Once the prerequisites above are met, actually signing the FreeCAD.app bundle is straightforward:
1. Create a folder that represents the final disk image. In general it should have the FreeCAD.app bundle in it, and an alias to /Applications.
2. Copy the file *src/Tools/macos_sign_and_notarize.sh* to the location that contains that folder (not to within the folder itself).
3. Edit the script to set the name of the folder as well as the release number information and architecture of your machine.
4. Set an environment variable called `FREECAD_SIGNING_KEY_ID` to the ID of your "Developer ID Application" certificate. You can find the
ID by running `security find-identity -p basic -v`.
5. Run the script. This will take several minutes, as it scans the bundle for all shared library and executable objects and signs them with
the specified certificate. It will automatically createa DMG, submit it for notarization, and staple the final authorization from Apple to
the disk image.

Once complete, the disk image is ready for distribution.
  
## Windows

  TBD

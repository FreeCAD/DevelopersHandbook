---
layout: default
---

# How to control object deletion

It is possible for a user to attempt to delete a DocumentObject while a Task
Dialog is attempting to update that object.  Handling this in a Task Dialog
is not tidy.

## Recommended reading:
- ViewProviders
- In/Out Lists

There are two methods in the ViewProvider that deal with this situation.

'''c++
bool canDelete(App::DocumentObject *obj)
'''
is called when Std_Delete is asked to delete obj, BUT it is called for each of
obj's parent in the InList.  This is where you would decide if you will allow
one of your children can be deleted.  In effect, the is canDeleteMyChildObject().
Return true to allow the child to be deleted, or false to prevent the deletion.

'''c++
bool onDelete(const std::vector<std::string> & parms)
'''
is also called when when Std_Delete is asked to delete a DocumentObject, but in
this case it is the DocumentObject's ViewProvider that is called.  Return true
to allow deletion of the DocumentObject, or false to prevent the deletion.

Note that it is possible to circumvent these methods via scripting.

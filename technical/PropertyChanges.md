---
layout: default
---


# Dealing with Property Changes

A brief introduction

Recommended reading:
- The Application Module
- [Property in the wiki](https://wiki.freecad.org/Property)

## The App side
- When a Property's value changes, its PropertyContainer (eventually a DocumentObject) executes the onChanged method with the Property as a parameter.  This is where you will take appropriate action based on the new property value. 
- Be cautious when inserting logic into onChanged and execute as it is possible to create a loop.
    
'''c++
void myFeature::execute()
{
    MyProperty->setValue(foo());
}
void myFeature::onChanged(App::Property property)
{
    if (property == MyProperty) {
        execute();
    }
}
'''
- The other interesting method involving property changes is mustExecute.   This is where you control whether or not your object participates in the next recompute cycle.


## The Gui side
- On the Gui side, we must look first at your derived ViewProvider, which is attached to your DocumentObject.  The ViewProvider is notified of changed properties for both itself and the DocumentObject it belongs to.
- For properties belonging to the ViewProvider (those on the View tab in the PropertyEditor), the relevant method is again onChanged.
- Whenever a DocumentObject Property changes, the updateData method of its attached ViewProvider is called. 


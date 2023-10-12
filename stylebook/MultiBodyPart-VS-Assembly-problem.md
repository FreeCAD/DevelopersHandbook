# What is the MultiBodyPart Vs. Assembly Problem?  

## What is an Assembly?  

An assembly is a bunch of independent objects that stick together. This Independent Objects normally are parts which either are developed to fit in several unrelated Assemblies or where third party objects.  

If you modify them you normally only use a kind of cut so it either fit into a certain place or get holes to place it or another part.  

## What is a MultiBodyPart (with external references)?  

This Problem increase in an environment with a solved TNP Problem. A MultiBodyPart is a Bunch of volumes that depending on each other by using external references.  

For example: You are creating a large machine that contains several parts, but you need to adjust the dimensions of the structure and the shell in which the parts are located to fit the contents or the planned environment. In this case, you prefer a solution where you only need to change some key values, such as length, width and height, to modify the U-beam structure and the sheet metal shell of the machine.  

## Where is the Problem?  

The Problem appear if you try to mix both. As clear the difference use-case seems. It is difficult to separate this two in real life. Especially If you are in an early stage of a new product.  

It could happen, that you create a MultiBodyPart but after that try to move a body with assembly mates to a new location.  

## Solution?  

### Use application limitation  

So you have to set up, or you get automatically a "MultiBodyPart" Object. The moment you use External references or use feature that split or merge Bodies. Bodies inside this object could not be moved independent to other Bodies of the same object except you opt-in a setting per object.  

Allow to use Assembly-Mates only in context of bodies that origin from the same feature. e.g. you split a body into two bodies, but now you need a little distant between them. In that case you could mate both bodies to have a specific distant.  
Assembly mates is a feature that relocate several Bodies according to several mates. This is similar to the sketch relation inside a sketch.  

### Have trained and informed User  

User which know what they do could create really cool and easy to maintaining machines. (Sadly we could not assume that.)  
With the freedom to mix assembly and MultiBody you could save a lot of time. And easy modify along planned parameters. This need a good overview about the current case. A feature-tree that shows relations in an understandable way. A help to create, removing and repairing such references as easy as possible is important.

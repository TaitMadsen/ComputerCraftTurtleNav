ComputerCraftTurtleNav
======================

Language: Lua
Developer: Tait Madsen
Application: turtles (robots) in ComputerCraft, a mod for Minecraft

The primary interface function in this repository is Map:goTo.  Most of the code here works in support of this function.  Map:goTo navigates the turtle from it’s current position to a goal position specified in the parameters.

A demo of this code in action can be found at:
http://www.youtube.com/watch?v=JCv6kOxcj2k


A MORE DETAILED OVERVIEW
========================
The “:” notation exhibited by Map:goTo will be briefly explained here.
Lua does not directly support Object Oriented Programming; however, OOP can be emulated by the use of metatables.  “:” indicates a metatable access, so if Foo is a class, f is an instance of class Foo, and bar() is a method in class Foo, bar() would be defined as:
>>> function Foo:bar()
and called as:
>>> f:bar()

Map:goTo employs an implementation of the A* algorithm to generate paths to either (a) the goal coordinates, or (b) waypoint coordinates that Map:goTo has generates for exploration.  The rest of Map is dedicated to handling state representations utilized by Map:goTo and A*.

The Map state representation utilizes the Nav class, which serves as a wrapper around a turtle’s native API - movement and turning.  On top of calling the turtle’s methods, however, Nav also keeps track of the turtle’s position and orientation with respect to its starting point.

As the reader may know, A* requires a priority queue, or heap.  The Heap file serves as a Lua implantation of such.  By using the fact that all “objects” in Lua are really hash tables, this heap can easily serve as a min heap or a max heap, and can prioritize any numeral-property of any object.


PERSISTANCE CHALLENGE
====================
One of the challenges of programming for ComputerCraft is that code execution can be terminated at ANY TIME (by the player exiting the game).  Turtles and computers in the game receive no warning of this termination, and thus must be prepared to restart the program from anywhere in the code.  My remedy to this problem is to save every change of state to file.  Nav does this.  This remedy however, is not a guaranteed solution, because it is still possible for the game world to terminate between the change of state and file write, even if they are only a line apart.  

**Code should use indent Tab Size: 4.**
Under no circumstances should indents be made with "spaces". If you discover code where it uses spaces, feel free to make them use indent Tab Size: 4 instead.

misc folder contains kits of our future heroes we are to implement with our system. To alleviate this process, we have several APIs to re-use, found in **system** folder.

We create objects in **compiletime**, not in World Editor. We do this because of **Wurst's compiletime**. This object creation happens in **objects** folder, along with storing all object IDs.

Any related UI system/task happens in the **ui** folder, such as our draft menu.

We wrap objects in Entity, or at least, attempt to do so everytime we want to create a unit (**EntitySystem.wurst**). The Unique IDs can be found in **Dictionary.md** in utils folder.

For naming conventions, avoid using **reserved names** such as step, in, base or even destroy.

If you want to destroy something, usually it is **destroy x**, not **x.destroy()** unless the object has a destroy helper function.

There is no need to run any **tests** or **build**, as this is in the scope of Warcraft III, not a programming language such as c++ to build/run code.

There is no need to create any **PR**.

**The project adheres to following coding conventions and rules:**

You must remember that vararg functions can only have one parameter
You must not confuse Wurst with Lua, so no ";" symbols and such
You must remember that Wurst has reserved names on variables, eg. destroy, step, to, in, for
You can not use arrays in function parameters
You must use correct lambda syntax, if you plan to use them
You must avoid semicolons, as they are not a thing in Wurst
You must not use "public" etc on functions, when they are already in a "public" class, they are reachable by default
You can not do this: functions annotated '@compiletime' may not take parameters. Note: The annotation marks functions to be executed by wurst at compiletime.
You must give code that has same indentation as my code to avoid warning: Mixing tabs and spaces for indentation.
You can get a unit icon path using BlzGetAbilityIcon(unitID).
You may not do invalid comparisons, like int == null
You must use let for type inference when the type is clear from context
You must remember that tuples and tuple types are supported and commonly used
You must remember that interface implementations require proper inheritance syntax
You must remember objects created with new need proper cleanup/destruction, use destroy object for manual memory management when needed, not obj.destroy. ondestroy is the destructor.
You must be aware of reference counting and circular references
You must remember imports must be declared at the top with proper syntax
You must remember that package structure and dependencies matter
You must use @config annotations for configuration
You must remember event callbacks have specific syntax patterns
You must use proper closure syntax for event handlers
You must remember timer callbacks follow specific lambda patterns
You must remember string interpolation uses specific syntax
You must remember string comparison methods differ from other languages
You must remember class constructors use construct() syntax
You must use "this" keyword properly in class methods
You must remember static vs instance method declarations
You muse use proper null-safety patterns
You must remember efficient coding patterns specific to WC3 engine
You must avoid creating objects in frequently called functions
You must use object pooling when appropriate
You must handle player indexing (0-based vs 1-based contexts)
You must remember unit/item handle management
You must remember proper cleanup of temporary objects
You must remember that variables defined as let are constants, thus can not be changed
You must remember that variables can be defined as "var", these are changeable
You must remember that Annotation @config can only be used in config packages (package name has to end with '_config'
You must remember that function parameter variables are constants, thus can not be changed
You must remember that when converting vJass to Wurst, when possible, use Classes.
You must remember there is no such thing as BlzFrameGetAbsX or BlzFrameGetAbsY
You must remember that you can not "destroy" framehandles
You must remember Type parameters can only be bound to ints and class types
You must remember that Wurst does not have anything called "continue"
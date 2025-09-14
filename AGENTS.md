Code should use indent Tab Size: 4.
Under no circumstances should indents be made with "spaces". If you discover code where it uses spaces, feel free to make them use indent Tab Size: 4 instead.

misc folder contains kits of our future heroes we are to implement with our system. To alleviate this process, we have several APIs to re-use, found in system folder.

We create objects in compiletime, not in World Editor. We do this because of Wurst's compiletime. This object creation happens in objects folder, along with storing all object IDs.

Any related UI system/task happens in the ui folder, such as our draft menu.

We wrap objects in Entity, or at least, attempt to do so everytime we want to create a unit (EntitySystem.wurst). The Unique IDs can be found in Dictionary.md in utils folder.

For naming conventions, avoid using reserved names such as step, in, base or even destroy.

If you want to destroy something, usually it is "destroy x", not "x.destroy()" unless the object has a destroy helper function.

There is no need to run any tests or "build", as this is in the scope of Warcraft III, not a programming language such as c++ to build/run code.

There is no need to create any PR.
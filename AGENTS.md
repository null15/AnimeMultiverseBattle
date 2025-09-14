## Project Structure
- Objects are created in compiletime rather than the World Editor; object creation and IDs are stored in the objects folder.
- UI-related tasks belong in the ui folder, such as the draft menu.
- Wrap objects in Entity when creating units (EntitySystem.wurst); unique IDs are documented in utils/Dictionary.md.

## Directory structure:
└── null15-animemultiversebattle/
    ├── README.md
    ├── AGENTS.md
    ├── wurst.build
    ├── wurst_run.args
    ├── _build/
    │   └── dependencies/
    │       ├── Frentity/
    │       │   ├── doc/
    │       │   ├── imports/
    │       │   └── wurst/
    │       │       ├── war3map.j
    │       │       ├── buff/
    │       │       ├── entity/
    │       │       ├── physics/
    │       │       ├── player/
    │       │       ├── test/
    │       │       └── util/
    │       ├── InventoryUtils/
    │       │   └── wurst/
    │       │       ├── InventoryEvent/
    │       │       ├── ItemRecipe/
    │       │       ├── ItemRestriction/
    │       │       ├── SmoothItemPickup/
    │       │       └── StackNSplit/
    │       ├── wurst-bonus-handler/
    │       │   └── wurst/
    │       ├── wurst-table-layout/
    │       │   ├── imports/
    │       │   └── wurst/
    │       └── wurstStdlib2/
    │           └── wurst/
    │               ├── _handles/
    │               │   └── primitives/
    │               ├── _wurst/
    │               │   └── assets/
    │               ├── closures/
    │               ├── data/
    │               ├── dummy/
    │               ├── event/
    │               ├── file/
    │               ├── math/
    │               ├── objediting/
    │               │   └── presets/
    │               └── util/
    ├── imports/
    │   └── ui/
    │       ├── includes/
    │       └── textures/
    ├── misc/
    │   └── kits/
    │       ├── heroes/
    │       └── items/
    └── wurst/
        ├── game/
        │   └── heroes/
        │       ├── All Might/
        │       └── Nagato/
        ├── system/
        │   ├── core/
        │   ├── math/
        │   ├── objects/
        │   │   ├── abilities/
        │   │   └── units/
        │   └── ui/
        │       └── components/
        │           └── draft/
        └── utils/

- _build folder contains the standard library files, function wrappers, and among others by the Wurstscript Programming Language developers themselves.
- imports folder is the files in the "Map" itself, such as mp3 files or textures, assets would be another word.
- misc folder contains kits of heroes to be implemented.
- wurst_run.args is the file which determines how the compiler should compile our project.
- wurst.build is the file which determines how many players there can be, which team they are in, map name, developer name, filename, dependencies and so on.
- BaseMap.w3x is the source map in which we compile our Scrips (.wurst) into. Result map appears in _build folder, with the filename instructed in wurst.build file.

## Coding Conventions
- Code should use indent Tab Size: 4; never use spaces.
- For naming conventions, avoid reserved names such as step, in, base, or destroy.
- To destroy something, usually use `destroy x` rather than `x.destroy()` unless a destroy helper function exists.
- Vararg functions can only have one parameter.
- Do not confuse Wurst with Lua, so no `;` symbols.
- Wurst has reserved names on variables, e.g., destroy, step, to, in, for.
- Arrays cannot be used in function parameters.
- Use correct lambda syntax when using lambdas.
- Avoid semicolons; they are not a thing in Wurst.
- Do not declare functions `public` inside a `public` class; they are reachable by default.
- Functions annotated `@compiletime` may not take parameters; the annotation runs the function at compiletime.
- Maintain consistent indentation; avoid mixing tabs and spaces.
- Use `BlzGetAbilityIcon(unitID)` to get a unit icon path.
- Do not make invalid comparisons like `int == null`.
- Use `let` for type inference when the type is clear from context.
- Tuples and tuple types are supported and commonly used.
- Interface implementations require proper inheritance syntax.
- Objects created with `new` need proper cleanup; use `destroy object` for manual memory management, not `obj.destroy`. `ondestroy` is the destructor.
- Be aware of reference counting and circular references.
- Declare imports at the top with proper syntax.
- Package structure and dependencies matter.
- Use `@config` annotations for configuration.
- Event callbacks have specific syntax patterns.
- Use proper closure syntax for event handlers.
- Timer callbacks follow specific lambda patterns.
- String interpolation uses specific syntax.
- String comparison methods differ from other languages.
- Class constructors use `construct()` syntax.
- Use the `this` keyword properly in class methods.
- Distinguish between static and instance method declarations.
- Use proper null-safety patterns.
- Employ efficient coding patterns specific to the WC3 engine.
- Avoid creating objects in frequently called functions.
- Use object pooling when appropriate.
- Handle player indexing (0-based vs 1-based contexts) correctly.
- Manage unit and item handles carefully.
- Clean up temporary objects properly.
- Variables defined as `let` are constants and cannot be changed.
- Variables defined as `var` are changeable.
- The `@config` annotation can only be used in config packages whose names end with `_config`.
- Function parameter variables are constants and cannot be changed.
- When converting vJass to Wurst, use classes when possible.
- There is no such thing as `BlzFrameGetAbsX` or `BlzFrameGetAbsY`.
- You cannot `destroy` framehandles.
- Type parameters can only be bound to ints and class types.
- Wurst does not have `continue`.

## Testing
- There is no need to run any tests or builds.

## PR Guidelines
- There is no need to create any PR.
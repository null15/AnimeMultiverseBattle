# AnimeMultiverseBattle Agent Guide

**API Snapshot:** AnimeMultiverseBattle is a Wurst-based Warcraft III project built on Frentity and the wurstStdlib that orchestrates entities, abilities, projectiles, and UI into a hero-centric arena experience. Custom gameplay APIs extend these foundations to manage time manipulation, tethering, transformations, and other advanced combat interactions.

## Codebase Layout
- Object generation happens at compiletime; see `wurst/system/objects` for ability, buff, and unit definitions backed by `objectIDs.wurst`.
- Place UI-related logic in `wurst/system/ui` (e.g., draft UI components and chat); hero implementations live in `wurst/game`.
- All units should be wrapped with the `Entity` hierarchy (`system/core/EntitySystem.wurst`) for consistent data/buff control.
- `_build` caches compiled maps plus external dependencies (Frentity, InventoryUtils, wurst bonus handler, wurstStdlib2, table layout helpers).
- `imports` stores in-map assets (models, sounds, UI art), and `misc/kits` holds hero/item design kits awaiting implementation.
- `wurst_run.args` configures compiler targets; `wurst.build` controls lobby settings, metadata, teams, and dependency pins.
- `BaseMap.w3x` is the source map that receives compiled scripts; the compiled result is emitted into `_build` under the name defined in `wurst.build`.

## Core Gameplay APIs
### Entity, effect, and data flow
- **EntitySystem** (`wurst/system/core/EntitySystem.wurst`): wraps units, effects, and timers in `EffectsEntity` objects, tracks pauses, integrates with TimeStop, and exposes helpers for safely adjusting scale, position, and time state of effects.
- **EffectsSystem**: convenience layer for spawning, reusing, and cleaning effect entities, including instant flashes and automatic TimeStop synchronization.
- **ElementList**: central registry for mapping elemental spell IDs to elements for kit-wide queries.
- **SpellAPI** (`system/math/SpellAPI.wurst`): helpers for cone math, angle normalization, and spatial checks.
- **Utility** (`wurst/utils/Utility.wurst`): lightweight helpers such as `unit.isGiant()` classification and queued `stop` orders for safely resetting AI.

### Movement, control, and combat flow
- **Missiles** (`system/core/Missiles.wurst`): Chopinski-inspired projectile framework with adjustable collision, terrain awareness, callbacks for unit/item/destructable hits, and timer-driven motion.
- **BindingProjectileSystem**: builds missiles that bind victims to anchors, manages tether visuals/effects, and keeps them compatible with TimeStop.
- **Tether**: maintains distance limits between units with optional durations and automatic cleanup when anchors/targets die or stop existing.
- **Pull**: periodic controller that drags units toward points or casters while respecting crowd-control immunities and terrain checks.
- **Knockback**: 3D physics for bouncing/sliding units, configurable friction/gravity, destructable interaction, and hooks for terrain-height providers.
- **Teleport**: safe placement utilities to move single units or groups (with spreading) and keep Frentity entities synchronized to new heightmaps.
- **TimeStop**: freezes units, effects, projectiles (basically everything), keeps track of stacked pauses & timestops, restoring original turn rates/time scales, and exposing timers for delayed unfreeze. When making abilities, ensure the ability takes TimeStop into consideration, all abilities interact with TimeStop one way or another, and they have to stay compatible.
- **Transformation**: handles hero morphs, ability level swaps, skins, and progress-bar tracking with optional callbacks when forms end.
- **Radar** (WIP): periodic enemy minimap pings centered on a source unit.
- **SoundSystem** (WIP): positional playback that iterates heroes and plays `SoundDefinition` instances for players inside a radius.

### UI, heroes, and content scaffolding
- **UI** (`wurst/system/ui`): chat system hooks, draft hero data, and nested `components/draft` for frame layouts. Combine with `wurst-table-layout` when assembling custom frames.
- **Heroes** (`wurst/game/heroes`): each hero folder implements abilities using the systems above (e.g., All Might's missiles, pulls, and transformations; Nagato's binding and summons).
- **Objects** (`wurst/system/objects`): compiletime ability/buff/unit creation scripts; extend these to register new IDs alongside the existing hero constants.

## External Frameworks and Dependencies
### Frentity ( `_build/dependencies/Frentity` )
- Provides the base `Entity`/`UnitEntity`/`FxEntity` classes, lifecycle loop (`EntityManagement.startEntityLoop`), and effect/texttag wrappers that our systems extend. Review `doc/DOCUMENTATION.md` for entity design patterns, buff usage, FText helpers, and how to attach entities to spawned units/items.
- Use Frentity buffs (`wurst/buff`) for stacking shield/auras, and `entity/Entity.wurst` subclasses when creating new unit logic—our `EntitySystem` builds on top of these abstractions.

### Wurst standard library highlights ( `_build/dependencies/wurstStdlib2/wurst` )
- **Closures** (`closures/`): typed callback helpers (`ClosureTimers`, `ClosureEvents`, `ClosureFrames`, `Execute`) for timers, events, frame scripts, and deferred work—stick with them instead of raw trigger callbacks.
- **Dummy systems** (`dummy/`): `DummyCaster`/`InstantDummyCaster` for instant spell casts, `DummyRecycler` for pooling dummy units, and `DummyDamage` or `Fx` helpers for effect spam without leaks.
- **Data structures** (`data/`): `HashMap`, `HashSet`, `HashList`, `LinkedList`, `Table`, and `BitSet` modules used heavily throughout the core systems; prefer these implementations for performance and iterators.
- **Math** (`math/`): vector/angle helpers, `Quaternion` for rotation composition and interpolation, `Matrices`, `Raycast`, `Interpolation`, `Bitwise`, and `BigNum` for advanced calculations.
- **Events** (`event/`): damage detection (`DamageEvent`, `DamageDetection`), order tracking (`LastOrder`), region hooks (`OnUnitEnterLeave`), and general `RegisterEvents` utilities.
- **File and sync** (`file/`): `FileIO`, `SaveLoadData`, `Serializable`, `ChunkedString`, `SyncSimple`, and `Base64` for persistence, serialization, and multiplayer-safe data exchange.
- **Utility layer** (`util/`): `UnitIndexer`, `GroupUtils`, `TerrainUtils`, `EffectUtils`, `TimerUtils`, `GameTimer`, `MapBounds`, `SoundUtils`, `Simulate3dSound`, `Preloader`, `Time`, and string/printing helpers that power many gameplay scripts.
- **Handle wrappers** (`_handles/`): high-level wrappers for every Warcraft handle (`Unit`, `Ability`, `Effect`, `Group`, `Timer`, `Widget`, etc.) plus `primitives/` for color and vector helpers—use these classes instead of raw handles to stay aligned with Frentity and the stdlib.
- **Other toolkits**: `dummy/Fx2` for advanced effect management, `objediting/` presets for compiletime object generation, and `_wurst/assets` for default art references.

### Additional dependencies
- **InventoryUtils**: stack & split mechanics, smooth pickup behavior, inventory event hooks, item restrictions, and recipes—wire these into hero kits to standardize inventory UX.
- **wurst-bonus-handler**: configurable API for adjusting stats (life, mana, regen, damage, attributes, etc.) via hidden abilities; remember to respect its max bonus constants.
- **wurst-table-layout**: frame layout utilities that pair well with `system/ui` when building complex UI compositions.

## Assets, kits, and configuration
- Use the `imports/` directory for models, textures, and sounds referenced by abilities (e.g., Nagato assets, generic dummy models, UI art).
- `misc/kits` contains hero and item design documents or Wurst snippets slated for future implementation—consult these before authoring new abilities.
- The top-level `README.md` and `Hello.wurst` illustrate initialization patterns, including how to spawn test units and wire chat commands via the event system.

## Coding Conventions
### Formatting and structure
- Indent with tabs (size 4) wherever possible; only fall back to four spaces if a tool forbids tabs, and never mix in 8-space indents.
- Keep indentation consistent and avoid mixing tabs and spaces.
- No semicolons—Wurst is not Lua.
- Declare imports at the top of each package using proper syntax; respect package structure and dependencies.

### Naming and language rules
- Avoid reserved names such as `step`, `in`, `base`, `destroy`, `handle`, `ability`, `for`, etc. and other Wurst keywords for variables/functions.
- Wurst reserves identifiers like `destroy`, `step`, `to`, `in`, and `for`; pick alternative names when in doubt.
- Vararg functions may only declare a single parameter.
- Arrays cannot be used as function parameters.
- Do not declare methods `public` inside `public` classes—members are exported by default.
- `@compiletime` functions take no parameters and run during compilation.
- The `@config` annotation belongs only in packages ending with `_config`; use `@configurable` constants elsewhere for tunables.
- Function parameters are immutable.
- Not allowed to set Function parameters values in the parameters itself.
- There is no `continue` keyword in Wurst.
- `return` statements belong at the end of functions (not inside constructors).

### Types and expressions
- Prefer `let` when the type is obvious and the value should remain constant; use `var` for mutable bindings.
- Do not compare primitives to `null` (e.g., avoid `int == null`).
- Follow Wurst's null-safety patterns—check for `null` before dereferencing and prefer guard clauses.
- Tuples are available and encouraged where a lightweight struct fits.
- Use proper inheritance syntax when implementing interfaces.
- Distinguish clearly between static and instance methods.
- Use `this` explicitly when accessing members inside classes as needed.
- Understand Wurst string interpolation and comparison semantics—they differ from other languages.
- Class constructors use the `construct()` syntax.
- Type parameters may only bind to `int` or class types.

### Memory, handles, and lifecycle
- Destroy objects with `destroy x` (or dedicated helpers) instead of calling `x.destroy()` unless a helper exists.
- Objects created with `new` require explicit cleanup (`ondestroy` is the destructor hook). Be mindful of reference cycles and manual reference counting.
- There is no such things as "continue" or "super.destroy()" or params with "...", eg: function test(type x, unit watcher...).
- Clean up temporary objects promptly; avoid creating new objects inside hot loops, and prefer pooling patterns when appropriate.
- Manage unit, item, and player handles carefully—indexing is often 0-based in code even when UI shows 1-based players.
- You cannot `destroy` framehandles; treat them as persistent once created.
- Use `BlzGetAbilityIcon(unitID)` to resolve ability icons.
- Timer callbacks, event handlers, and closures must use the standard closure syntax from the stdlib.
- Employ efficient Warcraft III patterns (e.g., object pooling, recycling dummies) to prevent leaks.

### Patterns, UI, and conversions
- When converting vJass code, prefer class-based designs to take advantage of Wurst semantics.
- Do not rely on nonexistent frame APIs such as `BlzFrameGetAbsX` or `BlzFrameGetAbsY`.

## Testing
- Launch the map via the Wurst setup (`F1` → “Run a Wurst map”) and select `BaseMap.w3x` to exercise gameplay changes.

## PR Guidelines
- The upstream workflow typically builds and tests locally; external contributors ordinarily do not raise manual pull requests.
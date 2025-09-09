# Module Usage Guide

## AnimationWrapper
Use `AnimationController` to play animations on a unit.
```wurst
let anim = new AnimationController(u)
anim.playAttack()
```

## ElementList
Categorize abilities by element.
```wurst
registerElementalSpell(Element.fire, ALL_MIGHT_Q)
```

## Knockback
`knockbackUnit` wraps the advanced `Knockback3` system.
```wurst
knockbackUnit(target, caster.getPos(), 400., 0.5)
```

## Missiles
Create and launch projectiles.
```wurst
let m = new Missiles(from, to)
m.setSpeed(800.)
m.launch()
```

## Pull
Draw units toward a point.
```wurst
pullUnit(u, center, 300., 0.5)
```

## StatusEffects
Apply timed crowd control and buffs.
```wurst
stunUnit(target, 1.)
slowUnit(target, 0.5, 3.)
```

## Transformation
Temporarily change a unit's animation state.
```wurst
transformUnit(caster, 5., "slam")
```

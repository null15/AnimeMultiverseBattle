# --------------------------------------- [My_Notes] ---------------------------------------

# Decided to give up on native abilities for the most part and code the AI logic myself, apparently melee AI is stupid.
# How AI works: Timer handles running the state --> Events run swapping states

# --------------------------------------- [System] ---------------------------------------

1. **Time Stop** should put a "pause" on current acting spells, not "abruptly" end them. Meaning, during another player's midcast, it should resume where it left off after the **Time Stop** ends (if they got affected by **Time Stop**).

2. Sounds should follow the same logic. Any sound should be killed/stopped. After the affected ability/unit is no longer affected by **Time Stop**, does the Sound play again (play the sound anew again), using the API `sound.playPosition(int millisecs)` to play where it left off. There is no API to get a sounds current played duration, therefore a Sound Class to constantly keep track of played sounds and what duration they currently are at (ms) should be implemented. There is an API to get a sounds duration in length, for a way to track if the played duration tracker is not exceeding this length. One has to be careful about Desync problems with this API however, as it can return different sizes, thus debatable if to use or not.

```js
/** Returns sound length in milliseconds.

    Beware that sound lengths of game assets may differ between different locales,
    and thus return a different duration.
    If you use a async duration in a synced manner, it will cause a desync. */
public function sound.getDuration() returns int
    return GetSoundDuration(this)
```

3. Channel based abilities abruptly ends when the unit is paused/pausedEx, therefore all spells should no longer be based on channel, and an alternative approach to this issue has to be implemented. Perhaps with the help of Frames/OSKEY listener or issueOrders could be one approach to this problem.

4. Nagato R, Teleport, works uniquely in that, even if Nagato (the caster) gets **Time Stopped**, the Teleport will still go through, because **Animal Path** is the one actually teleporting, Nagato is simply controlling it. Therefore there is two ways to stop Teleport, either Stunning/Silencing/issueStop/etc on Nagato, to stop the ability (this means Animal Path is no longer being controlled and can't teleport. **Time Stop** does not end casts, as stated on #1). However if **Animal Path** gets **Time Stopped**, the Teleportation will be put on hold until the **Time Stop** ends. Once **Time Stop** ends, does it resume again where it left off. Nagato (the caster) is still "casting/channeling" the Teleportation, albeit delayed due to **Animal Path** currently being **Time Stopped**.

Similarly to Chakra Rod, if **Time Stopped**, the duration of Chakra Rod will not go down while **Time Stopped**, and only resume when it is no longer **Time Stopped**.
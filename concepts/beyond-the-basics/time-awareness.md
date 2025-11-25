# Time Awareness

In autonomous, every second counts. Even the most reliable and fast routines can fall short if they don’t account for the clock. Without time awareness, your robot risks:

* Running out of time before finishing a scoring cycle.
* Wasting the final seconds driving nowhere instead of parking.
* Missing an opportunity to grab last-second points with a last-ditch attempt score.

This is where **time-aware strategies** come into play. By keeping track of the remaining time in a match, your robot can make better decisions dynamically.



## Real-World Scenarios

* The robot is mid-cycle with only 1 second left—so it pushes for maximum speed to score the game element.
* With 3 seconds remaining, the robot knows it doesn't have enough time to attempt one more cycle—so it chooses to park and secure guaranteed points.
* A subsystem can automatically change modes at the 30-second teleop mark (e.g., preparing a hang mechanism).

The possibilities are truly endless.



## Using Marrow

To make this easier, Marrow provides [`TimerEx`](../../core-features/timerex.md).

Instead of writing your own timers, [`TimerEx`](../../core-features/timerex.md) gives you a clean and readable interface for checking elapsed/remaining time and triggering behaviors based on the match clock.

For a full breakdown of how to use it in your own code, see the [`TimerEx` reference page](../../core-features/timerex.md).

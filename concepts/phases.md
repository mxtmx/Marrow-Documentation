---
description: How to leverage phases for time-aware strategies
---

# Phases

In FTC competitions, matches have distinct phases. For example, in autonomous, you have the driving phase and then a park phase. In teleop, you have the main driving phase and the endgame. Each phase has different objectives and time constraints.

Without phase awareness, your robot treats the entire match as one continuous period. This leads to:

* Spending too much time on low-value tasks when you should be securing endgame points.
* Attempting complex maneuvers when you should be parking for guaranteed points.
* Losing track of how much time you have left for the current objective.

**Match phases** solve this problem by dividing your match into logical segments with their own durations. Your code can react to phase transitions, changing strategy dynamically as the match progresses.

***

## Real-World Scenarios

* Your autonomous runs scoring cycles for 28 seconds, then parks in the final 2 seconds.
* Your teleop executes aggressive scoring for 100 seconds, then switches to endgame behavior (hang, climb, etc.) in the final 20 seconds.
* A subsystem knows it's in endgame and prioritizes hang mechanism deployment over other tasks.
* Your command scheduler switches to a different command group when entering the final phase.

***

## Using Marrow

To make this easier, Marrow provides [`PhaseManager`](../automations/phasemanager.md).

Instead of manually tracking time and implementing phase logic yourself, you create a `PhaseManager` instance that automatically transitions your match through custom-defined phases and provides methods to query the current phase, remaining time, and phase transitions.

For a full breakdown of how to use it in your own code, see the [`PhaseManager` reference page](../automations/phasemanager.md).

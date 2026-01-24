---
description: package com.skeletonarmy.marrow.phases
---

# PhaseManager

## Overview

`PhaseManager` tracks the current phase of the match and provides time information. It allows you to define custom match phases and automatically transitions through them based on elapsed time.

Use `PhaseManager` to implement time-aware autonomous and teleop strategies. Register listeners to trigger different command groups when phases change, or query the current phase and remaining time to make dynamic decisions.

Check out the ["Phases" page](../concepts/phases.md) to learn how to use `PhaseManager` for phase-aware behaviors.

***

## Creating Phases

A `Phase` represents a segment of the match with a name, duration, and optional time unit. Durations default to seconds but can use any `TimeUnit`:

```java
// Seconds (default)
Phase autonomousPhase = new Phase("Autonomous", 28);
Phase parkPhase = new Phase("Park", 2);

// Custom time units
Phase phase = new Phase("Quick", 500, TimeUnit.MILLISECONDS);
```

**Phase Equality**: Phases are compared **by name only**. Two phases with the same name are considered equal, even if they have different durations or time units. This affects both `equals()` and `hashCode()` behavior.

***

## Usage

To use `PhaseManager`, you need to:

1. **Initialize** it at the start of your OpMode with your phases:
   ```java
   Phase autonomousPhase = new Phase("Autonomous", 28);
   Phase parkPhase = new Phase("Park", 2);
   PhaseManager.init(this, autonomousPhase, parkPhase);
   ```

2. **Update** it once per loop iteration:
   ```java
   PhaseManager.update();
   ```

3. **Query** the current phase and time as needed:
   ```java
   if (PhaseManager.isCurrentPhase("Park")) {
       driveToParking();
   }
   
   if (PhaseManager.getTimeRemaining() < 5) {
       // Do something with final seconds
   }
   ```

The timer starts automatically when the match begins (when the robot state becomes `RUNNING`).

***

## Common Methods

### Phase Methods

**`getName()`**

Get the name of a phase:

```java
Phase phase = new Phase("Endgame", 20);
String name = phase.getName(); // "Endgame"
```

**`getDuration()` / `getUnit()`**

Get the duration and time unit as originally specified:

```java
Phase phase = new Phase("Quick", 500, TimeUnit.MILLISECONDS);
double duration = phase.getDuration(); // 500
TimeUnit unit = phase.getUnit(); // TimeUnit.MILLISECONDS
```

**`getDurationSeconds()`**

Get the duration converted to seconds (useful when phases use different time units):

```java
Phase phase = new Phase("Quick", 500, TimeUnit.MILLISECONDS);
double seconds = phase.getDurationSeconds(); // 0.5
```

**`toString()`**

Get the phase name as a string:

```java
Phase phase = new Phase("Endgame", 20);
String str = phase.toString(); // "Endgame"
```

### PhaseManager Methods

**`isCurrentPhase(String name)` / `isCurrentPhase(Phase phase)`**

Check which phase you're in:

```java
if (PhaseManager.isCurrentPhase("Park")) {
    // Execute parking routine
}
```

**`getCurrentPhase()`**

Get the current phase object:

```java
Phase current = PhaseManager.getCurrentPhase();
```

**`getElapsedTime()`**

Total elapsed time since match start (in seconds):

```java
double elapsed = PhaseManager.getElapsedTime();
```

**`getTimeRemaining()` / `getPhaseTimeRemaining()`**

Time remaining in the match or current phase (in seconds):

```java
double matchRemaining = PhaseManager.getTimeRemaining();
double phaseRemaining = PhaseManager.getPhaseTimeRemaining();
```

**`getTotalMatchDuration()`**

Total match duration (sum of all phase durations):

```java
double total = PhaseManager.getTotalMatchDuration();
```

**`addPhaseListener(PhaseListener)` / `removePhaseListener(PhaseListener)`**

Listen to phase transitions. Register a `PhaseListener` that will be called whenever the robot enters a new phase:

```java
PhaseManager.addPhaseListener(newPhase -> {
    if (newPhase.getName().equals("Endgame")) {
        // ENTERING ENDGAME!
    }
});

// Remove specific listener
PhaseManager.removePhaseListener(myListener);
```

**`clearPhaseListeners()`**

Remove all registered phase listeners at once:

```java
PhaseManager.clearPhaseListeners();
```

***

## Example: Phase-Based Behavior

```java
@TeleOp
public class MyTeleOp extends LinearOpMode {
    @Override
    public void runOpMode() {
        Phase teleopPhase = new Phase("Teleop", 100);
        Phase endgamePhase = new Phase("Endgame", 20);
        
        PhaseManager.init(this, teleopPhase, endgamePhase);
        
        PhaseManager.addPhaseListener(phase -> {
            if (phase.getName().equals("Endgame")) {
                telemetry.addLine("ENTERING ENDGAME!");
            }
        });
        
        waitForStart();
        
        while (opModeIsActive()) {
            PhaseManager.update();
            
            if (PhaseManager.getTimeRemaining() < 5) {
                telemetry.addLine("FINAL 5 SECONDS");
            }
            
            telemetry.addData("Phase", PhaseManager.getCurrentPhase().getName());
            telemetry.addData("Time Remaining", String.format("%.1f", PhaseManager.getTimeRemaining()));
            telemetry.update();
        }
    }
}
```

***

## Notes

- **Phase ordering matters**: Phases are transitioned in the order you provide to `init()`. Make sure your durations align with your actual match strategy.
- **Match start detection**: The timer starts when the robot state becomes `RUNNING` (i.e., when `waitForStart()` completes).
- **Phase listeners are resilient**: Exceptions in phase listeners won't break your match. Errors are logged to telemetry.
- **Phases are compared by name**: Two `Phase` objects are equal if they have the same name, regardless of duration or time unit. This means `equals()` and `hashCode()` are based on the phase name only.
- **PhaseManager is stateful**: `PhaseManager` is a static manager. Don't instantiate it directly; call static methods instead.
- **Phases are transitioned in order**: The match progresses through phases sequentially. The last phase continues until the match ends, even if elapsed time exceeds its duration.

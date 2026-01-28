---
description: package com.skeletonarmy.marrow.phases
---

# PhaseManager

## Overview

`PhaseManager` tracks the current phase of the match and provides time information. Create an instance with your custom match phases, and it automatically transitions through them based on elapsed time.

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

***

## Usage

To use `PhaseManager`, you need to:

1. **Create an instance** in your OpMode's `init()` with your phases:
   ```java
   Phase autonomousPhase = new Phase("Autonomous", 28);
   Phase parkPhase = new Phase("Park", 2);
   PhaseManager phaseManager = new PhaseManager(this, autonomousPhase, parkPhase);
   ```

2. **Update** it once per loop iteration:
   ```java
   phaseManager.update();
   ```

3. **Query** the current phase and time as needed:
   ```java
   // Check by phase reference
   if (phaseManager.isCurrentPhase(parkPhase)) {
       driveToParking();
   }
   
   // Or check by name
   if (phaseManager.isCurrentPhase("Park")) {
       driveToParking();
   }
   
   if (phaseManager.getTimeRemaining() < 5) {
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
// Check by name
if (phaseManager.isCurrentPhase("Park")) {
    // Execute parking routine
}

// Check by phase reference
if (phaseManager.isCurrentPhase(parkPhase)) {
    // Execute parking routine
}
```

**`getCurrentPhase()`**

Get the current phase object:

```java
Phase current = phaseManager.getCurrentPhase();
```

**`getElapsedTime()`**

Total elapsed time since match start (in seconds):

```java
double elapsed = phaseManager.getElapsedTime();
```

**`getTimeRemaining()` / `getPhaseTimeRemaining()`**

Time remaining in the match or current phase (in seconds):

```java
double matchRemaining = phaseManager.getTimeRemaining();
double phaseRemaining = phaseManager.getPhaseTimeRemaining();
```

**`getTotalMatchDuration()`**

Total match duration (sum of all phase durations):

```java
double total = phaseManager.getTotalMatchDuration();
```

**`addPhaseListener(PhaseListener)` / `removePhaseListener(PhaseListener)`**

Listen to phase transitions. Register a `PhaseListener` that will be called whenever the robot enters a new phase:

```java
phaseManager.addPhaseListener(newPhase -> {
    if (newPhase.getName().equals("Endgame")) {
        // ENTERING ENDGAME!
    }
});

// Remove specific listener
phaseManager.removePhaseListener(myListener);
```

**`clearPhaseListeners()`**

Remove all registered phase listeners at once:

```java
phaseManager.clearPhaseListeners();
```

***

## Example: Phase-Based Behavior

```java
@TeleOp
public class MyTeleOp extends LinearOpMode {
    private PhaseManager phaseManager;
    private Phase teleopPhase;
    private Phase endgamePhase;
    
    @Override
    public void runOpMode() {
        teleopPhase = new Phase("Teleop", 100);
        endgamePhase = new Phase("Endgame", 20);
        
        phaseManager = new PhaseManager(this, teleopPhase, endgamePhase);
        
        phaseManager.addPhaseListener(phase -> {
            if (phase.getName().equals("Endgame")) {
                telemetry.addLine("ENTERING ENDGAME!");
            }
        });
        
        waitForStart();
        
        while (opModeIsActive()) {
            phaseManager.update();
            
            if (phaseManager.getTimeRemaining() < 5) {
                telemetry.addLine("FINAL 5 SECONDS");
            }
            
            telemetry.addData("Phase", phaseManager.getCurrentPhase().getName());
            telemetry.addData("Time Remaining", String.format("%.1f", phaseManager.getTimeRemaining()));
            telemetry.update();
        }
    }
}
```

***

## Notes

- **Phase ordering matters**: Phases are transitioned in the order you provide to the constructor. Make sure your durations align with your actual match strategy.
- **Match start detection**: The timer starts when the robot state becomes `RUNNING` (i.e., when `waitForStart()` completes).
- **Phase listeners are resilient**: Exceptions in phase listeners won't break your match. Errors are logged to telemetry.
- **Instance-based design**: `PhaseManager` is an instance-based class. Create an instance using `new PhaseManager(opMode, phases...)` and call methods on that instance.
- **Phases are transitioned in order**: The match progresses through phases sequentially. The last phase continues until the match ends, even if elapsed time exceeds its duration.

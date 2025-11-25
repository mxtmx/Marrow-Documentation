---
description: package com.skeletonarmy.marrow
---

# TimerEx

## Overview

`TimerEx` is an extended timer utility built on top of `ElapsedTime`. It provides convenient methods for tracking elapsed time, remaining time, and includes extra features such as pause/resume support.

`TimerEx` can be used as:

* A simple stopwatch (no duration provided).
* A countdown timer for match time.

Check out the ["Time Awareness" page](../concepts/beyond-the-basics/time-awareness.md) for advanced usage.

***

## Usage

### Setup

To use `TimerEx`, simply instantiate it with your desired settings (default unit is seconds):

```java
TimerEx stopwatch = new TimerEx();
TimerEx stopwatch = new TimerEx(TimeUnit unit);

TimerEx timer = new TimerEx(double duration);
TimerEx timer = new TimerEx(double duration, TimeUnit unit);
```

To start the timer, call `start()`:

```java
TimerEx timer = new TimerEx(30);
timer.start();
```



### Methods

The `TimerEx` class provides the following methods:

| Method                        | Description                                                     |
| ----------------------------- | --------------------------------------------------------------- |
| `start()`                     | Starts the timer (safe to call multiple times, only runs once). |
| `restart()`                   | Resets the timer.                                               |
| `pause()`                     | Pauses the timer.                                               |
| `resume()`                    | Resumes the timer.                                              |
| `getElapsed()`                | Returns the time passed since the timer was started.            |
| `getRemaining()`              | Returns the remaining time for the duration.                    |
| `isLessThan(double timeLeft)` | Returns `true` if remaining time is less than `timeLeft`.       |
| `isMoreThan(double timeLeft)` | Returns `true` if remaining time is greater than `timeLeft`.    |
| `isDone()`                    | Returns `true` if the timer reached 0.                          |
| `isOn()`                      | Returns `true` if the timer is currently running.               |



## Example Usage

Here’s an example autonomous that uses `TimerEx` together with SolversLib’s command system:

{% hint style="info" %}
This is by no means a functional autonomous program.
{% endhint %}

```java
@Autonomous
public class MyAuto extends CommandOpMode {
    TimerEx matchTime = new TimerEx(30); // 30 second autonomous
    DriveSubsystem driveSubsystem = new DriveSubsystem();

    @Override
    public void initialize() {
        schedule(
                // Display the remaining time
                new RunCommand(() -> telemetry.addData("Time Remaining", matchTime.getRemaining())),

                // Choose actions based on time left
                new ConditionalCommand(
                    new ScoreCommand(), // If enough time remains
                    new ParkCommand(driveSubsystem), // If almost out of time
                    () -> matchTime.isMoreThan(3)
                ),

                // Attempt a fast score if only a few seconds remain
                new ConditionalCommand(
                    new ScoreCommand(), // If enough time remains
                    new FastScoreCommand(), // If almost out of time
                    () -> matchTime.isMoreThan(3)
                ),

                // Automatically park when 5 seconds remain
                new SequentialCommandGroup(
                        new WaitUntilCommand(() -> matchTime.isLessThan(5)),
                        new ParkCommand(driveSubsystem)
                )
        );
    }

    @Override
    public void run() {
        super.run();
        telemetry.update();

        matchTime.start(); // Runs only once
    }
}
```

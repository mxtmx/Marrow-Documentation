---
description: package com.skeletonarmy.marrow.<command_library>
---

# RetryCommand

## Overview

`RetryCommand` is a powerful command for building adaptable and reliable command sequences. It executes a command and, if the specified condition is not met upon completion, automatically retries up to the specified amount of times.

This is especially useful for actions that may fail on the first attempt, such as vision-based alignment, object grabbing, or precise mechanism positioning.

Check out the ["Retries" page](../concepts/beyond-the-basics/retries.md) for more info.

{% hint style="info" %}
**Available in:**

🟢 [SolversLib](https://docs.seattlesolvers.com/command-base/command-system/convenience-commands#retrycommand) — included natively (Marrow not required)\
🟠 [FTCLib](https://docs.ftclib.org/ftclib/) — provided through Marrow\
🟠 [NextFTC](https://nextftc.dev/) — provided through Marrow

See [Installation](../getting-started/quickstart.md) for setup details.
{% endhint %}

***

## Usage

### Setup

To use `RetryCommand`, you need to provide the constructor with:

* **Command to run** – the initial action you want to execute.
* **(Optional) Alternative command to run on retries** – lets you customize the retry behavior per attempt (e.g., switching to a vision-assisted command if the initial attempt fails).
* **Success condition** – a boolean supplier that checks whether the action was successful. If this condition returns `false`, the command will be retried.
* **Maximum number of retries** – defines the maximum number of times the command can be retried.

```java
new RetryCommand(
    new GrabCommand(claw),   // Command to run.
    () -> claw.isGrabbed(),  // If this condition is false
    5                        // retry up to 5 times.
)
```

Or with an alternative retry command:

```java
new RetryCommand(
    new GrabCommand(claw),                  // Command to run initially.
    new DetectAndGrabCommand(claw, vision), // Command to run on each retry.
    () -> claw.isGrabbed(),                 // If this condition is false
    5                                       // retry up to 5 times.
)
```



## Example Usage

Here’s an example autonomous that uses `RetryCommand` together with SolversLib’s command system:

{% hint style="info" %}
This is by no means a functional autonomous program.
{% endhint %}

```java
@Autonomous
public class MyAuto extends CommandOpMode {
    private ClawSubsystem clawSubsystem;
    private VisionSubsystem visionSubsystem;

    @Override
    public void initialize() {
        clawSubsystem = new ClawSubsystem(hardwareMap);
        visionSubsystem = new VisionSubsystem(hardwareMap);

        schedule(
            new SequentialCommandGroup(
                // First, try to grab. If unsuccessful, retry grabbing up to 3 times.
                new RetryCommand(
                    new GrabCommand(clawSubsystem),
                    () -> clawSubsystem.isHoldingGameElement(),
                    3
                ),
                
                // ---- OR ----
                
                // First, try to grab. If unsuccessful, try to detect and grab using vision up to 3 times.
                new RetryCommand(
                    new GrabCommand(clawSubsystem),
                    new DetectAndGrabCommand(clawSubsystem, visionSubsystem), 
                    () -> clawSubsystem.isHoldingGameElement(),
                    3
                )
            )
        );
    }
}
```



## Source Code

You can check out the source code on the [GitHub repository](https://github.com/Skeleton-Army/Marrow/tree/main/customLibraries).

If you are using an unsupported command-based library and want to implement this command in your codebase, you may do so.

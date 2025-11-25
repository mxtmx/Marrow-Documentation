---
description: package com.skeletonarmy.marrow.prompts
---

# Prompter

## Overview

`Prompter` is a pre-match configuration tool that displays a sequence of interactive prompts as telemetry. This allows you to easily set parameters such as alliance color, starting position, and strategy using the gamepad—right before a match.

This eliminates the need for multiple OpModes with hardcoded settings, reducing clutter.

<figure><img src="../../.gitbook/assets/WhatsApp Video 2025-09-08 at 22.09.07.gif" alt="A GIF showing Prompter in action" width="360"><figcaption></figcaption></figure>

***

## Usage

### Setup

To create a `Prompter`, simply instantiate it with the current `OpMode` instance:

```java
Prompter prompter = new Prompter(this);
```



### Enqueuing Prompts

Prompts are added to the `Prompter` using `prompt(String, Prompt<T>)`.

```java
prompter.prompt("alliance", new OptionPrompt<>("Select Alliance", Alliance.RED, Alliance.BLUE))
        .prompt("delay", new ValuePrompt("Start Delay", 0.0, 10.0, 0.0, 1.0))
        .prompt("enablePark", new BooleanPrompt("Enable Park?", true));
```

Each prompt is tied to a key so you can retrieve the result later:

```java
Alliance alliance = prompter.get("alliance");
double delaySeconds = prompter.get("delay");
boolean shouldPark = prompter.get("enablePark");
```



### Conditional Prompts

You can conditionally add prompts using a `Supplier<Prompt<T>>`. This lets you decide whether to show a prompt based on earlier choices.

```java
prompter.prompt("parkLocation", () -> {
    if (prompter.get("enablePark").equals(true)) {
        return new OptionPrompt<>("Select Park Location", 1, 2);
    }
    return null; // Skip if the driver chose not to park
});
```

You can retrieve the result using `getOrDefault(string, T)`. If there is no selection for the prompt, it will return the provided default value.

```java
int parkLocation = prompter.getOrDefault("parkLocation", 0);
```



### Using Results

You can run a function after all prompts are complete using `onComplete(Runnable)`. You should retrieve prompt values inside this function, since only then are they guaranteed to be set.

```java
prompter.onComplete(() -> {
    Alliance alliance = prompter.get("alliance");
    double delay = prompter.get("delay");
    boolean enablePark = prompter.get("enablePark");
    int parkLocation = prompter.getOrDefault("parkLocation", 0);
});
```



### **Running the** Prompter

To start the `Prompter`, simply call `run()` in your loop function. This will run until all prompts are completed.

```java
prompter.run();
```



### Controls

Use the following gamepad controls to navigate the prompt interface:

* <kbd>D-PAD</kbd>: Navigate the prompt
* <kbd>A</kbd>: Confirm the current selection
* <kbd>B</kbd>: Go back to the previous prompt



## Example Usage

```java
@Autonomous
public class MyAuto extends OpMode {
    enum Alliance {
        RED,
        BLUE
    }

    private Prompter prompter = new Prompter(this);

    @Override
    public void init() {
        prompter.prompt("alliance", new OptionPrompt<>("Select Alliance", Alliance.RED, Alliance.BLUE))
                .prompt("delay", new ValuePrompt("Start Delay", 0.0, 10.0, 0.0, 1.0))
                .prompt("enablePark", new BooleanPrompt("Enable Park?", true))
                .prompt("parkLocation", () -> {
                    if (prompter.get("enablePark").equals(true)) {
                        return new OptionPrompt<>("Select Park Location", 1, 2);
                    }
                    return null; // Skip if the driver chose not to park
                })
                .onComplete(this::onPromptsComplete);
    }
    
    public void onPromptsComplete() {
        Alliance alliance = prompter.get("alliance");
        double delay = prompter.get("delay");
        boolean enablePark = prompter.get("enablePark");
        int parkLocation = prompter.getOrDefault("parkLocation", 0);

        telemetry.addData("Selected Alliance", alliance);
        telemetry.addData("Selected Delay", delay);
        telemetry.addData("Is Parking?", enablePark);
        telemetry.addData("Selected Park Location", parkLocation);
        telemetry.update();
    }

    @Override
    public void init_loop() {
        prompter.run();
    }

    @Override
    public void loop() {}
}
```

### What the Driver Sees

On init, the `Prompter` will display the following on the Driver Hub:

```
Select Alliance:

1) RED <
2) BLUE
```

Prompts will appear one at a time in this format until all selections have been made.

---
description: package com.skeletonarmy.marrow.prompts
---

# ValuePrompt

## Overview

`ValuePrompt` is a numeric input prompt used in pre-match configuration screens with [`Prompter`](./). It allows drivers to adjust a value within a defined range using the gamepad. The current value, bounds, and increment are clearly displayed in telemetry and can be confirmed with a button press.

***

## Usage

To create a new prompt, provide a header, minimum, maximum, default value, and increment:

```java
new ValuePrompt("Select a Start Delay:", 0.0, 10.0, 2.0, 0.5)
```

This will display a value prompt starting at 2, adjustable in increments of 0.5, clamped between 0 and 10. The selected value is returned when the user presses the <kbd>A</kbd> button.

### Controls

* <kbd>D-PAD UP / RIGHT</kbd>: Increase value
* <kbd>D-PAD DOWN / LEFT</kbd>: Decrease value
* <kbd>A</kbd>: Confirm selection

### Telemetry Output Example

```
Select a Start Delay:

< 2.0 >
```

After pressing <kbd>D-PAD UP</kbd>:

```
Select a Start Delay:

< 2.5 >
```

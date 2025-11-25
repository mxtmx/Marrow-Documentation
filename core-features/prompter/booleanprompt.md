---
description: package com.skeletonarmy.marrow.prompts
---

# BooleanPrompt

## Overview

`BooleanPrompt` is a simple yes/no toggle used in pre-match configuration screens with [`Prompter`](./). It allows drivers to switch between `true` and `false` values using the `D-PAD` and confirm the selection with a button press. The current selection is clearly shown as "Yes" or "No" in telemetry.

***

## Usage

To create a new prompt, provide a header and a default value:

```java
new BooleanPrompt("Enable Auto Score?", true)
```

This will display a prompt that starts with `"Yes"` and toggles between `"Yes"` and `"No"`. The selected value is returned when the user presses the <kbd>A</kbd> button.

### Controls

* <kbd>Any D-PAD Direction</kbd>: Toggle value (Yes / No)
* <kbd>A</kbd>: Confirm selection

### Telemetry Output Example

```
Enable Auto Score?

Current Value: Yes
```

After pressing any <kbd>D-PAD</kbd> direction:

```
Enable Auto Score?

Current Value: No
```

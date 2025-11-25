---
description: package com.skeletonarmy.marrow
---

# LynxUtil

## Overview

`LynxUtil` is a utility class that provides easy one-liner methods to handle bulk caching.

***

## Usage

### Set Bulk Caching Mode

Set the bulk caching mode for all Lynx modules in a given `HardwareMap`. Bulk caching can improve performance by reducing the number of hardware reads.

```java
// Automatic bulk reads
QoL.setBulkCachingMode(hardwareMap, LynxModule.BulkCachingMode.AUTO);

// Manual bulk reads
QoL.setBulkCachingMode(hardwareMap, LynxModule.BulkCachingMode.MANUAL);
```

{% hint style="info" %}
**Tip:** If you're unfamiliar with bulk reads, refer to the [GM0 Bulk Reads](https://gm0.org/en/latest/docs/software/tutorials/bulk-reads.html) explanation for details.
{% endhint %}

### Clear Bulk Cache

Clears the bulk data cache for all Lynx modules. In `MANUAL` bulk caching mode, clearing the cache is necessary so that subsequent sensor readings are up-to-date and not using stale data.

```java
QoL.clearBulkCache(hardwareMap);
```

{% hint style="info" %}
**Tip:** Clearing the cache is unnecessary in `AUTO` mode, as it automatically updates data each cycle. For more information, see the [GM0 Bulk Reads](https://gm0.org/en/latest/docs/software/tutorials/bulk-reads.html) explanation.
{% endhint %}

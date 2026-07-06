---
title: Power On/Off
description: AIEdge Micro AI Agent Server — Proper Startup & Shutdown Procedures
---

# Power On/Off

## Startup Procedure

### Normal Startup

The AIEdge device **powers on automatically when connected to power** — there is no separate power switch.

```
Connect Power → Auto Power-On → System Boot → Services Ready
    (0s)           (0~2s)         (20~30s)      (30~60s)
```

### Startup LED Sequence

The device has a single blue power LED:

| Time Marker | LED | Description |
|:---------|:-------|:-----|
| Power connected | 🔵 Blinking Blue | Hardware power-on |
| After 5 seconds | 🔵 Blinking Blue | Bootloader |
| After 20 seconds | 🔵 Slow Blinking | Linux kernel boot |
| After 30 seconds | 🔵 Solid Blue | System ready |
| After 45 seconds | 🔵 Solid Blue | Agent services ready |

!!! success "Startup Complete"
    When the **blue LED turns solid** and you can SSH into the device, startup is complete.

---

## Shutdown Procedure

### Normal Shutdown (Recommended)

!!! warning "Important"
    **Always perform a soft shutdown via command line.** Never unplug the power directly!

#### Shutdown via SSH

```bash
# SSH into the device
ssh root@[device IP]

# Run the shutdown command
shutdown -h now
```

Wait for the blue LED to turn off (about 10 seconds), then disconnect the power adapter.

### Forced Shutdown (Emergency Only)

!!! danger "Caution"
    Forced shutdown may cause **data loss** or **filesystem corruption**. Only use in these situations:

    - System unresponsive for more than 5 minutes
    - Cannot access via SSH
    - Device is abnormally hot with a burning smell

Forced shutdown procedure:

1. Press and hold the **reset button** for 10+ seconds
2. Or directly unplug the Type-C power cable (included with select models)

After a forced shutdown, the device will **automatically perform a filesystem check** on next power-up — startup may take 2-3 minutes.

---

## Restart Procedure

### Soft Restart (Recommended)

```bash
# Run after SSH login
shutdown -r now
# Or
reboot
```

Restart takes approximately **40-60 seconds**.

---

## LED Quick Reference

| LED | State | Meaning |
|:-------|:-----|:-----|
| 🔵 Blue | Solid | System running normally |
| 🔵 Blue | Slow blinking | System booting |
| 🔵 Blue | Fast blinking | System updating |
| ⚫ Off | — | Not powered |

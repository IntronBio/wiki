---
title: Unboxing Checklist
description: AIEdge Micro AI Agent Server — Unboxing & Inspection Steps
---

# Unboxing Checklist

## Step 1: Inspect the Outer Packaging

When you receive the package, check the outer packaging first:

!!! warning "Outer Packaging Checklist"
    - [ ] Is the outer packaging intact — no serious crushing, damage, or water stains?
    - [ ] Are the seals intact — no signs of re-packaging?
    - [ ] Does the shipping label match your order information?

If the packaging is seriously damaged or the seals appear tampered with, please **refuse delivery** and contact sales or technical support immediately.

---

## Step 2: Verify the Packing List

After opening the package, check each item against the table below:

| # | Item | Standard Qty | Check |
|:-----|:-----|:---------|:-------|
| 1 | AIEdge main unit | 1 | No scratches or dents |
| 2 | Power adapter | 1 | DC or Type-C, intact (included with select models) |
| 3 | Quick Start Guide | 1 | Available on request, under development |
| 4 | Warranty Card | 1 | Available on request, under development |

!!! note "Ethernet / HDMI Cables"
    Ethernet and HDMI cables are not included in the package. Contact customer service if you need to purchase them separately.

!!! note "Serial Number Location"
    The device serial number is located on the **bottom label** and on the **side of the outer packaging**. You'll need the serial number for warranty service — please keep it safe.

---

## Step 3: Inspect the Main Unit

Remove the main unit and inspect it under good lighting:

- [ ] No deformation, cracks, or noticeable scratches on the body
- [ ] Interface panel is not warped or loose
- [ ] Ventilation grilles are not blocked
- [ ] Bottom anti-slip pads are intact

---

## Step 4: Power On

After confirming the device looks good, connect power:

1. Connect the power adapter to the device's power port
2. Plug into an outlet; the front panel blue LED will light up:
    - :material-power: **Blinking blue** (~30 seconds) → System booting
    - :material-power: **Solid blue** → Boot complete, system ready

Once the device is ready, choose one of the following methods to access the system:

=== "Method 1: SSH Login (Recommended)"

    Find the device IP in your router admin panel (hostname: `aiedge`), then SSH from your computer:

    ```bash
    ssh root@192.168.1.xxx
    ```

    Default password: `hybio.tech`

=== "Method 2: Direct HDMI Connection"

    Connect the device to a monitor via HDMI, attach a keyboard, and you'll see the Ubuntu terminal login screen. Enter username `root` and password `hybio.tech` to proceed.

---

## Troubleshooting Abnormal Conditions

| Issue | Possible Cause | Action |
|:---------|:---------|:---------|
| Blue LED not lit | Adapter failure / loose connection | Re-plug, check outlet power |
| Blue LED blinking for >2 min | System boot failure | Press and hold reset for 10 seconds, then retry |
| Physical damage found | Shipping damage | Take photos, contact after-sales for replacement |
| Missing accessories | Packing omission | Contact sales for re-shipment |

!!! danger "Important"
    If the device shows **obvious burning smell, smoke, or abnormal heat**, **unplug immediately** and do not continue using it! Contact after-sales support.

---

## Next Step

After accessing the system, continue to **[First Deployment](deploy.md)** to configure your API Key and start agent services.

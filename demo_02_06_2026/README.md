# Demo — 2 June 2026

> **OsmAnd iOS** · Sprint fixes · 2 issues resolved

---

## #5355 — Stuttering / jerky map movement during navigation

**Task:** [#5355](https://github.com/osmandapp/OsmAnd-iOS/issues/5355) · **PR:** [#5425](https://github.com/osmandapp/OsmAnd-iOS/pull/5425)

### Problem

During navigation or navigation simulation, the map movement occasionally became jerky — stuttering or jumping roughly twice per minute with no consistent pattern. The issue persisted regardless of visual settings (Globe view, 3D mode) and felt like a brief synchronization hiccup between location updates and map rendering.

Two separate causes combined to produce this effect:

1. **`OAMyPositionLayer`** — every location update unconditionally cancelled *all* marker animations. When a compass heading update arrived mid-position-animation, it interrupted the smooth arrow movement, causing a visible jerk.
2. **`OAMapViewTrackingUtilities`** — `movingTime` could be `0` on the first GPS update (no previous location) or shorter than `1s` on fast GPS intervals, causing the camera to snap rather than animate smoothly.

### Fix

1. Heading-only updates (`animationDuration <= 0`) that arrive while a position animation is already running now cancel and restart *only* the azimuth animation — the position animation continues uninterrupted.
2. `NAV_ANIMATION_TIME` (`1s`) is now used as a minimum floor for `movingTime` in navigation mode, ensuring the camera always has a smooth animation budget.

### Before / After

**⚠️ Before**

<details><summary>Show video</summary>
<video src="https://github.com/user-attachments/assets/441b5a21-c6b1-44e9-bede-c9deb4af8060"></video>
</details>

**✅ After**

<details><summary>Show video</summary>
<video src="https://github.com/user-attachments/assets/1640e67f-8663-4559-bbec-b3208e0d0e9a"></video>
</details>

---

## #3241 — Show touches in Development plugin

**Task:** [#3241](https://github.com/osmandapp/OsmAnd-Issues/issues/3241) · **PR:** [#5436](https://github.com/osmandapp/OsmAnd-iOS/pull/5436)

### What was added

Added a **Show touches** toggle to the Appearance group of the OsmAnd Development plugin. When enabled, a component displays visual touch indicators on the screen — every touch is highlighted in real time, making it easier for QA and testing teams to demonstrate and report interactions.

### Result

<details><summary>Show video</summary>
<video src="https://github.com/user-attachments/assets/aeec3aef-3914-4d64-b5e8-dda2baf569a3"></video>
</details>

---

*Generated: 2 June 2026*

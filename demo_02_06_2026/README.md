# Demo — 2 June 2026

> **OsmAnd iOS** · Sprint fixes · 1 issue resolved

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

<table>
  <tr>
    <th align="center">⚠️ Before</th>
    <th align="center">✅ After</th>
  </tr>
  <tr>
    <td>
      <video src="files/5355_before.mp4" controls muted loop playsinline width="320"></video>
    </td>
    <td>
      <video src="files/5355_after.mp4" controls muted loop playsinline width="320"></video>
    </td>
  </tr>
</table>

---

*Generated: 2 June 2026*

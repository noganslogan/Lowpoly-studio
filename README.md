# FORM Low-Poly Studio v0.2.2: Trustworthy Editing

This release implements milestone 1 of the code review. It remains a primitive-based editor, with the same version 1 project format. Existing valid FORM projects remain compatible.

## Deploy from GitHub

In Cloudflare, open the existing `lowpoly` Worker and select **Settings → Builds → Connect**. Authorize GitHub and select this repository.

| Setting | Value |
|---|---|
| Production branch | `main` |
| Root directory | Repository root |
| Build command | `node tests/regression.cjs` |
| Deploy command | `npx wrangler deploy` |

The Wrangler configuration targets the existing Worker and serves `public/index.html`. Pushes to the production branch trigger a build and deployment. Open the existing Worker URL in Safari to use the updated app.

## Changes
- Anchored the mobile More menu to the header.
- Made snap and grid settings accessible in the mobile Edit panel.
- Commit active transforms before switching tools, starting a pinch, saving, exporting, undoing, or leaving the page.
- Cancel interrupted pointer transforms back to their starting state. Escape also cancels an active transform.
- Increased drag threshold from 2 to 6 pixels to reduce accidental edits.
- Corrected all pyramid face winding and normals.
- Ground placement now uses the lowest transformed mesh vertex, including rotated and unevenly scaled objects.
- Reject unsupported/inherited geometry types, invalid picking IDs, extreme transforms, and imports over 10 MB.
- Allocate valid unused object IDs when the current ID reaches the picking limit.
- Validate camera data consistently on import and startup; persist camera changes after navigation.
- Preserve input-native text undo while editing fields.
- Recover GPU resources when WebGL restores a lost context.
- Show an autosave-failure warning on phones and desktops.
- Added mobile safe-area spacing, wider numeric controls, and larger color targets.

## Controls
Tap an object to select it. Drag objects to transform: Move operates on X/Z, Rotate on Y, and Scale on all axes. Numeric fields allow other axes. Drag background to orbit; right-drag also orbits; middle-drag or Shift-background-drag pans. Pinch or wheel zooms. Mobile Edit includes workspace settings. More contains Undo, Redo, New, Open, and Save. Save downloads an editable JSON project; Export downloads GLB.

## Verification
Run `node tests/regression.cjs` with Node.js 18 or newer. The suite covers outward primitive normals, exact ground placement across all primitives with rotation/nonuniform scale, interrupted-touch undo/redo, cancellation, tool switching, invalid imports, ID allocation, save/reopen, camera recovery, mobile settings state, context-loss state, and GLB binary structure.

These are real geometry and JavaScript state tests using a lightweight DOM stub. GPU rendering, layout, downloaded-file behavior on iOS, and physical iPhone gestures still need device testing. Browser execution was unavailable in the build environment. Context-restored GPU rendering has not been exercised here.

## Remaining scope
Axis gizmos, grouping, multiselect, orthographic views, editable meshes, extrusion, and per-face color belong to subsequent milestones. Autosave remains one browser-local project slot; download backups before New or Open. WebGL picking still uses the display framebuffer, so edge selection remains a future rendering improvement.

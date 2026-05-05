# Qwinkle's Part-Icles 2

A custom particle emitter plugin for Roblox Studio. Turn regular instances into fully customizable emitters with graph-driven animation, nested particles, and a built-in texture inventory.

## Supported emitter types

Any of these can be transformed into a Part-Icle:

- **BaseParts** (Part, MeshPart, SpecialMesh). Emit actual 3D geometry, not billboards.
- **Models**. Emit whole Models as particles, with per-life Scale animation.
- **Attachments**. Parent-local CFrame, supports nesting particles inside other particles.
- **Beams**. Multi-state Transparency and Color blender over time, per-segment width control.
- **Trails**. Direct property editing with range support.
- **PointLights**. Animated Range, Brightness, Color.
- **Blur, Bloom, ColorCorrection, Atmosphere**. Screen-space effect emitters. Plays once or animates in place.
- **ImageLabels**. 2D screen-space particles with spritesheet flipbook cycling.

## Nesting

Any transformed item can live inside another transformed item. When the outer emitter fires, the emitted clone carries its nested emitters with it, and each nested emitter runs its own full lifecycle on the clone.

- Put a Beam inside a Part to emit a mesh that trails a beam behind itself.
- Put an Attachment with ParticleEmitters inside a Model so each emitted Model fires its own particles from authored positions.
- Stack ImageLabels inside ImageLabels for compound 2D effects with shared parent-relative sizing.
- Works across every combination of supported types.

EmitDelay on nested clones is cleared automatically so they fire the instant the outer emit runs (the parent's delay already elapsed). No wiring required; nesting is detected at emit time.

## Graph Editor

Author any property over the particle's life.

- Bezier and linear interpolation in the same graph. Toggle handles on or off per keypoint.
- Per-particle envelope (random variance around the curve). Works with both linear and bezier modes.
- 20 built-in presets. Linear, Ramp Down, Bell, Step, Bounce, and 13 easing curves (Sine, Quad, Cubic, Expo in In/Out/InOut flavors, plus Smooth).
- Simplify panel. Live reduction slider powered by the Ramer-Douglas-Peucker algorithm, plus an auto-minimize button that finds the smallest keypoint count within 5% deviation.
- 50-step undo/redo.
- Copy/paste graphs across properties. Ranges, handles, and keypoints all preserved; values auto-scale to the target range.
- **Env +** bulk envelope input. Adds a scalar to every keypoint at once.
- Multi-select keypoints via box-drag, ghost-preview before inserting a new keypoint, keyboard nudging with Shift for coarse/fine steps.

## Animatable properties

Over 70 properties across all types are graph-driven. A few that are probably unexpected:

- **Beam:** Width0, Width1, CurveSize0, CurveSize1, Segments, LightInfluence, TextureLength, TextureSpeed
- **PointLight:** Range and Brightness animate per particle
- **Bloom:** Intensity, Size, Threshold (dynamic glow curves)
- **ColorCorrection:** Brightness, Contrast, Saturation, TintColor (full color grading over a particle's life)
- **Atmosphere:** Density, Offset, Glare, Haze, Color, Decay (sky and fog animation per emit event)
- **ImageLabel:** SizeScaleX, SizeScaleY, RotSpeed, Position, and UDim2 Size
- **Model:** Scale over life
- **Part:** Brightness

## Inventory

Built-in texture and flipbook management. No tab-switching to a browser, no one-off manual uploads.

### Remote library

- 7800+ textures curated in categories (Shapes, Combat, Elements, Radiance, Misc, Other)
- Real-time search across name, ID, and keywords
- Multi-select category filter
- Tabs for All, Static, Flipbook, Favourites, Recent
- Favourites persist across sessions. Recent tracks the last 20 inserted items.

### Local inventory

- Upload PNG files directly from your computer with the Studio file picker
- Spritesheets can be uploaded from disk as well (they enter the library as spritesheet assets)
- Files are serialized and pushed to your Roblox account automatically. No manual upload step.
- Supports both individual textures and named flipbook sequences
- Persists in plugin settings plus a ServerStorage backup (survives reinstall)
- Rename, delete, favourite, and insert from the same panel

*(FYI: local upload requires Studio's beta features `CreateAssetAsync` and `EditableMesh / EditableImage Static Conversion API` enabled in Studio Settings.)*

### MeshFlipbook library

- 50+ curated pre-built flipbook sequences
- 24 FPS animated preview before insert
- One-click insert

### Spritesheet dissection

- Enter asset IDs or pull them from the current selection. Works on any public Roblox asset.
- Pick columns and rows; plugin splits the sheet into individual texture frames.
- Batch mode: queue dozens of spritesheets at once and dissect them all into MeshFlipbook sequences in one pass.
- The flipbook emit path also auto-detects native sheet dimensions at runtime, so emits render without scaling artifacts.

## Tools

Accessible from the Q-menu:

- **Resize:** scale selected items by 0.5x to 2x, with options to scale spawn volume and descendants.
- **Retime:** stretch or compress animation duration, with envelope scaling.
- **Hue:** HSV shift on any ColorSequence property, including Beam Blenders.
- **Clipboard:** copy property values between items, with per-type search filtering.
- **Shifter:** bulk math (add, subtract, multiply, divide) on values, with envelope scaling.
- **Scan:** detect and fix structural issues in a selection.
- **Insert Module:** drop the runtime module into ServerScriptService for live-game use.
- **Emit Code:** generate ready-to-paste Lua snippets for Moon Animator 2 and in-game scripts.

Plus:

- **Motion Preview:** live 3D trajectory visualization. Shows 20 envelope variants using the real emit simulation, and updates as you edit.
- **Link system:** Weld, Follow, or Pivot link modes. LinkSource can be the workspace Camera (cutscenes, HUD effects) or any picked Object.

## Animate mode

Non-destructive looping for orbits, pulses, and motion graphics. The source item stays editable while the animation plays. Per-instance loop toggle.

## Theme editor

- 10 built-in presets: Default Gold, Midnight Blue, Crimson Dark, Forest Green, Pitch Black, Monochrome, Dracula, Nord, Sunset, Custom
- 6 individual color slots (Accent, Background, Blob, Slider, Scrollbar, Text Accent)
- 60+ font options
- Panel and Blob opacity sliders
- Font scale from 50% to 200%
- Import and export themes as JSON

## Installation

1. Download **`QwinklesParticles2.rbxmx`** from this repository
2. Open Roblox Studio
3. Open the Explorer (**View → Explorer**)
4. Right-click **ServerStorage** → **Insert from File**
5. Select the downloaded `.rbxmx` file. A `Part_IclesV2` model appears under ServerStorage.
6. **Right-click** the new `Part_IclesV2` → **Save as Local Plugin...** → choose any save location and confirm.
7. **Restart Roblox Studio.**
8. After restart, the plugin toolbar button "Qwinkle's Part-Icles 2" will appear.

**Why two steps?** Inserting the file only places the model under ServerStorage — that does not register it as a plugin. "Save as Local Plugin" copies the model into Studio's plugin folder, which is what makes Studio load it as an actual plugin on the next launch. After this first install, future updates are picked up automatically (see below).

## Updates

The plugin auto-updates in-place. When a new version is available:

1. Open or toggle the plugin from the toolbar
2. A green banner will tell you the new version has been downloaded into ServerStorage
3. Right-click the new `Part_IclesV2` in ServerStorage → **Save as Local Plugin...**
4. Restart Studio

No GitHub visits, no re-inserting files.

## Purchase

Free 7-day trial starts the first time the plugin runs. After that, pick one of:

### Gumroad ($20 USD)

1. https://qwinkletee.gumroad.com/l/qwinklesparticles2
2. Enter your **Roblox User ID** in the custom field at checkout. (Open your Roblox profile; the number in the URL is your ID.)
3. You're whitelisted automatically. Reopen the plugin in Studio.

### Robux (8000 Robux)

1. Buy the gamepass: https://www.roblox.com/game-pass/1779373735/Qwinkles-PartIcles-2
2. Set your **Roblox inventory to Public** (Settings → Privacy)
3. Make sure you're verified with **Bloxlink** on the Discord server
4. In the `#whitelist` channel, run `/whitelist`
5. The bot verifies your purchase and whitelists you.

### Ko-fi / PayPal ($20 USD)

1. https://ko-fi.com/qwinkletee, click "Support QwinkleTee"
2. **Paste your Roblox User ID in the Message field before paying.** This is required. Without it, the whitelist cannot attach to your account.
3. You're whitelisted automatically. Reopen the plugin in Studio.

If you forgot to include your Roblox ID on a Ko-fi purchase, DM QwinkleTee on Discord with your Ko-fi receipt and Roblox User ID.

## Support

Join the Discord server for support, bug reports, and feature requests: https://discord.gg/GbAZwpTT

## License

All rights reserved. Provided as-is for use in Roblox Studio. Redistribution, decompilation, or reverse engineering is prohibited.

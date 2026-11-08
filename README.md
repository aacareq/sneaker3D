# sneaker3D — AERO GLIDE 3D showcase

A premium, single-file 3D product page for the fictional **AERO GLIDE** sneaker.
Everything (HTML, CSS, JavaScript, SVG icons) lives in [`index.html`](index.html).
[three.js r187](https://threejs.org) is loaded from jsDelivr through an import map,
so there's no build step.

- **Real model loading:** `./shoe.glb` is loaded with `GLTFLoader`, with a streamed download and an accurate progress bar.
  - Draco, Meshopt and KTX2 decoders are imported *only* when the file declares those extensions.
- **Auto-normalisation:** the model is measured, oriented (length → X, toe forward), pitched, scaled, centred and grounded.
  - Any model scale or axis convention works.
- **Responsive framing:** the camera distance and look-at are solved from the *actual* glass panel layout, so the shoe always sits in the free space between the panels.
  - This holds on any screen size or orientation, and the framing re-solves on resize.
- **Motion:** a damped turntable with auto-rotation and drag-with-inertia.
  - Touch pinch-zoom, wheel zoom and a hover bob.
  - Mouse-tracking parallax moves the camera, the model tilt and the backdrop glow.
- **Luxury dark-studio lighting:**
  - A custom PMREM softbox environment.
  - A warm key light, a colorway-tinted rim, a kicker and an overhead spot.
  - An overhead shadow light that casts a soft **VSM shadow** onto a glossy fading floor, plus a contact shadow, a light pool and an orbit ring.
- **Glassmorphism UI:**
  - The `AERO GLIDE` title, description, animated price tag and a hover-animated, magnetic `Pre-Order Now` button.
  - Tabs, tech sheet, size picker and colorway dock, toasts, a loader and an error screen.
- **Colorways:** the bundled model ships `KHR_materials_variants`. The swatches switch the real glTF material variants and re-theme the lights and UI.
- **Optimised:**
  - Module preloads, and the model is preloaded in parallel with the JavaScript.
  - Shaders are compiled before reveal, and variant textures are pre-uploaded in idle time.
  - Adaptive resolution holds the frame rate; the render loop *sleeps* when nothing moves.
  - Visibility-aware timing, WebGL context-loss recovery, and GPU teardown on page exit.
- **Resilient:**
  - If `shoe.glb` is missing, a procedurally generated sneaker is shown and a notice explains why.
  - If WebGL or the CDN is unavailable, a clear error screen appears.

## Run it

The page fetches `shoe.glb`, so it must be served over HTTP. Opening the file from disk
(`file://`) blocks the fetch and the procedural fallback model is shown instead.

```bash
# any static server works, for example:
python3 -m http.server 8080        # then open http://localhost:8080
npx serve .                        # or this
```

To deploy, upload `index.html` and `shoe.glb` to any static host (Vercel, Netlify,
GitHub Pages, S3, …). You don't need a server runtime.

## Use your own shoe

Replace `shoe.glb` with any glTF 2.0 binary:

- **Any scale or orientation.** The model is auto-normalised; the taller end is treated as the heel.
- **Compression is fine.** Draco (`KHR_draco_mesh_compression`), Meshopt and KTX2/Basis (`KHR_texture_basisu`) are supported.
- **Material variants** (`KHR_materials_variants`) become colorway swatches automatically.
  - Variant names matching a palette in `COLORWAYS` (`midnight`, `beach`, `street`) use that palette.
  - Other names get a cycled palette and a title-cased label.
- **Without variants**, the swatches switch the *studio lighting* palette instead.
- **Animations** in the file are played automatically.

## Controls

| Action | Mouse / trackpad | Touch | Keyboard |
| --- | --- | --- | --- |
| Rotate | drag | drag | `←` `→` |
| Orbit up/down | drag vertically | drag vertically | `↑` `↓` |
| Zoom | wheel / `+` `−` buttons | pinch | `+` `−` |
| Reset view | double-click / reset button | double-tap | `0` |
| Toggle auto-rotate | top-bar button | top-bar button | `R` |
| Colorway | dock swatches | dock swatches | `1`–`9` |
| Close tech sheet | — | tap the stage | `Esc` |

Auto-rotation resumes after 2.4 s of inactivity. With `prefers-reduced-motion`, the
intro, auto-rotation, bob and parallax are disabled.

## Customise

Everything tunable sits at the top of the module script in `index.html`:

- **`CONFIG`:**
  - model URL and CDN
  - camera FOV, elevation and azimuth
  - turntable speed and idle-resume delay
  - drag and parallax strength, zoom limits
  - hero pose, hover/bob, intro length
  - DPR cap, shadow softness, environment intensity and exposure
- **`COLORWAYS`:** per palette, the swatch gradient, UI accents, rim/key/under light colours and procedural-model materials.
- **CSS custom properties** in `:root`:
  - typography and spacing
  - glass tint
  - the accent colours (updated live by the active colorway)

The pre-order flow keeps the reservation (size, colorway, pair number) in
`localStorage` under `aeroglide:reservation:v1`. Activating the button again lets
the shopper review or release it. To connect a real checkout, call your
endpoint from `onPreorder()`.

## Browser support

Current Chrome, Edge, Firefox and Safari (desktop and mobile) with WebGL 2.
Import maps are required (Safari 16.4+).

## Credits

- 3D model: **"Materials Variants Shoe"** © 2021 Shopify, Inc., licensed under
  [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
  Source: [Khronos glTF Sample Assets](https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/MaterialsVariantsShoe).
- Rendering: [three.js](https://threejs.org) (MIT).
- Fonts: Archivo, Manrope and JetBrains Mono via Google Fonts (OFL).
- AERO GLIDE, AERO Atelier and AeroFoam are fictional names for this demo.

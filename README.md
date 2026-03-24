# Focal Point & Smart Crop Custom Element

A lightweight custom element for **Kontent.ai** that helps editors generate crop URLs from a selected image using either:

- **Focal Point Crop** (manual focus selection), or
- **Smart Crop** (AI-driven smart cropping).

It provides a visual workflow for selecting focal points, previewing common aspect ratios, and saving ready-to-use transformed asset URLs into the custom element value.

## What It Does

- Loads an image from:
  - direct URL input, or
  - Kontent.ai asset picker (`CustomElement.selectAssets`).
- Supports two crop modes:
  - **Focal Point Crop**: click image to define focus coordinates.
  - **Smart Crop**: generates smart-crop URLs (`fit=crop&crop=smart`).
- Generates preview links for preset ratios and custom `w/h`.
- Shows a focal marker and optional crop guide overlay in focal mode.
- Persists results as JSON via `CustomElement.setValue(...)`.

## Main Features

- Crop mode toggle (**Focal** / **Smart**).
- Preset crop previews for common aspect ratios.
- Custom dimensions preview (`w`, `h`).
- Focal point coordinate readout (`x`, `y` + percent).
- Visual crop guide:
  - Off / preset / custom overlay options
  - Custom overlay can be resized with handles
  - Corner resize keeps ratio; side resize changes one axis
  - Disabled in Smart Crop mode

## Output Value Structure

The custom element stores a JSON object with URLs for each generated variant.

Example:

```json
{
  "assetUrl": "https://.../image.jpg",
  "fX": 1234,
  "fY": 567,
  "cropMode": "focal",
  "assetUrlLandscape": "https://.../image.jpg?rect=...",
  "assetUrlSquare": "https://.../image.jpg?rect=...",
  "assetUrlVertical": "https://.../image.jpg?rect=...",
  "assetUrlPortrait45": "https://.../image.jpg?rect=...",
  "assetUrlStable43": "https://.../image.jpg?rect=...",
  "assetUrlPanorama": "https://.../image.jpg?rect=...",
  "assetUrlCustom": "https://.../image.jpg?rect=..."
}
```

Notes:

- `fX` / `fY` are saved only when `cropMode` is `"focal"`.
- In smart mode, generated URLs use `w`, `h`, `fit=crop`, `crop=smart`.
- In focal mode, generated URLs use `rect=x,y,w,h`.

## Typical Editor Flow

1. Load image (paste URL or select from Kontent.ai assets).
2. Choose **Focal Point Crop** or **Smart Crop**.
3. (Focal mode) Click image to set focal point.
4. Inspect preset and custom previews.
5. Front end page apps than can use the generated URLs from saved JSON fields.

## Tech Notes

- Single-file implementation (`index.html`) with vanilla HTML/CSS/JS.
- Uses Kontent.ai Custom Element API (`CustomElement.init`, `setValue`, `setHeight`, `selectAssets`).
- Designed to run both:
  - embedded in Kontent.ai editor, and
  - standalone for local testing.

# 3D Transformation Visualizer

An interactive web tool that shows how 3D transformations (move, scale, rotate) change a shape, and how they combine into a single 4×4 matrix. Built for learning computer graphics and multimedia systems concepts.

It is one HTML file with no dependencies and no build step. The 4×4 matrices and the perspective projection are computed in plain JavaScript and drawn with the Canvas 2D API, so every step of the math is visible in the code.

This is the 3D companion to `transformation-visualizer.html` (2D, 3×3 matrices).

## Features

- **Three transformations** with sliders: Move (X, Y, Z), Scale (X, Y, Z), and Rotate (X, Y, Z in degrees). Negative scale values produce reflections.
- **Live 4×4 matrix.** The combined matrix `M` updates as you change any value.
- **Order of operations.** Choose any of 6 orders of Scale, Rotate, and Move. Changing the order changes the result, which shows that matrix multiplication is not commutative.
- **Step view.** "Steps" draws each intermediate stage in its own color with a label. "Play steps" animates the operations one after another.
- **Matrix math, step by step.** For each stage, shows `operation × previous = result` with real numbers.
- **Pivot choice.** Scale and Rotate can pivot around the origin or around the shape's own center.
- **Shapes.** Block, Pyramid, and Prism. One face of each shape is colored amber so you can see how the shape turns.
- **Orbit camera.** Drag the canvas to rotate the view. Use the Zoom slider or the mouse wheel to zoom. Toggle Perspective on or off to compare perspective and orthographic views.
- **Reference scene.** Colored X (red), Y (green), and Z (blue) axes and a floor grid.
- **Presets.** Turn 90° around X, Y, or Z, double, half, and flip X.
- **Export.** Save the canvas as a PNG, or copy the current matrix as text.
- **Mobile friendly.** The canvas stays pinned at the top of the screen while you use the controls. Dark mode follows the device setting.

## Tech Stack

- HTML, CSS, and vanilla JavaScript
- HTML Canvas 2D API for drawing
- Pointer Events for drag and touch input

## Getting Started

### Prerequisites

- Any modern browser (Chrome, Edge, Firefox, Safari)

### Installation

No installation is needed. Download `transformation-visualizer-3d.html`.

### Usage

Open the file in a browser:

```bash
# macOS
open transformation-visualizer-3d.html

# Linux
xdg-open transformation-visualizer-3d.html
```

On Windows, double-click the file. On a phone, open it from your file manager or host it on any static site.

Basic workflow:

1. Move the sliders for Move, Scale, and Rotate.
2. Drag the canvas to look at the shape from other angles.
3. Read the matrix under **Combined matrix M (4×4)**.
4. Open **Matrix math, step by step** to see how `M` was built.
5. Change **Order** and compare the result. Use **Play steps** to watch each operation happen in turn.

## How the math works

The tool uses homogeneous coordinates. A point `(x, y, z)` is written as the column vector `[x, y, z, 1]ᵀ` and transformed by `[x′, y′, z′, 1]ᵀ = M · [x, y, z, 1]ᵀ`. The coordinate system is right-handed with Y pointing up.

| Operation | Matrix |
|---|---|
| Move | `[[1,0,0,tx],[0,1,0,ty],[0,0,1,tz],[0,0,0,1]]` |
| Scale | `diag(sx, sy, sz, 1)` |
| Rotate X | `[[1,0,0,0],[0,cos,−sin,0],[0,sin,cos,0],[0,0,0,1]]` |
| Rotate Y | `[[cos,0,sin,0],[0,1,0,0],[−sin,0,cos,0],[0,0,0,1]]` |
| Rotate Z | `[[cos,−sin,0,0],[sin,cos,0,0],[0,0,1,0],[0,0,0,1]]` |

- Angles are in degrees. The three rotations are combined as `R = Rz · Ry · Rx`, so the shape rotates around X first, then Y, then Z.
- Operations are multiplied so that the first operation is the rightmost matrix. For the order "Scale → Rotate → Move", `M = T · R · S`.
- When the pivot is the shape center, Scale and Rotate use `T(p) · A · T(−p)`, where `p` is the pivot point. Move is not affected by the pivot.

### Camera and projection

The camera orbits the origin. Each point is rotated by the camera's yaw (around Y) and pitch (around X), then projected onto the screen:

- Perspective: the on-screen scale of a point is `D / (D − z)`, where `D` is the camera distance set by the Zoom slider.
- Orthographic: the scale is the same for every point.

Faces are sorted from far to near and drawn in that order (painter's algorithm).

## Project Structure

```
transformation-visualizer-3d.html   # HTML, CSS, and JavaScript in one file
README-3d.md
```

Inside the HTML file, the script is organized as: shape data (vertices and faces), 4×4 matrix helpers (`mul`, `mats`, `stages`), camera projection (`proj`), drawing (`draw`), UI updates (`ui`), then event handlers for the sliders, play animation, export, and canvas orbit input.

## Known Issues / Limitations

- Faces are depth-sorted by their average depth and there is no back-face culling. Large or intersecting faces can sometimes be drawn in the wrong order.
- Rotation uses fixed-order Euler angles (X, then Y, then Z). Some combinations can lose a degree of freedom (gimbal lock), and the sliders will not always feel intuitive.
- The pivot is limited to the origin or the shape's original center. It does not follow the shape after a Move.
- Features available in the 2D version are not included here: shear, challenge mode, custom pivot, custom matrix input, and drawing your own shape.
- "Copy matrix" depends on browser clipboard permission and may be blocked on some pages.

## Roadmap

- Shear and a custom matrix input
- Quaternion rotation to compare with Euler angles
- Challenge mode in 3D
- Merge the 2D and 3D tools into one app with tabs

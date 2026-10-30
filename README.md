# Transformation Visualizer

An interactive web tool that shows how 2D transformations (move, scale, shear, rotate) change a shape, and how those operations combine into a single 3×3 matrix. Built for learning computer graphics and multimedia systems concepts.

It is one HTML file. It has no dependencies, no build step, and works offline.

## Features

- **Four transformations** with sliders: Move, Scale, Shear, and Rotate. Negative scale values produce reflections.
- **Drag to move.** Drag the shape on the canvas to change its Move values.
- **Live matrix.** The combined 3×3 homogeneous matrix `M` updates as you change any value.
- **Order of operations.** Choose any of 24 orders of Scale, Shear, Rotate, and Move. Changing the order changes the result, which shows that matrix multiplication is not commutative.
- **Step view.** "Show steps" draws each intermediate stage in its own color with a label. "Play steps" animates the operations one after another.
- **Matrix math, step by step.** For each stage, shows `operation × previous = result` with real numbers.
- **Pivot choice.** Rotate, Scale, and Shear can pivot around the origin, the shape center, or a custom point set by dragging the red dot.
- **Shapes.** F, Square, Triangle, Arrow, or a polygon you draw yourself.
- **Challenge mode.** A green target outline appears. Match it using the controls. A live match percentage is shown, and the challenge is marked solved when the shapes line up. Three levels: Easy (move), Medium (move and rotate), Hard (move, rotate, and scale).
- **Your own matrix.** Type the six editable cells of a 3×3 matrix and apply it directly to the shape. "Copy current M here" fills the cells from the current sliders.
- **Presets.** Flip left-right, flip up-down, turn 90°, double, half, and shear.
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

No installation is needed. Download `transformation-visualizer.html`.

### Usage

Open the file in a browser:

```bash
# macOS
open transformation-visualizer.html

# Linux
xdg-open transformation-visualizer.html
```

On Windows, double-click the file. On a phone, open it from your file manager or host it on any static site.

Basic workflow:

1. Move the sliders, or drag the shape on the canvas.
2. Read the matrix under **Combined matrix M**.
3. Open **Matrix math, step by step** to see how `M` was built.
4. Change **Order** and compare the result.
5. Try **Start challenge** to practice.

## How the math works

The tool uses homogeneous coordinates. A point `(x, y)` is written as the column vector `[x, y, 1]ᵀ` and transformed by `[x′, y′, 1]ᵀ = M · [x, y, 1]ᵀ`.

| Operation | Matrix |
|---|---|
| Move | `[[1,0,tx],[0,1,ty],[0,0,1]]` |
| Scale | `[[sx,0,0],[0,sy,0],[0,0,1]]` |
| Shear | `[[1,hx,0],[hy,1,0],[0,0,1]]` |
| Rotate | `[[cos θ,−sin θ,0],[sin θ,cos θ,0],[0,0,1]]` |

- Positive angles rotate counter-clockwise. Angles are entered in degrees.
- Operations are multiplied so that the first operation is the rightmost matrix. For the order "Scale → Rotate → Move", `M = T · R · S`.
- When the pivot is not the origin, Scale, Shear, and Rotate use `T(p) · A · T(−p)`, where `p` is the pivot point. Move is not affected by the pivot.

## Project Structure

```
transformation-visualizer.html   # HTML, CSS, and JavaScript in one file
README.md
```

Inside the HTML file, the script is organized as: shape data, matrix helpers (`mul`, `mats`, `stages`), drawing (`draw`), UI updates (`ui`), then event handlers for the sliders, challenge mode, export, and canvas pointer input.

## Known Issues / Limitations

- The pivot is a fixed point on the plane. It does not follow the shape after a Move.
- Points in the draw-your-own-shape mode snap to a 0.5 grid. The shape needs at least 3 points, otherwise the F shape is used.
- Challenge targets are generated without negative scale, so reflections are not part of the challenges. Matching compares vertices of the same shape, so a drawn shape with many symmetric vertices may score unevenly.
- "Copy matrix" depends on browser clipboard permission and may be blocked on some pages.
- 2D only. A 3D version with 4×4 matrices is not included.

## Roadmap

- 3D transformations with 4×4 matrices (for example with Three.js)
- Reflections in challenge mode
- Inverse matrix display and "undo transformation"

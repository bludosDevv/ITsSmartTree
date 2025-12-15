# 🌳 ITs Smart Tree - Ultimate Procedural Engine

**ITs Smart Tree** is a lightweight, single-file 3D procedural vegetation generator built entirely in JavaScript and WebGL (Three.js). 

It generates unique, organic 3D trees in real-time using recursive algorithms and procedural texture generation. It features a complete UI for customization and supports exporting to industry-standard 3D formats (GLB & OBJ) for use in Blender, Unity, Unreal Engine, or Godot.

![Tree Engine Preview](https://via.placeholder.com/800x400?text=ITs+Smart+Tree+Preview)

## ✨ Features

*   **100% Procedural:** No external assets or textures required. All bark and leaf textures are generated on-the-fly using the HTML5 Canvas API.
*   **Recursive Growth Algorithm:** Simulates natural tree branching patterns (splitting, gravity, noise/jitter).
*   **High Performance:**
    *   Uses `InstancedMesh` for foliage rendering (rendering thousands of leaves with a single draw call).
    *   Merges trunk geometries using `BufferGeometryUtils` for optimized topology.
*   **Dual Render Modes:**
    *   **Realistic:** Smooth branches with Catmull-Rom splines and crossed-plane leaves.
    *   **Voxel (LOD):** Cube-based generation for low-poly/voxel art styles.
*   **Export Pipeline:**
    *   **GLB (glTF Binary):** Best for web and modern engines.
    *   **OBJ:** Universal format (includes a custom geometry baker to convert instances to static mesh).

## 🚀 Quick Start

Because this is a single-file application, deployment is instant.

1.  **Download** the `index.html` file.
2.  **Run** it using a local server (recommended for module security) or simply drag and drop it into a browser (modern browsers may require a local server due to CORS policies on ES Modules).
    *   *VS Code Tip:* Right-click `index.html` and select "Open with Live Server".
3.  **Use:** Tweak parameters in the UI and click "Generate New Tree".

## 🛠️ How It Works (The Engine)

The core logic resides inside the `<script type="module">` block at the bottom of the file. Here is a breakdown of the architecture:

### 1. The Recursive `grow()` Function
The engine uses a recursive function to simulate biological growth. 
*   **Inputs:** Position, Direction, Length, Radius, and Depth (Recursion limit).
*   **Logic:** 
    1.  Calculates the end point of a branch.
    2.  Adds "Noise" (random jitter) to simulate organic imperfections.
    3.  Applies "Gravity" (bends the vector downwards based on branch weight).
    4.  Generates geometry (Tube for realistic, Box for voxel).
    5.  **Recursion:** If `depth > 0`, it spawns new `grow()` calls from the end point, branching out based on the `Spread Angle` and `Density` settings.

### 2. Geometry & Optimization
*   **Trunks:** We don't add every branch to the scene immediately. Instead, we push geometries to an array and use `BufferGeometryUtils.mergeGeometries()` to create one single mesh. This drastically reduces GPU overhead.
*   **Leaves:** We use `THREE.InstancedMesh`. We calculate the transformation matrix (position, rotation, scale) for every leaf, but

Create a complete, self-contained single HTML file (index.html) for an interactive 3D Rubik's Cube web application and solver. All necessary HTML, CSS, and JavaScript code must be embedded within this single file. Use the Three.js library for 3D rendering, loaded via CDN.

**Core Features:**

1.  **3D Cube Display:**
* Render a standard 3x3x3 Rubik's Cube in the center of the view.
* Use standard Rubik's Cube face colors (White, Yellow, Red, Orange, Blue, Green).
* Ensure individual cubelets are distinct.

2.  **User Interaction:**
* Allow the user to rotate the entire cube's view using right-click + drag (orbit controls).
* Allow zooming in/out using the mouse wheel.
    * Allowing clicking a part of the rubiks cube and dragging it to move that part(Both regular and ')

3.  **Cube Manipulation Controls (UI Panel):**
    * Create a clear UI panel (e.g., using HTML/CSS positioned absolutely or relatively).
    * Include buttons for standard face rotations using standard notation: F, F', B, B', U, U', D, D', R, R', L, L'.
    * Include buttons for middle slice rotations: M, M', E, E', S, S'.
    * Clicking a rotation button should animate the corresponding face/slice turn smoothly on the 3D cube model.
    * When rotating using right-click + drag, make sure the faces are recalculated so the f is still the one in the front and not in the back. Only recalculate 3 seconds after the last drag rotation so it isn't constantly calculating.

4.  **Scramble Functionality:**
    * Include a "Scramble" button.
    * When clicked, apply a sequence of 20-25 random, valid Rubik's Cube moves to the cube state.
    * Animate the scrambling sequence visually on the 3D cube.
    * Display a status message like "Scrambling..." during the process.

5.  **Solve Functionality:**
    * Include a "Solve" button.
    * When clicked, implement an efficient Rubik's Cube solving algorithm (like Kociemba's two-phase algorithm or a similar efficient method) to find the solution steps from the current scrambled state.
    * Animate the solving sequence visually on the 3D cube, applying the solution moves step-by-step. The animation should be clear and reasonably paced.
    * Display a status message like "Solving..." during the process and "Cube solved!" upon completion.


**Technical Requirements & Style:**
* Use modern JavaScript (ES6+).
* Embed all CSS within `<style>` tags and all JavaScript within `<script>` tags in the single HTML file.
* Load Three.js library from a reliable CDN (e.g., cdnjs or jsdelivr).
* Code should be well-structured, commented, and reasonably efficient.
* The UI should be clean and functional.
* Animations for rotations, scrambling, and solving should be smooth.


Ensure the final output is a single, runnable HTML file that accurately implements these features.

**Script URLs needed:**
```javascript
<script type="importmap">
{
  "imports": {
    "three": "https://cdn.jsdelivr.net/npm/three@0.177.0/build/three.module.min.js",
    "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.177.0/examples/jsm/"
  }
}
</script>
```

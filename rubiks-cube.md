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
* Create a clear UI panel (e.g., using HTML/CSS positioned absolutely or relatively).
* Include buttons for standard face rotations using standard notation: F, F', B, B', U, U', D, D', R, R', L, L'.
* Include buttons for middle slice rotations: M, M', E, E', S, S'.
* Clicking a rotation button should animate the corresponding face/slice turn smoothly on the 3D cube model.
* When rotating using right-click + drag, make sure the faces are recalculated so the f is still the one in the front and not in the back. Only recalculate 3 seconds after the last drag rotation so it isn't constantly calculating.

4.  **Scramble Functionality:**
* Include a "Scramble" button.
* When clicked, apply a sequence of 20-25 random, valid Rubik's Cube moves to the cube state.
* Animate the scrambling sequence visually on the 3D cube.
* Display a status message like "Scrambling..." during the process.

5.  **Solve Functionality:**
* Include a "Solve" button.
* When clicked, implement an efficient Rubik's Cube solving algorithm (like Kociemba's two-phase algorithm or a similar efficient method) to find the solution steps from the current scrambled state.
* Animate the solving sequence visually on the 3D cube, applying the solution moves step-by-step. The animation should be clear and reasonably paced.
* Display a status message like "Solving..." during the process and "Cube solved!" upon completion.


**Technical Requirements & Style:**
* Use modern JavaScript (ES6+).
* Embed all CSS within `<style>` tags and all JavaScript within `<script>` tags in the single HTML file.
* Load Three.js library from a reliable CDN (e.g., cdnjs or jsdelivr).
* Code should be well-structured, commented, and reasonably efficient.
* The UI should be clean and functional.
* Animations for rotations, scrambling, and solving should be smooth.


Ensure the final output is a single, runnable HTML file that accurately implements these features.

**Script URLs needed:**
```javascript
<script type="importmap">
{
  "imports": {
    "three": "https://cdn.jsdelivr.net/npm/three@0.177.0/build/three.module.min.js",
    "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.177.0/examples/jsm/",
    "cube": "https://cdn.jsdelivr.net/npm/cubejs@1.3.2/lib/cube.min.js",
    "solve": "https://cdn.jsdelivr.net/npm/cubejs@1.3.2/lib/solve.min.js"
  }
}
</script>
```


# cube.js
## Examples

`cube.js` gives you basic cube manipulation:

- Web:

```javascript
// Create a new solved cube instance
const cube = new Cube();

// Apply an algorithm or randomize the cube state
cube.move("U F R2 B' D2 L'");
cube.randomize();

// Create a new random cube
const randomCube = Cube.random();
```

- Node:

```javascript
const Cube = require('cubejs');

// Create a new solved cube instance
const cube = new Cube();

// Apply an algorithm or randomize the cube state
cube.move("U F R2 B' D2 L'");
cube.randomize();

// Create a new random cube
const randomCube = Cube.random();
```

From `solve.js` you can use the methods below, to solve the cube using [Herbert Kociemba's two-phase algorithm](http://kociemba.org/cube.htm).

- Web:

```javascript
// This takes 4-5 seconds on a modern computer
Cube.initSolver();

// These typically take from 0.01s to 0.4s, rarely up to 2s
cube.solve();        // => "D2 B' R' B L' B ..."
randomCube.solve();  // => "R B' R U' D' R' ..."
```

To offload the solving to a web worker, use `async.js` and `worker.js`:

```javascript
Cube.asyncInit('lib/worker.js', function() {
    // Initialized
    Cube.asyncSolve(randomCube, function(algorithm) {
        console.log(algorithm);
    });
});
```

- Node:

```javascript
const Cube = require('cubejs');

// This takes 4-5 seconds on a modern computer
Cube.initSolver();

// These typically take from 0.01s to 0.4s, rarely up to 2s
cube.solve();        // => "D2 B' R' B L' B ..."
randomCube.solve();  // => "R B' R U' D' R' ..."
```


## API Reference

All functionality is implemented in the `Cube` object.
There are a bunch of files, of which `cube.js` is always required.

### `cube.js`

`cube.js` gives you basic cube manipulation.

#### Class methods

##### `Cube([state])`

The constructor:
- without arguments, constructs an identity cube (i.e. a solved cube).
- with an argument, clones another cube or cube state.

```javascript
const cube = new Cube();
const other = new Cube(cube);
const third = new Cube(cube.toJSON());
```

##### `Cube.fromString(str)`

Returns a cube that represents the given facelet string.
The string consists of 54 characters, 9 per face:

```javascript
"UUUUUUUUUR...F...D...L...B..."
```

`U` means a facelet of the up face color, `R` means a facelet of the right face color, etc.

The following diagram demonstrates the order of the facelets:

```
             +------------+
             | U1  U2  U3 |
             |            |
             | U4  U5  U6 |
             |            |
             | U7  U8  U9 |
+------------+------------+------------+------------+
| L1  L2  L3 | F1  F2  F3 | R1  R2  R3 | B1  B2  B3 |
|            |            |            |            |
| L4  L5  L6 | F4  F5  F6 | R4  R5  R6 | B4  B5  B6 |
|            |            |            |            |
| L7  L8  L9 | F7  F8  F9 | R7  R8  R9 | B7  B8  B9 |
+------------+------------+------------+------------+
             | D1  D2  D3 |
             |            |
             | D4  D5  D6 |
             |            |
             | D7  D8  D9 |
             +------------+
```

##### `Cube.random()`

Return a new, randomized cube.

```javascript
const cube = Cube.random();
```

##### `Cube.inverse(algoritm)`

Given an algorithm (a string, array of moves, or a single move), returns its inverse.

```javascript
Cube.inverse("F B' R");    // => "R' B F'"
Cube.inverse([1, 8, 12]);  // => [14, 6, 1]
Cube.inverse(8);           // => 6
```

See below for numeric moves.

#### Instance methods

##### `init(state)`

Resets the cube state to match another cube.

```javascript
const random = Cube.random();
const cube = new Cube();
cube.init(random);
```

##### `identity()`

Resets the cube to the identity cube.

```javascript
cube.identity();
cube.isSolved();  // => true
```

##### `toJSON()`

Returns the cube state as an object.

```javascript
cube.toJSON();  // => {cp: [...], co: [...], ep: [...], eo: [...]}
```

##### `asString()`

Returns the cube's state as a facelet string. See `Cube.fromString()`.

##### `clone()`

Returns a fresh clone of the cube.

##### `randomize()`

Randomizes the cube in place.

##### `isSolved()`

Returns `true` if the cube is solved (i.e. the identity cube), and `false` otherwise.

##### `move(algorithm)`

Applies an algorithm (a string, array of moves, or a single move) to the cube.

```javascript
const cube = new Cube();
cube.isSolved();  // => true
cube.move("U R F'");
cube.isSolved();  // => false
```

See below for numeric moves.

#### Numeric moves

Internally, cube.js treats moves as numbers.


| Move | Number |
|------|--------|
| U    | 0      |
| U2   | 1      |
| U'   | 2      |
| R    | 3      |
| R2   | 4      |
| R'   | 5      |
| F    | 6      |
| F2   | 7      |
| F'   | 8      |
| D    | 9      |
| D2   | 10     |
| D'   | 11     |
| L    | 12     |
| L2   | 13     |
| L'   | 14     |
| B    | 15     |
| B2   | 16     |
| B'   | 17     |

### `solve.js`

`solve.js` implements [Herbert Kociemba's two-phase algorithm](http://kociemba.org/cube.htm)
for solving the cube quickly in nearly optimal number of moves.
The algorithm is a port of his simple Java implementation without symmetry reductions.

For the algorithm to work, a computationally intensive precalculation step is required.
Precalculation results in a set of lookup tables that guide the heuristic tree search.
Precalculation takes 4-5 seconds on a typical modern computer, but migh take longer on older machines.

After precalculation is done, solving a cube with at most 22 moves typically takes 0.01-0.4 seconds, but may take up to 1.5-2 seconds.
Again, these figures apply for a modern computer, and might be bigger on older machines.

The maximum search depth (numer of moves) can be configured.
The deeper search is allowed, the quicker the solving is.
There's usually no reason to change the default of 22 moves.

#### Class methods

##### `Cube.initSolver()`

Perform the precalculation step described above. This must be called before calling `solve()`.

##### `Cube.scramble()`

Generate a random scramble by taking a random cube state, solving it, and returning the inverse of the solving algorithm.
By applying this algorithm to a cube, you end up to the random state.

#### Instance methods

##### `solve([maxDepth])`

Return an algorithm that solves the cube as a string. `maxDepth` is
the maximum number of moves in the solution, and defaults to 22.

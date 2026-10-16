# Basic Three.js WebXR Project with Keyboard Controls

*Note: This project is under development and not yet ready for testing.*

This is a basic Three.js WebXR project that sets up a VR scene with controller support, a laser pointer, and keyboard controls for movement and digging (as in classic Boulder Dash).

## How to Run

1. Ensure you have a web browser that supports WebXR (such as Firefox Reality, Chrome on Android with WebXR enabled, or a VR headset browser).
2. Open `index.html` in your browser.
3. The scene is rendered inline by default. Click the "Enter VR" button to start an immersive VR session.
4. Use the VR controllers to see the laser pointer and interact with the diamond (pointing at the diamond and pressing the trigger will change its color).
5. Use the keyboard:
   - W, A, S, D keys to move forward, left, backward, right.
   - Hold Shift while pressing W, A, S, D to dig (remove) the adjacent ground tile in that direction without moving.

## Grid System

The game features a grid system where each tile is 1x1 meter. The grid size is defined in the level JSON. The grid is represented by a 2D array with the following tile types:
- 0: Empty (no mesh rendered)
- 1: Dirt (diggable, rendered as a brown plane)
- 2: Wall (solid, not diggable, rendered as a dark gray cube)

The player can move onto empty or dirt tiles but cannot move through walls. Digging (Shift+WSAD) removes dirt tiles, turning them into empty tiles.

## Level Format

The game loads a level from a JSON object. The default level is embedded in `index.html` as `DEFAULT_LEVEL`. To create a custom level, modify this JSON object or, in the future, load it from an external file.

The level JSON has the following structure:

```json
{
  "width": 10,
  "height": 10,
  "timeLimit": 180,
  "diamondsRequired": 3,
  "grid": [
    ["W","W","W","W","W","W","W","W","W","W"],
    ["W","D","D","D","D","D","D","D","D","W"],
    ...
  ],
  "objects": [
    {"type": "player", "x": 1, "y": 1},
    {"type": "diamond", "x": 5, "y": 5}
  ]
}
```

- `width` and `height`: dimensions of the grid (must be equal for now, as the game assumes a square grid).
- `timeLimit`: time limit for the level in seconds (not yet implemented).
- `diamondsRequired`: number of diamonds needed to complete the level (not yet implemented).
- `grid`: a 2D array of characters representing each tile:
  - `W`: Wall
  - `D`: Dirt
  - ` `: Empty (space character)
- `objects`: an array of objects placed in the level:
  - `player`: the starting position of the player (x, y grid coordinates). If not provided, defaults to (1,1).
  - `diamond`: a diamond object to collect (x, y grid coordinates). Multiple diamonds can be specified.

The player is placed on the tile at the given coordinates, which should be a dirt tile.

## Default Level

The default level is a 10x10 grid with walls around the edges and dirt inside. The player starts at (1,1) and there is a single diamond at (5,5).

## Project Structure

- `index.html`: The main HTML file that loads Three.js from CDN and sets up the scene with a grid-based ground, VR controllers, and keyboard input handling. The level JSON is embedded in this file.
- `README.md`: This file.

## Notes

- The project uses Three.js version 0.162.0 from CDN.
- The grid size and tile types are defined in the level JSON.
- Controller input is handled to display a laser pointer and detect clicks on diamonds and other objects.
- Keyboard input is handled for movement (WSAD) and digging (Shift+WSAD).
- The VR button is provided by Three.js's VRButton helper.

## Development

To modify the project, edit the `index.html` file. The JavaScript is embedded in the HTML for simplicity.

To change the level, modify the `DEFAULT_LEVEL` object in the script.

For a more advanced setup, consider using a build system and npm packages, and loading levels from external JSON files.

## License

This project is for educational purposes and can be used as a starting point for WebXR experiments.
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

The game features a 10x10 grid system where each tile is 1x1 meter. The grid is initialized with walls around the edges and dirt inside. The grid is represented by a 2D array with the following tile types:
- 0: Empty (no mesh rendered)
- 1: Dirt (diggable, rendered as a brown plane)
- 2: Wall (solid, not diggable, rendered as a dark gray cube)

The player can move onto empty or dirt tiles but cannot move through walls. Digging (Shift+WSAD) removes dirt tiles, turning them into empty tiles.

## Project Structure

- `index.html`: The main HTML file that loads Three.js from CDN and sets up the scene with a grid-based ground, a diamond object, VR controllers, and keyboard input handling.
- `README.md`: This file.

## Notes

- The project uses Three.js version 0.162.0 from CDN.
- The ground is a 10x10 grid of tiles (each 1x1 unit) that can be dug away with Shift+WSAD.
- A diamond object is placed in front of the starting position for interaction with the laser pointer.
- Controller input is handled to display a laser pointer and detect clicks on the diamond.
- Keyboard input is handled for movement (WSAD) and digging (Shift+WSAD).
- The VR button is provided by Three.js's VRButton helper.

## Development

To modify the project, edit the `index.html` file. The JavaScript is embedded in the HTML for simplicity.

For a more advanced setup, consider using a build system and npm packages.

## License

This project is for educational purposes and can be used as a starting point for WebXR experiments.
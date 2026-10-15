# Implementation Plan for Boulder Dash in WebXR

## 1. Introduction and Goals
Recreate the classic 2D grid-based puzzle game Boulder Dash in immersive WebXR using modern web technologies. The game will preserve core mechanics: player-guided Rockford collects diamonds while avoiding falling boulders and hazards, with real-time physics and chain reactions. Target platforms include VR headsets (Meta Quest, HTC Vive) and browser-based WebXR support.

## 2. Architecture Overview
- **Rendering Engine**: Three.js with WebXR module for VR/AR support
- **Game State**: Fixed-timestep logic loop (10 ticks/second) synchronized with render loop
- **Data Model**: 2D grid array representing cave layout, with each cell containing object type and state
- **Entity Component System**: Lightweight ECS for object behaviors (movement, falling, interactions)
- **Input System**: Abstraction for VR controllers, gaze-based selection, and fallback keyboard/mouse
- **Level System**: JSON-based level format with metadata (time limit, diamond count, object placement)
- **Audio System**: Positional audio using Web Audio API for footsteps, falls, explosions

## 3. Key Components
- **Scene Manager**: Handles Three.js scene, camera, lights, and WebXR session
- **Grid System**: Converts grid coordinates to 3D world positions (1 unit = 1 meter)
- **Object Factory**: Creates 3D meshes and attaches behavior components based on object type
- **Physics Handler**: Custom gravity-based falling simulation (not full physics engine)
- **Interaction Controller**: Processes input to move Rockford or interact with adjacent cells
- **Rules Engine**: Implements game logic (falling, explosions, amoeba growth, magic walls)
- **UI/HUD**: Displays diamond count, timer, and cave info using CSS3D or sprite fonts
- **Level Loader**: Parses level data and populates initial grid state

## 4. Interaction Design
- **Primary Input**: VR controllers (thumbstick/trackpad for grid-based movement, trigger for action)
- **Alternative Input**: 
  - Gaze + click (controller trigger or tap) for selection
  - Keyboard fallback (arrow keys for movement, space for action)
- **Movement System**: 
  - Grid-aligned movement (one cell per input)
  - Smooth transition between cells (0.1s interpolation)
  - Teleport option for comfort (configurable)
- **Interaction Rules**:
  - Move into empty/dig: Replace dirt with player, leave empty space
  - Push: Not implemented (original game doesn't allow pushing)
  - Death: On contact with falling boulder, firefly, butterfly, or exploding amoeba
  - Special: Magic wall interaction via proximity or timed activation

## 5. Physics Considerations
- **Falling Simulation**: 
  - Objects fall at fixed speed (1 cell per 0.3s)
  - Fall triggers when cell below becomes empty (dirt dug or object moved)
  - Chain reactions handled in game tick (process all falls simultaneously)
- **Collision Detection**: 
  - Grid-based (no complex collision meshes needed)
  - Check target cell before movement/falling
- **Explosions**: 
  - Butterfly/firefly: Convert to 9 diamonds (butterfly) or nothing (firefly)
  - Affect adjacent cells (up/down/left/right)
- **Amoeba Behavior**: 
  - Growth cycle: Empty → Amoeba → Wall (when enclosed) or Boulder (when overgrown)
  - Growth rate: 1 unit per tick, max 200 units
- **Performance**: 
  - Fixed update loop independent of render FPS
  - Object pooling for frequently spawned items (diamonds, explosion effects)
  - Frustum culling for distant cave sections

## 6. Rendering Approach
- **Visual Style**: 
  - Semi-realistic cave textures with Three.js MeshStandardMaterial
  - Animated water/dripping effects using shader modifiers
  - Glowing emissive materials for diamonds and magic walls
- **Object Representation**:
  - Player: Low-poly Rockford model with walking animation
  - Boulders: Sphere with rock texture, slight rotation while falling
  - Diamonds: Octahedron with sparkling emissive effect
  - Dirt: Textured planes with parallax scrolling
  - Walls: Cube with varied rock textures
  - Magic Wall: Animated texture cycling between states
  - Firefly/Butterfly: Sprite-based with fluttering animation
  - Amoeba: Green blob with pulsating shader
  - Exit: Animated door frame that opens when conditions met
- **Lighting**: 
  - Ambient cave lighting with directional torch simulation from player
  - Dynamic shadows for falling objects (simplified)
  - Emissive glow for collectibles and hazards
- **VR Optimization**: 
  - Multi-resolution rendering for Quest
  - Fixed foveated rendering where supported
  - Minimizing draw calls via texture atlases and InstancedMesh for repetitive objects (dirt, walls)

## 7. Level Loading System
- **Format**: JSON structure with:
  ```json
  {
    "width": 40,
    "height": 25,
    "timeLimit": 180,
    "diamondsRequired": 20,
    "grid": [
      ["W","W","W",...],
      ["W","D"," ",...],
      ...
    ],
    "objects": [
      {"type": "player", "x": 5, "y": 5},
      {"type": "diamond", "x": 10, "y": 10, "count": 3}
    ]
  }
  ```
- **Parsing**: Convert to internal grid state and object instances
- **Progression**: Load next cave on exit, track score and lives
- **Editor**: In-dev level editor using same grid system (post-MVP)

## 8. Milestones and Estimated Effort
| Milestone | Description | Estimated Effort |
|-----------|-------------|------------------|
| 1 | Project setup: Three.js WebXR boilerplate, basic scene with controllers | 1 day |
| 2 | Grid system implementation: Coordinate conversion, basic cell rendering | 1 day |
| 3 | Static level rendering: Walls, dirt, diamonds, exit from JSON | 1 day |
| 4 | Player movement: Grid-based locomotion with collision (walls/dirt) | 1 day |
| 5 | Falling mechanics: Boulders and diamonds with simple gravity | 1 day |
| 6 | Object interactions: Digging, magic walls, basic explosions | 1 day |
| 7 | Advanced objects: Fireflies, butterflies, amoeba growth cycles | 2 days |
| 8 | Game state tracking: Diamond count, timer, exit condition, lives | 1 day |
| 9 | Audio system: Footsteps, falls, explosions, ambient cave sounds | 1 day |
| 10 | UI/HUD: Diamond counter, timer display, pause menu | 0.5 days |
| 11 | Polishing: Visual effects, particle systems, animation refinements | 1.5 days |
| 12 | Testing & debugging: Cross-device validation, comfort optimization | 1 day |
| **Total** |  | **13 days** |

## 9. Critical Files for Implementation
- /home/mtoulon/dev/boulder-dash/src/main.ts
- /home/mtoulon/dev/boulder-dash/src/game.ts
- /home/mtoulon/dev/boulder-dash/src/objects.ts
- /home/mtoulon/dev/boulder-dash/src/level.ts
- /home/mtoulon/dev/boulder-dash/src/render.ts

## Sources Consulted
- Wikipedia: [Boulder Dash (video game)](https://en.wikipedia.org/wiki/Boulder_Dash_(video_game))
- Arno's Boulder Dash Fansite: [Characters and Objects](https://boulderdash.nl/chars.php?lang=en)
- Puzzlebyrinth Analysis: [Boulder Dash 1984 Mechanics](https://puzzlebyrinth.com/en/articles/boulder-dash-1984)
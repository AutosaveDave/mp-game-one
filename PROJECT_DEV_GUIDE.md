# Project Development Guide - Overview

## Project Overview
The project is an in-browser 3D online multiplayer game wrapped in a React app. 

## Key Technologies
1. React App
    - **React** using **Vite** for faster build time
    - **MUI** for components and themeing
2. Game (frontend)
    - **Three.js** and/or **react-three-fiber** for 3D rendering
    - **cannon-es** for physics
    - **drei** for additional functionality (if using react-three-fiber)
3. **Firebase** (backend)
    - **Firebase Authentication** for user authentication using the "email/password" sign-in method
    - **Firebase Firestore** for user data, friend requests, friends, game invites, and asset metadata
    - **Firebase Realtime Database** for game world state and player positions in each current game session
    - **Firebase Cloud Functions** for game logic, validation, and notifications
    - **Firebase Hosting** for serving the React app
    - **Firebase CLI** for running emulators (for local testing) and deploying cloud functions, database rules, etc

---

# Development Guide for React App

## React App Overview
The React app provides the following functionality:
1. **Login and Signup**
    - logging in
    - signing up
    - logging out
2. **User Account Management**
    - selecting or uploading an avatar image for user profile
    - changing username
    - changing password
    - deleting account
3. **Friends Management**
    - sending friend requests
    - cancelling sent friend requests
    - accepting or declining friend requests
    - creating new friend groups
    - inviting friends to your friend groups
    - accepting invitations to friend groups
4. **Game Session Management**
    - starting a game session lobby
    - sending game session invites to friends
    - accepting or declining game session invites from friends
    - viewing friends' current game sessions and game session lobbies
    - joining friends' current game sessions and game session lobbies (without requiring a game invite)
5. **Messaging**
    - sending messages to friends or groups of friends
    - viewing and replying to incoming messages from friends
6. **Game Session Lobby** *(the staging screen before a game session is started)*
    - inviting friends to the game session lobby
    - editing game settings (only the user who created the game session lobby can do this). Editable game settings should include things like game mode, map/area, rules, difficulty, etc
    - selecting a character to use in the game (all users in the lobby can do this)
    - selecting a loadout to use in the game (all users in the lobby can do this)
    - viewing messages sent in the game session's text chat (these messages are visible to all users in the lobby)
    - sending messages in the game session's text chat (these messages are visible to all users in the lobby)
    - setting "ready" status (the user who created the game session lobby cannot start the game until all users in the lobby are ready)
    - starting the game once all users in the lobby are ready (only the user who created the game session lobby can do this). Starting the game takes all users in the lobby into the game.

---

# Game
The game is a cover-based shooter with an orthographic camera view (similar to the camera view in games like Starcraft, Diablo, or Age of Empires).

## Game Controls
Players should be able to play the game using their mouse and keyboard or a game controller. **It is important that players be able to use a Playstation 5 controller connected to a PC or Mac.**

### Ground Movement Controls
1. **Mouse and Keyboard**
    - Players can make their character **walk/run** in a direction relative to the camera using the **W**, **A**, **S**, and **D** keys on their keyboard
    - By default, the player's character **walks** when moving (unless the player is holding down the **left shift key**)
    - The player's character **runs** when moving while the **left shift key** is held down
2. **Controller**
    - Players can make their character **walk/run** in a direction relative to the camera using the **left joystick** of their controller
    - By default, the player's character **walks** when moving (unless the player is holding down their controller's **B button**)
    - The player's character **runs** when moving while their controller's **B button** is held down

### Dodge-Roll Movement Controls
1. **Mouse and Keyboard**
    - While on the ground, players can make their character perform a quick **dodge-roll** in their current movement direction by tapping (pressing then very quickly releasing) the **left shift key**
2. **Controller**
    - While on the ground, players can make their character perform a quick **dodge-roll** in their current movement direction by tapping (pressing then very quickly releasing) the **B button** on their controller

### Jumping and Aerial Movement Controls
1. **Mouse and Keyboard**
    - Players can make their character **jump** by pressing **spacebar** while on the ground
    - Players can activate an aerial ability if they have one equipped by pressing (or for some abilities, by holding down) **spacebar** while airborne
2. **Controller**
    - Players can make their character **jump** by pressing their controller's **A button** while on the ground
    - Players can activate an aerial ability if they have one equipped by pressing (or for some abilities, by holding down) their controller's **A button** while airborne

### Cover Interactions Controls
1. **Mouse and Keyboard**
    - Players can **attach to cover** by tapping (pressing then very quickly releasing) the **left shift key** while near a valid cover object.
    - Players can **detach from cover** by tapping the **left shift key** again, moving away from the cover, or by jumping.
    - While attached to cover, players can move left/right along the cover using **A** and **D** keys.
    - Players can **vault over cover** (if the cover is low enough) by pressing **spacebar** while attached to cover.
2. **Controller**
    - Players can **attach to cover** by tapping the **B button** while near a valid cover object.
    - Players can **detach from cover** by tapping the **B button** again, moving away from the cover, or by jumping.
    - While attached to cover, players can move left/right along the cover using the **left joystick**.
    - Players can **vault over cover** (if the cover is low enough) by pressing the **A button** while attached to cover.

### Aiming and Shooting Controls
1. **Mouse and Keyboard**
    - Players select a target using the **mouse cursor** (clicking on or near a potential target selects that target).
    - Players shoot by pressing the **left mouse button**.
    - Players can reload by pressing the **R key**.
2. **Controller**
    - Players select a target using the **right joystick**.
    - Players shoot by pressing the **right trigger (R2)**.
    - Players can reload by pressing the **X button**.

### Ability and Item Usage Controls
1. **Mouse and Keyboard**
    - Players can use their equipped ability by pressing **Q** (or another assigned key).
    - Players can use consumable items by pressing **E** (or another assigned key).
2. **Controller**
    - Players can use their equipped ability by pressing the **L1 button**.
    - Players can use consumable items by pressing the **L2 button**.

---

## Camera System
- The camera uses an **orthographic projection** and is positioned above the play area, angled to provide a clear view of the action (similar to Starcraft/Diablo/Age of Empires).
- The camera follows the player's character smoothly, keeping them centered or slightly offset depending on movement direction.
- Camera zoom level is fixed or can be adjusted with the mouse scroll wheel or controller triggers (optional).
- The camera should avoid clipping into level geometry and provide clear visibility of the player and nearby threats.

## Character Actions and Abilities
- Characters can walk, run, dodge-roll, jump, attach/detach/vault cover, aim, shoot, reload, and use abilities/items as described above.
- Each action should have clear animation states and transitions.
- Abilities may include dashes, shields, projectiles, or other effects (define each ability in a data-driven way for easy expansion).

## 3D Rendering and Scene Management
- Use **Three.js** or **react-three-fiber** for rendering.
- Organize the scene into layers: background, environment, cover objects, characters, projectiles, effects, UI overlays.
- Use **drei** helpers for camera controls, lighting, and asset loading if using react-three-fiber.
- Optimize draw calls and use instancing for repeated objects.

## Physics Integration
- Use **cannon-es** for physics simulation (character movement, collisions, projectiles, cover interactions).
- Sync physics state with rendering loop for smooth visuals.
- Ensure physics is deterministic for multiplayer synchronization.

## Multiplayer Synchronization
- Use **Firebase Realtime Database** to synchronize player positions, actions, and game state.
- Each game session has a unique room ID; all player and world state updates are scoped to this session.
- Use client-side prediction and server reconciliation to minimize perceived lag.
- Use **Firebase Cloud Functions** for authoritative validation of critical actions (e.g., shooting, ability use, scoring).

## Game State and Event Flow
- Game states: lobby, loading, in-game, game-over.
- Use a state machine to manage transitions and events (player join/leave, round start/end, victory/defeat).
- All state changes should be reflected in the UI and synchronized across clients.

## Asset Management
- Store 3D models, textures, and audio assets in Firebase Storage or bundle with the app.
- Use asset preloading and loading screens to ensure smooth gameplay.
- Optimize assets for web delivery (compressed formats, LODs).

## Performance Considerations
- Use requestAnimationFrame for rendering/game loop.
- Throttle network updates to a reasonable rate (e.g., 20Hz for positions).
- Profile and optimize for low-end hardware and browsers.

## Testing and Debugging
- Unit test core logic (movement, abilities, state transitions).
- Integration test multiplayer flows (joining, syncing, disconnects).
- Use debug overlays for physics, network state, and performance metrics.
- Provide a way to simulate network lag and dropped packets for robustness testing.

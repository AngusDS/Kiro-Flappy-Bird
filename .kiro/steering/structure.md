# Project Structure

```
flappy-kiro/
├── index.html          # Entire game — HTML, CSS, and JS in one file
├── assets/
│   ├── ghosty.png      # Ghost sprite
│   ├── jump.wav        # Jump sound effect
│   └── game_over.wav   # Game over sound effect
├── img/
│   └── example-ui.png  # Screenshot reference
└── .kiro/
    ├── specs/
    │   └── flappy-kiro/  # Spec documents for the game feature
    └── steering/         # AI assistant guidance files
```

## Code Organization (inside `index.html`)

All game logic lives in a single `<script>` block, organized into self-contained modules using the IIFE/closure pattern:

| Module | Responsibility |
|---|---|
| `CONFIG` | Central constants — canvas size, physics values, colors, asset paths |
| `assetLoader` | Preloads images and audio before game start |
| `STATES` | Enum of game states: `WAITING`, `PLAYING`, `GAME_OVER` |
| `gameState` | Tracks current state and high score |
| `ghost` | Ghost physics (position, velocity, gravity, jump) and rendering |
| `pipeManager` | Pipe spawning, movement, recycling, and rendering |
| `checkCollision` / `rectOverlap` | Pure functions for AABB collision detection |
| `scoreSystem` | Score tracking, high score, and score bar rendering |
| `audioSystem` | Wraps audio playback with silent error handling |
| `renderer` | Background, clouds, score bar, waiting/game-over overlays |
| `handleInput` | Unified input handler for Space key and canvas click |
| `gameLoop` | Main `requestAnimationFrame` loop — update then render |

## Conventions
- All magic numbers belong in `CONFIG`, not inline in logic
- Modules expose only what other modules need — keep internal state private via closures
- Rendering and update logic are kept separate (update first, render after)
- New game systems should follow the same IIFE module pattern with a `reset()` method

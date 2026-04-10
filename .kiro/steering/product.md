# Flappy Kiro

A retro-style browser-based endless scrolling game. The player controls a ghost character, tapping/clicking to fly upward while avoiding green pipes that scroll from right to left. The game features a hand-drawn sketch aesthetic with a light blue background, static clouds, and a score bar at the bottom.

## Core Gameplay
- Single-button input (Space or click) to make the ghost jump
- Pipes spawn at random heights with a fixed gap
- Score increments each time the ghost passes through a pipe gap
- Session high score is tracked and persisted until page reload
- Three states: WAITING → PLAYING → GAME_OVER → PLAYING (restart)

## Assets
- Ghost sprite: `assets/ghosty.png`
- Jump sound: `assets/jump.wav`
- Game over sound: `assets/game_over.wav`

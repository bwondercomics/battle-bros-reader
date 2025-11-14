# Battle Bros: Cyber Chase - Endless Runner

An addictive endless runner arcade game inspired by Subway Surfers! Chase through cyberspace, collect coins, dodge obstacles, and beat your high score!

## How to Play

1. Open `chase-game.html` in any modern web browser
2. Click "START GAME" to begin
3. Use **← LEFT** and **→ RIGHT** arrow keys to switch between lanes
4. **Collect coins** 🪙 to increase your score and build combos
5. **Grab power-ups** 💎 for special abilities
6. **Avoid obstacles** ⚠️ or you'll lose a life!
7. **Survive as long as possible** - you have 3 lives!

## Game Mechanics

### Controls
- **Arrow Left (←)**: Move to the left lane
- **Arrow Right (→)**: Move to the right lane

### Endless Runner Gameplay
- Your character automatically moves forward through endless cyberspace
- Navigate between 3 lanes to collect coins and avoid obstacles
- Each obstacle hit costs 1 life - lose all 3 and it's game over!
- Coins appear in patterns - collect them all for maximum score
- Build combos by dodging consecutive obstacles (+5 bonus per 5 combo)
- Speed gradually increases as you travel further
- The game tracks your high score locally

### Coins & Scoring
- **Collect Coin**: +10 points (or +20 with 2x Score power-up)
- **Dodge Obstacle**: +10 points + combo bonus
- **Combo System**: Build streaks by dodging obstacles without getting hit
- Coins spawn in various patterns across lanes

### Power-Ups (Last 5 seconds)
- **🧲 Magnet**: Automatically attracts nearby coins to you
- **🛡️ Shield**: Protects you from one obstacle hit
- **2X Score**: Doubles all points earned

### Lives System
- Start with **3 lives** ❤️❤️❤️
- Lose 1 life when hitting an obstacle (unless you have a shield)
- Game ends when all lives are lost
- Shield power-up can save you from losing a life

### Difficulty Progression
- Speed increases every 1000m traveled
- Obstacles spawn more frequently as speed increases
- Challenge yourself to survive longer each run!

## Features

- 🎮 **Endless runner** gameplay - no finish line, just survival
- 🪙 **Coin collection** system with patterns
- 💎 **Three power-ups** with unique abilities
- ❤️ **Lives system** for extended gameplay
- 🔥 **Combo system** rewarding consecutive dodges
- 🏆 **High score tracking** saved locally
- 📊 **Detailed statistics** at game over
- 🌈 Retro cyberpunk aesthetic matching Battle Bros theme
- ✨ Neon glows, particle effects, and smooth animations
- 📱 Responsive design for desktop and mobile
- ⚡ Progressive difficulty for increasing challenge

## Scoring Strategy

**Maximize your score by:**
1. Collecting every coin in patterns
2. Maintaining high combos by dodging obstacles
3. Using the 2x Score power-up when many coins are nearby
4. Using the Magnet power-up to easily collect coin lines
5. Saving shields for when speed gets very high

## Game Statistics

The game tracks and displays:
- **Final Score**: Total points earned
- **Distance Traveled**: How far you ran (in meters)
- **Coins Collected**: Total coins grabbed
- **Obstacles Dodged**: Successful dodges
- **Best Combo**: Highest consecutive dodge streak
- **Time Survived**: How long you lasted (in seconds)
- **High Score**: Your personal best

## Technical Details

- Pure HTML5, CSS3, and JavaScript (no dependencies)
- Canvas-based rendering for smooth 60fps graphics
- RequestAnimationFrame game loop
- 3-lane system with smooth lane transitions
- LocalStorage for high score persistence
- Dynamic spawning algorithms for varied gameplay
- Collision detection system
- Particle explosion effects
- Progressive difficulty scaling

## Theme Integration

The game uses the official Battle Bros color palette:
- **Primary**: #00d9ff (Electric Cyan) - Player, UI, coins
- **Secondary**: #ff00ea (Hot Magenta) - Obstacles, magnet
- **Accent**: #ffed00 (Bright Yellow) - Shield, highlights
- **Background**: #0a0a12 (Deep Black)

## Browser Compatibility

Works in all modern browsers that support:
- HTML5 Canvas
- ES6 JavaScript
- CSS3 Animations
- LocalStorage
- requestAnimationFrame

Tested on: Chrome, Firefox, Safari, Edge

---

**Created for**: Battle Bros Comics  
**Website**: https://bwondercomics.com/  
**Game Type**: Endless Runner / Arcade

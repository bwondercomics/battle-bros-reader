# Battle Bros: Cyber Chase

A retro cyberpunk arcade game where you chase the ultimate power-up through cyberspace!

## How to Play

1. Open `chase-game.html` in any modern web browser
2. Click "START GAME" to begin
3. Use **← LEFT** and **→ RIGHT** arrow keys to dodge obstacles
4. Collect **🔷 Blue Boosts** to gain +100m of instant progress
5. Avoid **⚠️ Red Obstacles** that slow you down temporarily
6. Reach **5000m** to win!

## Game Mechanics

### Controls
- **Arrow Left (←)**: Move to the left lane
- **Arrow Right (→)**: Move to the right lane

### Gameplay
- Your character automatically moves forward, chasing the target
- Navigate between 3 lanes to avoid obstacles and collect boosts
- Each obstacle you hit reduces your speed by 50% for 1 second
- Blue boosts give you +100m of instant progress
- Dodging obstacles earns you +10 points
- **Progressive Difficulty**: Speed increases as you get closer to the target (up to 2.5x)!
- The game tracks your score, distance, speed, time, and performance

### Scoring
- **Dodge Obstacle**: +10 points
- **Collect Blue Boost**: +100 points (and +100m progress)
- **Hit Obstacle**: Speed penalty (50% reduction for 1 second)

### Performance Ratings
- **Perfect Run**: No obstacles hit - LEGENDARY!
- **Excellent**: Less than 10% hit rate
- **Great**: Less than 25% hit rate
- **Good**: Less than 50% hit rate
- **Complete**: 50%+ hit rate

## Features

- 🎮 Retro cyberpunk aesthetic matching Battle Bros theme
- 🌈 Neon colors with glow effects
- ✨ Particle effects for collisions and collections
- 📊 Real-time HUD showing distance, speed, and score
- 🏆 Enhanced victory screen with detailed statistics and performance ratings
- 📱 Responsive design for desktop and mobile
- ⚡ Smooth 60fps gameplay with progressive difficulty
- 🎨 Scanline CRT effects for authentic retro feel
- ⏱️ Time tracking and performance analysis

## Technical Details

- Pure HTML5, CSS3, and JavaScript (no dependencies)
- Canvas-based rendering for smooth graphics
- RequestAnimationFrame game loop
- 3-lane system with smooth transitions
- Dynamic obstacle and boost spawning
- Collision detection system
- Particle explosion effects
- Progressive difficulty scaling

## Game Balance

- **Target Distance**: 5000m (5x original distance)
- **Base Speed**: 0.8 units/frame (slower start)
- **Speed Scaling**: Increases to 2.5x base speed at finish
- **Expected Playtime**: 25-60 seconds depending on boost collection
- **Boost Value**: +100m instant progress
- **Challenge Level**: Moderate to challenging with progressive difficulty

## Theme Integration

The game uses the official Battle Bros color palette:
- **Primary**: #00d9ff (Electric Cyan)
- **Secondary**: #ff00ea (Hot Magenta)  
- **Accent**: #ffed00 (Bright Yellow)
- **Background**: #0a0a12 (Deep Black)

## Browser Compatibility

Works in all modern browsers that support:
- HTML5 Canvas
- ES6 JavaScript
- CSS3 Animations
- requestAnimationFrame

Tested on: Chrome, Firefox, Safari, Edge

---

**Created for**: Battle Bros Comics  
**Website**: https://bwondercomics.com/

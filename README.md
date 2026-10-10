# ⚡ Neon Pong Arcade — Cyber Overdrive

<!-- STREAMING_CHUNK:Documenting project overview and badges... -->
[![HTML5](https://img.shields.io/badge/HTML5-Canvas-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Web Audio API](https://img.shields.io/badge/Web_Audio_API-Synthesized-9900ee?logo=soundcharts&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
[![Vanilla JS](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-brightgreen)](#)

A high-octane, cyberpunk-themed reimagining of classic Pong built purely with **HTML5 Canvas**, **Vanilla JavaScript**, **Tailwind CSS**, and the native **Web Audio API**. Featuring reactive synthesized sound design, dynamic power-up orbs, 16:9 responsive virtual resolution, multi-touch mobile gameplay, and predictive AI difficulty tiers.

---

<!-- STREAMING_CHUNK:Detailing table of contents and highlights... -->
## 📋 Table of Contents

- [Features](#-features)
- [Live Demo & Quick Start](#-live-demo--quick-start)
- [Controls & Keybindings](#-controls--keybindings)
- [Power-Up System](#-power-up-system)
- [AI Difficulty Presets](#-ai-difficulty-presets)
- [Technical Architecture](#-technical-architecture)
  - [16:9 Virtual Coordinate Space](#1-169-virtual-coordinate-space)
  - [Web Audio API Synth Engine](#2-web-audio-api-synth-engine)
  - [Continuous Collision & Deflection Physics](#3-continuous-collision--deflection-physics)
  - [Particle & Shockwave FX Pipeline](#4-particle--shockwave-fx-pipeline)
- [Configuration & Customization](#-configuration--customization)
- [Browser Compatibility](#-browser-compatibility)
- [License](#-license)

---

<!-- STREAMING_CHUNK:Detailing core gameplay features... -->
## ✨ Features

- **Cyberpunk Visual Aesthetics**:
  - CRT scanline emulation and chromatic border glow.
  - Multi-stage neon plasma paddle hit flashes and expanding shockwave rings.
  - Speed-dependent dynamic ball comet trails (Cyan / Magenta / Solar Amber turbo glow).
  - Scrolling retro perspective court grid with dual neon center rings.
  - Animated typography utilizing Google Fonts (`Audiowide`, `Chakra Petch`, `Orbitron`, `Righteous`).

- **100% Asset-Free Procedural Audio**:
  - Zero external MP3/WAV files required.
  - Frequency-modulated sine, square, sawtooth, and triangle waves generated on the fly via the `Web Audio API`.
  - Dynamic pitch escalation: rally hits increase synthesizer pitch with consecutive returns.
  - Distinct sound profiles for paddle impacts, court bounces, scores, power-ups, and game-over fanfares.

- **Dual Game Modes**:
  - **1 Player vs. AI**: Challenge an intelligent predictive bot across 4 distinct difficulty modes.
  - **2 Players Local**: Play side-by-side on the same keyboard, mouse zones, or touchscreen.

- **Universal Responsiveness**:
  - Virtual resolution rendering (960×540) locked to an aspect ratio of 16:9 with DPR (device pixel ratio) scaling.
  - Fluid mouse tracking that prevents paddles from sticking to canvas borders.
  - Multi-touch gesture support allowing two players to control paddles simultaneously on touch displays.
  - Dedicated on-screen tactile virtual arrow buttons for mobile devices.

---

<!-- STREAMING_CHUNK:Formatting controls and keybindings table... -->
## 🎮 Controls & Keybindings

### Desktop Controls

| Player | Movement Keys | Alternate Input |
| :--- | :--- | :--- |
| **Player 1 (Cyan)** | <kbd>W</kbd> (Up) / <kbd>S</kbd> (Down) | Mouse pointer movement (Left court half) |
| **Player 2 (Magenta)** | <kbd>▲</kbd> (Up) / <kbd>▼</kbd> (Down) | Mouse pointer movement (Right court half in 2P) |
| **System** | <kbd>Space</kbd> (Pause / Resume) | <kbd>M</kbd> (Mute / Unmute Audio) |

### Mobile & Tablet Controls

- **Touch & Drag**: Touch and glide directly within your respective half of the court to position the paddle.
- **On-Screen D-Pad**: Use the responsive on-screen buttons below the court for quick tactile directional pulses.
- **Multi-Touch**: In **2-Player Local Mode**, two players can touch each side of the screen at the same time without touch cancellation.

---

<!-- STREAMING_CHUNK:Explaining game mechanics and power-ups... -->
## ⚡ Power-Up System

When enabled in settings, holographic power-up orbs periodically spawn onto the court. Colliding with an orb triggers an instant game-changing modifier:

| Power-Up | Color | Visual Icon | In-Game Effect |
| :--- | :--- | :---: | :--- |
| **Turbo Lightning** | `#ffb703` Solar Amber | ⚡ | Increases ball velocity by **+35%** (capped at maximum game speed). Produces a high-speed fiery comet trail. |
| **Mega Matrix** | `#00ff9d` Matrix Emerald | ↕ | Expands the paddle height of the player who hit the ball by **150%** for **8.5 seconds**. |
| **Time Dilation** | `#c084fc` Cosmic Purple | ⏳ | Decelerates the ball to **65% speed**, granting extra reaction time for tactical returns. |

---

<!-- STREAMING_CHUNK:Documenting AI presets and algorithmic details... -->
## 🤖 AI Difficulty Presets

The AI leverages predictive trajectory modeling with built-in human reaction delay and error margins:

| Preset | Speed Multiplier | Reaction Lag | Error Margin | Description |
| :--- | :---: | :---: | :---: | :--- |
| **Easy** | `0.52×` | `160 ms` | `±46 px` | Casual pace; frequently mistimes curved corner shots. |
| **Medium** | `0.74×` | `80 ms` | `±20 px` | Balanced arcade challenge suitable for everyday play. |
| **Hard** | `0.93×` | `20 ms` | `±7 px` | Fast-tracking opponent capable of defending hyperspeed rallies. |
| **Cyber (Insane)** | `1.25×` | `0 ms` | `0 px` | Perfect mathematical angle prediction and zero error margin. |

---

<!-- STREAMING_CHUNK:Detailing technical architecture and sound synthesis... -->
## 🛠️ Technical Architecture

### 1. 16:9 Virtual Coordinate Space
The simulation operates on a fixed virtual resolution of **`960 × 540`**.
```
(0, 0) ──────────────────────────────── (960, 0)
  │                                        │
  │   Player 1 [P1]        Player 2 [P2]   │
  │     (x: 35)              (x: 910)      │
  │                                        │
(0, 540) ────────────────────────────── (960, 540)
```
Canvas dimensions dynamically adapt to screen size and DPR (`window.devicePixelRatio`) while all game logic runs against normalized coordinates. Pointer and touch coordinates are translated to virtual coordinates before applying paddle offsets:
```javascript
const scaleX = CONFIG.VIRTUAL_WIDTH / rect.width;
const scaleY = CONFIG.VIRTUAL_HEIGHT / rect.height;
const virtualX = (clientX - rect.left) * scaleX;
const virtualY = (clientY - rect.top) * scaleY;
```

### 2. Web Audio API Synth Engine
Instead of loading audio assets over the network, `RetroAudio` initializes an `AudioContext` and dynamically provisions `OscillatorNode` and `GainNode` pairings:
- **Paddle Deflection**: Triangle wave oscillator modulated with an exponential pitch increase:
  $$\text{Pitch} = f_{\text{base}} + \min(\text{rally} \times 22, 400)\text{ Hz}$$
- **Court Wall Bounce**: Low-frequency square wave with exponential decay.
- **Goal Scored**: Pentatonic ascending arpeggio ($C_5, E_5, G_5, C_6, E_6$).
- **Game Over**: Triumphant major arpeggio on win, or a descending sawtooth cadence on defeat.

### 3. Continuous Collision & Deflection Physics
To eliminate paddle tunneling at hyperspeeds (up to `19.0 px/frame`), collisions are calculated using paddle boundaries combined with ball radii:
- The deflection angle is determined by the intersection point relative to the paddle center:
  $$\text{Intersect} = \frac{y_{\text{ball}} - y_{\text{paddle\_center}}}{h_{\text{paddle}} / 2}$$
  $$\theta = \text{clamp}(\text{Intersect}, -1, 1) \times \frac{\pi}{3.2}$$
- The resultant velocities are computed as:
  $$v_x = \text{dir} \times v_{\text{current}} \times \cos(\theta), \quad v_y = v_{\text{current}} \times \sin(\theta)$$

### 4. Particle & Shockwave FX Pipeline
- **Particles**: Decaying circular sparks with directional velocity, air friction dampening (`0.95`), and individual alpha life cycles.
- **Shockwaves**: Concentric stroke rings that expand radially via spring interpolation while fading out over time.

---

<!-- STREAMING_CHUNK:Outlining setup and installation instructions... -->
## 🚀 Quick Start & Installation

Because the application is packaged as a **self-contained single-file web application**, setup takes only seconds.

### Option 1: Direct Browser Launch
1. Clone or download the repository:
   ```bash
   git clone https://github.com/your-username/neon-pong-arcade.git
   cd neon-pong-arcade
   ```
2. Open `index.html` directly in any modern web browser (Google Chrome, Firefox, Edge, Safari, Brave).

### Option 2: Local Development Server
For the best experience (and to test responsive mobile layouts):
```bash
# Using Python 3
python3 -m http.server 8080

# Or using Node.js via npx
npx serve .
```
Navigate to `http://localhost:8080` in your web browser.

---

<!-- STREAMING_CHUNK:Documenting configuration constants and AI tuning... -->
## ⚙️ Configuration & Customization

All primary game parameters are organized in the `CONFIG` object inside `index.html`:

```javascript
const CONFIG = {
  VIRTUAL_WIDTH: 960,         // Internal logical width
  VIRTUAL_HEIGHT: 540,        // Internal logical height
  PADDLE_WIDTH: 15,           // Paddle thickness
  PADDLE_HEIGHT: 96,          // Default paddle length
  BALL_RADIUS: 9,             // Radius of the glowing ball
  BASE_BALL_SPEED: 7.2,       // Launch velocity
  MAX_BALL_SPEED: 19.0,       // Absolute speed cap
  BALL_ACCELERATION: 1.05,    // Acceleration multiplier per paddle hit
  PADDLE_SPEED: 9.5,          // Keyboard paddle velocity
  POINTS_TO_WIN: 7,           // Target score to claim victory
  POWERUPS_ENABLED: true,     // Toggle spawning of bonus orbs
  AI_DIFFICULTY: 'medium'     // 'easy' | 'medium' | 'hard' | 'insane'
};
```

---

<!-- STREAMING_CHUNK:Detailing browser compatibility and license... -->
## 🌐 Browser Compatibility

| Browser | Supported | Notes |
| :--- | :---: | :--- |
| **Google Chrome / Chromium** | ✅ | Full support (Hardware acceleration recommended) |
| **Mozilla Firefox** | ✅ | Full support |
| **Apple Safari (macOS / iOS)** | ✅ | Full support (AudioContext unlocked on first user tap) |
| **Microsoft Edge** | ✅ | Full support |
| **Mobile Chrome / Safari** | ✅ | Touch drag and on-screen controls fully enabled |

---

## 📄 License

This project is licensed under the **MIT License**. Feel free to modify, distribute, and enhance the arcade experience as you see fit.

---

<p align="center">
  Built with ❤️, pure mathematics, and neon laser vibes.
</p>

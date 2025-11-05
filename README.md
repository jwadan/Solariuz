# ☀️ Solariuz — Complete Technical Wiki

## 🧩 Overview
**Solariuz** is a **space roguelike shooter** built with **React** and the **HTML5 Canvas API** for real-time rendering.  
The player faces **progressively harder enemy waves**, each featuring **unique bosses with multiple phases**, temporary power-ups, and permanent upgrades purchased with earned credits.

The goal: **survive as long as possible**, **earn credits**, and **unlock achievements**.

---

## ⚙️ Technologies Used
- **React** — component structure and state management
- **Canvas API** — real-time rendering and collisions
- **Lucide React** — vector icons
- **React Hooks** — (`useState`, `useRef`, `useEffect`, `useCallback`)
- **Controlled mutable architecture** — heavy use of refs for performance

---

## 🧱 Core Structure
### Main Component
```tsx
export default function Solariuz() { ... }
```
The root component that:
- Manages global game state.
- Controls the main render loop (`requestAnimationFrame`).
- Handles rendering of menus, HUD, transitions, and gameplay.

---

## 🎮 Game States & Refs

### Game States (`useState`)
| State | Type | Description |
|--------|------|-------------|
| `gameState` | `'menu' | 'playing' | 'gameover'` | Overall game mode |
| `score`, `wave`, `credits` | `number` | Player score, current wave, and in-game credits |
| `totalCredits` | `number` | Persistent credits between runs |
| `waveTime` | `number` | Remaining time in current wave |
| `showUpgradeCards` | `boolean` | Toggles upgrade card menu |
| `upgradeOptions` | `any[]` | Holds three randomly selected upgrades |
| `bossActive` | `boolean` | Whether a boss is currently active |
| `currentMenu` | `string` | Active UI menu (main, stats, help, etc.) |
| `achievements`, `showAchievement` | `list` | Achievement tracking and popup display |
| `waveStats` | `object` | Stores stats from the previous wave |
| `currentBossInfo` | `object` | Displays the active boss’s name, HP, and phase |

---

### References (`useRef`)
Used to **store mutable entities** without triggering React re-renders:

- `playerRef`: Player data (position, health, damage, speed, etc.)
- `enemiesRef`, `bulletsRef`, `enemyBulletsRef`, `particlesRef`, `powerupsRef`: Live entity arrays
- `bossRef`: Current boss instance
- `statsRef`: Persistent and session statistics
- `permanentRef`: Permanent upgrades (health, damage, etc.)
- `temporaryRef`: Temporary upgrades applied during play
- `keys`: Keyboard input tracking
- `rafRef`: Reference to the active animation frame loop

---

## 🚀 Entities

### Player
Main properties:
- `x, y, width, height`
- `health`, `maxHealth`
- `speed`, `damage`, `fireRate`
- `lastShot`

Abilities:
- Move in 8 directions (WASD or Arrow Keys)
- Fire with multiple shot patterns depending on upgrades

---

### Enemies
Enemy archetypes include both **basic** and **advanced** classes:

- **Scout** — fast, fragile fighter  
- **Soldier** — balanced combatant  
- **Tank** — slow, durable enemy  
- **Sniper** — stationary long-range shooter  
- **Kamikaze** — charges directly into the player  
- **Mothership** — spawns smaller enemies  
- **Homer** — fires homing missiles  
- **Pulsar** — alternates between attack and defense modes  
- **Ghost** — turns invisible periodically  
- **Electric** — balanced energy-based enemy

---

### Bosses
Each boss features **distinct phases**, **attack behaviors**, and **visual identities**.

| Icon | Boss | Behavior | Phases |
|------|------|-----------|--------|
| 🚀 | **Carrier** | Spawner | Launches fighters and missiles |
| 💥 | **Artillery** | Sniper | Precise shots and triple-fire bursts |
| 👑 | **Necropolis** | Hybrid | Spiral patterns, summons minions |
| 🛡️ | **Titan** | Tank | Shield walls, shockwaves, and rage mode |

Bosses scale in **health and damage** based on the wave number.

---

## 💥 Combat System

### Player Bullets
- Can be **standard**, **double**, **triple**, **piercing**, **homing**, or **explosive**.  
- Uses `circleOverlap()` for accurate collision detection with enemies.

### Enemy Bullets
- Regular or homing projectiles.  
- Damage the player on contact with shield and explosion effects.

### Collision Methods
- `rectOverlap(a, b)` → Rectangle collision detection.  
- `circleOverlap(bullet, enemy)` → Circular precision collision.

---

## ⚡ Upgrade System

### Temporary Upgrades (per run)
- `doubleShot`, `tripleShot`, `rapidFire`, `shield`, `piercing`
- `homingShots`, `explosiveShots`, `lifeSteal`, `damageUp`, `speedUp`

### Permanent Upgrades (persistent)
- `maxHealth`, `damageBoost`, `speedBoost`, `creditMultiplier`

Permanent upgrades are purchased with **total credits** after each game.

---

## 🏆 Achievements

A robust in-game achievement system rewarding player progression.

| ID | Name | Description |
|----|------|--------------|
| `first_blood` | **First Blood** | Destroy your first enemy |
| `boss_slayer` | **Boss Slayer** | Defeat your first boss |
| `survivor` | **Survivor** | Survive for 5 minutes |
| `killing_spree` | **Killing Spree** | Defeat 100 enemies |
| `veteran` | **Veteran** | Reach wave 5 |
| `rich` | **Space Tycoon** | Earn 1000 credits in one game |
| `perfectionist` | **Perfectionist** | Complete a wave without taking damage |
| `carrier_hunter` | **Carrier Hunter** | Defeat the Carrier boss |
| `artillery_master` | **Artillery Master** | Defeat the Artillery boss |
| `necropolis_king` | **Necropolis King** | Defeat the Necropolis boss |
| `titan_slayer` | **Titan Slayer** | Defeat the Titan boss |

Achievements trigger with visual pop-ups lasting 3 seconds.

---

## 🌊 Wave & Progression System
- Each wave lasts **300 seconds (5 minutes)**.  
- A **boss** spawns when **60 seconds remain**.  
- After a boss is defeated, a **wave transition screen** displays performance stats.  
- All temporary upgrades reset for the next wave.

---

## 🧠 Performance & Memory Management
- `cleanupEntities()` ensures entity arrays remain within limits.  
- **Entity caps** (`MAX_ENTITIES`) prevent memory leaks:
  - 300 player bullets  
  - 200 enemy bullets  
  - 150 particles  
  - 20 powerups  
  - 50 enemies  
- `useRef` avoids unnecessary React re-renders.  
- All critical loops wrapped in `try/catch` to prevent crashes.

---

## 💡 UI & Menus
- **Main menu** with logo and navigation buttons.  
- **HUD (Heads-Up Display)** shows:
  - Score
  - Health
  - Wave & timer
  - Credits  
- **Upgrade cards** appear every 30 seconds.  
- **Wave transition** screen summarizes recent performance.

---

## 🔧 Key Functions
| Function | Description |
|-----------|-------------|
| `resetGame()` | Resets all temporary data and restarts gameplay |
| `tryFirePlayer()` | Handles bullet creation logic for the player |
| `spawnEnemy()` | Spawns random enemies based on current wave |
| `spawnBoss()` | Creates a boss based on wave number |
| `executeBossBehavior()` | Executes AI patterns specific to each boss |
| `updateBossPhase()` | Manages boss phase transitions |
| `handleBossDefeat()` | Handles boss death, rewards, and wave progression |
| `startWaveTransition()` | Initiates wave transition countdown |
| `checkAchievements()` | Checks and unlocks new achievements |
| `buyPermanentUpgrade()` | Manages the purchase of permanent upgrades |

---

## 🧾 Statistics
All tracked in `statsRef`:
- Total enemies/bosses defeated
- Total damage dealt
- Total play time and best survival time
- Total lives lost
- Boss-specific defeat counters

---

## 🔄 Main Loop
```tsx
const loop = useCallback((timestamp: number) => {
  update(timestamp, dt);
  if (gameState === 'playing') requestAnimationFrame(loop);
});
```
Handles:
- Movement
- Collisions
- Spawning
- HUD updates
- Visual effects
- Time and progression logic

---

## 🧰 Future Enhancements
1. **Save progress in `localStorage`**
2. **Add sound effects and music (Howler.js)**  
3. **Alternative weapon systems**  
4. **Online leaderboards**  
5. **Touch controls for mobile support**  
6. **Improved particle and explosion effects**

---

## 📁 Recommended Project Structure
```
src/
 ├── App.tsx
 ├── components/
 │    ├── HUD.tsx
 │    ├── UpgradeMenu.tsx
 │    └── AchievementPopup.tsx
 ├── hooks/
 │    ├── useGameLoop.ts
 │    └── useAchievements.ts
 ├── utils/
 │    ├── collisions.ts
 │    ├── entityFactory.ts
 │    └── constants.ts
 └── assets/
      ├── sounds/
      ├── sprites/
      └── icons/
```

---

### 🪐 Project Summary
> **Solariuz** reimagines the roguelike space shooter genre with modern React architecture, boss-driven gameplay, and endless replayability.  
> Designed for performance, clarity, and scalability — Solariuz is ready to evolve into a complete standalone experience.

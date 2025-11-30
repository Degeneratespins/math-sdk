# Stake Engine Complete Setup Guide

## Overview
This guide covers your complete Stake Engine development environment. You now have all three essential repositories forked to your GitHub account.

## Your Forked Repositories

### 1. math-sdk (Python - Backend)
**Repository:** `Degeneratespins/math-sdk`
**Purpose:** Math engine for game logic, simulations, and optimization
**Language:** Python (86.7%) + Rust (13.0%)

**Key Features:**
- Define game rules and mechanics
- Simulate game outcomes
- Optimize win distributions
- Generate lookup tables and game books
- Create configuration files for frontend/backend

**Directory Structure:**
```
games/
  ├── template/          # Start here for new games
  ├── 0_0_cluster/      # Example game
  ├── 0_0_expwilds/     # Example game
  └── ...
src/                   # Core engine code
tests/                 # Test files
optimization_program/  # Game balancing tools
```

### 2. web-sdk (TypeScript - Frontend)
**Repository:** `Degeneratespins/web-sdk`
**Purpose:** Frontend framework for creating visual slot games
**Language:** TypeScript
**Tech Stack:** PixieJS, Svelte

**Key Features:**
- In-browser game rendering
- Visual animations and effects
- UI components
- Integration with math engine outputs
- Multi-device support
- Multi-language/currency support

**Directory Structure:**
```
apps/                  # Game applications
packages/              # Reusable packages
documentation/         # Frontend docs
```

### 3. ts-client (TypeScript - API Client)
**Repository:** `Degeneratespins/ts-client`
**Purpose:** TypeScript client to communicate with Stake Engine API
**Language:** TypeScript (97.6%) + JavaScript (2.4%)

**Key Features:**
- API communication layer
- Type-safe API calls
- Protocol handling

## Complete Game Development Workflow

### Phase 1: Math Engine (Backend)

1. **Clone math-sdk:**
   ```bash
   git clone git@github.com:Degeneratespins/math-sdk.git
   cd math-sdk
   ```

2. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Start from Template:**
   ```bash
   cp -r games/template games/my_game
   ```

4. **Game Files Structure:**
   ```
   games/my_game/
   ├── library/              # Auto-generated
   │   ├── books/           # Game logic files
   │   ├── books_compressed/
   │   ├── configs/         # Config files
   │   ├── forces/
   │   └── lookup_tables/   # CSV files with probabilities
   ├── reels/               # Reel configurations
   ├── readme.txt           # Game description
   ├── run.py              # Main simulation entry
   ├── game_config.py      # Game configuration
   ├── game_executables.py # Executable functions
   ├── game_calculations.py# Calculation logic
   ├── game_events.py      # Event handlers
   ├── game_override.py    # Override functions
   └── gamestate.py        # Core game state (REQUIRED)
   ```

5. **Key File: gamestate.py**
   - Must include `run_spin()` function
   - This is the entry point for each game round
   - Handles independent simulation states

6. **Run Simulation:**
   ```bash
   cd games/my_game
   python run.py
   ```

7. **Generated Files:**
   - `library/books_compressed/*.jsonl.zst` - Compressed game logic
   - `library/lookup_tables/*.csv` - Probability tables
   - `library/configs/config.json` - Backend config
   - `library/configs/config_fe.json` - Frontend config
   - `library/configs/config_math.json` - Math optimization config

### Phase 2: Frontend (web-sdk)

1. **Clone web-sdk:**
   ```bash
   git clone git@github.com:Degeneratespins/web-sdk.git
   cd web-sdk
   ```

2. **Install Dependencies:**
   ```bash
   npm install
   # or
   yarn install
   ```

3. **Development:**
   ```bash
   npm run dev
   ```

4. **Integration:**
   - Link frontend to math engine's `config_fe.json`
   - Ensure visual elements match game logic
   - Test with generated game books

### Phase 3: API Integration (ts-client)

1. **Clone ts-client:**
   ```bash
   git clone git@github.com:Degeneratespins/ts-client.git
   cd ts-client
   ```

2. **Install:**
   ```bash
   npm install
   ```

3. **Use in Your Project:**
   ```typescript
   import { StakeEngineClient } from '@stake-engine/ts-client';
   
   const client = new StakeEngineClient({
     // configuration
   });
   ```

## Required File Format for Stake Engine

For each game mode, you need minimum 3 files:

### 1. Index File (`index.json`)
```json
{
  "modes": [
    {
      "name": "base",
      "cost": 1.0,
      "events": "books_base.jsonl.zst",
      "weights": "lookUpTable_base_0.csv"
    },
    {
      "name": "bonus",
      "cost": 100.0,
      "events": "books_bonus.jsonl.zst",
      "weights": "lookUpTable_bonus_0.csv"
    }
  ]
}
```

### 2. Lookup Table (CSV)
Format: `ID, Probability, Payout`
```
0,0.15,1.5
1,0.25,2.0
2,0.30,5.0
```

### 3. Game Logic (`.jsonl.zst`)
Compressed JSON lines with game outcomes

## Development Stack Summary

**Backend (Math):**
- Python 3.x
- Rust (for optimization)
- Compression: zStandard

**Frontend:**
- TypeScript
- PixieJS (rendering)
- Svelte (UI framework)
- Node.js/npm

**Infrastructure:**
- Carrot RGS (Remote Gaming Server)
- Handles 1M+ bets per second
- 10 billion wagers per month capacity

## Key Documentation Links

- **Math SDK Docs:** https://stakeengine.github.io/math-sdk/
- **Game Structure:** https://stakeengine.github.io/math-sdk/math_docs/overview_section/game_struct/
- **Config Files:** https://stakeengine.github.io/math-sdk/math_docs/gamestate_section/configuration_section/config_overview/
- **File Format:** https://stakeengine.github.io/math-sdk/rgs_docs/data_format/
- **Frontend Docs:** https://stakeengine.github.io/math-sdk/fe_docs/get_started/

## Quick Start Checklist

- [ ] Clone all 3 repositories
- [ ] Install Python dependencies (math-sdk)
- [ ] Install Node.js dependencies (web-sdk, ts-client)
- [ ] Copy template game from `games/template`
- [ ] Modify `gamestate.py` with your game logic
- [ ] Run simulation to generate game books
- [ ] Verify generated files in `library/` folder
- [ ] Set up frontend with generated configs
- [ ] Test integration
- [ ] Deploy to Stake Engine platform

## Revenue Model

**Stake Engine offers:**
- 10% GGR (Gross Gaming Revenue) royalties
- Monthly payments
- No hidden fees
- Direct access to Stake's player base
- $3.31 billion in turnover generated in past year

## Game Examples in Repository

Explore these example games in `math-sdk/games/`:
- `0_0_cluster` - Cluster pays mechanic
- `0_0_expwilds` - Expanding wilds
- `0_0_lines` - Traditional line-based slots
- `0_0_scatter` - Scatter symbol mechanics
- `0_0_ways` - Ways to win mechanics  
- `fifty_fifty` - 50/50 game example
- `template` - **START HERE for new games**

## Support & Resources

- GitHub Issues in respective repositories
- Official Documentation: https://stakeengine.github.io/math-sdk/
- Stake Engine Platform: https://stake-engine.com/

## Next Steps

1. **Learn:** Study the example games in `games/` folder
2. **Experiment:** Modify template game with simple changes
3. **Build:** Create your unique game mechanics
4. **Test:** Run extensive simulations
5. **Polish:** Develop frontend visual experience
6. **Deploy:** Upload to Stake Engine platform

---

**Last Updated:** November 30, 2025  
**Your GitHub:** @Degeneratespins
**Forked From:** @StakeEngine

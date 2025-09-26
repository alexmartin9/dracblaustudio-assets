---
title: Procedural Paths Generator
---

# Procedural Paths Generator — Documentation

## Overview
- Generate tilemap‑based paths with branching and constraints.
- Author map presets as ScriptableObjects (Map SO) and switchable at runtime.
- Optional difficulty filtering based on path proximity and tower coverage.
- Spawn build nodes and decorative/obstacle props around paths.
- Paths start from the left half of the map boundaries and end in a specified end point. Easily extendable.

## Requirements
- Unity 2021 or newer (tested with LTS).
- 2D Tilemap package enabled.

## Quick Start
1. Open the Demo scene and enter play time.
2. Click the top left button to create a new map. Click multiple times and see how they change.
3. Experiment with the parameters in the ProceduralMapGenerator component (more on that later).
4. When you like a configuration, create a Map SO and copy the parameters used.
5. Drag the Map prefab from the demo into your project, or recreate your own prefab following the demo structure.

## Demo Breakdown
The demo contains two key pieces:
1) Map prefab.
A single prefab that groups everything the generator needs:
- ProceduralMapsGenerator (script) — Core generator.
- MapCleaner (script) — Provides ResetMap() to clear generated content.
- Grid — Rectangular grid. Set the cell size to match your assets (demo uses 3×3×3).
- Path — Empty Tilemap to be filled by the generator.
- Background — Sprite shown behind paths; can be set in the inspector or via Map SO.
- BuildingNodes — Empty transform; the generator spawns tower build nodes under this.
- Obstacles — Empty transform; the generator spawns obstacles/decoration outside paths under this.

2) UI Buttons
Buttons wired to call Generate and Reset at runtime. You can call the same public methods from your own UI or game flow.

## Tools & Features

### 1. ProceduralMapsGenerator
Generates a random map from parameters. Two modes:
- Experimentation: Uncheck Should Use Map SO Params to tweak values directly in the component. The intended use is to first experiment with values here and when you like a map configuration, copy the parameters into a MapSO (see below). 
- Runtime / data driven: Check Should Use Map SO Params to read all parameters from a Map SO. 

General Parameters:
- Map — The Map SO that defines visuals and generation data.
- Starting Points Offset — Minimum distance between starting points so they don’t overlap at start.
- Building Nodes Margin Offset — Minimum spacing between spawned build nodes.

Map Parameters (overrides when not using Map SO):
- Map Width / Height — Number of tiles per row/column.
- Num Starting Points — Initial paths the map will have (or end leaves for mining for example).
- Spawn Left Probability (0–1) — Chance a step moves left/back. (If set to 0, the path will never move left, if 1 it will only move left.)
- Spawn Right Probability (0–1) — Chance a step moves right. (If set to 0, the path will never move right, if 1 it will only move right.)
- Max Line Length — Maximum tiles allowed in a straight line (higher → straighter paths).
- N Building Nodes — How many tower nodes to spawn.

Map Difficulty Filter (optional):

Enable Should Filter Map Difficulty to keep only maps within a difficulty range computed by a heuristic. The current implementation estimates difficulty based on path proximity, tower coverage, and overlap. You can extend CalculateMapDifficulty() to fit your game.
-Min / Max Diff Metrics — Target difficulty range.
- Tower Ranges — A list of your tower ranges (e.g., [3, 1, 1.5]).

### 2. MapSO (Map ScriptableObject)
This is how you can store data about map’s creation blueprints. Pre-define the following:
- MapN — Identifier.
- Background Sprite — Background behind paths.
- MapTilesPathSO — Tiles to use for each path type (see below).
- Tower Node Prefab — Prefab instantiated at build nodes.
- Edge Border Prefab — Prefab placed at top/bottom map borders.
- Random Obstacles — List of obstacle/decoration prefabs to spawn outside paths.
- Random Obstacles Probabilities — One‑to‑one probabilities for each obstacle prefab.
- Castle Prefab — Castle to instantiate for this map.
- Map Width / Height — Tiles per row/column.
- Num Starting Points — Initial branches.
- Spawn Left Probability (0–1) — Chance to step left/back.
- Spawn Right Probability (0–1) — Chance to step right.
- Max Line Length — Max tiles in a straight segment.
- N Building Nodes — Number of build nodes.
- Min / Max Diff Metrics — Difficulty range (for filtering).


### 3. MapTilesPathSO
Stores tiles used per path shape/type, for example:
- TileRL - Tile containing a path that moves left to right.
- TileNoD - Tile containing the triple intersection of 3 paths (up, right, left) and no path down.

### 4. ObstaclesManager
A singleton that spawns obstacles/decoration around the generated paths. Called by ProceduralMapsGenerator; tweak amounts here.
- Obstacles Quantity — 0 disables obstacles; 1 uses all. (Use intermediate values if you expose density controls.)

### 5. MapCleaner
Provides ResetMap() to remove all generated tiles, nodes, and obstacles.

### 6. Other Utils Codes 
Helper functions used by ProceduralMapsGenerator; kept separate for clarity.


## This package works great with
- Fantasy TD Maps — A collection of map “skins” / sprites (backgrounds, path tiles, decorations) to add visual variety to the map generation. (https://assetstore.unity.com/packages/slug/331610)
- Fantasy TD: Towers & Projectiles — A collection of tower, upgrades and projectile sprites that pair perfectly with the maps. (https://assetstore.unity.com/packages/slug/331598)

## Support
Email <a href="dracblaustudio@gmail.com">dracblaustudio@gmail.com</a> with:
- Steps to reproduce
- Logs / screenshots / minimal repro project (if possible)
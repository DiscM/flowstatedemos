# Earth Challenge QuickStart

This guide turns the demo projects in this folder into 30-60 minute Earth Challenge builds. It is intentionally loose: each section gives you a theme, the files to open first, what to add, and a short tutorial path.

Core prompt: make something that helps someone feel what is at stake, or what is possible, around climate change.

Solarpunk themes to lean on:

- Repair over replace
- Communal over corporate
- Grown over manufactured
- Local over global
- Accessible over exclusive
- Joyful over austere
- Conflict from scarcity, tradeoffs, disaster, and cooperation instead of villains

Source links:

- [Solarpunk Field Guide](https://speedkills.notion.site/solarpunk-field-guide-36f75dec86d88166ac53e445fcb6d084)
- [Getting Started](https://speedkills.notion.site/Getting-Started-38775dec86d88074995aeffcab52cc49)
- [Earth Month Game Design Challenge](https://challenge.getflowstate.xyz/)

## How To Use This Guide

1. Pick one base demo.
2. Pick one challenge idea from that demo's section.
3. Do the 30-minute path first.
4. If time remains, add one item from the 60-minute stretch list.
5. Keep the final playable loop tiny: one goal, one visible progress indicator, one ending moment.

Recommended order for beginners:

1. `platformer`
2. `truck_town`
3. `multiplayer_bomber`
4. `voxel`
5. `dynamic_split_screen`
6. `2.5d`

## Shared Additions For Any Demo

These are useful in almost every project.

### Add A Challenge Label

Add a `CanvasLayer` with a `Label` named `ChallengeLabel`, or use an existing UI label if the project already has one.

Suggested text format:

```text
Seeds: 0/12
Restore the rooftop garden before sunset.
```

### Add A Tiny Challenge Script

Create a script named `challenge.gd` in the project folder when you need a simple objective tracker.

```gdscript
extends Node

@export var target_count := 5
var progress := 0

@onready var label: Label = $"../CanvasLayer/ChallengeLabel"

func _ready() -> void:
	update_label()

func add_progress(amount := 1) -> void:
	progress += amount
	update_label()

func update_label() -> void:
	if progress >= target_count:
		label.text = "Complete: the neighborhood is ready."
	else:
		label.text = "Progress: %s/%s" % [progress, target_count]
```

Do not worry if your first version is rough. A visible counter and a clear ending are enough for a strong prototype.

## Project 1: `platformer`

Best for: collectible quests, rooftop rescue runs, seed gathering, repair part hunts, radio tower climbs.

Open first:

- `platformer/project.godot`
- `platformer/game.tscn`
- `platformer/coin/coin.tscn`
- `platformer/coin/coin.gd`
- `platformer/player/player.gd`

Existing mechanics to reuse:

- The player already moves, jumps, shoots, falls, and respawns.
- Coins already detect the player and increment `body.coins`.
- `player.gd` already updates the floating coin counter.
- The main scene already contains many coins and enemies.

Good challenge ideas:

- Rooftop Garden Run
- Tool Library Dash
- Pollinator Platform Trail
- Compost Chain Reaction
- Seed Vault Discovery
- Radio Signal Rescue
- The Last Cool Room
- Neighborhood Bulletin Board

### What To Add

For a 30-minute version:

- Rename the idea of `coins` into a climate resource: seeds, tools, compost, repair parts, or water canisters.
- Change the player counter from only a number into a goal label.
- Move or delete some coin instances in `game.tscn` so the path feels intentional.
- Add one ending condition when enough resources are collected.

For a 60-minute version:

- Add 2-3 resource types using duplicated coin scenes.
- Add a final drop-off zone such as `GardenDropoff`, `RepairCafe`, or `CoolingCenter`.
- Add short narrative text when the player completes the goal.
- Tune enemy placement so hazards feel like heat, debris, distance, or time pressure instead of monsters.

### Loose Tutorial: Rooftop Garden Run

Goal: collect seed packets and water canisters to restore a rooftop garden.

1. Open `platformer/game.tscn`.
2. Find the `Coins` node. Keep 10-20 coins and delete or move the rest for a shorter route.
3. Open `platformer/player/player.gd`.
4. Find the counter update in `_physics_process`, where `%CoinCount.text = str(coins)`.
5. Change the visible counter to say what the player is collecting.

Example:

```gdscript
%CoinCount.text = "Seeds: " + str(coins) + "/20"
%CoinCount.get_node(^"Parallax").text = %CoinCount.text
%CoinCount.get_node(^"Parallax2").text = %CoinCount.text
%CoinCount.get_node(^"Parallax3").text = %CoinCount.text
%CoinCount.get_node(^"Parallax4").text = %CoinCount.text
```

6. Add a completion check below the counter update.

```gdscript
if coins >= 20:
	%CoinCount.text = "Garden restored"
```

7. Optional: duplicate `coin.tscn` into `seed.tscn` and adjust the material color to green or gold.
8. Run `platformer/project.godot` and make sure the player can finish in under five minutes.

### Loose Tutorial: Radio Signal Rescue

Goal: collect antenna repair parts at high points.

1. Open `platformer/game.tscn`.
2. Move coins to high platforms, ramps, or risky jumps.
3. Rename the counter to `Parts: 0/8`.
4. Add a simple rule: once enough parts are collected, the radio station is restored.
5. Optional stretch: place a visible antenna using a simple `MeshInstance3D` cylinder or box near the final platform.

Suggested final text:

```text
Signal restored. The neighborhood can hear the storm updates again.
```

### Judging Prompt

Ask players:

- What did you restore?
- Who benefits from it?
- What does the player understand by the end that they did not at the start?

## Project 2: `truck_town`

Best for: delivery challenges, power budgeting, storm prep, mutual aid logistics, festival setup.

Open first:

- `truck_town/project.godot`
- `truck_town/car_select/car_select.tscn`
- `truck_town/town/town_scene.tscn`
- `truck_town/town/town_scene.gd`
- `truck_town/vehicles/vehicle.gd`

Existing mechanics to reuse:

- Vehicle selection already works.
- Driving, braking, boost, horn, headlights, speedometer, and camera changes already work.
- `town_scene.gd` already manages moods: sunrise, day, sunset, night.
- The town already has trees, lamps, roads, and a UI layer.

Good challenge ideas:

- Community Battery Relay
- Solar Storm Triage
- Shared Meal Sprint
- Festival Before Sunset
- Festival Power Budget
- Neighborhood Mood Shift

### What To Add

For a 30-minute version:

- Add 3-5 `Area3D` delivery zones to `town_scene.tscn`.
- Add a `Label` to the existing UI for delivery progress.
- Add a variable in `town_scene.gd` such as `deliveries_done`.
- When the truck enters a zone, increment progress and update the label.

For a 60-minute version:

- Add a timer: "Storm arrives in 180 seconds."
- Give each destination a name: Clinic, Garden, Cooling Center, Workshop.
- Add a limited resource budget, such as 3 battery deliveries for 5 possible stations.
- Tie progress to mood changes: day becomes sunset as time runs out, lights turn on when power is delivered.

### Loose Tutorial: Solar Storm Triage

Goal: deliver supplies before a storm arrives.

1. Open `truck_town/town/town_scene.tscn`.
2. Add a new `Node3D` named `DeliveryZones`.
3. Under it, add 3 `Area3D` nodes named `ClinicZone`, `GardenZone`, and `WorkshopZone`.
4. For each `Area3D`, add a `CollisionShape3D` with a `BoxShape3D`.
5. Move each zone to a memorable spot in the town.
6. Add a `Label` under the existing `GameUI` or `CarUI`, named `ChallengeLabel`.
7. Open `truck_town/town/town_scene.gd`.
8. Add state near the top:

```gdscript
var deliveries_done := 0
var deliveries_needed := 3
var delivered_zones := {}

@onready var challenge_label: Label = %ChallengeLabel
```

9. Add a helper function:

```gdscript
func complete_delivery(zone_name: String) -> void:
	if delivered_zones.has(zone_name):
		return
	delivered_zones[zone_name] = true
	deliveries_done += 1
	if deliveries_done >= deliveries_needed:
		challenge_label.text = "Storm prep complete"
		mood = Mood.SUNSET
	else:
		challenge_label.text = "Deliveries: %s/%s" % [deliveries_done, deliveries_needed]
```

10. Connect each delivery zone's `body_entered` signal to a handler.

```gdscript
func _on_delivery_zone_body_entered(body: Node3D, zone_name: String) -> void:
	if body.is_in_group(&"car"):
		complete_delivery(zone_name)
```

11. If signal binding feels slow in the editor, make three separate handlers instead:

```gdscript
func _on_clinic_zone_body_entered(body: Node3D) -> void:
	if body.is_in_group(&"car"):
		complete_delivery("Clinic")
```

12. Run the scene and check that each delivery counts once.

### Loose Tutorial: Festival Power Budget

Goal: choose which stations receive limited stored solar power.

1. Add 5 delivery zones: Lights, Music, Food, Cooling Tent, Transit.
2. Set `deliveries_needed` to 3, not 5.
3. When the player delivers to 3 zones, lock the rest and show final text.
4. Make the ending say what was powered and what had to wait.

Suggested ending:

```text
The festival opens with food, cooling, and transit powered.
The town writes down what to improve before the next heatwave.
```

### Judging Prompt

Ask players:

- Which delivery mattered most?
- What did they choose not to power?
- Did the town feel more alive by the end?

## Project 3: `multiplayer_bomber`

Best for: clearing debris, salvage, repair cafes, cooperative board challenges, stormproofing.

Open first:

- `multiplayer_bomber/project.godot`
- `multiplayer_bomber/lobby.tscn`
- `multiplayer_bomber/world.tscn`
- `multiplayer_bomber/rock.tscn`
- `multiplayer_bomber/rock.gd`
- `multiplayer_bomber/bomb.gd`
- `multiplayer_bomber/score.gd`

Existing mechanics to reuse:

- Players can move around a tile map.
- Bombs already affect objects in range.
- Rocks already call `Score.increase_score`.
- The game already checks when all rocks are gone.

Good challenge ideas:

- Repair Cafe Stories
- Repair Over Replace
- Solar Tile Salvage
- Stormproof Workshop

### What To Add

For a 30-minute version:

- Rename rocks as broken objects, debris, or salvage piles.
- Rename bombs as repair charges.
- Change score text from competitive points to shared progress.
- Change the winner message into a community completion message.

For a 60-minute version:

- Add a shared timer.
- Add 2-3 special salvage objects with labels.
- Add safe zones or workshop zones that must be protected.
- Make the end condition cooperative: the group wins when enough repair work is done.

### Loose Tutorial: Repair Cafe Stories

Goal: each cleared object reveals a small neighborhood repair story.

1. Open `multiplayer_bomber/world.tscn`.
2. Decide what rocks represent: old fan, broken radio, bike light, cracked water filter.
3. Open `multiplayer_bomber/score.gd`.
4. Change `increase_score` so it updates a shared label, or add a new shared variable:

```gdscript
var repairs_done := 0
var repairs_needed := 10
```

5. In `increase_score`, increment `repairs_done` and update all score labels or the winner label.

```gdscript
repairs_done += 1
if repairs_done >= repairs_needed:
	$"../Winner".set_text("Repair cafe complete")
	$"../Winner".show()
```

6. Open `multiplayer_bomber/rock.gd`.
7. Add a small list of story lines:

```gdscript
const STORIES := [
	"Fixed a box fan for the cooling center.",
	"Recovered copper wire for the solar shed.",
	"Repaired a bike light for the night ride.",
	"Cleaned a water filter for the garden."
]
```

8. When a rock explodes, choose one story and show it in a label. The fastest path is to reuse the `Winner` label briefly, or add a new `Label` named `StoryLabel` to `world.tscn`.
9. Run a local host game and check that clearing objects advances shared progress.

### Loose Tutorial: Solar Tile Salvage

Goal: clear debris to uncover reusable solar tiles.

1. Keep the normal board layout.
2. Pick a subset of rocks to count as solar tile salvage.
3. Change score language to `Tiles salvaged`.
4. Set the completion target lower than the full rock count, such as 12.
5. Optional stretch: when a rock is cleared, spawn a small yellow square or label where it was.

### Judging Prompt

Ask players:

- Did the mechanics feel like repair, or still like destruction?
- What wording or visual change helped most?
- Did players cooperate naturally?

## Project 4: `voxel`

Best for: building, restoration, before/after scenes, habitat corridors, accessibility routes.

Open first:

- `voxel/project.godot`
- `voxel/player/player.tscn`
- `voxel/player/player.gd`
- `voxel/world/voxel_world.gd`
- `voxel/world/terrain_generator.gd`
- `voxel/world/chunk.gd`

Existing mechanics to reuse:

- The player can move in first person.
- The player can place and break blocks.
- Blocks are selected with previous/next controls or pick block.
- Terrain generation already supports flat grass.

Good challenge ideas:

- Pollinator Corridor Builder
- Heat Island Rescue Map
- Accessible Route Builder
- Unbury the River
- Old World Device

### What To Add

For a 30-minute version:

- Use the existing block placement as the whole challenge.
- Define a start point, an end point, and 3 required build features.
- Add a simple instruction label to the menu or scene.
- Judge manually by checking whether the player built the requested space.

For a 60-minute version:

- Limit the allowed block palette to a few symbolic materials.
- Add a simple checklist UI.
- Add start/end markers using colored blocks or simple `MeshInstance3D` objects.
- Optional: add a script that checks whether certain marker areas contain blocks.

### Loose Tutorial: Pollinator Corridor Builder

Goal: build a connected habitat path across a damaged area.

1. Open `voxel/project.godot` and run the game once to understand block controls.
2. Open `voxel/player/player.gd`.
3. Find `_selected_block`. This controls the starting block.
4. Pick 3-4 block IDs to represent habitat pieces:

```text
Grass = habitat floor
Leaves = pollinator shelter
Water or glass = water source
Wood = bee hotel or sign post
```

5. Add challenge instructions to an existing menu label, or create a simple `CanvasLayer` with:

```text
Build a pollinator corridor from spawn to the far marker.
Required: flowers/shelter, water, shade, and a walkable path.
```

6. If you want a stricter beginner experience, temporarily prevent block cycling outside your chosen range in `player.gd`.
7. Place two visible markers in the world: start and end. These can be simple colored blocks or `MeshInstance3D` cubes.
8. Let players build for 15-25 minutes, then do a walkthrough.

### Loose Tutorial: Heat Island Rescue Map

Goal: transform a hot, exposed area into a cooler public route.

1. Start from the flat terrain mode.
2. Define a rectangular area as the "hot block."
3. Ask players to add:

- A shaded path
- Rest spots
- Water or cooling features
- A route wide enough to navigate

4. Optional stretch: add a final screenshot or walkthrough where the player explains their design.

### Judging Prompt

Ask players:

- Can someone cross the map comfortably?
- Where would a person rest?
- What changed from extraction/consumption to care/maintenance?

## Project 5: `dynamic_split_screen`

Best for: two-player cooperation, distance/connection metaphors, bridge building, mutual aid navigation.

Open first:

- `dynamic_split_screen/project.godot`
- `dynamic_split_screen/split_screen.tscn`
- `dynamic_split_screen/player.gd`
- `dynamic_split_screen/camera_controller.gd`

Existing mechanics to reuse:

- Two players already move independently.
- The screen splits when players separate.
- The world already has walls and a shared play space.
- The split line is customizable through `camera_controller.gd`.

Good challenge ideas:

- Two Communities, One Bridge
- Bridge of Compromise
- Mutual Aid Maze

### What To Add

For a 30-minute version:

- Add 2-3 paired activation zones.
- Add a shared progress label.
- Require both players to stand near matching stations.
- End with a message that the bridge or route is complete.

For a 60-minute version:

- Add a simple sequence: station 1, then station 2, then station 3.
- Add different station names: Water, Food, Transit, Clinic.
- Change split line color/thickness to reflect distance or cooperation.
- Adjust walls so players must communicate.

### Loose Tutorial: Two Communities, One Bridge

Goal: two players must coordinate to build bridge sections.

1. Open `dynamic_split_screen/split_screen.tscn`.
2. Add a `CanvasLayer` with a `Label` named `ChallengeLabel`.
3. Add a `Node3D` named `BridgeStations`.
4. Add paired `Area3D` nodes:

- `Station1A` near Player 1
- `Station1B` near Player 2
- `Station2A`
- `Station2B`

5. Give each `Area3D` a `CollisionShape3D`.
6. Create a new script named `bridge_challenge.gd` and attach it to `BridgeStations`.
7. Track whether each player is inside the current pair.

Example shape:

```gdscript
extends Node3D

var current_station := 1
var player_a_ready := false
var player_b_ready := false

@onready var label: Label = $"../CanvasLayer/ChallengeLabel"

func _ready() -> void:
	label.text = "Meet at bridge station 1"

func set_ready(player_id: int, ready: bool) -> void:
	if player_id == 1:
		player_a_ready = ready
	else:
		player_b_ready = ready
	if player_a_ready and player_b_ready:
		current_station += 1
		player_a_ready = false
		player_b_ready = false
		label.text = "Bridge section complete"
```

8. Connect each station's `body_entered` and `body_exited` signals to call `set_ready`.
9. Run the scene and test with both control schemes.

### Loose Tutorial: Mutual Aid Maze

Goal: players guide each other through different parts of the same map.

1. Move walls in `split_screen.tscn` to create two partial routes.
2. Place supplies on one side and repair stations on the other.
3. Add a rule: a pickup only counts when both players reach their next stations.
4. Encourage players to talk out loud while navigating.

### Judging Prompt

Ask players:

- Did the split screen make distance feel meaningful?
- Did both players have something useful to do?
- Did the ending feel like reconnection?

## Project 6: `2.5d`

Best for: small diorama scenes, symbolic spaces, transit hubs, cooling centers, garden layouts.

Open first:

- `2.5d/project.godot`
- `2.5d/assets/demo_scene.tscn`
- `2.5d/assets/player/player_25d.tscn`
- `2.5d/assets/player/player_math_25d.gd`
- `2.5d/addons/node25d/node_25d.gd`

Existing mechanics to reuse:

- The player can move and jump in a 2.5D space.
- The scene already contains platforms.
- The project supports several view modes.
- `Node25D` converts 3D positions into 2D presentation.

Good challenge ideas:

- No Ads Transit Hub
- The Last Cool Room
- Festival Before Sunset
- Community Skill Tree

### What To Add

For a 30-minute version:

- Rearrange platforms into a small public space.
- Add labels for community features: shade, water, repair, transit, garden, library.
- Add a simple checklist.
- Make the player walk through the finished space.

For a 60-minute version:

- Duplicate platform nodes into symbolic stations.
- Add a few collectible or activation zones.
- Add an end state when all stations are visited.
- Lock the view mode to the one that reads best for your diorama.

### Loose Tutorial: No Ads Transit Hub

Goal: build a small transit hub that serves people instead of selling to them.

1. Open `2.5d/assets/demo_scene.tscn`.
2. Duplicate a few existing platform nodes.
3. Arrange them into a central station shape.
4. Add simple labels or sprites for:

- Bike parking
- Water refill
- Repair kiosk
- Community board
- Solar canopy
- Lending library

5. Add a `CanvasLayer` with checklist text:

```text
Transit Hub Checklist
- Shade
- Water
- Repair
- Shared information
```

6. Optional: add `Area2D` or `Area3D` triggers so the checklist completes as the player visits each station.
7. Run the scene and make sure the route is readable from the default view.

### Loose Tutorial: The Last Cool Room

Goal: prepare a cooling center during a heatwave.

1. Pick one platform as the cooling center.
2. Add four stations around it: fans, plants, curtains, battery packs.
3. Add a label that tells the player what remains.
4. When all stations are visited, show:

```text
Cooling center ready. Nobody has to face the heat alone.
```

### Judging Prompt

Ask players:

- Can you tell what the place is for without reading a paragraph?
- Does the space feel public and welcoming?
- What object or station communicates care best?

## Challenge Idea Index By Difficulty

### Difficulty 1: Easiest Remix

- `platformer`: Rooftop Garden Run
- `platformer`: Tool Library Dash
- `platformer`: Pollinator Platform Trail
- `platformer`: Compost Chain Reaction
- `platformer`: Seed Vault Discovery
- `platformer`: Neighborhood Bulletin Board
- `multiplayer_bomber`: Repair Cafe Stories

### Difficulty 2: Light Gameplay Additions

- `truck_town`: Community Battery Relay
- `truck_town`: Solar Storm Triage
- `truck_town`: Shared Meal Sprint
- `truck_town`: Festival Before Sunset
- `truck_town`: Festival Power Budget
- `truck_town`: Neighborhood Mood Shift
- `platformer`: Radio Signal Rescue
- `platformer` or `2.5d`: The Last Cool Room

### Difficulty 3: Scene Building Or Systems Remix

- `voxel`: Pollinator Corridor Builder
- `voxel`: Heat Island Rescue Map
- `voxel`: Accessible Route Builder
- `voxel`: Unbury the River
- `dynamic_split_screen`: Two Communities, One Bridge
- `dynamic_split_screen`: Bridge of Compromise
- `dynamic_split_screen`: Mutual Aid Maze
- `multiplayer_bomber`: Repair Over Replace
- `multiplayer_bomber`: Solar Tile Salvage
- `multiplayer_bomber`: Stormproof Workshop

### Difficulty 4: Most Ambitious For 60 Minutes

- `platformer`: Cryptic Garden Notebook
- `platformer` or `voxel`: Old World Device
- `2.5d`: No Ads Transit Hub
- `truck_town` or `platformer`: Storm Resource Council
- `platformer`: Community Skill Tree

## Final Polish Checklist

Use this checklist in the last 10 minutes:

- The player knows the goal within 10 seconds.
- The progress indicator changes when the player does the right thing.
- The challenge can be completed in under five minutes.
- The ending text names the community benefit.
- At least one visual or mechanic says "repair, care, or cooperation" instead of only "win."


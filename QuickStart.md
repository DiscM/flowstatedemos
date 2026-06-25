# Earth Challenge QuickStart
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

- You already move, jump, shoot, fall, and respawn.
- Coins already detect you and increment `body.coins`.
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
- Microgrid Cable Connector
- Seed Bombing Run

### What To Add

For a 30-minute version:

- Rename the idea of `coins` into a climate resource: seeds, tools, compost, repair parts, or water canisters.
- Change the counter from only a number into a goal label.
- Move or delete some coin instances in `game.tscn` so the path feels intentional.
- Add one ending condition when enough resources are collected.

For a 60-minute version:

- Add 2-3 resource types using duplicated coin scenes.
- Add a final drop-off zone such as `GardenDropoff`, `RepairCafe`, or `CoolingCenter`.
- Add short narrative text when you complete the goal.
- Tune enemy placement so hazards feel like heat, debris, distance, or time pressure instead of monsters.

### Loose Tutorial: Rooftop Garden Run

Goal: collect seed packets and water canisters to restore a rooftop garden.

1. Open `platformer/game.tscn`.
2. Find the `Coins` node. Keep 10-20 coins and delete or move the rest for a shorter route.
3. Open `platformer/player/player.gd`.
4. Find the counter update in `_physics_process`, where `%CoinCount.text = str(coins)`.
5. Change the visible counter to say what you are collecting.

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
8. Run `platformer/project.godot` and make sure you can finish in under five minutes.

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

Ask:

- What did you restore?
- Who benefits from it?
- What do you understand by the end that you did not at the start?

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
- Mobile Repair Unit
- Food Forest Harvest

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
3. When you deliver to 3 zones, lock the rest and show final text.
4. Make the ending say what was powered and what had to wait.

Suggested ending:

```text
The festival opens with food, cooling, and transit powered.
Write down what to improve before the next heatwave.
```

### Judging Prompt

Ask:

- Which delivery mattered most?
- What did you choose not to power?
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

- You can move around a tile map.
- Bombs already affect objects in range.
- Rocks already call `Score.increase_score`.
- The game already checks when all rocks are gone.

Good challenge ideas:

- Repair Cafe Stories
- Repair Over Replace
- Solar Tile Salvage
- Stormproof Workshop
- Wildfire Firebreak
- Water Filtration Network

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

Ask:

- Did the mechanics feel like repair, or still like destruction?
- What wording or visual change helped most?
- Did you cooperate naturally?

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

- You can move in first person.
- You can place and break blocks.
- Blocks are selected with previous/next controls or pick block.
- Terrain generation already supports flat grass.

Good challenge ideas:

- Pollinator Corridor Builder
- Heat Island Rescue Map
- Accessible Route Builder
- Unbury the River
- Old World Device
- Floodplain Restorer
- Vertical Forest Architect

### What To Add

For a 30-minute version:

- Use the existing block placement as the whole challenge.
- Define a start point, an end point, and 3 required build features.
- Add a simple instruction label to the menu or scene.
- Judge manually by checking whether you built the requested space.

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
8. Spend 15-25 minutes building, then do a walkthrough.

### Loose Tutorial: Heat Island Rescue Map

Goal: transform a hot, exposed area into a cooler public route.

1. Start from the flat terrain mode.
2. Define a rectangular area as the "hot block."
3. Add:

- A shaded path
- Rest spots
- Water or cooling features
- A route wide enough to navigate

4. Optional stretch: add a final screenshot or walkthrough to explain your design.

### Judging Prompt

Ask:

- Can you cross the map comfortably?
- Where can you rest?
- What changed from extraction/consumption to care/maintenance?

## Project 5: `dynamic_split_screen`

Best for: two-player cooperation, distance/connection metaphors, bridge building, mutual aid navigation.

Open first:

- `dynamic_split_screen/project.godot`
- `dynamic_split_screen/split_screen.tscn`
- `dynamic_split_screen/player.gd`
- `dynamic_split_screen/camera_controller.gd`

Existing mechanics to reuse:

- You and a partner already move independently.
- The screen splits when you separate.
- The world already has walls and a shared play space.
- The split line is customizable through `camera_controller.gd`.

Good challenge ideas:

- Two Communities, One Bridge
- Bridge of Compromise
- Mutual Aid Maze
- Geothermal Valve Tuners
- Wildlife Corridor Guide

### What To Add

For a 30-minute version:

- Add 2-3 paired activation zones.
- Add a shared progress label.
- Require both of you to stand near matching stations.
- End with a message that the bridge or route is complete.

For a 60-minute version:

- Add a simple sequence: station 1, then station 2, then station 3.
- Add different station names: Water, Food, Transit, Clinic.
- Change split line color/thickness to reflect distance or cooperation.
- Adjust walls so you must communicate.

### Loose Tutorial: Two Communities, One Bridge

Goal: coordinate with a partner to build bridge sections.

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
7. Track whether each of you is inside the current pair.

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

Goal: guide each other through different parts of the same map.

1. Move walls in `split_screen.tscn` to create two partial routes.
2. Place supplies on one side and repair stations on the other.
3. Add a rule: a pickup only counts when both of you reach your next stations.
4. Talk out loud while navigating.

### Judging Prompt

Ask:

- Did the split screen make distance feel meaningful?
- Did both of you have something useful to do?
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

- You can move and jump in a 2.5D space.
- The scene already contains platforms.
- The project supports several view modes.
- `Node25D` converts 3D positions into 2D presentation.

Good challenge ideas:

- No Ads Transit Hub
- The Last Cool Room
- Festival Before Sunset
- Community Skill Tree
- Rainwater Gravity Feed
- Bike-Powered Outdoor Cinema

### What To Add

For a 30-minute version:

- Rearrange platforms into a small public space.
- Add labels for community features: shade, water, repair, transit, garden, library.
- Add a simple checklist.
- Walk through the finished space.

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

6. Optional: add `Area2D` or `Area3D` triggers so the checklist completes as you visit each station.
7. Run the scene and make sure the route is readable from the default view.

### Loose Tutorial: The Last Cool Room

Goal: prepare a cooling center during a heatwave.

1. Pick one platform as the cooling center.
2. Add four stations around it: fans, plants, curtains, battery packs.
3. Add a label that tells you what remains.
4. When all stations are visited, show:

```text
Cooling center ready. Nobody has to face the heat alone.
```

### Judging Prompt

Ask:

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
- `platformer`: Seed Bombing Run
- `multiplayer_bomber`: Repair Cafe Stories

### Difficulty 2: Light Gameplay Additions

- `truck_town`: Community Battery Relay
- `truck_town`: Solar Storm Triage
- `truck_town`: Shared Meal Sprint
- `truck_town`: Festival Before Sunset
- `truck_town`: Festival Power Budget
- `truck_town`: Neighborhood Mood Shift
- `truck_town`: Mobile Repair Unit
- `truck_town`: Food Forest Harvest
- `platformer`: Radio Signal Rescue
- `platformer`: Microgrid Cable Connector
- `platformer` or `2.5d`: The Last Cool Room
- `multiplayer_bomber`: Wildfire Firebreak
- `2.5d`: Bike-Powered Outdoor Cinema

### Difficulty 3: Scene Building Or Systems Remix

- `voxel`: Pollinator Corridor Builder
- `voxel`: Heat Island Rescue Map
- `voxel`: Accessible Route Builder
- `voxel`: Unbury the River
- `voxel`: Floodplain Restorer
- `voxel`: Vertical Forest Architect
- `dynamic_split_screen`: Two Communities, One Bridge
- `dynamic_split_screen`: Bridge of Compromise
- `dynamic_split_screen`: Mutual Aid Maze
- `dynamic_split_screen`: Geothermal Valve Tuners
- `multiplayer_bomber`: Repair Over Replace
- `multiplayer_bomber`: Solar Tile Salvage
- `multiplayer_bomber`: Stormproof Workshop
- `multiplayer_bomber`: Water Filtration Network
- `2.5d`: Rainwater Gravity Feed

### Difficulty 4: Most Ambitious For 60 Minutes

- `platformer`: Cryptic Garden Notebook
- `platformer` or `voxel`: Old World Device
- `2.5d`: No Ads Transit Hub
- `truck_town` or `platformer`: Storm Resource Council
- `platformer`: Community Skill Tree
- `dynamic_split_screen`: Wildlife Corridor Guide

## Additional Challenge & Game Loop Pairings

Here are more examples of climate-focused challenge themes paired with concrete gameplay mechanics you can implement:

### Project 1: `platformer` (Vertical / Collectible Loops)
- **Microgrid Cable Connector**: 
  - *Game Loop*: Connect solar battery packs across high rooftops. Instead of collecting coins, you carry cable segments to sockets at high points to bridge communities.
- **Seed Bombing Run**:
  - *Game Loop*: Reach barren building ledges within a limit. Touching a ledge spawns ivy meshes and flowers, turning concrete gray into green.

### Project 2: `truck_town` (Logistics / Delivery Loops)
- **Mobile Repair Unit**:
  - *Game Loop*: Drive a tool-filled truck to broken farm machinery around town. Each location requires you to stop, wait 5 seconds for repair, and proceed before the harvest heatwave hits.
- **Food Forest Harvest**:
  - *Game Loop*: Gather ripe produce from community orchards and deliver them to neighborhood distribution pantries before sunset.

### Project 3: `multiplayer_bomber` (Grid / Action Loops)
- **Wildfire Firebreak**:
  - *Game Loop*: Clear combustible dry brush (replacing rocks) to build a firebreak around the community greenhouse. Bombs represent controlled burns.
- **Water Filtration Network**:
  - *Game Loop*: Clear debris blocks to connect piping and gravel filters, restoring the neighborhood's clean water flow.

### Project 4: `voxel` (3D Construction / Environmental Loops)
- **Floodplain Restorer**:
  - *Game Loop*: Excavate pathways and place wetland blocks (spongy grass or water) to divert rain channels away from residential structures.
- **Vertical Forest Architect**:
  - *Game Loop*: Build trellises and plant moss/ivy blocks on concrete structures to lower local ambient temperature.

### Project 5: `dynamic_split_screen` (Cooperative Coordination Loops)
- **Geothermal Valve Tuners**:
  - *Game Loop*: You and a partner must locate and stand on pressure valves on opposite sides of a geothermal plant simultaneously to prevent a blackout.
- **Wildlife Corridor Guide**:
  - *Game Loop*: One character guides a migrating animal herd while the partner clears barriers, opens gates, and redirects traffic on their side of the screen.

### Project 6: `2.5d` (Diorama / Activation Loops)
- **Rainwater Gravity Feed**:
  - *Game Loop*: Assemble a gravity-fed water system with gutter pipes and collection barrels across platforms to capture rain for the dry season.
- **Bike-Powered Outdoor Cinema**:
  - *Game Loop*: Setup bike generators and run wiring to power a community film screening. You must activate dynamos in sequence to keep the movie running.

## Generic Climate & Solarpunk Game Ideas (Engine-Agnostic)

If you are building a custom project from scratch using web technologies (HTML/JS/Canvas) or another game engine, here are generic, engine-agnostic game loops to spark inspiration. They focus on community, restoration, and care instead of typical conflict/extraction mechanics.

### 1. The Shared Tool Library (Logistics / Inventory Management)
- **Theme**: Mutual aid, tool sharing, and community maintenance.
- **Core Loop**: You manage a neighborhood tool shed. Residents arrive with repair requests (e.g., "fixing a leaky window" or "pruning community orchards"). You must select the appropriate tools, hand them over, and check them back in when returned, performing repairs on the tools when their wear levels get too high.
- **Loose Guidelines**: Track tool inventory and wear states. Introduce a dynamic queue of incoming requests with different urgency levels. The goal is to maximize neighborhood satisfaction and tool longevity while staying within a limited community budget.

### 2. Microgrid Balancing Act (Puzzle / Resource Allocation)
- **Theme**: Renewable energy, battery storage, and dynamic power distribution.
- **Core Loop**: Direct power from fluctuating renewable sources (solar panels active during the day, wind turbines active during storms) into a shared battery bank, then allocate that power to critical city structures.
- **Loose Guidelines**: Define power inputs based on changing weather conditions and power demands for different buildings (e.g., clinic, cooling center, water pumps, residential lighting). When demand exceeds generation, choose which sectors to brown out. The game is lost if critical sectors remain unpowered for too long.

### 3. Rewilding the City (Grid-Based / Spatial Puzzle)
- **Theme**: Urban agriculture, green corridors, and reducing the urban heat island effect.
- **Core Loop**: Place green infrastructure tiles (shade trees, bioswales, insect hotels, native wild meadows, rain gardens) onto a grid representing a hot, concrete-paved street.
- **Loose Guidelines**: Give each tile type a set of resource costs (soil, water, compost) and positive local impacts (shade trees reduce local heat, bioswales capture storm runoff, native meadows boost biodiversity). The goal is to reach a target temperature reduction and biodiversity index before a heatwave strikes.

### 4. Mutual Aid Courier (Navigation / Hazard Avoidance)
- **Theme**: Direct action, community care, and disaster resilience.
- **Core Loop**: Navigate a map during an extreme weather event (like a flood or blizzard) to deliver critical supplies (water filters, medicine, battery packs, radios) to isolated neighbors.
- **Loose Guidelines**: You have limited carrying capacity and physical stamina or battery life. Navigating through hazard zones (flooded streets, high winds) drains your stamina faster. You must coordinate visits to community relief hubs to restock and rest.

### 5. Compost Chain Reaction (Physics / Merge Puzzle)
- **Theme**: Closing the loop on waste, soil health, and organic restoration.
- **Core Loop**: Drop or slide organic waste items (food scraps, dried leaves, cardboard, coffee grounds) into a compost bin to combine them. Matching organic waste types merges them, advancing them through decomposition stages until they become rich organic compost.
- **Loose Guidelines**: Implement simple grid or physics-based merging (like a matching puzzle). The goal is to generate a target amount of high-quality compost to nourish a community crop field before the planting season ends.

### 6. The Migration Guide (Pathfinding / Environmental Influence)
- **Theme**: Habitat restoration and ecological corridors.
- **Core Loop**: Help a migrating group of wild animals cross a fragmented landscape safely. You do not control the animals directly; instead, you build bridges, clear waste, plant edible flora, and block hazardous areas ahead of them.
- **Loose Guidelines**: The animals use automatic pathfinding to traverse the screen. You have a limited resource pool (or build actions) to edit paths, disable fences, or plant safe zones. The goal is to guide a minimum percentage of the herd safely to the other side.

## Final Polish Checklist

Use this checklist in the last 10 minutes:

- The goal is clear within 10 seconds.
- The progress indicator changes when you do the right thing.
- The challenge can be completed in under five minutes.
- The ending text names the community benefit.
- At least one visual or mechanic says "repair, care, or cooperation" instead of only "win."


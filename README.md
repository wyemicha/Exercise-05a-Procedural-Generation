# Exercise-05a-Procedural-Generation
Exercise for MSCH-C220, 22 March 2021

This exercise is designed to give you some brief experience with creating a procedurally-generated 2D maze. It is based on the excellent tutorial by [KidsCanCode](https://kidscancode.org/blog/2018/08/godot3_procgen1/).

Fork this repository. When that process has completed, make sure that the top of the repository reads [your username]/Exercise-04c-Levels. *Edit the LICENSE and replace BL-MSCH-C220-S21 with your full name.* Commit your changes.

Clone the repository to a Local Path on your computer.

Open Godot. Import the project.godot file and open the "Procedural Generation" project.

The first step is to create the tiles we will use to generate the maze. Under the Scene menu, select "New Inherited Scene…". Select res://Maze/Tile.tscn. In the new scene, change the name of the Tile node to 00, and then change the Texture property of the Sprite node to res://Assets/00.png. Save the scene as res://Maze/00.tscn (*make sure you save it in the Maze folder or the script will not work!*).

Repeat these steps for scenes 01–15. When you are done, you should have sixteen new scenes in the Maze folder (named 00.tscn–15.tscn). Each of these scenes should have a different texture for the Sprite node. For example, res://Maze/05.tscn should use the res://Assets/05.png file as the texture for the Sprite in that scene.

Now open res://Maze/Maze.gd. Replace the contents of the script with the following:
```
const N = 1
const E = 2
const S = 4
const W = 8

var cell_walls = {Vector2(0, -1): N, Vector2(1, 0): E, 
				  Vector2(0, 1): S, Vector2(-1, 0): W}

var map = []
var tiles = [
	load("res://Maze/00.tscn")
	,load("res://Maze/01.tscn")
	,load("res://Maze/02.tscn")
	,load("res://Maze/03.tscn")
	,load("res://Maze/04.tscn")
	,load("res://Maze/05.tscn")
	,load("res://Maze/06.tscn")
	,load("res://Maze/07.tscn")
	,load("res://Maze/08.tscn")
	,load("res://Maze/09.tscn")
	,load("res://Maze/10.tscn")
	,load("res://Maze/11.tscn")
	,load("res://Maze/12.tscn")
	,load("res://Maze/13.tscn")
	,load("res://Maze/14.tscn")
	,load("res://Maze/15.tscn")
]


var tile_size = 64  # tile size (in pixels)
var width = 20  # width of map (in tiles)
var height = 12  # height of map (in tiles)

func _ready():
	randomize()
	tile_size = Vector2(tile_size,tile_size)
	make_maze()
	
func check_neighbors(cell, unvisited):
	# returns an array of cell's unvisited neighbors
	var list = []
	for n in cell_walls.keys():
		if cell + n in unvisited:
			list.append(cell + n)
	return list
	
func make_maze():
	var unvisited = []  # array of unvisited tiles
	var stack = []
	# fill the map with solid tiles
	for x in range(width):
		map.append([])
		map[x].resize(height)
		for y in range(height):
			unvisited.append(Vector2(x, y))
			map[x][y] = N|E|S|W
	var current = Vector2(0, 0)
	unvisited.erase(current)
	while unvisited:
		var neighbors = check_neighbors(current, unvisited)
		if neighbors.size() > 0:
			var next = neighbors[randi() % neighbors.size()]
			stack.append(current)
			var dir = next - current
			var current_walls = map[current.x][current.y] - cell_walls[dir]
			var next_walls = map[next.x][next.y] - cell_walls[-dir]
			map[current.x][current.y] = current_walls
			map[next.x][next.y] = next_walls
			current = next
			unvisited.erase(current)
		elif stack:
			current = stack.pop_back()
	for x in range(width):
		for y in range(height):
			var t = tiles[map[x][y]].instance()
			t.position = Vector2(x,y)*tile_size
			t.name = "Tile_" + str(x) + "_" + str(y)
			add_child(t)
```

As described in the KidsCanCode tutorial and in the demonstration video, this script uses a Recursive Backtracker algorithm to produce a fully-connected randomized maze. Run the res://Maze/Maze.tscn scene and see the resulting maze. The contents of Maze.gd would be a great snippet to add to your gists.

Quit Godot. In GitHub desktop, add a summary message, commit your changes and push them back to GitHub. If you return to and refresh your GitHub repository page, you should now see your updated files with the time when they were changed.

Now edit the README.md file. When you have finished editing, commit your changes, and then turn in the URL of the main repository page (https://github.com/[username]/Exercise-05a-Procedural-Generation) on Canvas.

The final state of the file should be as follows (replacing the "Created by" information with your name):
```
# Exercise-05a-Procedural-Generation
Exercise for MSCH-C220, 22 March 2021

An implementation of a procedurally-generated 2D maze.

## Implementation
Built using Godot 3.2.3

The tile sprites were downloaded from [Kenney.nl](https://kenney.nl/assets/road-textures).

## References
This project is an adaptation of the excellent tutorial from [KidsCanCode](https://kidscancode.org/blog/2018/08/godot3_procgen1/)

## Future Development
None

## Created by 
Jason Francis
```
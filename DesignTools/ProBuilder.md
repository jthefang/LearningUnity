- Probuilder is a 3D modeling tool built into Unity
  - Install with Window > Package Manager
- Following this [tutorial](https://www.youtube.com/watch?v=PUSOg5YEflM&ab_channel=Brackeys)

## Table of Contents

- [Table of Contents](#table-of-contents)
- [Intro to UI](#intro-to-ui)
- [Shape generator](#shape-generator)
- [Polygon shape tool](#polygon-shape-tool)
- [Scale a face](#scale-a-face)
- [Extrude](#extrude)
- [Create extra geometry easily via edge loops](#create-extra-geometry-easily-via-edge-loops)
  - [To Create walls](#to-create-walls)
- [You can move edges](#you-can-move-edges)
- [Face tools](#face-tools)
- [Collapse vertices](#collapse-vertices)
- [Object tools](#object-tools)
  - [Mirroring objects](#mirroring-objects)
  - [Merging objects](#merging-objects)
- [Material editor](#material-editor)
  - [Setting default texture for ProBuilder](#setting-default-texture-for-probuilder)
- [UV Editor](#uv-editor)

## Intro to UI

- Tools > ProBuilder > ProBuilder Window
- Right click on toolbar to switch between Icon mode and Text mode
- Top bar = selection modes
  - Object selection
  - Vertex selection (allows you to move vertices of objects)
  - Edge selection
  - Face selection
- Tools are color coded
  - red = face tools
  - blue = selection tools
    - can grow and shrink selection area
  - green = object tools
    - mirror objects
      - Alt + click allows you to change the setting of this tool (notice it has a cog next to the tool indicating this ability)
    - export model 
      - bring it into a 3D modeling software to add texture, etc.
  - orange = editor tools
    - shape generator, polygon tool
- Select vertices and edges and move them to snap to the grid
- Delete faces by selecting them > Backspace

## Shape generator

- Let's you easily create common 3D shapes
  - cubes, cones, pyramids, doors, pipes, arches, stairs
- Alt click this tool for more options on how to modify the shape

## Polygon shape tool

- Click to drop points to draw out polygon
  - Use mouse to control height of extrusion
  - Creates an object
- Objects created with ProBuilder comes with colliders!

## Scale a face

- Face selection tool > Select a face
- R for the scaling tool > Click and drag the handle to scale the face
  - Shift -> Scale will create a new geometry/surface on the face you selected and scale that

## Extrude

- W for Move tool 
- Select face and Shift + move it along an axis to extrude it out

## Create extra geometry easily via edge loops

- Select an edge -> Edge loop tool will create an edge loop perpendicular to this edge
- You can move the edge loop to define extrusions (walls, platforms), holes (doors), etc.
- Double click and edge to select the whole loop

### To Create walls

- Select faces > Move > Shift Click to extrude

## You can move edges

- Double click an edge to select all edges it is continuous with

## Face tools

- Select a face
- Any red colored tool icon in the toolbar can affect faces
  - e.g. Beveling tool
  - Subdivide faces
  - Flip the face normal (make it upside down)
  - Detach face from object

## Collapse vertices

- Select 2 vertices that share an edge > collapse tool
- You can get really creative to create the geometry you want

## Object tools

### Mirroring objects

- Object select tool
- Make sure you're in Pivot (not center mode). The mirroring is done around the pivot

### Merging objects

- Select multiple object > merge tool
- This will change the pivot => use Freeze Transform tool to reset the pivot to 0, 0, 0
- Colliders might disappear => need to add a Mesh Collider to the merged object

## Material editor

- Add color and materials to the model
- Drag in materials you want to be able to apply to models
- Right click Project > Create Material
  - Shader > ProBuilder > Standard Vertex Color
  - Click Base Map to choose the texture
    - Unhide textures > Use GridBox Default texture for prototyping (comes with ProBuilder)
      - texture has gridlines
  - Change the tint
- Select object or face in ProBuilder 
  - => Use Alt + # shortcut in Material Editor to apply material to it

### Setting default texture for ProBuilder

- Edit > Preferences > ProBuilder
  - Mesh Settings > Drag in material to the Material field

## UV Editor

- You can change the UV mapping of each surface
- Can render the UVs to paint textures on them

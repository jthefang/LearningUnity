- Probuilder is a 3D modeling tool built into Unity
  - Install with Window > Package Manager
- Following this [tutorial](https://www.youtube.com/watch?v=PUSOg5YEflM&ab_channel=Brackeys)

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

## You can move edges

- Double click an edge to select all edges it is continuous with

## Face tools

- Select a face
- Any red colored tool icon in the toolbar can affect faces
  - e.g. Beveling tool
  - Subdivide faces
  - Flip the face normal (make it upside down)
  - Detach face from object

## Material editor

- Add color and materials to the model
- Drag in materials you want to be able to apply to models
- Right click Project > Create Material
  - Shader > ProBuilder > Standard Vertex Color
  - Choose Texture
  - Change the tint
- Select object or face in ProBuilder 
  - => Use Alt + # shortcut in Material Editor to apply material to it

## UV Editor

- You can change the UV mapping of each surface
- Can render the UVs to paint textures on them

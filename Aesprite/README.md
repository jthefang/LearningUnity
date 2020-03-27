Based off this video (https://www.youtube.com/watch?v=Md6W79jtLJM)

- Pen tool `B`: basic drawing tool
    - Left click will place down foreground color
    - Right click will place down background color (= an erase as long as background color (bottom right) is set to `Mask`)
    - can change `size` and `shape` of brush in top left
    - `Pixel-perfect` can help you draw 
    - Turn on a `symmetry option` (top bar OR View > Symmetry options) to draw symmetrical w.r.t. some image axis (vertical or horizontally)
    - `Ink` option in top bar
        - `Alpha compositing` lets you choose the opacity of the brush
- Line tool `L`
    - Holding `Shift` will snap the line to more perfect angles
    - Holding `Ctrl` will center the line on your initial starting location
- Curve line `Shift` + `L`
    - After placing your line, you'll have an option to curve the line twice
- Shape tool `U`
    - `U` = rectangle tool 
        - pressing `U` again toggles filled/outlined rectangle
    - `Shift` + `U` = ellipse tool
        - pressing again toggles filled/outlined ellipse
    - Holding `Shift` as you place the shape will make it be even along both axes (e.g. square and circle respectively)
    - Holding `Ctrl` centers shape on your initial click
- Contour `D`
    - fills in the polygon you drew after you release the click 
- Polygon tool `Shift` + `D`
    - mix of line and contour tool
- Eraser `E`
- Eyedropper `I`
    - Or hold `Alt` + click for quick use
- Paint bucket `G`
    - Top bar `Continous` will fill in all regions (of the layer) of a previous color with your paint bucket color
    - Similarly to fill in all regions of a certain color with your paint bucket color, select a part of the canvas => use paint bucket to only color regions inside that selection tool!
- Selection tool `M`, Magic wand `W`
    - Can drag to move selection around 
    - Draw tool will only draw within the selection area (<-- very powerful)
    - Turning `continuous` on with the magic wand allows you to select all areas of a certain color
        - Hold `Shift` to collect selections
    - Right click will deselect an area
    - `Ctrl` + `D` will deselect everything

# Layers
- `Tab` to show layers
    - layers are the rows
    - timeline for animation frames are the columns
- Double click a layer to get its properties
    - rename it 
    - mode: normal 
    - opacity of layer
- `Shift` + `N` to create new layer
- Right click to delete or duplicate layer
- Merge down = merging 2 layers together

# Colors
- Select color + `F4` to get the color palette (get hex and RGB values of that color)
- Top left bar above colors:
    - Down arrow = sort palette (e.g. by luminance, brightness)
- Create gradient between colors:
    - Left color palette > Drag end || to add colors
    - Modify last color to be the end color of the gradient
    - Modify first color to be the start color of the gradient
    - Drag select colors from first to last => Down arrow > Gradient to generate gradient
- 3rd button = preset color palettes
    - really cool color sets!
- Options
    - save and import palettes
    - create palette from current sprite

# Canvas
    - To pan around
        - middle mouse click + drag
        - hold `Space` + left click + drag 
    - Hold `Space`
        - top bar: `100%`, `Center (canvas)`, `Fit (canvas to) screen`
    - Zoom with mouse wheel

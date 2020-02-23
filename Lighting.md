
# Add 2D Lighting
- [https://www.youtube.com/watch?v=nkgGyO9VG54](https://www.youtube.com/watch?v=nkgGyO9VG54)
- Unity 2019.2 or later
- Window --> Package manager --> Install Lightweight RP (render pipeline)
- Project Settings --> Graphics --> see if you have an asset for Render Pipeline Asset
	- if not: Project --> Create --> Rendering --> Universal Render Pipeline --> Pipeline Asset
	- name this `LWRP Asset` then drag this asset into the asset slot in the Project Settings
- For 2D lights:
	- Click the LWRP asset in the Project view --> Inspector --> General --> Renderer List
	- Project --> Create --> Universal Render Pipeline --> 2D Renderer --> drag this to the data slot for the LWRP asset under its Default Renderer Type field in the Inspector
        - name this `2D Renderer Data`
	- Change all sprites to use Sprite Renderer material: Sprite-Lit-Default
		- do this in one shot by Edit --> Render Pipeline --> Lightweight Render Pipeline --> 2D Renderer --> Upgrade Scene (or Project) to 2D Renderer
- Directory setup: 
    - `Assets/Lighting`
        - `2D Renderer`
        - `LWRP Asset`
        - `LWRP Asset_Renderer`
- Hierarchy -> Light -> 2D -> Pick light type
	- Target Sorting Layers - set to "All" to light up all layers with this light
	- Intensity - how bright the light is
	- Color - the color of the light
	- Fall off - how drastically light falls off over distance (create cool glow effect)
	- Inner/Outer radius - center and radius of light
- **NOTE** After adding lights, your game will be dark unless you add a global light that targets `All` sorting layers
- Point light - add light that emits from a point
- Global light - lights up all elements in the scene uniformly
- Sprite light - add a sprite that will determine shape of light
- Free form light - create a shape that will be light
- Script
```c#
using UnityEngine.Experimental.Rendering.LWRP;
...
	torch = transform.Find("PointLight").gameObject.GetComponent<Light2D>();
	torch.intensity = lightIntensity;
	torch.pointLightOuterRadius = torchRadius;
	torch.color = Color.white;
```
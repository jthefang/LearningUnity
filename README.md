# Table of Contents
- [Unity UI](#unity-ui)
- [Assets](#assets)
	- [Where to find resources](#resources)
- [Materials](#materials)
- [GameObjects](#gameobjects)
	- [Prefabs](#prefab)
	- [Camera object](#camera-object)
	- [Components](#adding-components)
		- [Physics](#physics)
			- [RigidBody](#rigidbody)
	- [Labels](#labels)
	- [GameManager](#gamemanager)
- [Hierarchy](#hierarchy)
- [Sprites](#sprites)
	- [Sprite Layers](#sprite-layers)
- [Game Physics](#game-physics)
- [Scripts](#scripts)
	- [Collision detection](#collision-detection)
	- [Random rotations](#random-rotations)
	- [Custom player control](#custom-player-control)
	- [Top down shooter](#top-down-shooter)
- [Input Manager](#input-manager)
- [Audio](#audio)
	- [Random pitch](#random-pitch)
- [UI](#ui)
	- [Buttons](#buttons)
	- [Hooking it up to a script](#hooking-it-up-to-a-script)
	- [Add animations to buttons](#add-animations-to-buttons)
	- [Creating a sliding menu](#creating-a-sliding-menu)
# Unity UI
- Hand tool - `Q`
	- allows you to pan around the scene
	- `Right click + WASD` to move around in the scene
		- + `Q, E` to go up and down
		- + `Shift` to move faster
- Gizmo = 3d geometry or texture giving info about the GameObject (e.g. wind direction)
	- e.g. coordinate system in the top right
		- click on either axis to switch the scene to look along that axis
		- clicking the middle changes between perspective and isometric/orthographic views
	- `x` = red, `y` = green, `z` = blue 
- Translate tool - `W` 
	- click + drag plane to move object along a plane
- Rotate tool - `E`
	- free rotation - click on object
- Scale tool - `R`
	- scale along 1 axis or proportionally along all 3 (click center of the object and drag)
- Rect tool - `T`
	- for 2D objects
	- resize, rescale, rotate 2D assets + reset pivot point
- Transform tool - `Y`
	- combines translate, rotate and scale tools into 1 
- Center vs Pivot
	- if you select 2 GameObjects, this changes the pivot to be the center between the 2 or completely inside one of the objects
- Global mode vs Local mode
	- toggles coordinate system of the selected GameObject to be that of the global game or of the local object itself
- Click on GameObject --> Hover over scene --> `F` to focus on that object
- 2 - toggles 2D mode in the scene view
- Game play buttons: play, pause, step
	- step through frame by frame
	- pause to make changes
	- NOTE: any changes made during play mode disappear once you exit! This is just to allow you to experiment!
- Window menu to add views to your Layout
	- you can save your custom Layout in the Layout dropdown
- GameManager = normal C# script but is "special" in the sense that it's the brains of the game
- `Alt + Click` expand triangle in file browser - expands entire hierarchy for that item
# Assets
- FBX file = 3D model + animations + textures
	- could be represented as normal jpg --> Fix Now in Unity to add the normal map (?)
- Organization
	- Animations
	- Models - your GameObject models (`.fbx`)
		- the actual GameObject models you want to click and drag into the scene/hierarchy 
		- to view the model without the textures, i.e. the wireframes: Scene view --> Shaded dropdown select --> select wireframe
	- Materials
	- Prefabs
	- Presets
		- In Inspector --> Component --> Presets (2nd top right icon)
		- you can save / load presets (configuration) for a component
	- Scenes
		- where you should store the scenes you create in the hierarchy (click and drag from hierarchy to store scene in Project --> Scenes folder) 
	- Scripts
	- Sounds
	- Textures (`.psd`)
- assets typically packaged into a **unity package**
	- Assets --> Import package --> Custom package --> select the `.unity` file to import the assets package into unity
## Resources
- Music
	- https://www.bensound.com/
	- http://dig.ccmixter.org/
	- https://buffer.com/library/background-music-video
- Sound FX
	- https://www.zapsplat.com/
	- https://freesound.org/
- Colors
	- http://streetcrossergame.com/
- Art and sound, misc resources
	- https://www.kenney.nl/
- Game Dev Sites
	- [Gamasutra](https://www.gamasutra.com/) for blogs and articles
	- [Pixel Propspector](https://pixelprospector.com/)
	- [TigSource](https://forums.tigsource.com/index.php)
- Make audio seamlessly loop in Audacity:	
	- [Video](https://www.youtube.com/watch?v=hiwC05zMaFw)
- Pixel sprites:
	- https://www.spriters-resource.com/
# Materials
- Determine the look and feel of an object
	- uses a **shader** (simple program written in C-like language for GPU rendering) to give texture
- Project --> Create --> Material
	- preview the material by pulling up the display view at the bottom of the inspector
	- Standard shader gives plenty of flexibility
	- To have an object fade out set **rendering mode: Fade**
	- albedo is the actual texture (drag a texture file to this field)
- Apply the material to a GameObject by dragging it onto the object in the scene
	- This adds a material component to the GameObject
# GameObjects 
- everything in the hierarchy view is a GameObject
- Transform component - controls position, rotation and scale
	- hover over the axis and drag left and right to increment/decrement by small amounts
- `Cmd + D` to duplicate GameObjects in the Hierarchy view
	- multiple GameObjects can share the same name (usually good if they all behave as one entity = a set of columns => also group them under the same parent and name as well)
- Check/uncheck the box next to the GO's name in the inspector to toggle the GO (display/not display)
- Add other components to control behavior and appearance of the GameObject
	- e.g. lighting to illuminate the GameObject
	- e.g. Audio source to accompany the GameObject
- Blue = instance of a model (container of GameObjects) / connected to other objects with a Prefab
	- black = standard GameObject
	- brown = lost prefab connection
## Prefab
- abstraction for a GameObject = underlying "class" that you can use as blueprint for a bunch of GameObject instances
- => changing the Prefab changes all GameObject instances connected to it
	- or if you change a GameObject instance: Inspector --> Overrides --> Apply to all (this will apply the changes you made to the GO to the Prefab => all GO instances)
- always modify a Prefab instead of the GameObject directly (unless you're experimenting)
- create a prefab by dragging GameObject from hierarchy view to Project folder
- Open prefab (Inspector) to edit --> back button in Hierarchy to return to Scene view
## Camera object
- Projection: **perspective (for 3d games)** = depth, **orthographic (2d)** = no depth perspective
- Clear Flags --> solid color, Background --> black color
	- to get a solid black background
- Camera component
	- field of view = zoom of the camera
- Have camera follow player by:
	- CameraMount GameObject => Add Camera as a child to this object
	- Add a `CameraMovement.cs` script to CameraMount 
		```c
		public  class CameraMovement : MonoBehaviour {
			public  GameObject followTarget;
			public  float moveSpeed;
			// FixedUpdate is called at fixed rate independent of frame rate
			void FixedUpdate()
			{
				if (followTarget != null) {
					transform.position = Vector3.Lerp(transform.position, followTarget.transform.position, Time.deltaTime * moveSpeed); //eases camera to follow target with a certain speed
				}
			}
		}
	```
- set Follow Target = player
- Align scene view with camera view by clicking on Scene view → Select Main Camera in Hierarchy → GameObject -> Align View to Selected
## Adding components
- Copy/paste entire component values by clicking the Gear icon (top right)
- Scripts
	- select GameObject in Hierarchy --> Component --> Scripts --> select the script component you want to add (e.g. Ship Controller)
	- Misc:
		- you can define (public) fields in your script that can be accessed in the Inspector Editor 
		- scripts can require other components to be included along with it (e.g. audio source component with an Ion Cannon component to the spaceship GameObject)
	- e.g. Screen Wrap:
		- add the body mesh element to renderers...this allows the game object to wrap around/Pacman teleport when it goes off screen
### Physics
- By default GameObjects are treated as **static colliders** (Unity optimizes it for collision detection, but if you move the object around in the game => engine has to reoptimize) => don't use this option for moving objects
- e.g. select GameObject --> Inspector --> Add component --> Physics --> RigidBody
#### RigidBody 
- if you want the GameObject to be able to have collisions with other objects
- uncheck gravity if you don't want to apply gravity
	- OR set `Gravity Scale` to 0
- Constraints --> `Freeze Rotation Z` for 2D games
- check **is kinematic** if the **object will be controlled manually** (i.e. you're updating the object's `transform` directly, rather than just letting the physics engine move it via forces, velocity and what-not) and you want to **register collisions** (be notified)
	- uncheck kinematic if the object will be given velocity and not controlled (e.g. a bullet)
	- **Interpolate** to have the object move smoothly
- **Drag** - friction
- **Angular drag** - damping/friction when rotating
- **Freeze position** - disallows GameObject movement along the checked axes, e.g. checking Y means the GameObject can't move up/down
#### SphereCollider 
- the GameObject's "boundaries"
	- green outline = collision boundary
	- set center to change position of the collider (hitbox for registering a collision)
	- check is trigger to listen for trigger events from other kinematic objects (e.g. a collision)
		- a kinematic GameObject can't register normal collisions
- MeshCollider => check convex
	- To create a collider on a 3D object 
## Labels 
- GameObjects visible in the Scene view, but invisible during gameplay
- Hierarchy → Right click --> Create --> Create empty
	- create label by clicking on the cube in Inspector of the new GameObject --> click the blue capsule
	- this will annotate the GameObject in the scene view (i.e. create a label)
	- change size of this label by clicking Scene View --> Gizmos --> 3D Icons (drag to right to increase size)
- To create spawn point
	- To position it exactly on top (or in same plane) as another object (e.g. for collisions to occur), drag object to be a child of the specific GameObject (not container) you want it to be aligned with => set position to 0, 0, 0 => you can readjust the hierarchy to have the new object be a sibling again instead of a descendant
- Group GameObjects (e.g. marine head and body by **parenting them under an empty GameObject**)
	- **a GameObjects Transform is always relative to its parent**
## GameManager
- This is itself a GameObject 
	- create empty in Hierarchy --> add component --> scripts --> GameManage.c#
- It can have some configurable fields
	- add prefabs, GameObjects (e.g. to let it know what [prefab] to spawn where [GameObj loc])
- Can require an Audio Source component to have game music (check the loop option for this) 
# Hierarchy
- `Cmd + Delete` to delete a GameObject
# Sprites
- Sprite modes
	- Single - single image sprite
	- Multiple - sprite with multiple elements (e.g. animations, spritesheets, etc)
		- spritesheets - draw multiple sprites together in one image 
		- every image used in your game = one draw call => could be a potential issue if you have thousands of sprites
	- Polygon - custom polygon shaped sprite (e.g. Triangle, Square, Pentagon, Hexagon, etc)
- Filter mode --> Point (no filter) to remove ugly blur
- **Sprite Editor** (click sprite --> sprite editor) lets you slice and edit sprites
	- you can slice automatically or manually via **Grid By Cell Size** => give width and height of slices (X, Y respectively), slice and apply
- Sprite will be invisible unless you render it: GameObject --> Add component --> Sprite Renderer --> Drag image to Sprite field
- **Pixels per unit** field of sprite
	- [width of sprite image / pixels per unit] * [x scale] = [width of sprite in units on screen]
- Sprite image quality and compression
	- click on sprite --> Inspector --> max size, compression, crunch compression
	- play with what gives the best quality for performance
- Add animation --> Create clip --> Add property --> Sprite Renderer --> Sprite 
## Sprite Layers
- Sprite renderer --> **order in layer** property (higher = more in front)
	- this is the order the Unity engine will render the sprites
	- default is 0 => for overlapping sprites give them an ordering so that the unity engine consistently renders them in the right layer ordering (else they'll flicker overlap each other)
- Layers dropdown (top right) --> Edit Layers --> Sorting layers --> click to add layers (top layers are rendered first/below the subsequent layers)
	- GO --> Inspector --> Sprite Renderer --> Sorting Layer dropdown, select which layer this object should belong to
# Game Physics
- Edit --> Project Settings --> Project 2D --> gravity Y (set negative value for real gravity)
- Every object that should interact with gravity and other physics objects should have a **Collider 2D** (for collision detection) and **Rigidbody 2D** (uses unity's physics engine) component
	- Rigidbody 2D - gravity will affect sprite, can control sprite with scripts using forces
		- change to **Kinematic** if you want to move bodies via a transform component (not just gravity alone), **Dynamic** (leaves under control of Unity's gravity), **Static** if they don't move at all
		- **mass, linear drag, angular drag** properties
	- Collider 2D - sprite will interact with other objects
		- Polygon 2D colliders - more performance heavy than Box/Circle Collider 2D components, but more precise physical interaction
	- click GO --> Inspector --> Edit Collider to create/delete points and mold the Polygon Collider to better fit the shape of your sprite
	- uses `OnCollisionEnter2D` to handle collisions with other objects 
- Colliders can be used in **Trigger** mode - won't physically collide, but instead they'll trigger `OnTriggerEnter2D()` on the attached scripts
- Can assign **Physics2D materials** to colliders to control properties like bounciness and friction
# Scripts
- script derives from class called `MonoBehaviour`
- Public fields in scripts will be editable in the Unity Inspector
	- You can make private fields **serializable** to have them also editable in the Inspector
	```c
	[SerializeField]
	private int maxHeight = 3;
	```
- **Tag** a game object to reference it in your script, e.g. `player = GameObject.FindGameObjectWithTag("Player");`
	- select GO --> Inspector --> Tag dropdown --> select tag (e.g. `Player`)
- `Update()`
	- occurs at every single frame (=> no heavy processing)
- `OnEnable()`
	- when GameObject is enabled OR when inactive GameObject reactivates
		- deactivate GameObjects when you don't need it for a while but will need it later
- `Start()`
	- called once in script's lifetime (before Update is called) => do setup/initialization
- `Destroy()`
	- called right before GameObject is destroyed => clean up (e.g. shut down network connections)
## Collision detection
- When another collider touches this GameObject's collider => it triggers `OnCollisionEnter`
- Add a script to the GameObject and modify `OnCollisionEnter` to control collision behavior
```c#
   void OnCollisionEnter(Collision collision) {
       if (!collided) {
           collided = true;
           GameObject clown = gameObject.transform.parent.gameObject; //current GO's parent GO
           Animator clownAnimator = clown.GetComponent<Animator>();
           clownAnimator.SetTrigger("hit");
       }
   }
```
- The `collision` object contains info about contact points and impact velocities
## Random rotations
```c#
       float randomX = UnityEngine.Random.Range(10f, 100f);
       float randomY = UnityEngine.Random.Range(10f, 100f);
       float randomZ = UnityEngine.Random.Range(10f, 100f);
 
       Rigidbody bomb = GetComponent<Rigidbody>();
       bomb.AddTorque(randomX, randomY, randomZ);
```
## Layer masks and coroutines
- selectively filters out certain layers
	- commonly used with raycasts (i.e. have bomb explosion stop at the wall blocks)
- Unity Editor --> Layer dropdown (top right) --> Add user layer by naming it something
	- add game object --> Inspector --> layer to the custom layer
- Coroutines are kind of like threads (?) 
```c#
public  GameObject explosionPrefab;
public  LayerMask levelMask;

// Start is called before the first frame update
void Start() {
	Invoke("Explode", 3f);
}

void Explode() {
	//spawn explosion at bomb's position
	Instantiate(explosionPrefab, transform.position, Quaternion.identity);
	
	StartCoroutine(CreateExplosions(Vector3.forward));
	StartCoroutine(CreateExplosions(Vector3.right));
	StartCoroutine(CreateExplosions(Vector3.back));
	StartCoroutine(CreateExplosions(Vector3.left));

	//disable bomb's renderer, making it invisible
	GetComponent<MeshRenderer>().enabled = false;
	//disable collider, allowing players to move through/walk into explosion
	transform.Find("Collider").gameObject.SetActive(false);
	//destroys bomb after 0.3s => all explosions will spawn before bomb is destroyed
	Destroy(gameObject, .3f);
}

private  IEnumerator CreateExplosions(Vector3 direction) {
	for (int i = 1; i < 3; i++) { //explosion will reach 2 units (meters)
		RaycastHit hit; //info about what and at which position Raycast hits
		Physics.Raycast(transform.position + new  Vector3(0, 0.5f, 0), direction, out hit, i, levelMask); //sends raycast from center of bomb towards direction, outputs result to hit, i = distance ray should travel, use layer mask to make sure ray only checks for blocks in the level and ignores player and other colliders

		if (!hit.collider) { //if no hit => its a free tile => spawn an explosion there
			Instantiate(explosionPrefab, transform.position + (i * direction), explosionPrefab.transform.rotation);
		} else { //hit a block/wall in the layer mask
			break; //break => explosion doesn't jump over walls
		}
		yield  return  new  WaitForSeconds(.05f); //wait 0.05s before next iteration => explosion looks like its expanding outwards
	}
}
```
## Custom player control
```c#
public class PlayerMovement : MonoBehaviour
{
    public float acceleration;
    public float maxSpeed;
    private Rigidbody rigidBody;
    private KeyCode[] inputKeys; //array of keycodes used to detect input
    private Vector3[] directionsForKeys; //array of Vector3 variables for directional data

    // Start is called before the first frame update
    void Start()
    {
        inputKeys = new KeyCode[] {KeyCode.W, KeyCode.A, KeyCode.S, KeyCode.D};
        directionsForKeys = new Vector3[] {Vector3.forward, Vector3.left, Vector3.back, Vector3.right};
        rigidBody = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void FixedUpdate() //framerate independent Update (used when working with rigidbodies), fired at constant interval
    {
        for (int i = 0; i < inputKeys.Length; i++) {
            var key =  inputKeys[i];
            if (Input.GetKey(key)) { //if this key was fired
                Vector3 movement = directionsForKeys[i] * acceleration * Time.deltaTime; // time it took to complete the last frame
                movePlayer(movement);
            }
        }
    }

    void movePlayer(Vector3 movement) {
        if (rigidBody.velocity.magnitude * acceleration > maxSpeed) {
            //if current speed exceeds maxSpeed => force slows player down (enforcing speed limit)
            rigidBody.AddForce(movement * -1);
        } else { //apply force to rigid body causing it to move
            rigidBody.AddForce(movement); 
        }
    }
}
```
## Top down shooter
- GameObjects:
	- CameraRig --> Main Camera (child)
	- Player (3D sphere) - this is a prefab (for GameController respawn player)
		- RigidBody for collisions
		- Player.cs, PlayerMovement.cs (see above), PlayerShooting.cs
	- Enemy (3D cube) - this is a prefab (for spawning)
		- RigidBody for collisions
		- Enemy.cs
	- Projectile (3D capsule) - this is a prefab (for shooting)
		- RigidBody for collisions
		- Projectile.cs
	- Enemy Producer
		- EnemyProducer.cs
	- GameController
		- GameController.cs
```c#
using System;
public  class Player : MonoBehaviour { // on player
	public  int health = 3;
	public  event  Action<Player> onPlayerDeath; //C# event lets you broadcast changes in objects to any listeners (i.e. the GameController script)
	void collidedWithEnemy(Enemy enemy) {
		enemy.Attack(this);
		if (health <= 0) {
			if (onPlayerDeath != null) {
				onPlayerDeath(this);
			}
		}
	}
	void OnCollisionEnter(Collision col) {
		Enemy enemy = col.collider.gameObject.GetComponent<Enemy>(); //enemy is a custom script attached to an enemy GO
		if (enemy) { //player can collide with other objects that aren't Enemy's, make sure this isn't what we detected collision with
			collidedWithEnemy(enemy);
		}
	}
}
```
```c#
public class Enemy : MonoBehaviour { // on enemy GO
	public  float moveSpeed;
	public  int health;
	public  int damage;
	public  Transform targetTransform; //this will be the player's transform
	// Update is called once per frame
	void FixedUpdate() {
		if (targetTransform != null) {
			this.transform.position = Vector3.MoveTowards(this.transform.position, 				targetTransform.transform.position, Time.deltaTime * moveSpeed); //follow the player
		}
	}

	public  void TakeDamage(int damage) {
		health -= damage;
		if (health <= 0) {
			Destroy(this.gameObject); //destroy itself
		}
	}

	public  void Attack(Player player) { //custom script attached to Player GO
		player.health -= this.damage;
		Destroy(this.gameObject);
	}
	
	public  void Initialize(Transform target, float moveSpeed, int health) {
		this.targetTransform = target;
		this.moveSpeed = moveSpeed;
		this.health = health;
	}
}
```
```c#
public class Projectile : MonoBehaviour {
	public  float speed;
	public  int damage;

	Vector3 shootDirection; //determines where projectile will go

	void FixedUpdate() {
		this.transform.Translate(shootDirection * speed, Space.World);
	}

	public  void FireProjectile(Ray shootRay) {
		this.shootDirection = shootRay.direction;
		this.transform.position = shootRay.origin;
		rotateInShootDirection();
	}

	void OnCollisionEnter(Collision col) {
		Enemy enemy = col.collider.gameObject.GetComponent<Enemy>();
		if (enemy) {
			enemy.TakeDamage(damage);
		}
		Destroy(this.gameObject);
	}
	
	void rotateInShootDirection() { //rotates this Projectile object towards its shoot direction
		Vector3 newRotation = Vector3.RotateTowards(transform.forward, shootDirection, 0.01f, 0.0f);
		transform.rotation = Quaternion.LookRotation(newRotation);
	}
} 

public  class PlayerShooting : MonoBehaviour { //on player GO
	public  Projectile projectilePrefab; //reference to projectile prefab => create new instance on each player firing
	public  LayerMask mask; //used to filter GameObjects

	void shoot(RaycastHit hit) {
		var projectile = Instantiate(projectilePrefab).GetComponent<Projectile>(); //instantiates a projectile prefab and gets its Projectile script component so it can be initialized
		var pointAboveFloor = hit.point + new  Vector3(0, this.transform.position.y, 0); //x, z coordinates = where ray cast from mouse click position hits (ensures projectile is parallel to ground)
		var direction = pointAboveFloor - transform.position; //position from Player --> ptAboveFloor
		var shootRay = new  Ray(this.transform.position, direction);
		//Debug.DrawRay(shootRay.origin, shootRay.direction * 100.1f, Color.green, 2); //for debugging purposes (you can see this in the scene view)

		Physics.IgnoreCollision(GetComponent<Collider>(), projectile.GetComponent<Collider>()); //ignore collisions between Player collider and Projectile collider (o/w triggered before the projectile even flies off)
		projectile.FireProjectile(shootRay); //set trajectory for projectile (see Projectile.cs)
	}
	
	// Casts a ray from camera to pt where mouse clicked, checks if ray intersects a GO with the given LayerMask
	void raycastOnMouseClick() {
		RaycastHit hit;
		Ray rayToFloor = Camera.main.ScreenPointToRay(Input.mousePosition);

		//Debug.DrawRay(rayToFloor.origin, rayToFloor.direction * 100.1f, Color.red, 2);
		if (Physics.Raycast(rayToFloor, out hit, 100.0f, mask, QueryTriggerInteraction.Collide)) {
			shoot(hit);
		}
	}
	
	// Update is called once per frame
	void Update()
	{
		bool mouseButtonDown = Input.GetMouseButtonDown(0); //left click
		if (mouseButtonDown) {
			raycastOnMouseClick();
		}
	}
}
```
- Raycasts point in a direction (invisible rays), any GameObjects that it intersects with will be registered by Unity
	- use a mask to filter out any unwanted objects in that ray path
```c#
public  class EnemyProducer : MonoBehaviour {	
	public  bool shouldSpawn; //toggles spawning
	public  Enemy[] enemyPrefabs; //pick random enemy prefab from this and instantiate it (e.g. set size = 1, element 0 = Enemy prefab)
	public  float[] moveSpeedRange; //enemy speed range (e.g. size = 2, element0 = 3, element1 = 8)
	public  int[] healthRange; //enemy health range (e.g. size = 2, element0 = 2, element1 = 6)

	private  Bounds spawnArea; //the BoxCollider size we put on this GameObject
	private  GameObject player; //reference to player that you'll pass to the enemy objects

	public  void SpawnEnemies(bool shouldSpawn) {
		if (shouldSpawn) {
			player = GameObject.FindGameObjectWithTag("Player");
		}
		this.shouldSpawn = shouldSpawn;
	}

	// Start is called before the first frame update
	void Start() {
		spawnArea = this.GetComponent<BoxCollider>().bounds;
		SpawnEnemies(shouldSpawn);
		InvokeRepeating("spawnEnemy", 0.5f, 1.0f); //call after 0.5 secs, then every 1s
	}

	Vector3 randomSpawnPosition() {
		float x = Random.Range(spawnArea.min.x, spawnArea.max.x);
		float z = Random.Range(spawnArea.min.z, spawnArea.max.z);
		float y = 0.5f; //just the height of our stage (game specific)

		return  new  Vector3(x, y, z);
	}

	void spawnEnemy() {
		if (shouldSpawn == false || player == null) {
			return;
		}

		int index = Random.Range(0, enemyPrefabs.Length);
		var newEnemy = Instantiate(enemyPrefabs[index], randomSpawnPosition(), Quaternion.identity) as  Enemy;
		var randomMoveSpeed = Random.Range(moveSpeedRange[0], moveSpeedRange[1]);
		var randomHeath = Random.Range(healthRange[0], healthRange[1]);
		newEnemy.Initialize(player.transform, randomMoveSpeed, randomHeath); //instantiate the enemy (custom Initialize function, see enemy.cs)
	}
}
```
- This is the game object that spawns the Enemies
	- the public fields are set in the Inspector
	- the `BoxCollider` attached to the GameObject gives the bounds within which the enemies will spawn
```c#
public  class GameController : MonoBehaviour {
	public  EnemyProducer enemyProducer;
	public  GameObject playerPrefab;

	// Start is called before the first frame update
	void Start() {
		var player = GameObject.FindGameObjectWithTag("Player").GetComponent<Player>();
		player.onPlayerDeath += onPlayerDeath; //subscribes for the onPlayerDeath event
	}

	void onPlayerDeath(Player player) {
		enemyProducer.SpawnEnemies(false); //stop enemy production
		Destroy(player.gameObject);
		Invoke("restartGame", 3); //invoke after 3 seconds
	}
	
	void restartGame() {
		var enemies = GameObject.FindGameObjectsWithTag("Enemy");
		foreach (var enemy in enemies) {
			Destroy(enemy);
		}
		var playerObject = Instantiate(playerPrefab, new  Vector3(0, 0.5f, 0), Quaternion.identity) as  GameObject;
		var cameraRig = Camera.main.GetComponent<CameraRig>();
		cameraRig.target = playerObject;
		enemyProducer.SpawnEnemies(true);
		playerObject.GetComponent<Player>().onPlayerDeath += onPlayerDeath; //add event subscription
	}
}
```
# Input Manager
- Edit --> Project Settings --> Input --> Expand axes
- size = number of inputs to the game (default 18 is plenty)
- fields for an input
	- Name = how you'll reference the input in code
	- Descriptive/Negative name = how the input will be read to the user
	- Negative/Positive buttons = actual buttons/keys used for the input (e.g. +: right, -: left)
	- Alt Negative/Positive buttons = alternative buttons (e.g. a, d)
	- Gravity, Sensitivity determine how slippery the controls are
		- sensitivity = responsiveness on key down to get to full value (0 to +/-1), 
		- gravity = responsiveness on key up to return back to 0
	- other fields mostly for functionality of analog sticks

```c#
public class PlayerController : MonoBehaviour {
    public float moveSpeed = 50.0f; //public variables are editable in Inspector
    
    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
		//Time.deltaTime = amount of time since last Update call
        Vector3 pos = transform.position; //gets position of current GameObject
        pos.x += moveSpeed * Input.GetAxis("Horizontal") * Time.deltaTime; //Gets horizontal input, updates the position
        pos.z += moveSpeed * Input.GetAxis("Vertical") * Time.deltaTime;
        transform.position = pos;
    }
}
```
# Audio
- **AudioSource** component on a GameObject is what will actually play the sound (from an **AudioClip** file) in 2/3D space
	- In 3D space: volume will vary based on how far the AudioSource is from the object listening to it (the **AudioListener** component, which is on the camera/player usually)
		- AudioSource → 3D sound settings → Doppler (doppler effect) and Rolloff (how much volume quiets down the farther its source is from AudioListener, set min and max distance)
		- Closer objects should have more of a linear rolloff
	- In 2D space audio source will play at consistent volume regardless of distance to AudioListener
- Can set up **triggers**, i.e. conditions that cause AudioSources to play
- AudioSource properties:
	- Play on awake: play clip as soon as GO is created
	- Loop: make audio loop
- `.ogg` or `.wav` are lossless formats (preferred over .mp3)
- Scripting
```c#
public AudioClip deathSound; // Drag and drop audio file to this script component in Unity
		...
void OnCollisionEnter(Collision col)
   {
       if (col.gameObject.GetComponent<Animal>() && !col.gameObject.GetComponent<Animal>().isDead)
       {
           Destroy(col.gameObject);
           GetComponent<AudioSource>().PlayOneShot(deathSound);
       }
   }
```
- Add this death sound to the tractor that kills the animals in `OnCollisionEnter` => only have to add it in one place (instead of each individual animal) + animal can't play the sound because it's GameObject is destroyed
- `PlayOneShot` - ensures the sound plays to completion (and won't be cutoff even if `PlayOneShot` is called again)
## Random pitch
`audioSource.pitch = Random.Range(0.8f, 1.5f);`
- Gives more spontaneity to your sounds (e.g. individualistic sheep sounds)
# UI
- UI is added as a scene (put in Scenes folder)
	- nice to also have a UI folder in Assets with subfolders for Fonts, Menu (background images, buttons, icons and game art)
	- you want UI assets to have Inspector -> Texture Type = **Sprite (2D and UI)**
- To have the UI scale with screens size:
	- Canvas GO -> Canvas Scaler component (Inspector) -> **UI Scale Mode**: Scale with screen size 
	- [Link](https://answers.unity.com/questions/1040610/re-sizing-ui-text-font-relative-to-screen-size.html)
- Creating an element: GameObject -> UI -> Image
	- will create an Image GameObject as child of a Canvas object
	- **Canvas** - root object for all UI elements
	- **Image** - non-interactive control that displays a sprite
		- has source image field in Inspector --> Image (Script) where you can drag an image file
		- click **Set Native Size** in Inspector to set image to a better aspect ratio (e.g. 1136x640)
	- **EventSystem** - processes and routes input events to objects within a scene (e.g. manages raycasting)
- Fonts
	- drag font to Text GO --> Inspector --> Font field
- Making background fill up screen (no matter the resolution)
	- Canvas --> Canvas Scaler --> UI Scale Mode
		- change this to **Scale with Screen Size** and X: 1136, Y:640, Match = 1
		- this makes it so that the UI looks the same on any device
	- Image --> Add component --> Aspect Ratio Fitter --> Aspect Mode: set to **Envelope Parent** (now image fills the screen)
- **Anchors**
	- control the position and size of your UI elements relative to their parent object
	- position/transform of UI element = position of its **pivot** (blue circle) is relative to its **anchors** (white markers)
		- 0,0 is the top left corner of the Canvas
	- you can move your anchors around using the rect tool `T`
		- you can split them, which allows you to squash your UI elements in the horizontal or vertical directions
		- Quick select anchor position by clicking rectangle above `Anchors` in Inspector (or custom drag them after pressing `T`)
			- `Stretch` means the UI element will be fixed either vertically/horizontally and stretch horizontally/vertically 
	- can rotate your UI element around the pivot
		- hold `Alt` while scaling to scale around the pivot point
	- Scale (negative values = flips image) is not the same as size (width and height, can't be negative)
- To make floor of background not get cut off
	- Disable its Aspect Ratio Fitter
	- Set Pivot to X: 0.5, Y: 0
	- Re-enable Aspect Ratio Fitter
- GO -> UI -> **Panel**
	- to make a settings dialog panel
	- default is alpha of 100 (change color --> A level to 255 to remove this transparency)
- **Toggle**
	- basically a checkbox
	- contains a **background** (image that is always visible), **checkmark** (image that is only visible when toggle is active/ON), **label** (label displayed next to toggle)
	- script component --> Is On controls the default state of the toggle checkbox
	- to have the toggle affect something, add + On Value Changed event in the Inspector
		- e.g. to mute the game music, drag GO with the game music as an AudioSource component to this event element --> Function --> AudioSource --> mute (in dynamic bool section, which sets mute = toggle's is active status)
- **Slider**
	- for range of values
	- contains a **background** (image that shows bounds of slider and its inner area when not filled, i.e. handle all the way left), **handle** (image for handle), **fill** (image that stretches to show the value of the slider)
	- script component --> value 
		- determines the sliders default value
	- to have the slider affect something, + On Value Changed in Inspector
		- e.g. to change volume of game music, drag GO with game music as AudioSource to this event element --> Function --> AudioSource --> volume (in dynamic float section)
- Text
	- set horizontal overflow to overflow if you don't want the text to wrap
## Buttons
- GameObject -> UI -> Button
	- Has Text GO child and a Button (script) attached to it
- use **9-slice-scaling** so that one small image scales to fit all button sizes
	- click on the image file asset --> Inspector --> `Sprite editor` --> change border to L: 14, R:14, B: 16, T:16 (essentially divide the image into the 9 sections) --> click apply at top of the sprite editor
	- change button GO --> Inspector --> Image Type: sliced, Button Transition: sprite swap 
		- then you can select different images for the highlighted/pressed state of the button (make sure to apply the 9 slice scaling above to these images as well)
- NOTE: the Button's script --> Transition --> **Color Tint** property will default highlight the button when it's hovered over, and tint it when it's clicked
## Hooking it up to a script
- can't call static methods => create a UIManager GameObject with a `UIManager.cs` script
- `restartDialog.SetActive(false);` to have UI object `restartDialog` be hidden
- To reload the current scene
```c#
public  void RestartGame() {
	Application.LoadLevel(Application.loadedLevelName);
}
```
- to have the button start another scene:
	- Open File --> Build settings --> Drag the scenes you want to build into the dialog --> close it
	```c#
	using UnityEngine.SceneManagement; //allows you to load other scenes

	public  class UIManager : MonoBehaviour {
		public  void StartGame() {
			SceneManager.LoadScene("RocketMouse"); //a scene we added to the Build Settings
		}
	}
	```
	- Then on the start button --> Inspector --> On click list --> + add --> Drag UIManager GO to this new item --> Click dropdown function --> UIManager (script) --> select the StartGame() function
	- this makes the button trigger the function that loads the new scene
- to have button trigger a state transition/animation
```c#
public  class UIManager : MonoBehaviour {
	public  Animator startButton; //hook these up to the StartButton GameObject in inspector (will automatically get the animator component of it)
	public  Animator settingsButton;
	public  Animator dialog;

	public  void OpenSettings() {
		startButton.SetBool("isHidden", true);
		settingsButton.SetBool("isHidden", true);
		dialog.SetBool("isHidden", true); //transitions dialog from Idle state to SettingsDialogSlideIn in our animator
	}

	public  void CloseSettings() {
		startButton.SetBool("isHidden", false);
		settingsButton.SetBool("isHidden", false);
		dialog.SetBool("isHidden", true);
	}
	...
}
```
## Add animations to buttons
- just like you would on any normal GameObject (see above)
- if you want your UI elements to fly off the screen, change the anchors (e.g. if you want them to fly off top of screen, change anchors to top stretch, so that positioning is relative to the top of the screen => you can guarantee the position is off the top edge of the screen)
## Creating a sliding menu
- Add a button to the bottom left hand side of the canvas
- Add a Panel "mask" as the child of the button (with same width, but height = amount of buttons you want to fit in the menu)
	- a mask = window for content (can be hidden)
- Add a Panel for content as child of masking panel
	- add a background image to this
- Add buttons to panel content
- Animate the panel content to slide in and out of the parent panel mask









# Use case
If your scripts depend on other scripts to load before it does, this is a way to ensure those dependencies are met. 

e.g. Script A depends on Script B, C, D to load fully before it can begin initialization.

# How to use
- Attach `ScriptDependencyManager` to a new empty GameObject.
    - In the Inspector: `DependentScripts` is a list of list of scripts that will depend on other scripts to load
    - `ScriptDependencies` is a list of list of scripts. Each list of scripts is a set of dependencies for a corresponding set of dependent scripts.
    - This means `DependentScripts.Count == ScriptDependencies.Count` and all scripts in `DependentScripts[i]` are dependent on all scripts in `ScriptDependencies[i]`
    - These lists can be managed via the Unity editor
```c#
/* 
    e.g. Example below shows that:
        GameManager depends on ObjectPooler
    AND
        GameManager, SpriteManager and ObjectPooler all depend on ScoreManager and UIManager
*/
DependentScripts = [
    ['GameManager.cs'],
    ['GameManager.cs', 'SpriteManager.cs', 'ObjectPooler.cs']
];
ScriptDependencies = [
    ['ObjectPooler.cs'],
    ['ScoreManager.cs', 'UIManager.cs']
];
```
- For all dependent scripts, they must be of type `IDependentScript` and implement `OnAllDependenciesLoaded`.
- For all script dependencies (scripts that other scripts are dependent on), they must be of type `ILoadableScript` and implement `OnScriptInitialized`.
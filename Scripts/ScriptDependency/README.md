# Use case
If your scripts depend on other scripts to load before it does, this is a way to ensure those dependencies are met. 

e.g. Script A depends on Script B, C, D to load fully before it can begin initialization.

# How to use
- Attach `ScriptDependencyManager` to a new empty GameObject.
    - In the Inspector: `DependentScripts` is a list of scripts that will depend on other scripts to load
    - `ScriptDependencies` is a list of list of scripts. Each list of scripts is a set of dependencies for a corresponding dependent script.
    - This means `DependentScripts.Count == ScriptDependencies.Count`
- For all dependent scripts, they must be of type `IDependentScript` and implement `OnAllDependenciesLoaded`.
- For all script dependencies (scripts that other scripts are dependent on), they must be of type `ILoadableScript` and implement `OnScriptInitialized`.
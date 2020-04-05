Brackeys video: https://www.youtube.com/watch?v=491TSNwXTIg

- Create your own custom Unity editor window

# Code
```c#
using UnityEngine;
using UnityEditor;

public class ExampleWindow : EditorWindow {
    string myString = "Hello, World!";
    Color color;

    /**
        This will make the window show up if you click Window > Example in the Unity editor 
    */
    [MenuItem("Window/Example")]
    public static void ShowWindow() {
        //Create window if not on screen, else just focus on it 
        string windowTitle = "Example";
        EditorWindow.GetWindow<ExampleWindow>(windowTitle);

    }

    void OnGUI() {
        //window code goes here
        GUILayout.Label("This is a label.", EditorStyles.boldLabel);

        //add text field called "Name"
        myString = EditorGUILayout.TextField("Name", myString);

        //add button
        if (GUILayout.Button("Press me")) {
            Debug.Log("Button was pressed");
        }

        //color field
        color = EditorGUILayout.ColorField("Color", color);
        if (GUILayout.Button("Colorize")) {
            //get all currently selected gameObjects and change their color to the color field
            foreach(GameObject obj in Selection.gameObjects) {
                Renderer renderer = obj.GetComponent<Renderer>();
                if (renderer != null) {
                    renderer.sharedMaterial.color = colo;
                }
            }
        }
    }
}
```
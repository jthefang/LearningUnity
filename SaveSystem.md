- Based off this [tutorial](https://www.youtube.com/watch?v=XOjd_qU2Ido&ab_channel=Brackeys)
- Consider using the Easy Save Unity package mentioned in the video

## Make your class Serializable

- Make a data structure class that is Serializable
  - It has a constructor and fields
  - The field should be of the most basic C# data types to be Serializable
- Binary formatter transforms an object into a binary file and vice versa
```c#
[System.Serializable]
public class SomeObjectData {
    public int level;
    public int health;
    public float[] position;

    public SomeObjectData(SomeObject obj) { //turn the object into this Serializable object
        ...
    }
}
```

## The save system

```c#
using UnityEngine;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

public static class SaveSystem {

    public static void SaveSomeObject(SomeObject obj) {
        BinaryFormatter formatter = new BinaryFormatter();
        string path = Application.persistentDataPath + "/player.fun"; //can use any extension you want! it's just a binary file
        FileStream stream = new FileStream(path, FileMode.Create);

        SomeObjectData data = new SomeObjectData(obj);

        formatter.Serialize(stream, data);
        stream.Close();
    }

    public static SomeObjectData LoadSomeObject() {
        string path = Application.persistentDataPath + "/player.fun";

        if (File.Exists(path)) {
            BinaryFormatter formatter = new BinaryFormatter();
            FileStream stream = new FileStream(path, FileMode.Open);

            SomeObjectData data = formatter.Deserialize(strean) as SomeObjectData;
            stream.Close(); //IMPORTANT!

            return data;
        } else {
            Debug.LogError("Save file not found in " + path);
            return null;
        }
    }

}
```

## Using the save system

```c#
public class SomeObject: MonoBehaviour {
    public void SomeSaveMethod() {
        SaveSystem.SaveSomeObject(this);
    }

    public void LoadSomeObject() {
        SomeObjectData data = SaveSystem.LoadPlayer();

        level = data.level;
        health = data.health;

        Vector3 position;
        position.x = data.position[0];
        ...
        transform.position = position;
    }
}
```

## Extra: I/O to text files

```c#
using UnityEngine;
using UnityEditor;
using System.IO;

public class HandleTextFile
{
    static void WriteString()
    {
        string path = "Assets/Resources/test.txt";

        //Write some text to the test.txt file
        StreamWriter writer = new StreamWriter(path, true);
        writer.WriteLine("Test");
        writer.Close();

        //Re-import the file to update the reference in the editor
        AssetDatabase.ImportAsset(path); 
        TextAsset asset = Resources.Load("test");

        //Print the text from the file
        Debug.Log(asset.text);
    }

    static void ReadString()
    {
        string path = "Assets/Resources/test.txt";

        //Read the text from directly from the test.txt file
        StreamReader reader = new StreamReader(path); 
        Debug.Log(reader.ReadToEnd());
        reader.Close();
    }
}
```
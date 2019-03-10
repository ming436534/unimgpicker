# unimgpicker

Image picker for Unity iOS/Android

![unimgpicker_ios](doc/unimgpicker_ios.gif)
![unimgpicker_android](doc/unimgpicker_android.gif)

## Getting Started

Define Photo Library Usage Description on `Unimgpicker/Editor/NSPhotoLibraryUsageDescription.txt`

ex: **Unimgpicker/Editor/NSPhotoLibraryUsageDescription.txt**

```
Use the image to create your profile.
```

## Demo

Read image, create texture and render it on the Cube(MeshRenderer).

```csharp
using UnityEngine;
using System.Collections;

namespace Kakera
{
    public class PickerController : MonoBehaviour
    {
        [SerializeField]
        private Unimgpicker imagePicker;

        [SerializeField]
        private MeshRenderer imageRenderer;

        void Awake()
        {
            // Unimgpicker returns the image file path.
            imagePicker.Completed += (string path) =>
            {
                StartCoroutine(LoadImage(path, imageRenderer));
            };
        }

        public void OnPressShowPicker()
        {
            // With v1.1 or greater, you can set the maximum size of the image
            // to save the memory usage.
            imagePicker.Show("Select Image", "unimgpicker", 1024);
        }

        private IEnumerator LoadImage(string path, MeshRenderer output)
        {
            var url = "file://" + path;
            var www = new WWW(url);
            yield return www;

            var texture = www.texture;
            if (texture == null)
            {
                Debug.LogError("Failed to load texture url:" + url);
            }

            output.material.mainTexture = texture;
        }
    }
}
```

## Development

- The Android project depends on Window
    - Because it loads `classes.jar` from the Unity Application path.
    - Change the path in `Uniimgpicker/build.gradle` depending on your `classes.jar` file location

## Differences between the original project
- Update to support Unity 2018
- Fixed the MultiDex error by including the `classes.jar` with `provided`
- Update dependencies in the `unimgpicker/build.gradle`
- Only include the `XcodeConfigurator.cs` file when the platform is iOS
- - Changed the file structure in Assets/Plugins/Android

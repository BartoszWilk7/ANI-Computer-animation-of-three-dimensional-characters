# Character Animation and Movement Script for Unity

This Unity script is designed to control the animation and movement of a 3D character. It utilizes frame-based animation to rotate the character's legs, clothing, and other body parts to simulate a walking animation. The camera also responds to mouse input for rotation and translation.

## Features
- Frame-based animation for character walking.
- Camera control via mouse (rotation and translation).
- Support for rotating body parts such as legs and clothing.
- Automatic character movement upward (simulating walking).

## Setup

### Prerequisites
- Unity (any version that supports C# scripting).
- A 3D character model with a rig, preferably named "unitychan" or something similar.
- The model must have body parts like legs and clothing properly named and organized in the hierarchy.

### Script Integration
1. Attach the `AppCtrl.cs` script to a GameObject in your scene (typically an empty GameObject or the main camera).
2. Assign your 3D character model (e.g., "unitychan") to the `girlCharacter` field in the Inspector.
3. Ensure the body parts like `Character1_LeftUpLeg`, `Character1_RightUpLeg`, `Character1_LeftLeg`, and `Character1_RightLeg` are correctly assigned.
4. The script will automatically search for the body parts in the hierarchy and assign them to the appropriate variables.

## How It Works

### Key Components:
- **Character Modeling**: The character’s body parts (legs, arms, clothing) are assigned as `GameObject` variables in the script.
- **Camera Rotation**: The camera can be rotated by dragging the mouse while holding the left mouse button. Right mouse button allows the camera to be translated.
- **Animation Logic**: The script uses a `klatka` counter to simulate walking animation by rotating the character's body parts over different frames. After a full cycle of animation, it repeats.

### Update Logic:
- **`klatka`**: This is the frame counter that determines which animation frame should be displayed.
- The script checks the value of `klatka` and applies different rotations to the legs and clothing based on the current phase of the animation.
- **Movement**: The character is slightly moved upwards with each frame (`transform.Translate(0.0f, 0.005f, 0.0f)`) to simulate walking.
  
### Example:
- **Walking Animation**: The character's legs rotate in a cycle over several frames, simulating walking.
- **Camera Control**: Left-click and drag to rotate the camera; right-click and drag to translate the camera.

## Controls
- **Left Mouse Button**: Rotate the camera.
- **Right Mouse Button**: Translate the camera.

## Troubleshooting
- Ensure that your character model is correctly set up in Unity with the right hierarchy and named objects.
- Check that the script is attached to the appropriate GameObject (usually an empty GameObject or camera).

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing
Feel free to fork the repository and create a pull request with improvements or fixes.

## Authors
- **Bartosz Wilk** – Initial script and development
- **Jagoda Kuczek** – Collaboration and design

## Acknowledgements
- Special thanks to the Unity community for their support and resources.

---

## Code

```csharp
// Bartosz Wilk, Jagoda Kuczek

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class AppCtrl : MonoBehaviour
{
    public GameObject girlCharacter;

    private GameObject LUPleg;
    private GameObject RUPleg;
    private GameObject Lleg;
    private GameObject Rleg;
    private GameObject Rubranie;
    private GameObject Lubranie;

    private int klatka;
    private int cycle;
    private float mouse_dostance_x;

    void Start()
    {
        Debug.Log("start");

        klatka = 1;
        cycle = 0;

        GameObject mArm;
        mArm = GameObject.Find("unitychan/Character1_Reference/Character1_Hips/Character1_Spine/Character1_Spine1/Character1_Spine2/Character1_LeftShoulder/Character1_LeftArm");
        mArm.transform.Rotate(0, 75, 0);
        mArm = GameObject.Find("unitychan/Character1_Reference/Character1_Hips/Character1_Spine/Character1_Spine1/Character1_Spine2/Character1_RightShoulder/Character1_RightArm");
        mArm.transform.Rotate(0, -75, 0);

        string nazwa = "unitychan/Character1_Reference/Character1_Hips/";

        //nazwa += "Character1_LeftUpLeg";
        LUPleg = GameObject.Find(nazwa + "Character1_LeftUpLeg");
        RUPleg = GameObject.Find(nazwa + "Character1_RightUpLeg");
        Lleg = GameObject.Find(nazwa + "Character1_LeftUpLeg/Character1_LeftLeg");
        Rleg = GameObject.Find(nazwa + "Character1_RightUpLeg/Character1_RightLeg");
        Rubranie = GameObject.Find(nazwa + "J_R_Skirt_00");
        Lubranie = GameObject.Find(nazwa + "J_L_Skirt_00");
    }

    Vector2 pos;

    void Rotatecam()
    {
        float Sensitivity = 0.2f;
        if (Input.GetMouseButtonDown(0))
        {
            pos = Input.mousePosition;
        }
        if (Input.GetMouseButton(0))
        {
            transform.localEulerAngles += new Vector3((Input.mousePosition.y - pos.y) * Sensitivity, (-Input.mousePosition.x + pos.x) * Sensitivity, 0);
            pos = Input.mousePosition;
        }

        if (Input.GetMouseButtonDown(1))
        {
            pos = Input.mousePosition;
        }
        if (Input.GetMouseButton(1))
        {
            Sensitivity = 0.02f;
            transform.Translate((Input.mousePosition.x - pos.x) * Sensitivity, 0.0f, (Input.mousePosition.y - pos.y) * Sensitivity);
            pos = Input.mousePosition;
        }
    }

    // Update is called once per frame
    void Update()
    {
        klatka++;
        Rotatecam();

        if (klatka < 50)
        {
            RUPleg.transform.Rotate(0, 0, -1);
            Rubranie.transform.Rotate(0, 0, -0.6f);
            LUPleg.transform.Rotate(0, 0, 0.3f);
            Rleg.transform.Rotate(0, 0, 1);
            girlCharacter.transform.Translate(0.0f, 0.005f, 0.0f);
        }
        else if (klatka >= 50 && klatka < 100)
        {
            RUPleg.transform.Rotate(0, 0, 1);
            Rubranie.transform.Rotate(0, 0, 0.6f);
            LUPleg.transform.Rotate(0, 0, -0.3f);
            Rleg.transform.Rotate(0, 0, -1);
            Lleg.transform.Rotate(0, 0, 1);

            girlCharacter.transform.Translate(0.0f, 0.005f, 0.0f);
        }
        else if (klatka >= 100 && klatka < 150)
        {
            RUPleg.transform.Rotate(0, 0, 0.3f);
            LUPleg.transform.Rotate(0, 0, -1);
            Lubranie.transform.Rotate(0, 0, -0.6f);

            girlCharacter.transform.Translate(0.0f, 0.005f, 0.0f);
        }
        else if (klatka >= 150 && klatka < 200)
        {
            RUPleg.transform.Rotate(0, 0, -0.3f);
            LUPleg.transform.Rotate(0, 0, 1);
            Lubranie.transform.Rotate(0, 0, 0.6f);
            Lleg.transform.Rotate(0, 0, -1);
            Rleg.transform.Rotate(0, 0, 1);

            girlCharacter.transform.Translate(0.0f, 0.005f, 0.0f);
        }
        else if (klatka >= 200 && klatka < 250)
        {
            if (cycle < 3)
            {
                RUPleg.transform.Rotate(0, 0, -1);
                Rubranie.transform.Rotate(0, 0, -0.6f);
                LUPleg.transform.Rotate(0, 0, 0.3f);
                girlCharacter.transform.Translate(0.0f, 0.005f, 0.0f);
            }
            else
            {
                Rleg.transform.Rotate(0, 0, -1);
            }
        }
        else if (cycle < 3)
        {
            klatka = 50;
            cycle++;
        }
    }
}

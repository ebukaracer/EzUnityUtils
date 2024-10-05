# SaveManager
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-blue)](http://makeapullrequest.com) [![License: MIT](https://img.shields.io/badge/License-MIT-blue)](https://github.com/ebukaracer/ebukaracer/blob/ebukaracer-resources/LICENSE.md)

**SaveManager** is a Unity package that provides a flexible way for saving and loading game data. It supports multiple storage backends, including PlayerPrefs and browser local storage for WebGL builds.

- [Download SaveManager.unitypackage latest](https://github.com/ebukaracer/EzUnityUtils/releases/download/SaveManager-v1.0.0/SaveManager.unitypackage)

## Features
- **Multiple Storage Backends**: Supports PlayerPrefs and browser's LocalStorage.
- **Easy Integration**: Simple API for saving and loading data.
- **Customizable**: Easily extendable to support additional storage backends.
- **Data Types**: Supports saving and loading of integers, floats, strings, and Booleans.
- **Demo**: Includes a Demo scene to help you quickly get started: `Assets/SaveManager/Demo`

## Installation
Drag and drop the downloaded **.unitypackage** above into your Unity project's `Assets` directory. You can omit to include the demo project during import:
![SM2.png](https://github.com/ebukaracer/ebukaracer/blob/ebukaracer-resources/SaveManager-Images/SM2.png)

## Usage

### Saving Data:
```csharp
SaverFactory.Saver.SaveInt("highscore", 100);
SaverFactory.Saver.SaveFloat("volume", 0.75f);
SaverFactory.Saver.SaveString("playerName", "Racer");
SaverFactory.Saver.SaveBool("isMusicOn", true);
```

### Loading Data:
``` csharp
// With default value set to '0'
int highscore = SaverFactory.Saver.GetInt("highscore", 0);
float volume = SaverFactory.Saver.GetFloat("volume", 1.0f);
string playerName = SaverFactory.Saver.GetString("playerName", "Guest");
bool isMusicOn = SaverFactory.Saver.GetBool("isMusicOn", false);
```

### Checking for existence of Data:
``` csharp
bool hasHighscore = SaverFactory.Saver.Contains("highscore");
```

### Clearing Data:
``` csharp
SaverFactory.Saver.Clear("highscore");
SaverFactory.Saver.ClearAll();
```

## Note for Users
> For WebGL builds, ensure this package was imported fully into your project before deploying, the demo scene is still optional by the way.

> Ensure that `ENABLE_LSS` scripting define symbol is added to your project's scripting defining symbols section. Confirm that it is present after this package has been installed, since it adds it automatically during import, otherwise manually add it.

> This package by default, uses `playerprefs` in the unity editor but resolves to browser's `localstorage` after deployment. 

**Save Manager** package was built with WebGL build persistent saving support in mind so as to eliminate the issue of erasing of game data whenever multiple builds to the same project are made. If this isn't a problem for you and you'd want to rely only on `PlayerPrefs` way of saving game data, feel free to import and use only `PlayerPrefsSaver.cs` file as well as the `.asmdef` associated with this package:
![SM1.png](https://github.com/ebukaracer/ebukaracer/blob/ebukaracer-resources/SaveManager-Images/SM1.png)

## Contributing    
Contributions are welcome! Please open an issue or submit a pull request.
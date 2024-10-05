# EzSaver 
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-blue)](http://makeapullrequest.com) [![License: MIT](https://img.shields.io/badge/License-MIT-blue)](https://github.com/ebukaracer/ebukaracer/blob/ebukaracer-resources/LICENSE.md)

Efficient File Save-System for Unity Games.
- [Download EzSaver.unitypackage latest](https://github.com/ebukaracer/EzUnityUtils/releases/download/EzSaver-v2.0.0/EzSaver.unitypackage)
- [Read EzSaver Docs](https://github.com/ebukaracer/EzUnityUtils/blob/pkg-EzSaver/DOCS.md)

## Features
- Includes a Demo scene
- Includes an Editor Window for certain operations
- Supports file/game-data saving
- Encryption support (AES)
- Supports custom-type json serialization
- Supports method chaining
- *..and much more.*

## Installation
Drag and drop the downloaded **.unitypackage** above into your Unity project's `Assets` directory. You can omit to include the demo project during import. Check out the setup guide [here](https://github.com/ebukaracer/EzUnityUtils/blob/main/SETUPGUIDE.md)

## Usage
*After properly setting up this package, you're ready to start consuming the [APIs](https://github.com/ebukaracer/EzUnityUtils/blob/pkg-EzSaver/DOCS.md#public-apis)*:

### Initialize a save-file:
``` csharp
internal class Person  
{  
    public int Age { get; set; }  
}

private Person _person;
private int _highscore;
private EzSaver _ezSaver;

private void Awake()  
{
	// Loads up a save file
	_ezSaver = new EzSaver("Save.txt").Load();
	
	// Access previously saved content or use default values assuming not available:
	_person = _ezSaver.Read("Person", new Person());
	_highscore = _ezSaver.Read("Highscore", _highscore);
}
```

### Update data to be saved: 
``` csharp
public void AddScore(int amount)  
{
	_highscore += amount;  
	_person.Age = _highscore;
}
```

### Write save-data to temporary storage:
``` csharp
_ezSaver  
    .Write("Person", _person)  
    .Write("Highscore", _highscore);
```

### Save changes to save-file (📌):
``` csharp
// Find a suitable method to call Save() once.
private void OnDestroy()  
{
	_ezSaver.Save();
}
```

### Remove save-items:
``` csharp
_ezSaver  
    .Clear("Person", _person)  
    .Clear("Highscore", _highscore);
```

### Delete save-file:
``` csharp
_ezSaver.DeleteFile();
```

## Dependencies
This package depends on **newtonsoft-json; v3.2.1**, kindly install from the unity package manager:
1. Hit `(+)` and select `Add package by name` 
2. Paste the package name: `com.unity.nuget.newtonsoft-json` and hit `Add`

## Contributing  
Contributions are welcome! Please open an issue or submit a pull request.
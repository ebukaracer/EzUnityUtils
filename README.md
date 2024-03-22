# EzSaver 
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-blue.svg)](http://makeapullrequest.com) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/ebukaracer/ebukaracer/blob/ebukaracer-resources/LICENSE.md)

Efficient File Save-System for Unity Games.
- [Download](https://github.com/ebukaracer/EzUnityUtils/releases/download/v1.0.0/EzSaver.unitypackage)
- [Docs](https://github.com/ebukaracer/EzUnityUtils/blob/pkg-EzSaver/DOCS.md)

## Installation
Drag and drop the downloaded **.unitypackage** to Assets folder and follow the setup guide [here](https://github.com/ebukaracer/EzUnityUtils/blob/main/SETUPGUIDE.md)

## Features
- Optional Demo scene
- GUI Window for certain operations
- Supports file/game-data saving
- Encryption support (aes)
- Supports custom-type serialisation (newtonsoft-json)
- Supports method chaining
- *..and much more.*

## Usage Guide
After properly setting up this package, you're ready to start consuming the APIs:

### Examples

Initialise a save-file:
```
internal class Person  
{  
    public int Age { get; set; }  
}

private Person _person;
private int _highscore;
private EzSaver _ezSaver;

private void Awake()  
{
	_ezSaver = new EzSaver("Save.txt").Load();
	
	// Access previously saved contents or use default values assuming not available:
	_person = _ezSaver.Read("Person", new Person());
	_highscore = _ezSaver.Read("Highscore", _highscore);
}
```
Add data to be saved: 
```
public void AddScore(int amount)  
{
	_highscore += amount;  
	_person.Age = _highscore;
	_ezSaver.Write("Person", _person)
}
```

Write save-items to cache storage:
```
_ezSaver  
    .Write("Person", _person)  
    .Write("Highscore", _highscore);
```

Commit changes to save-file (important):
```
// Find a suitable method to call Commit()
private void OnDestroy()  
{
	_ezSaver.Commit();
}
```

Remove save-items:
```
_ezSaver  
    .Clear("Person", _person)  
    .Clear("Highscore", _highscore);
```

Delete save-file:
``` 
_ezSaver.DeleteFile();;
```

## Dependencies
- NewtonSoftJson - Bundled with Unity.

## Contribution
Feel free to open an `issue` If you discovered any bug or feel like something is missing. 

*PS: Kindly, drop a ⭐ if found useful.*
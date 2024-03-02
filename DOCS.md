
### Quick Overview
Refer to [Examples](https://github.com/ebukaracer/EzUnityUtils/tree/pkg-EzSaver#examples) for quick guide on creating and initialising a save-file in your project scripts.

## Public APIs

#### `EzSaver`.

- `Write<T>(string key, T value): EzSaver`
- `Commit(): void`
- `void Commit(): void`
-
- `Load(): EzSaver`
- `Read<T>(string key, T defaultValue = default): T`
- `TryRead<T>(string key, out T result): bool`
-
- `Exists(string key): bool`
- `ItemCount(): int`
- `GetAllKeys(): IEnumerable<string>`
- `Clear(string key): bool`
- `ClearAll(): void`
- `DeleteFile(): bool`

## Best Practices
### Initialisation
```
// Save.txt file opting-out for encryption/decryption.
_ezSaver = new EzSaver("Save.txt").Load();
```
By default, the `EzSaver` class does not encrypt/decrypt a save-file provided during initialisation(see above), however, you can explicitly set `useAes:` to `true` assuming you intend to do so:
```
// Save.txt file opting-in for encryption/decryption.
_ezSaver = new EzSaver("Save.txt", true).Load();
```
⚠ *If you already have an encrypted save-file, be sure to set `useAes:` to `true`, so as to decrypt it during initialisation, otherwise its contents would be lost.*

Omitting to add any extension after the save-file name, would default it to `.json`:
```
// A Save.json file would be created instead.
_ezSaver = new EzSaver("Save").Load();
```

Part of the `EzSaver` constructor is: `EzSaver(IReader reader, ILog log, EzSaverSettings settings = default)`:
- The `reader` was used for testing purposes, but readily available in case you'd want to provide your custom implementation for file operations, such as `LoadString()`, `SaveString()`, `Delete()`  
- The `log` was also used for testing purposes, you can also implement the interface to provide a custom way of logging messages to the console, methods such as: `Warn()`, `Error()`, `Log()` are available. 
- The `settings` variable of type `EzSaverSettings` provides some useful members such as `SecurityMode: SecurityMode`, `Encrypter: IEncrypter`, the former is an Enum that indicates  the type of security mode to use - whether or not to enable encryption/decryption. Currently only `Aes` is available, but if you need additional modes, you can always implement the `IEncrypter` interface and provide your own. Be sure to include it in the `SecurityMode` Enum too. Most times, It will be unnecessary to initialise the `settings` class, since by default, it gets initialised whenever this constructor is used: `EzSaver(string filename, bool useAes = false)`

### Security
- Since this package uses `Aes` security for encryption/decryption, it is worth mentioning that the same key that was used for encryption is also used for decryption!
- Also, save-files can share the same key for encryption/decryption and that same key must be used across. In a situation where a previous save-file was encrypted and a different key is used for its decryption, all its contents would be lost and irrecoverable!
- Fortunately, this package maintains key consistency for encryption/decryption - a save key is always available and can be viewed or changed via the editor window.

### Save location
By default, all save-files reside at: `Application.persistentDataPath/EzSaver/Saves`, this is to maintain a clean hierarchy and promote consistency. However, if you really need to change that location, you can do that in `GlobalSettings.cs` script.

### Miscellaneous
- Call `EzSaver.ToString()` to inspect the **JSON** representation of your save-contents and as well display them to the console using `Debug.Log(EzSaver.ToString())`
- Instead of always calling `EzSaver.Load()` for every class you would be saving in values, you can have a single class/gameobject for that, by utilising **MonoBehaviour's** `DontDestroyOnload`, you'll get to eliminate some performance spikes. 
- Tip to enable/disable debug messages coming from this package can be found in `EzLogger.cs` script.

## Editor Window
![img](https://github.com/ebukaracer/EzUnityUtils/blob/pkg-EzSaver/DOCS.md)
- Location: `Racer -> EzSaver -> Menu`
- Every button available in that window has self-explanatory tooltip attached to it, upon hover.
- ⚠️ *The `Save-File:` label-field only accepts an existing save-file that is available at `Save-File Path:` location.*
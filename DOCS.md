
## Quick Overview
Refer to [Examples](https://github.com/ebukaracer/EzUnityUtils/tree/pkg-EzSaver#examples) for quick guide on creating and initializing a save-file in your project scripts.

## Public APIs

#### `new EzSaver(..)`.

``` csharp
Write<T>(string key, T value): EzSaver
Commit(): void
void Commit(): void

Load(): EzSaver
Read<T>(string key, T defaultValue = default): T
TryRead<T>(string key, out T result): bool

Exists(string key): bool
ItemCount(): int
GetAllKeys(): IEnumerable<string>
Clear(string key): bool
ClearAll(): void
DeleteFile(): bool
```

#### `EzSaverFactory`.
``` csharp
Create(..) : EzSaver
```

## Best Practices
### Initialization
**Constructors**:
1. `public EzSaver(string contentSource, bool isLiteral = false, bool useSecurity = false)`
2. `public EzSaver(IReader reader, ILog log, EzSaverSettings settings = default)`

``` csharp
// Save.txt file opting-out for encryption/decryption.
_ezSaver = new EzSaver("Save.txt").Load();
```
*By default, the `EzSaver` class does not encrypt/decrypt a save-file provided during initialization(see above). However, you can explicitly set `useSecurity:` to `true` assuming you intend to do so*:
``` csharp
// Save.txt file opting-in for encryption/decryption.
_ezSaver = new EzSaver("SaveFile.txt", false, true).Load();
```

*Also, omitting to add any extension after the save-file name, would default it to `.json`*:
``` csharp
// A Save.json file would be created.
_ezSaver = new EzSaver("SaveFile").Load();
```

*Assuming you had a previously `.json` or `encrypted` string literal defined somewhere in your code and containing the save-content, you can pass the string to EzSaver's constructor overload*: 
``` csharp
string _jsonLiteral @"{
  ""Person"": {
    ""Age"": 25
  },
  ""Highscore"": 25
}   
";
_ezSaver = new EzSaver("_jsonLiteral", true, false).Load();

// or

string _encryptedLiteral = "qwertvtkrgrefefrf34fv"
_ezSaver = new EzSaver("_encryptedLiteral", true, false).Load();
```
*You must explicitly set `useLiteral = true` during initialization so as to inform EzSaver you're not loading the save-content from a file. You can also set `useSecurity` to either true or false here, it doesn't matter whether it's an encrypted string literal or not, it will eventually get decrypted and will no longer be encrypted further on, since already, `useSecurity = false`*.   

Parts of EzSaver's **constructors** are:
1. `EzSaver(IReader reader, ILog log, EzSaverSettings settings = default)`:
2. `EzSaver(string literal, ILog log, EzSaverSettings settings = default)`

- `reader`: was used for testing purpose, but readily available in case you would want to provide a custom implementation for file operations, such as: `LoadString()`, `SaveString()`, `Delete()`  
- `log`: was also used for testing purpose. You can also implement the interface to provide a custom way of logging messages to the console, methods such as: `Warn()`, `Error()`, `Log()` are available. 
- `settings`: which is of type `EzSaverSettings` provides some useful members such as `SecurityMode: SecurityMode`, `Encrypter: IEncrypter`, the former is an Enum that indicates  the type of security mode to use - whether or not to enable encryption/decryption. Currently only `AES` is available, but if you need additional modes, you can always implement the `IEncrypter` interface and provide your own. Be sure to also include it in the `SecurityMode` Enum. Often times, It will be unnecessary to initialize the `settings` class, since by default, it gets initialized whenever this constructor is used: `EzSaver(string filename, bool useSecurity = false)`
- `literal`: Indicates whether the string you passed is a string-literal(which contain the encrypted/save-content directly) or is coming from a file (by default).

### Security
- Since this package uses `AES` security for encryption/decryption, it is worth mentioning that the same key that was used for encryption is also used for decryption!
- Save-files can share the same key for encryption/decryption and that same key must be used across. In a situation where a previous save-file was encrypted and a different key is used for its decryption, all its contents would be lost and irrecoverable!
- Fortunately, this package maintains key consistency for encryption/decryption - a save key is always available and can be viewed or changed via the editor window.

### Save location
By default, all save-files reside at: `Application.persistentDataPath/EzSaver/Saves`, this is to maintain a clean hierarchy and promote consistency. However, if you really need to change that location, you can do that in `GlobalSettings.cs` script.

### Miscellaneous
- Instead of always creating `new EzSaver(..)`  instances for every class that uses it, you can use the factory class: `EzSaverFactory.Create(..)`  to quickly create an EzSaver instance(one and only) and use that instance across your entire app. 
- To enable/disable console messages coming from this package locate `EzLogger.cs` script.

## Editor Window
Path: `Racer -> EzSaver -> Menu`

![img](https://github.com/ebukaracer/EzUnityUtils/blob/pkg-EzSaver/DOCS.md)

- Every button available in that window has self-explanatory tooltip attached to it, upon hover.
- The `Save-File:` field only accepts an existing save-file that is available at `Save-File Path:` location.
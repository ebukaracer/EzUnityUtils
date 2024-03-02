
# Setup Guide
Detailed guide on how to reference any of the packages in your project. By using `EzSaver` package as example, same steps are applicable to other packages containing an `assembly definition` file.

Nevertheless, I'd assume you already downloaded and added the `.unitypackage` file to your project.

### Add Package Reference
*After you have imported this package, you need to add its reference to your project.*

⚠️ Kindly skip this step If you are not using an `Assembly Definition Asset` in your project:
	
Otherwise, click on your project's `Assembly Definition Asset` file, under `Assembly Definition References` click the (+) icon and add a `EzSaver` reference, then apply changes below.

### Configure Debug Logs
 Log messages issued from this package might not appear without explicitly enabling them:
	
 Under  `Project Settings` > `Player` > `Other Settings` > `Scripting Define Symbols`, hit the (+) icon trice and for each entry add:  `ENABLE_LOG` | `ENABLE_LOG_WARNING` | `ENABLE_LOG_ERROR` 
	
⚠️ Be sure to remove the entries above when you're set to build your project, hence, by hitting the (-) icon for each entry.

### Calling APIs
📌Once the above steps are properly setup, you're ready to start utilising the package's public APIs.
	
📌Be sure to include: `using Racer.EzSaver;` or `using Racer.pkg_Name;` to any external classes calling any of the package's public APIs. 
	
📌An optional demo Scene may be contained in the package and will probably walk you through, you can omit to import the demo scene as well.

### Useful Resources
- [.asmdef files in Unity](https://bit.ly/3exDWNz) 
- [Scripting Define Symbols](https://bit.ly/3yGVWvS).
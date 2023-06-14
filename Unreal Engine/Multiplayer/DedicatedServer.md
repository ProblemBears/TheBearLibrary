<h1 align = "center"> Setting up a Dedicated Server hosted with Amazon GameLift </h1>

## Prerequisites
1. First make sure you have a C++ project and that you open it with an [Unreal Engine from Source]() build as these are the prerequisites to be able to create [builds for a dedicated server](https://docs.unrealengine.com/5.1/en-US/setting-up-dedicated-servers-in-unreal-engine/#tutorial)

## Guides Followed
- [Setting up Dedicated Servers (UE Docs)](https://docs.unrealengine.com/5.1/en-US/setting-up-dedicated-servers-in-unreal-engine/)
- [Amazon GameLift UE (Youtube Playlist)](https://www.youtube.com/watch?v=3_iBuko39JA&list=PLuGWzrvNze7LEn4db8h3Jl325-asqqgP2&index=1&pp=iAQB)
- [Networking and Multiplayer (UE Docs)](https://docs.unrealengine.com/5.1/en-US/networking-and-multiplayer-in-unreal-engine/)
- [Amazon Game Lift for UE 5.1.1 Docs](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-engines-setup-unreal.html)

## 1 - Create Server / Client Build Targets
### 1.1 - Server
1. Navigate to your project's root directory in File Explorer
2. Right-click on your project's `*.uproject` file and select `Generate Visual Studio project files...`
3. Open the generated `*.sln` Visual Studio solution file in **Visual Studio (VS)**. 
4. If your project doesn't already have one, create a `<PROJECT_NAME>Server.Target.cs` in the `<PROJECT_DIRECTORY>/Source` directory
    - ADD INSTRUCTIONS HERE!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
5. Change the `Solution Configuration` to `Development Server`
6. From the menu bar, select `Build > Build Solution`. This builds the dedicated server for your project
7. After the build process successfully finishes, you can find the newly built files in `<PROJECT_DIRECTORY>/Binaries/Win64`, in particular, the executable `<PROJECT_NAME>Server.exe`
### 1.2 - Client
1. With VS still open from the last step, change the `Solution Configuration` to `Development Client`
2. If your project does not already have one, create a `<PROJECT_NAME>Client.Target.cs` in the `<PROJECT_DIRECTORY>/Source` directory
    - ADD INSTRUCTIONS HERE!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
3. From the menu bar, select `Build > Build Solution`. This builds the client game for your project
4. After the build process successfully finishes, you can find the newly built files in `<PROJECT_DIRECTORY>/Binaries/Win64`, in particular, the executable `<PROJECT_NAME>Client.exe`

### 1.3 - Set the Server Default Map

1. In the menu bar, navigate to `Edit > Project Settings...`
2. `Project > Maps & Modes`
3. Change `Default Maps > Advanced > Server Default Map to` a map you want your server to start in
4. When packaging the server build, we also have to make sure that these maps are included in `Packaging > Packaging > Advanced > List of maps to include in package build`
5. Now you can close the `Project Settings` window

### 1.3 - Let them cook!
- You have noow built both the dedicated server and the clients that connect to the server. If you try to run the server and clients from VS, you'll get errors. This is because you haven't cooked the content. To cook the content, do the following sequence for the server and then the client :
1. Start the `Development Editor` either from VS or by navigating to `UnrealEditor.exe` in your project's directory
2. Set 
    - `Platforms > Windows > Binary Configuration` to `Development`
    - `Platforms > Windows > Build Target` to `Server` (`Client` when you want to cook the client)
3. Run Cook from `Platforms > Windows > Content Management`
    - If the build succeeds we can check the files created from the cook in `<PROJECT_DIRECTORY>/Saved/Cooked/WindowsServer` (or `WindowsClient`)

### 1.4 - Test the Servers / Clients
1. **Start the dedicated server** : In a terminal, run the following command in your project's root directory
    ```
    ./Binaries/Win64/<PROJECT_NAME>Server.exe -log
    ```
2. **Connect Clients to Dedicated Server** : In a terminal, run the following command in your project's root directory
    ```
    ./Binaries/Win64/<PROJECT_NAME>Client.exe 127.0.0.1:7777 -WINDOWED -ResX=800 -ResY=450
    ```
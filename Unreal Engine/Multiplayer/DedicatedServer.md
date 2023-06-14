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
## 2 - Configure Maps & Modes 
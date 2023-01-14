<h1 align="center"> Unreal Engine Programming with C++ </h1>

<h2 align="center"> Setting up your Environment </h2>

### Changing your Source Code Editor
- `Edit` &rarr; `Editor Preferences` &rarr; `General` &rarr; `Source Code` &rarr; `Source Code Editor`

### Using VS Code
- Once you have VS Code as your Code Editor, whenever you make a new class you should make sure to click `Tool` &rarr; `Refresh Visual Studio Code Project`. **THIS GETS RID OF RED SQUIGLES WHEN PROGRAMMING IN NEW CLASSES IN C++ AND ACTIVATES INTELLISENSE**
- You can open VS Code via
    * `Tools` &rarr; `Open Visual Studio Code`
    * or the `.code-workspace` file in your project's root directory
- To recompile our Editor Binary do the following in VS Code
    1.  `Terminal` &rarr; `Run Build Task...` (or simply `Ctrl + Shift + B`)
    2. Click on the task with a similar naming convention to the following : `ProjectNameEditor Win64 Development Build`
- To navigate to Engine Declaration/Definitions you can
    1. Press `F12`
    2. Right click on the piece of code you want to know more about and press either of the following -
        1. `Go to Definition` ( `F12` )
        2. `Go to Declaration`
        3. `Go to References` ( `Shift + F12` )
- To search for a specific file (**helpful for #includes for .cpp files**)
    * `Ctrl + P`

### Using "Cheats" in Play Mode
- Use `~` while in Play Mode to open up a Cheat Code menu
    * Macro like functionality could be defined in your code that activate depending on values assigned to the "cheat"
- When you find the name of your cheat you can use `TAB` in order to auto complete it
- To see an expanded version of this log, press `~` twice

### Unreal Engine and the Command Line
```cmd
"<Where you installed Unreal>\UE_5.0\Engine\Binaries\Win64\UnrealEditor.exe" "<Where you have a UE Project>\PuzzlePlatforms.uproject" -game
```
* You can add the flag `-log` after `-game` in order to log what is happening in the CMD
* To load a specific map do this before `-game` : `/Game/{path to a map exluding Content directory}/someMapName`
* To create a game server instead of an actual visual game replace the `-game` flag with the `-server` flag
    * To connect a client to this server terminal instance. Open up another terminal with the following appended:  
    `<Your Local IP Address (127.0.0.1 works for all systems)> -game`
<h2 align="center"> Glossary </h2>

| Action | Syntax |
| --- | --- |
| Get A Pointer to a Component of the current Class| `AActor::FindComponentByClass<USomeComponent>()`|
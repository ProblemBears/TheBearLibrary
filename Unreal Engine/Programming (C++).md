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

<!--------------------------------------------------------------------------------------------------------------->
<h2 align="center"> Gameplay Classes: Objects, Actors, and Components </h1>

- The 4 main class types that you derive from for the majority og gameplay classes are - `UObject`, `AActor`, `UActorComponent`, and `UStruct`.
    * You can create types that don't derive from any of these classes, but they will not participate in the features that are built into the engine.
    * Classes that are built outside of the `UObject` hierarchy are usually used to integrate 3rd party libraries or wrap OS specific features.
### Unreal Objects (UObject)
- The base building block in the Engine is called `UObject`. This class, coupled with `UClass`, provides a number of the Engine's most important services:
    * Reflection of properties and methods
    * Serialization of properties
    * Garbage collection
    * Finding a UObject by name
    * Configurable values for properties
    * Networking support for properties and methods 
### AActor
- An `AActor` is a `UObject` that is meant to be part of the gameplay experience. All objects that can be placed into a level extend from this class
- Actors have their own behaviors (specialization through inheritance), but they also act as containers for a hierarchy of `Actor Components` (specialization through composition). This is done through the Actor's `RootComponent`, which contains a single `USceneComponent` that, in turn, can contain many others.
    * The Scene Component is a must, since it specifies the translation, rotation, and scale.
    * Actors have the following events that are called during their lifecycles : `BeginPlay`, `Tick`, and `EndPlay`
- Placing an Actor manually into a level is "easier" than spawning an Actor, because the Engine would need to know stuff like its initial transform, its current physical state, and registry with events like "Tick". For this reason, Unreal Engine has a method devoted to the spawning of an actor, `SpawnActor`(a member of `UWorld`).
    * We can get rid of an Actor by calling `Destroy`. During which, `Endplay` will run, enabling you to perform custom logic before the Actor goes to garbage collection.
    * Another option for controlling how long an Actor exists is to use the `Lifespan` member in the Actor's constructor.

### UStruct
- You simply mark a struct with USTRUCT() and you'll get some support from UE's reflection system (not garbage collection unfortunately)
### UActorComponent
- Actor Components (`UActorComponent`) have their own behaviors and are usually responsible for functionality that is shared across many types of Actors. They "support" the Actors. Components can also be attached to other components and they move relative to their parent component

<!---------------------------------------------------------------------------------------------------------------->
<h2 align="center"> Unreal Reflection System </h2>

- Unreal Engine uses its own implementation of reflection that enables dynamic features such as garbage collection, serialization, network replication, and Blueprint/C++ communication. **These features are opt-in**, meaning you have to add the correct markup to your types, otherwise UE will ignore them and not generate the reflection data for them.
- Some basic Reflection markup :
    * `UCLASS()` - Used to tell Unreal to generate reflection data for a class. The class must derive from `UObject`
    * `USTRUCT()` - Used to tell Unreal to generate reflection data for a struct
    * `GENERATED_BODY()` - UE replaces this with all the necessary boilerplate code that gets generated for the type
    * `UPROPERTY()` - Enables a member variable of a UCLASS or a USTRUCT to be used as a UPROPERTY. A UPROPERTY has many uses. It can allow the variable to be replicated, serialized, and accessed from Blueprints. They are also used by the garbage collector to keep track of how many references there are to a `UObject`
    * `UFUNCTION()` - Enables a class method of a UCLASS or a USTRUCT to be used as a UFUNCTION. A UFUNCTION can allow the class method to be called from Blueprints and used as RPCs, among other things
- Here is an example declaration of a UCLASS :
    ```cpp
    #include "MyObject.generated.h"

    UCLASS(Blueprintable)
    class UMyObject : public UObject
    {
        GENERATED_BODY()

    public:
        MyUObject();

        UPROPERTY(BlueprintReadOnly, EditAnywhere)
        float ExampleProperty;

        UFUNCTION(BlueprintCallable)
        void ExampleFunction();
    };
    ```
    * Unreal Engine generates all the reflection data and puts it in `MyObject.generated.h`. You must include this file as the last include in the header file that declares your type
    * The `UCLASS`, `UPROPERTY`, and `UFUNCTION` in this example include additional specifiers. There are too many specifiers to list here, but the following links can be used as reference :
        * [UCLASS Specifiers](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/GameplayArchitecture/Classes/Specifiers)
        * [UPROPERTY Specifiers](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/GameplayArchitecture/Properties/Specifiers)
        * [UFUNCTION Specifiers](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/GameplayArchitecture/Functions/Specifiers)
        * [USTRUCT Specifiers](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/GameplayArchitecture/Structs/Specifiers)

<h2 align="center"> Glossary </h2>

| Action | Syntax |
| --- | --- |
| Get A Pointer to a Component of the current Class| `AActor::FindComponentByClass<USomeComponent>()`|
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
<h2 align="center"> Essential Knowledge </h2>

### Class Naming Prefixes
- Unreal Engine provides tools that generate code for you during the build process. These tools have some class-naming expectations and will trigger warnings or errors if the names do not match the expectations. The list of class prefixes below delineates what the tools are expecting :
    * Classes derived from Actor prefixed with `A`, such as `AController`
    * Classes derived from Object are prefixed with `U`, such as `UComponent`
    * Enums are prefixed with `E`, such as `EFortificationType`
    * Interface classes are usually prefixed with `I`, such as `IAbilitySystemInterface`
    * Template classes are prefixed by `T`, such as `TArray`
    * Classes that derive from SWidget (Slate UI) are prefixed by `S`, such as `SButton`
    * Everything else is prefixed by the letter `F`, such as `FVector`
### Numeric Types
- Since different platforms have different sizes for basic types. UE4 provides the following types which you should use as an alternative :
    * `int8`/`uint8` (`16`, `32`, `64`), `float` (32-bit) and `double` (64-bit)
    * Unreal Engine has a template, [`TNumericLimits<T>`](https://docs.unrealengine.com/en-US/API/Runtime/Core/Math/TNumericLimits), for finding the minimum and maximum ranges a value type can hold
### Strings
- [FString](https://docs.unrealengine.com/en-US/API/Runtime/Core/Containers/FString)
    * `FString` is a mutable string, analogous to `std::string`. `FString` has large suite of methodsfor making it easy to work with strings. To create a new `FString`, use the `TEXT` macro :
        ```cpp
        FString MyStr = TEXT("Hello, Unreal 4!")
        ```
- [FText](https://docs.unrealengine.com/en-US/API/Runtime/Core/Internationalization/FText)
    * `FText` is similar to `FString`, but is meant for localized text. To create a new `FText`, use the `NSLOCTEXT` macro. This macro takes a namespace, key, and a value for the default language :
        ```cpp
        FText MyText = NSLOCTEXT("Game UI", "Health Warning Message", "Low Health!")
        ```
        You can also use the `LOCTEXT` macro, so you only have to define a namespace once per file. Make sure to undefine it at the bottom of your file.
        ```cpp
        // In GameUI.cpp
        #define LOCTEXT_NAMESPACE "Game UI"

        //...
        FText MyText = LOCTEXT("Health Warning Message", "Low Health!")
        //...

        #undef LOCTEXT_NAMESPACE
        // End of file
        ```
- FName
    * An `FName` stores a commonly recurring string as an identifier in order to save memory and CPU time when comparing them. 
    * Rather than storing the complete string many times across every object that refrences it, `FName` uses smaller storage footprint index that maps to a given string. **This stores the contents of the string once**, saving memory when that string is used across many objects
- TCHAR
    * The `TCHAR` type is used as a way of storing characters indepent of the character set being used, which may differ between platforms 
    * Under the hood, UE strings use `TCHAR` arrays to store data in the **UTF-16** encoding
    * You can access the raw data of UE strings by using the overloaded dereference operators. This is needed for some functions, such as `FString::Printf`, where the "%s" string format specifier expects a `TCHAR` instead of an `FString`
        ```cpp
        FString Str1 = TEXT("World");
        int32 Val1 = 123;
        FString Str2 = FString::Printf(TEXT("Hello, %s! You have %i points."), *Str1, Val1);
        ```
    * The `FChar` type provides a set of static utility functions for working with individual `TCHAR` characters
        ```cpp
        TCHAR Upper('A');
        TCHAR Lower = FChar::ToLower(Upper); // 'a'
        ```
        * The `FChar` type is defined as `TChar<TCHAR>` (as it is listed in the API)
<!--------------------------------------------------------------------------------------------------------------->
<h2 align="center"> Gameplay Classes: Objects, Actors, and Components </h2>

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

### UObjects and Garbage Collection
- UE uses the reflection system to implement a garbage collection system. In the garbage collector, there is a concept called the `root set`, which contains paths of references from objects to the root set, if there isn't a poth to some object, then it gets garbage collected
    ```cpp
    void CreateDoomedObject()
    {
        MyGCType* DoomedObject = NewObject<MyGCType>();
    }
    ```
    * The above function creates a new `UObject`, but does not store a pointer to it in any `UPROPERTY` or UE container, and it isn't a part of the root set. Eventually, the garbage collector will detect that this object is unreachable, and destroy it

<!---------------------------------------------------------------------------------------------------------------->
<h2 align="center"> Techniques </h2>

### Object/Actor Iterators
- Object Iterators are a very useful tool to iterate over all instances of a particular `UObject` type and its subclasses
    ```cpp
    // Will find ALL current UObject instances
    for (TObjectIterator<UObject> It; It; ++It)
    {
        UObject* CurrentObject = *It;
        UE_LOG(LogTemp, Log, TEXT("Found UObject named: %s"), *CurrentObject->GetName());
    }
    ```
    * Using Object Iterators may cause problems in Play in Editor
- When creating an Actor iterator, you need to give it a pointer to a `UWorld` instance. Many `UObject` classes, such as `APlayerController`, provide a `GetWorld` method to help you : 
    ```cpp
    APlayerController* MyPC = GetMyPlayerControllerFromSomewhere();
    UWorld* World = MyPC->GetWorld();

    // Like object iterators, you can provide a specific class to get only objects that are
    // or derive from that class
    for (TActorIterator<AEnemy> It(World); It; ++It)
    {
        // ...
    }
    ```
    * Actor iterators don't have the problem Object Iterators have, since it will only return objects being used by the current game world instance

<h2 align="center"> Glossary </h2>

| Action | Syntax |
| --- | --- |
| Get A Pointer to a Component of the current Class| `AActor::FindComponentByClass<USomeComponent>()`|
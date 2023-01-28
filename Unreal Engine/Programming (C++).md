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

### Containers
- Containers are classes whose primary function is to store collections of data. Each of these are dynamically sized, and so will grow to whatever size you need
- [TArray](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/TArrays)
    * `TArray` functions much like `std::vector` does, but offers a lot more functionality. Here Are some common operations : 
        ```cpp
        TArray<AActor*> ActorArray = GetActorArrayFromSomewhere();

        // Tells how many elements (AActors) are currently stored in ActorArray.
        int32 ArraySize = ActorArray.Num();

        // TArrays are 0-based (the first element will be at index 0)
        int32 Index = 0;
        // Attempts to retrieve an element at the given index
        AActor* FirstActor = ActorArray[Index];

        // Adds a new element to the end of the array
        AActor* NewActor = GetNewActor();
        ActorArray.Add(NewActor);

        // Adds an element to the end of the array only if it is not already in the array
        ActorArray.AddUnique(NewActor); // Won't change the array because NewActor was already added

        // Removes all instances of 'NewActor' from the array
        ActorArray.Remove(NewActor);

        // Removes the element at the specified index
        // Elements above the index will be shifted down by one to fill the empty space
        ActorArray.RemoveAt(Index);

        // More efficient version of 'RemoveAt', but does not maintain order of the elements
        ActorArray.RemoveAtSwap(Index);

        // Removes all elements in the array
        ActorArray.Empty();
        ```
    * `TArray` has the added benefit of having its elements garbage collected. This assurmes that the `TArray` stores `UObject`-derived pointers
- [TMap](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/TMap)
    * A `TMap` is a collection of key-value pairs, similar to `std::map`
    * You can use any type for the key, as long as it has a `GetTypeHash` function defined for it
    * Let us say you were creating a grid-based board game and needed to store and query what piece is on each square. A `TMap` would provide you with an easy way to do that. If the board is small and always the same size, there may be more efficient ways at going about this, but for this example, let's assume a larger board with relatively few pieces :
        ```cpp
        enum class EPieceType
        {
            King,
            Queen,
            Rook,
            Bishop,
            Knight,
            Pawn
        };

        struct FPiece
        {
            int32 PlayerId;
            EPieceType Type;
            FIntPoint Position;

            FPiece(int32 InPlayerId, EPieceType InType, FIntVector InPosition) :
                PlayerId(InPlayerId),
                Type(InType),
                Position(InPosition)
            {
            }
        };

        class FBoard
        {
        private:

            // Using a TMap, we can refer to each piece by its position
            TMap<FIntPoint, FPiece> Data;

        public:
            bool HasPieceAtPosition(FIntPoint Position)
            {
                return Data.Contains(Position);
            }
            FPiece GetPieceAtPosition(FIntPoint Position)
            {
                return Data[Position];
            }

            void AddNewPiece(int32 PlayerId, EPieceType Type, FIntPoint Position)
            {
                FPiece NewPiece(PlayerId, Type, Position);
                Data.Add(Position, NewPiece);
            }

            void MovePiece(FIntPoint OldPosition, FIntPoint NewPosition)
            {
                FPiece Piece = Data[OldPosition];
                Piece.Position = NewPosition;
                Data.Remove(OldPosition);
                Data.Add(NewPosition, Piece);
            }

            void RemovePieceAtPosition(FIntPoint Position)
            {
                Data.Remove(Position);
            }

            void ClearBoard()
            {
                Data.Empty();
            }
        };
        ```
- [TSet](https://docs.unrealengine.com/en-US/API/Runtime/Core/Containers/TSet)
    * A `TSet` stores a collection of unique values, similar to `std::set`
        ```cpp
        TSet<AActor*> ActorSet = GetActorSetFromSomewhere();

        int32 Size = ActorSet.Num();

        // Adds an element to the set, if the set does not already contain it
        AActor* NewActor = GetNewActor();
        ActorSet.Add(NewActor);

        // Check if an element is already contained by the set
        if (ActorSet.Contains(NewActor))
        {
            // ...
        }

        // Remove an element from the set
        ActorSet.Remove(NewActor);

        // Removes all elements from the set
        ActorSet.Empty();

        // Creates a TArray that contains the elements of your TSet
        TArray<AActor*> ActorArrayFromSet = ActorSet.Array();
        ```
- Container Iterators
    * Using iterators, you can loop through each elementof a container. Here is an example of what the iterator syntax looks like, using a `TSet`
        ```cpp
        void RemoveDeadEnemies(TSet<AEnemy*>& EnemySet)
        {
            // Start at the beginning of the set, and iterate to the end of the set
            for (auto EnemyIterator = EnemySet.CreateIterator(); EnemyIterator; ++EnemyIterator)
            {
                // The * operator gets the current element
                AEnemy* Enemy = *EnemyIterator;
                if (Enemy.Health == 0)
                {
                    // 'RemoveCurrent' is supported by TSets and TMaps
                    EnemyIterator.RemoveCurrent();
                }
            }
        }

        /************* Other supported operations you can use with iterators **************/

        // Moves the iterator back one element
        --EnemyIterator;

        // Moves the iterator forward/backward by some offset, where Offset is an integer
        EnemyIterator += Offset;
        EnemyIterator -= Offset;

        // Gets the index of the current element
        int32 Index = EnemyIterator.GetIndex();

        // Resets the iterator to the first element
        EnemyIterator.Reset();
        ```
- For-each Loop
    * Each container class also supports the "for each" style syntax to loop over elements. `TArray` and `TSet` return each element, whereas `TMap` returns a key-value pair
        ```cpp
        // TArray
        TArray<AActor*> ActorArray = GetArrayFromSomewhere();
        for (AActor* OneActor : ActorArray)
        {
            // ...
        }

        // TSet - Same as TArray
        TSet<AActor*> ActorSet = GetSetFromSomewhere();
        for (AActor* UniqueActor : ActorSet)
        {
            // ...
        }

        // TMap - Iterator returns a key-value pair
        TMap<FName, AActor*> NameToActorMap = GetMapFromSomewhere();
        for (auto& KVP : NameToActorMap)
        {
            FName Name = KVP.Key;
            AActor* Actor = KVP.Value;

            // ...
        }
        ```
        * You may need to add the appropriate notation if you use `auto`
- To use your own Types with TSet/TMap you have to define your own Hash Functions
    * The hash function would need to take a const pointer or reference to your type and return a `uint32`
        ```cpp
        class FMyClass
        {
            uint32 ExampleProperty1;
            uint32 ExampleProperty2;

            // Hash Function
            friend uint32 GetTypeHash(const FMyClass& MyClass)
            {
                // HashCombine is a utility function for combining two hash values.
                uint32 HashCode = HashCombine(MyClass.ExampleProperty1, MyClass.ExampleProperty2);
                return HashCode;
            }

            // For demonstration purposes, two objects that are equal
            // should always return the same hash code.
            bool operator==(const FMyClass& LHS, const FMyClass& RHS)
            {
                return LHS.ExampleProperty1 == RHS.ExampleProperty1
                    && LHS.ExampleProperty2 == RHS.ExampleProperty2;
            }
        };
        ```
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
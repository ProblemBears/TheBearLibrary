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

<h2 align="center"> Player Setup </h2>

### Create a Player Class
- It's convenient to always derive a Blueprint from a C++ class so that we could alter implementations in C++ later on
- Simply, click on the C++ folder in your Content Browser and then click the `Add` button &rarr; `New C++ Class`
    * In this case we want to make a new C++ class of a `Character` in order to start off with attributes such as walking

<h2 align="center"> Weapon Basics </h2>

### Line Tracing (Part 1)
1. We want to generate a `Line Trace` that is spawned from our player's camera to the "crosshair" location
    ```cpp
    void ASWeapon::Fire()
    {
        AActor* MyOwner = GetOwner();
        if (MyOwner)
        {
            // i
            FVector EyeLocation;
            FRotator EyeRotation;
            MyOwner->GetActorEyesViewPoint(EyeLocation, EyeRotation);

            FVector TraceEnd = EyeLocation + (EyeRotation.Vector() * 10000);

            FCollisionQueryParams QueryParams;
            QueryParams.AddIgnoredActor(MyOwner);
            QueryParams.AddIgnoredActor(this);
            QueryParams.bTraceComplex = true;

            FHitResult Hit;
            if( GetWorld()->LineTraceSingleByChannel(Hit, EyeLocation, TraceEnd, ECC_Visibility, QueryParams) )
            {
                //Blocking hit! Process damage
            }

            DrawDebugLine(GetWorld(), EyeLocation, TraceEnd, FColor::White, false, 1.0f, 0, 1.0f);
        }
    }
    ```
    1. The main function used to create a Line Trace is `GetWorld()->LineTraceSingleByChannel()`. With help from -
        1. `GetActorEyesViewPoint()` accepts two references that end up being modified. In this case, we want the function to return the `EyeLocation` not at the "nose of the mesh" but at the `CameraComp`'s location, so for this reason we **override** a virtual function used within this function (which we can see by going to the implementation at `Pawn.h`) in our `MyCharacter.cpp & .h`
            * The `EyeLocation` is used as the start location of our Trace
        2. 

<h2 align="center"> Glossary </h2>

| Action | Syntax |
| --- | --- |
| Get A Pointer to a Component of the current Class| `AActor::FindComponentByClass<USomeComponent>()`|
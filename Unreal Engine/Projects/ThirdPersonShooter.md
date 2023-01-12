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
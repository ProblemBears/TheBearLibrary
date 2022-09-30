<h1 style="text-align: center;"> Unreal Engine Animation </h1>

## Table of Contents
1. [Setting up a Third Person Character - Basics]()
2. [IK Rig Retargeting](#ik-rig-retargeting)

## Setting up a Third Person Character - Basics
1. Create a Blueprint with the **Character class**
    - A Character is a Pawn that comes with a [Character Movement Component](https://docs.unrealengine.com/4.27/en-US/InteractiveExperiences/Networking/CharacterMovementComponent/) which is used to traverse the world
    - In the Blueprint make sure that you change the Mesh Component to your desired Mesh and Animation Blueprint
    - In the Details Panel set the Transforms so that the Mesh is aligned w/ the Capsule & Arrow Components
    - Select the Mesh Component and add a Spring Arm Component as a child w/ the following tweaks:
        - Camera Settings > Use Pawn Control Rotation > &check;
        - Transform > Location > Z > 50
        - Camera > Socket Offset > Z > 30
    - Select the Spring Arm and add a Camera Component as a child
2. Create Input Key Mappings and Input Movement Events
    - Navigate to `Edit > Project Settings > Input`
    - Create Bindings for any Action and Axis Mappings you may need. Here are basic ones -
        | Action Mapping | Key Value || Axis Mapping | Key Value | Scale |
        | --- | --- | ---| --- | --- | --- |
        | Jump | Space Bar| | MoveForward | W | 1.0 |
        | Crouch | Left Ctrl | || S | -1.0 |
        | Sprint | Left shift | | MoveRight | D | 1.0 |
        |||| | A | -1.0 |
        |||| Turn | Mouse X | 1.0 |
        |||| LookUp | Mouse Y | -1.0 |
    -  Now you can use the Input Bindings you defined in your Character Blueprint's **Event Graph**. Here is an example what nodes should be placed to add movement based on input -
    ![TPS Animation Basics](../images/Unreal%20Engine/Animation/Setting%20Up%20a%20Third%20Person%20Character%20-%20Basics.png)

## Setting up a Third Person Character - Advanced Locomotion System

### Set Essential Variables
1. In you Basic Third Person Character's Blueprint in order to **free the mesh, and only use Input Rotation to move the SpringArm Camera**, tweak the following settings -
    - Class Defaults > Details Panel > Pawn > Use Controller Rotatation > &cross;
    - Components > Character Movement Component > Details Panel > Character Movement (Rotation Settings)
        - Use Controller Desired Rotation > &cross;
        - Orient Rotation to Movement > &cross;
        - (Optional) Max Walk Speed > 175
2. Create a **Blueprint Enumeration** called *LocomotionState* 
    - Add New Enumerators for every state needed. Some basic ones are : Idle, Walk, Run
3. Create an Animation Blueprint (using the appropriate Skeleton)
    - In the Event Graph do the following
        1. Initialization Event
            - Use `Cast` to convert the Pawn output of `TryGetPawnOwner` to our Character Blueprint.
            - Using this cast, we can store the following variables : **CharacterMovement** and the **Character**
            ![Initialization](../images/Unreal%20Engine/Animation/ThirdPerson%20AnimBP%20Initialization.png)
        2. Update Event
            - Create/Plugin a Function called *SetupEssentialData*. This will be in charge of setting up the values of the following variables - **Velocity, Acceleration, Speed, Max Speed, Direction**
            ![SetUpEssentialData](../images/Unreal%20Engine/Animation/SetupEssentialData.png)
            - Create/Plugin a Function called *FindMovementState*. This will be used to set the State of our LocomotionState (of the type LocomotionState which is the Enumeration we previously created) based on some of the previous variables we defined.
            ![FindMovementState](../images/Unreal%20Engine/Animation/FindMovementState.png)
            - (Optional) At this stage you can then plug in a `Print String` where you input your `Locomotion State Veriable`, in order to see the Locomotion State changing when you move around in Play Mode

### Set the Start Angle (for playing animations based on direction)
- After the *FindMovementState* Function in our **Update Event**, if our current state is Walking or Running, then, only for the first change to either state do we set the following variables - **Start Rotation, Primary Rotation, Secondary Rotation, Start Angle**. Otherwise, the state is idle or was re-converted to idle so we reset the `Do Once`
![Set the Start Angle](../images/Unreal%20Engine/Animation/SetTheStartAngle.png)
- (Optional) In order to see the results in Play Mode while moving. You can put a `Sequencer` infront of the `Update Event` node, with it's first execution being everything we have so far, and the second execution being a `Print String` that takes in the `Start Angle Variable`

### Create and Configure a State Machine
1. Navigate to your Animation Blueprint's **Animgraph** and create a **State Machine** called *Locomotion*
    - You can create **States** and double-click them to be able to drag in the appropriate Animation Sequence asset
2. Create the `Idle State`
    - **Parent** - The Entry Point
    - **Condition** - None
    - **Animation** - *Idle*
3. Create the `Walking State`
    - **Parent** - Idle State
    - **Condition** - If the LocomotionState Enum variable equals Walk (Make sure you place an arrow back to `IdleState` with a similar condition, except the Locomotion State equals Idle)
    - **Animation** - It's a State Machine called *WalkingSM* within this State where we define the following states - 
        - From the `Entry` we go to an `Empty State` that in turn goes to the following states depending on conditions based on the StartAngle variable that was previously defined -
            - `WalkFwdStart180_L` - If StartAngle is `InRange[-180, -135)`
            - `WalkFwdStart90_L` - If StartAngle is `InRange[-135, -45)`
            - `WalkFwdStart` - Can Enter Transition is ticked to true also set this Priority Order to 2 as opposed to every other's 1
            - `WalkFwdStart90_R` - If StartAngle is `InRange(45, 135]`
            - `WalkFwdStart180_R` - If StartAngle is `InRange(135, 180]`
        - Then from these 5 states we go to the `WalkFwdLoop` state
            - Drag + LMB to select all conditions to this state and tick the radio button in `Transition > Automatic Rule Based on Sequence`
    - **Note** - Make sure all of your Walking Animation Sequences (except for your Walking Forward Loop at the end) are set to 
        - Settings > Loop Animation > &cross;
        - Settings > Play Rate > Expose as Pin > and feed **a new variable called WalkStartPlayRate** into it with some arbitrary value like 1.3
- At this stage, if you test it out in Play Mode, your character should play the correct Idle to Walking animation based on what direction you walk, but it still shouldn't have the correct rotation

### Add Character Rotation
1. In your Animation Blueprint create a new function called 

## IK Rig Retargeting
1. IK Rigs
    1. Create two `IK Rig` assets
        - One for the source skeletal mesh and the other for the target skeletal mesh
    2. In both IK Rigs define the root by doing - `Hierarchy > {RMB} > Set Retarget Root`
        - The Root Bone is usually the **Hip**
    3. Then in the  IK Retargeting Panel for both IK Rigs create chains for every major limb you want to retarget, with minor limbs like fingers being optional.
    ![IK Rigs](../images/Unreal%20Engine/Animation/IK%20Rigs.png)
2. IK Retargeter
    1. Create and specify the source SK Mesh by doing - `Add > Animation > IK Retargeter`
    2. Add the target mesh
    3. You can sellect animations from the asset browser panel to preview animations
    4. Depending on the Rest Poses of the characters being retargeted, it may be necessary to edit this pose in the form of a `Retarget Pose`. This can be done by creating a `New Pose` from the tool bar controls and editing it to match the source
    5. Export
3. Batch Retargeting
    - Now you can even Right Click on appropriate assets and Retarget from the asset browser. It opens up a dialog that lets you choose youre IK Retargeter

<h1 style="text-align: center;"> Unreal Engine Rigging </h1>

## Table of Contents
1. [IK Rig Retargeting]()

## 1 - IK Rig Retargeting
1. IK Rigs
    1. Create two `IK Rig` assets
        - One for the source skeletal mesh and the other for the target skeletal mesh
    2. In both IK Rigs define the root by doing - `Hierarchy > {RMB} > Set Retarget Root`
        - The Root Bone is usually the **Hip**
    3. Then in the  IK Retargeting Panel for both IK Rigs create chains for every major limb you want to retarget, with minor limbs like fingers being optional.
    ![IK Rigs](../images/Unreal%20Engine/Rigging/1%20-%20IK%20Rigs.png)
2. IK Retargeter
    1. Create and specify the source SK Mesh by doing - `Add > Animation > IK Retargeter`
    2. Add the target mesh
    3. You can sellect animations from the asset browser panel to preview animations
    4. Depending on the Rest Poses of the characters being retargeted, it may be necessary to edit this pose in the form of a `Retarget Pose`. This can be done by creating a `New Pose` from the tool bar controls and editing it to match the source
    5. Export
3. Batch Retargeting
    - Now you can even Right Click on appropriate assets and Retarget from the asset browser. It opens up a dialog that lets you choose youre IK Retargeter

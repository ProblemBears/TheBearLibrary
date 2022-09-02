<h1 style="text-align: center;"> Unreal Engine Shaders </h1>

## Table of Contents
1. [What is a Shader?](#1---what-is-a-shader)
2. [Basics of PBR](#2---basics-of-pbr)
- [Node Glossary](#node-glossary)

## 1 - What is a Shader?
- What is a shader ? -
    - Code that controls the color of each pixel on the screen
    - Usually runs on the GPU
    - Unreal Engine's **Material Editor** is a node based interface to create shaders. The engine writes the shader code for us under the hood based on the nodes we wire together.

- Shaders can do lots of things -
    - Shaders should only manipulate the **Surface Properties**. Such as :
        - Base Color
        - Reflectance
        - Surface Normal
        - Metallicity
        - Roughness
    - The game engine handles the **Light Properties**. Such as :
        - Diffuse Light
        - Light Reflections
        - Shadows
        - Ambient Occlusion
        - Environment Reflections
        - Ambient Light
- Introduction to Unreal Engine Materials -

    |What| How|
    |---|---|
    | **To create a material** | *Content Drawer > Right-Click > Material* |
    | **To open the material editor** | *Double-click a material* |
    | **To connect nodes** | RMB-Drag from a Node's output to another node's input |
    | **To remove connections** | Alt + LMB on a Node's Input |
    
    - The naming convention for materials is | *M_SomethingDescriptive*

## 2 - Basics of PBR
- What is Physically Based Rendering (PBR)?
    - It imitates the actual behavior of light
    - Makes graphics look more real

- Two main components to PBR -
    - **Light Properties** (mainly controlled by the game engine)-
        - Direct Diffuse
        - Indirect Diffuse
        - Direct Specular
        - Indirect Specular
        - Shadows
        - Ambient Occlusion
    - **Surface Properties** (mainly controlled by shader devs) -
        - Base Color
        - Normal
        - Specular
        - Roughness
        - Metallic
    - In order for materials to look as realistic as possible, both of these components need to be correct.
    - We'll mainly work with surface properties but it's important to see what the engine does with light properties first.

- Light Categories
    - Light comes from **two** sources - 
        1. **Direct light sources** - where light is shining directly from the source to the surface in a single direction
        2. **Indirect light sources** - where light is bouncing around the environment and coming in from all directions before hitting the surface
    - After light hits the surface. There are **two** types of surface interactions -
        1. **Specular light** - leaves the surface in one main focused direction
        2. **Diffuse Light** - is scattered and leaves the surface in all directions
        - There are more outgoing light behaviours, but these are the main ones

- If we combine the incoming and outgoing behaviors (Light Categories). We end up with **Four Light Types** -
    ![Four Light Types](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2%20-%20Four%20Light%20Types.png)
    - **Direct Diffuse** - Light coming directly from the source and scattering in all directions
        - Computed with simple math in the shader
        ![Direct Diffuse](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2a%20-%20Direct%20Diffuse.png)
    - **Direct Specular** - Light coming directly from the source and being reflected in a focused direction
        - Computed with simple math in the shader
        ![Direct Specular](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2b%20-%20Direct%20Specular.png)
    - **Indirect Diffuse** - Light coming from the ambient environment and getting scattered by the surface
        - The most complex to calculate
        - Computed with the engine's global illumination solution - often rendered offline and baked into light maps
        ![Indirect Diffuse](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2c%20-%20Indirect%20Diffuse.png)
    - **Indirect Specular** - Light coming from the ambient environment and being reflected in a focused direction
        - Also complex to calculate
        - Most game engines provide several different ways to create this effect such as **reflection probes**, **planar reflections**, **SSR**, or **ray tracing**
        ![Indirect Specular](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2d-%20Indirect%20Specular.png)
    - All lighting together -
        ![All Together](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2e%20-%20All%20Together.png)
    - Now we can talk about **Surface Properties**...




## Node Glossary
| Node | Description|
|---|---|
| Texture Sample | Contains a reference to a Texture Map |
| Constant | A single number ( 0 = Black and 1 = White) |
| Constant3Vector | A Vector that can signify a color via RGB representation
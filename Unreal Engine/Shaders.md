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
- Light Properties
    - Light Categories
        - Light comes from **two** sources - 
            1. **Direct light sources** - where light is shining directly from the source to the surface in a single direction
            2. **Indirect light sources** - where light is bouncing around the environment and coming in from all directions before hitting the surface
        - After light hits the surface. There are **two** types of surface interactions -
            1. **Specular light** - leaves the surface in one main focused direction
            2. **Diffuse Light** - is scattered and leaves the surface in all directions
            - There are more outgoing light behaviours, but these are the main ones

    - If we combine the incoming and outgoing behaviors (Light Categories). We end up with **Four Light Types** -

        <div align="center">
        <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2%20-%20Four%20Light%20Types.png" width="300">
        </div>

        - **Direct Diffuse** - Light coming directly from the source and scattering in all directions
            - Computed with simple math in the shader'

            <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2a%20-%20Direct%20Diffuse.png" width="300">

        - **Direct Specular** - Light coming directly from the source and being reflected in a focused direction
            - Computed with simple math in the shader

            <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2b%20-%20Direct%20Specular.png" width="300">

        - **Indirect Diffuse** - Light coming from the ambient environment and getting scattered by the surface
            - The most complex to calculate
            - Computed with the engine's global illumination solution - often rendered offline and baked into light maps

            <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2c%20-%20Indirect%20Diffuse.png" width="300">

        - **Indirect Specular** - Light coming from the ambient environment and being reflected in a focused direction
            - Also complex to calculate
            - Most game engines provide several different ways to create this effect such as **reflection probes**, **planar reflections**, **SSR**, or **ray tracing**

            <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2d-%20Indirect%20Specular.png" width="300">

        - All lighting together -
            <div align="center">
            <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/2e%20-%20All%20Together.png" width="300">
            </div>

        - Now we can talk about **Surface Properties**...
- Surface Properties
    - Base Color
        - Defines the diffuse color of the surface
        - Out of range values will not light correctly so staying in range is critical
            - Real-world materials are not darker than 20 or brighter than 240 sRGB
            - Rough surfaces have a higher minimum - 50 sRGB
        - Do not include any lighting or shadows
        - Base color textures should look very flat
        - Use real-world measurements or captured data for best results

        ![Base Color](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/3a%20-%20Base%20Color.png)
        <div align="center">
        <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/3%20-%20Base%20Color.png" width="300" >
        </div>

    - Normals
        - Defines the shape of the surface
        - Each pixel represents a vector that indicates the direction that the surface is facing
        - even when the mesh is perfectly flat, a normal map can make it appear bumpy
        - Because they represent vector data, normal maps should never be hand-authored
            - A commonly used method is baking a normal map to a low res model from a high res model

        <div align="center">
        <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/4%20-%20Normal.png" width="300" >
        </div>

    - Specular
        - A multiplier for the direct and indirect specular lighting
        - Defines the reflectivity when looking straight at the surface
        - Non-metal surfaces reflect about 4% of the light
            - 0.5 represents 4% reflection (99.9% of times leave it at this value)
            - 1.0 represents 8% reflection (too high for most objects)
        - At glancing angles, all surfaces are 100% reflective
            - **Fresnel** - a formula that increases the reflectivity as the surface goes from pointing straight at you to being perpendicular to your view. This is handled by the engine.
        - Only use darker shades to mask areas that should not be reflective - like crevices

        ![Specular](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/5a%20-%20Specular.png)
        <div align="center">
        <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/5%20-%20Specular.png" width="300" >
        </div>

    - Roughness
        - Represents the roughness of the surface at a microscopic scale
        - White is rough and black is smooth
            - Smooth = tight, sharp reflections
            - Rough = blurry, diffused reflections
        - Controls the "focus" of the reflections
        - There are no technical constraints - completely artistic choice
            - Artists can use this map to define the "character" of the surface and show its history
            - Consider where a surface has been polished smooth, scuffed, or aged
        
        ![Roughness](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/6a%20-%20Roughness.png)
        <div align="center">
        <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/6%20-%20Roughness.png" width="300" >
        </div>

    - Metallic
        - When you use the root node in Unreal's Material Editor it's actually a combination of **two shaders in one**.
        You have a shader that defines **metal** objects and a shader that defines **nonmetal** objects  
        The value you pass into the Metallic Socket picks if you use the Metal or Nonmetal Shader
            - Metallic Socket = 0 -
                - What you pass to the Base Color Socket goes into the Diffuse of the object
                - What you pass to the Specular Socket defines the Reflectance of the object between 0-8%
            - Metallic Socket = 1 -
                - Your Diffuse is always black
                - What you pass into the Base Color socket is now your Reflectance from 0-100%
                - What you pass into the Specular Socket is completely ignored
        - Grayscale values are weird. Bes to use solid white or black
            - So in the Metallic Map, anything white is considered metal
            - In the Cobblestone examples we have no need for metal so the map is all black
        - When metallic is white, be sure to use correct base color values for metal
            - There's no such thing as dark metal. All metals are 180 sRGB or brighter
        
        ![Metallic](../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/7a%20-%20Metallic.png)
        <div align="center">
        <img src="../images/Unreal%20Engine/Shaders/2%20-%20Basics%20of%20PBR/7%20-%20Metallic.png" width="400" >
        </div>

    - Ambient Occlusion
        - A multiplier with the Indirect Diffuse so that the object doesn't get diffuse reflections in areas they don't belong
        - It's only applied if there is a Global Illumination Solution baked

- Conclusion
    - The engine handles the lighting
    - It's up to the shader artist to pass correct values into Material Sockets to get physically accurate materials


## Node Glossary
| Node | Description|
|---|---|
| Texture Sample | Contains a reference to a Texture Map |
| Constant | A single number ( 0 = Black and 1 = White) |
| Constant3Vector | A Vector that can signify a color via RGB representation
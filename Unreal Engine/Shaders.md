<h1 style="text-align: center;"> Unreal Engine Shaders </h1>

## Table of Contents
1. [What is a Shader?](#1---what-is-a-shader)
2. [Basics of PBR](#2---basics-of-pbr)
3. [What are Datatypes?](#3---what-are-data-types)
4. [Distortion Shader](#4---distortion-shader)
5. [Flipbook Animation](#5---flipbook-animation)
6. [Environment Blending](#6---environment-blending)
7. [Shader Performance Optimization](#7---shader-performance-optimization)
8. [Bump Offset and Parallax Occlusion Mapping](#8---bump-offset-and-parallax-occlusion-mapping)

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

## 3 - What are Data Types?
- Float
    - Scalar values
    - Gets duplicated in RGBa Channels and math operations
- Float2
    - Good for storing UV coordinates
- Float3
    - Good for color
- Float4
    - Good for storing colors with an Alpha channel
- Math operations are only valid among the same data type or a singular Float
    - Float + Float4 and Float3 + Float 3 &check;
    - Float3 + Float4 &cross;
- If you plug in a Float4 to an input that only takes up to Float3, Unreal Engine simply ignores the fourth channel

## 4 - Distortion Shader
1. Animated Scrolling
    - `TexCoord` + `Float2(0, 0.1)` &rarr; `Texture Sample(RGB)` &rarr; `Root[Base Color, Emissive color]` : Shifts the UVs down(positive) by 0.1.
    - If we replace the `Float2` with `Time`, which is a node that holds a **Float1 that increases linearly as long as the program runs**, then the "Shifting" effect would be animated moving diagonally in the positive directions
        - We can slow down Time by multiplying by something like a `Float2(0.5,0)` which signifies slowing movement of the U-direction by half and stopping movement of the V-direction completely
2. To offset UV coordintes by a different amount for every pixel (as opposed to uniform movement)
    - We make a noise texture
        - In Photoshop we can use the Render Clouds function with different RGB channels
    - Append the R & G channels of that texture to make a **Float2** that we can add with our `TexCoord`
        - This would result in an extremely noisy texture. We can minimize this effect by scaling it down using `Multiply(float2, 0.03)` **and then** adding it with the `TexCoord`
    - To animate our Noisy Offset Texture we can use the exact same scroll effect on this texture instead of the one connected to the root (our "Image Texture" that we're applying the "Offset Texture" to)
3. Finally, we don't want the animation of the Noisy Texture to look repetitive
    - We can duplicate the Scrolling noisy effect except we make it scroll in a different direction at a different speed rate
    - Then we add both of them
- Use Cases :
    - Heat Haze
    - Rippling water

<div align="center">
<img src="../images/Unreal%20Engine/Shaders/4%20-%20Distortion.png">
</div>

## 5 - Flipbook Animation
- What's a flipbook effect?
    - An animated texture map usually applied to particle billboards
    - VFX artists us them to represent very complex effects as cheaply as possible
- Creating a flipbook effect -
    - Create (or film) a complex visual effect or animation
    - Process each frame so that it's square
    - Arrange the frames in a texture atlas
    - Use a shader the plays back the frames
- Unreal Engine has a built-in `FlipBook` node that you need only provide the following things:
    - A `TexCoord` to `FlipBook[UVs]`
    - `Constants` to define `FlipBook: [# of Rows] & [# of Cols]`
    - `Time` to `FlipBook[Animation Phase]`. By Default this is 30FPS.
        - We can scale the time by multiplying with a Constant (by 0.8 would would make it 24FPS)
    ![UE Flipbook](../images/Unreal%20Engine/Shaders/5-%20Flipbook%20(UE).png)
- To implement a Flipbook effect without UE's built-in node:
    1. Get one frame of you FlipBook Texture
        - ( `Const(1)` / `Const2(col, rows)` ) * (`TexCoord`) &rarr; `Texture Sample[UVs]`
    2. Now we have to adjust the UVs so they slowly adjust from one frame to the next
        - Configure a `Time` node to ba **period** which increments to a limit and then goes back to 0
        - Multiply by a `Constant(FPS)` that signifies the amount of frames per second
        - Use `Floor` so that our 'shifting effect' stays at a certain digit, until `Time` reaches the next digit. Otherwise, it would be a scrolling animation
        - If we add this directly to the `TexCoord`  then the 'flipbook' would step through diagonally
    3. We'll use (ii) to increment the columns but to increment the row we need to add the following
        - We need a reference of our `Const2(cols,rows)` again
        - We need to extract the amount U from the `Const2` with a `ComponentMask`. We'll use this to divide our previous time by the amount of columns, so that when we complete the last column, the V increments.
        - We make sure that we also `Floor` this value
    4. Finally, we append both (ii) and (iii) and add it to the `TexCoord` which will be multiplied to our (i)
        - You can think of the append as a dynamic vector that changes over time
    ![My Flipbook](../images/Unreal%20Engine/Shaders/5%20-%20Flipbook%20(Implementation).png)

## 6 - Environment Blending
- **Scenario -**
    - Your environment artists comes to you and says "I'm tired of making rocks! We have levels in snow, deserts, jungles and I have to make different variations of rocks for each. Can you create a shader that applies these effects to my existing rocks?
- Implementation
    - The key to this is plugging in each Texture Map to you want to blend together to a `Lerp`
        - So blend Color Maps using one `Lerp` and their Normal Maps using another
    - Then we can manipulate the alpha value.   
        - In the case of blending Moss with a Rock, this would not look right. We would want Moss only at the top slightly trickling down, as opposed to it looking like a transparent overlay. Therefore, to add this effect we do the following:
            1. Use the `VertexNormalWS` to get the global direction of each vertex normal.
                - These normals are normalized so their limits are in the range [-1, 1]
            2. We only want the **up** and **down** values of the normals, hence since Unreal Egnine's Coordinate System is Z-up, we isolate the B component of the vertex normals using a `ComponentMask(-, -, B)`
                - We can do math here before feeding it to (c). `Power` would focus the "blend margin" towards the top, while adding by 1 would create a perfect horizon in the middle.
            3. Our `Lerp`'s Alpha parameter accepts values in the range [0, 1]. If it goes out of these bounds then ther interpolation may look weird. Therefore, we clamp the values received from the `ComponentMask` (since they may be negative) with `Clamp{min=0 & max=1}`
            4. Alternatively, we can throw away the first step (1) by deleting `Vertex Normals` and instead convert the tangents of our stone's Normal Map to World Space by plugging in the **RGB** to the `TransformVector{Tangenet Space -> World Space}`. **This gives it an extremely good blending effect**.
                - Also, even  if the rock is rotated, the effect still stays at the top!
    ![Environment Blending](../images/Unreal%20Engine/Shaders/6%20-%20Environment%20Blending.png)
- Conclusion
    - Now all it takes to add snow or sand to a rock, is a simple texture swap

## 7 - Shader Performance Optimization
- Three methods to calculate shader performance :
    1. Shader Complexity View Mode
        - Viewport &rarr; ViewMode &rarr; Optomization ViewMode &rarr; Shader Complexity `Alt+8`
        - Least best
    2. Shader Instruction Count
        - You can see this in your stats
        - Medium best
    3. Test on your target platform
        - Every platform has different performance capabilties
        - Most accurate, but also hardest
- Four methods to optimize your shaders
    1. Get rid of anything you're not using
    2. Refactor math formulas
    3. "Pipelining" - Combining components into float4s to do less math
        - In our Distortion Shader we could have a Vector4 take in both Vec2s and do the same calulcations, excpept later on we filter them back to individual Vec2s
    4. Texture Channel Packing
        - Sampling a lot of textures is expensive
        - Create an AmbientOcclusion Specular Metal Roughness texture (ASMR) by packing them into individual channels
        - This can be done in software like Photoshop

## 8 - Bump Offset and Parallax Occlusion Mapping
- Two methods of making normal mapping look more realistic
    1. Offset Mapping (basic effect, simplest)
        1. Import a Height Map
        2. Connect the R Channel of (a) to `BumpOffset`
        3. Connect the output of (b) to the "UVs" input of all your `Texture Samples`
        - BumpOffset shifts the white areas of your HeightMap based on the camera's position
        - IF you hadn't applied this, then your Normal Map would've looked flatter than it should
    ![Offset Mapping](../images/Unreal%20Engine/Shaders/8%20-%20Offset%20Mapping.png)
    2. Parallax Occlusion Mapping (best looking effect, most expensive)
        1. Plug in a `Texture Object` into a `ParallaxOcclusionMapping(Heightmap Texture (T2d))` (we need a Texture Object and not Sample, because the `ParallaxOcclusionMapping` does the sampling multiple times). Then define a Height Ratio and a HeightMap Channel.
        2. Connect the "Parallax Uvs" Output to every Texture Sample.
        - ParalaxOcclusionMapping does ray tracing
        - Increase Min Steps and Max Steps for better detail
    ![Parallax Occlusion Mapping](../images/Unreal%20Engine/Shaders/8%20-%20Parallax%20Occlusion%20Mapping.png)


## Node Glossary
| Node | Description|
|---|---|
| Texture Sample | Contains a reference to a Texture Map |
| Constant | A single number ( 0 = Black and 1 = White) | 
| Constant3Vector | A Vector that can signify a color via RGB representation |
| Mask | Filters the input inorder to output specific channels |
| Split Components | Splits the input into individual channels |
| Append | Stick two floats together |
| AppendMany | Stick more than two nodes together |
| Swizzle | Rearranges the order of a Float3 input |
| FlipBook | Allows you to use FlipBook textures |
| Frac | Throws away the left side of the decimal; leaving the right |
| Lerp | Blends between A and B depending on the Alpha value |
| Clamp | Limits input to a Lower and Outer bound |
| BumpOffset | Requires a Height Map in its Height input. Shifts based on black & white and the camera position |
| Texture Object | Contains the location of a texture without sampling |
| ParallaxOcclusionMapping | Used for better normals (See 8) |
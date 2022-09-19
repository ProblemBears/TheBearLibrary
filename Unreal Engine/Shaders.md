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
9. [Texture Compression and Settings](#9---texture-compression-and-settings)
10. [Cloth Shading](#10---cloth-shading)
11. [Volumetric Ice Shader](#11---volumetric-ice-shader)
12. [Rustling Leaves Shader](#12---rustling-leaves-shader)
13. [Rain Wetness Shader](#13---rain-wetness-shader)
14. [Rain Drops Shader](#14---rain-drops-shader)
15. [Rain Drip Shader](#15---rain-drip-shader)
16. [Rain Ripples Shader](#16---rain-ripples-shader)
17. [Rain Puddles Shader](#17---rain-puddle-shader)
18. [Complete Rain Shader](#18---complete-rain-shader)

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

## 9 - Texture Compression and Settings
- Texture Compression settings are important to get the right balance between performance and detail
- To see texture details *double-click* the Texture and look at it's **Details panel**
    - **Method :** Streamed means the engine loads it when a certain object gets near
    - **Format :** DXT1 is one of the best compression formats
    - **Number of Mips :** When you import a texture into UE it creates a **MipMap chain** which is copies of the texture but at lower resolutions (so from something like 4k to 4x4)
- In the details panel we can also change compression settings
    - Compress without Alpha: &check; - If you know you won't use the alpha channel for this texture
    - Compression Settings: 
        - For most use DXT1
        - For normals use normals, for asmr textures and height maps use mask
        - For height maps you can also use Grayscale for more quality since it stores a single channel
    - Have sRGB ticked mainly only for Color Maps
    - These changes would be visible in your Texture Node previews

## 10 - Cloth Shading
- If we look at images of cloth we see that lighting either effects the edges, or the middle
    - Silk is bright in the middle and dark on the edges
    - Cotton is brighter on the edges and darker in the middle
- When you see something like this you should think of `Fresnel` although in this case we won't use it because we want to customize it more.
- The following shader effect is based on Uncharted 2's cloth shading:
    1. A basic fresnel can be created by doing the `DotProduct` of `CameraVectorWS` and `PixelNormalWS`
        - Because, if the surface normal is pointing away from the camera it gives us a darker shade (the edges) and if it's pointing directly at us a lighter shade (the center)
    2. If we do `1-x` it inverts the dark and light areas.
    3. We can insert a `Power` to control the fall off (the higher the power the sharper the fall off) 
    4. After the fall off, we can use `Multiply` to tweak the brightness (or darkness) of the edges
    5. Duplicate all of this to and get rid of the inversion. We'll `Add` one that's inverted and one that's not inverted, in order to make the effect bright on the edges and center, with darkness in between.
        - The inversion is the outer fresnel
        - the non inversion is the inner fresnel
        - With the four controls (multiplies and powers) we can create all different kinds of cloth effects (like satin, silk, denim) just by adjusting the falloff and brightness
    6. Lastly add your Color and Normal Map (possibly scale their UVs with `Multiply` for more "grainy" cloth) and `Multiply` the Color Map with the Fresnel Effect (The Normal Map simply goes to the `Root[Normal]`)
        - We can also replace (i)'s `PixelNormalWS` with our Normal Map's RGB output
    ![Cloth Shading](../images/Unreal%20Engine/Shaders/10%20-%20Cloth%20Shading.png)

## 11 - Volumetric Ice Shader
- A **volumetric effect** simulates the appearance of objects inside other objects (like  ice)
- To start, we need a grayscale Perlin Noise texture. We wantt the noise to move around as if it were under the surface instead of on the surface.
    - Think of it as a sub-surface below the surface, that "shits" relative to our Camera vector.
    - A vector that has this "shifting" effect is a **Reflection Vector** but it reflects going going upwards, therefore, for since our sub surface is below the surface, we need to inver the reflection vector to create a **Transmission Vector**
    <div align="center">
        <img src="../images/Unreal%20Engine/Shaders/11%20-%20Transmission%20Vector.png" width="400">
    </div>
- To create this shader :
    1. To yield a "shifting" effect for the texture
        - `CameraVector`&rarr;`TransformVector{world->tangenet}` & `Float3(0,0,1)` plugged into `CustomReflectionVector[CameraVector,Normal]` respectively. Since it yield's a Float3 we also filter it with a `ComponentMask{R,G}`
    2. The texure would still be way behind the surface, so we need controls
        - To control the offset we wire the `CustomReflectionVector` to a `Mask`&rarr;`Abs` and then `Divide` it with a `Float1` (as the numerator; controls the offset). Multiply it by the masked Reflection Vector.
            - The lower the numerator, the slower the shifting effect
        - To control the amount of offset based on the resolution of the texture we do the same as above, except with the resolution of the texture. Multiply it by the previous Multiply.
        - `Add` this to the `TexCoord` before feeding it to the `Texture Sample`
    3. Still, the surface would still look smooth, we should give it blemishes.
        -We can do this by replacing the straight normal `Float(0,0,1)` with a Normal Map
    4. To offset some pixels more than others we can use the same grayscale Perlin Noise texture multiplied by 50 in place of our offset control
    ![Volumetric Ice Shader](../images/Unreal%20Engine/Shaders/11%20-%20Volumetric%20Ice%20shader.png)

## 12 - Rustling Leaves Shader
- If we were to give individual polygons to every leaf in a tree it would be very expensive. Therefore, we should just make one square polygon with a texture that has hundreds of leaves so we can make a shader that animates every individual leaf in that texture
- To create the shader :
    1. We need smooth alternating movement to simulate a breeze
        - We can already do a scrolling effect by adding `Time` and scaling it's speed.
        - Now we need to make sure the scroll alternates back and forth by connecting it to a `Sine` which stays in the range [-1, 1]. We can also scale this to stay at a lower range.
    2. Now we have to isolate individual leaves to make them wiggle in different amounts
        - We can do this by painting RGB where certain leaves are in a new Mask Texture (it doesn't have to be high quality or perfectly positioned)
        - Then we tweak the single channel time scale and sine scale in our (i) to three channels which we can multiply by our Mask Texture.
        - Finally, we split the components and add them before adding them to the `TexCoord`
    3. To give the leaves more of a 3D feel, we can add the following to our `Root[Normal]`
        - Scale the `Sine` range and multiply it by the Mask Texture before adding it to the `VertexNormalWS`c
    ![Alternating Leaves](../images/Unreal%20Engine/Shaders/12%20-%20Rustling%20Leaves%20Shader.png)

## 13 - Rain Wetness Shader
- What makes a surface look wet?
    - **Roughness and Specularity**
        - The **roughness** is low (a 0.007 is a good value)
        - The **specular** is low-ish (0.3 is good)
    - If the surface is **Porous**
        - it absorbs water in it's surface which makes it saturated and darker
- Rain covers a lot when it pours, therefore we should make this shader effect into a **Material Function** as follows:
    1. Create `Inputs` and `Outputs` for Roughness, Specularity, and Base Color. As well as `Inputs` for the controller of our wetness effect, **WetMask** and **Porousness**.
    2. If a material is wet it's **specularity** and **roughness** must `Lerp` towards wet values such as `[0.007, 0.3]` respectively. The interpolation is controlled by the `Input(WetMask)`
    3. If a material is not only wet, but also **porous** then we have to make it's colors more vibrant by **saturating** the base color and **darkening** it. The lerp is controlled by multiplying both the `Input(WetMask)` & `Input(Porousness)` and making sure to clamp it to the range [0, 1] with the `Saturation` node.
        - It's important to note `Saturation` is just a clamp function that has nothing to do with actually manipulating color saturation. The actual color saturation is done by feeding a negative to `Unsaturation` which is a node that converts colors to grayscales if positive.
    ![Wetness Function](../images/Unreal%20Engine/Shaders/13%20-%20Wetness%20Function.png)
- Finally, we make use of the Material Function by using it on a Material, where we feed it the appropriate inputs to define whether we want it to look wet or not
    - In the following case, we use it on a brick wall material from the UE Starting Content:
    ![Applied to a Material](../images/Unreal%20Engine/Shaders/13%20-%20Rain%20Wetness%20Shader.png)

## 14 - Rain Drops Shader
- We need a RainDrop Texture
    - RG Channels -  are used to create the normals of the drops
    - B Channel - will be used as a **temporal offset**, where each drop will appear at slightly different times
    - A Channel - Black signifies static drops, and White signifies dots that are animated in the surface
- The Rain Drop Shader will be created by extracting each of the channels described above, and applying nodes that create their intended effect -
    1. **RG Normals** -
        - When we extract the normals with `Append` we want to convert the UV Normals from the range [0, 1] to the range [-1, 1]. We can do this by `Multiply`ing by 2 and then subtracting by 1.
        - The last step should be appending a Z to make it a 3D Normal. We append 1 in this case.
    2. **B Animated Mask** -
        - We use a scaled `Time` and subtract it from the B channel. After which, we use `Frac` so that instead of being infinitely decreasing, our values is a decreasing and alternating decimal.
    3. **A Static Rain Drop Mask** -
        - We invert blackish colors to white, and also the reverse.
    4. **Rain only effects the top** -
        - It doesn't make sense for rain to drop from below, so we clamp the vertex normals using saturate, therefore, if some vector normal doesn't have a slight value in the positive-z direction, then it will not appear
        - We can also multiply by a control here that would effect whether it's raining or not.
    ![Rain Drops Shader](../images/Unreal%20Engine/Shaders/14%20-%20Rain%20Drops%20Shader.png)

## 15 - Rain Drip Shader
- We create a texture similarly packed as **#14**, for Rain Drips. In Photoshop we squiggle lines and add the following to each channel :
    - RG Channels - the normal map for the squigles
    - B - Mask for where the drips are gonna happen
    - A - Temporal Offset Mask
- The Rain Drip Shader will be created ad follows:
    1. World Space Project the packed texture
    2. Apply the animated mask by -
        - Importing a gray scale texture that gradients from white to black. This will be the "line" we scroll through the previous texture.
    3. A simple scrolling animation isn't what we want because it would simulate all rain drops dripping at the same time. Therefore, we need to apply our packed temporal offset.
    4. If a surface is permeable, the water can soak in so it slows down (cloth, stone). Whereas, if it's impermeable, (like metal) then it goes fast.
        - We want to append the B Channel (where the drips is happenign) with arbitrary `Float2` values for speed. Then we can `Lerp` between those values using the Alpha Channel (temporal offset) 
    5. We can create normals from the RGB channel of our first texture
        - Firstly convert it to a [-1, 1] range and append a 1 to make it a `Float3` (as normals should be)
        - Don't forget to multiply by your animated mask, in order to animate these normals
    6. Another thing to note, dripping should only happen on the side (not the tops) of objects, so we make a mask that only effects the sides and multiply it by our effect
    7. We can also define the roughness of the droplets by adjusting the contrast of our effect with a `Power` and then doing `1-x` before plugging it in to the `Root[Roughness]`
    ![Rain Drip Shader](../images/Unreal%20Engine/Shaders/15%20-%20Rain%20Drip%20Shader.png)

## 16 - Rain Ripples Shader
- The following Shader effect will be distributed throughout 3 Material Functions, and can then be applied through one function call in the Material we want to apply it to
- To create the Ripple Effect - 
    1. Import a Texture of circles that will represent the ripples
        - **R** - Is the grayscale we will use to create our rippling rings animation
        - **GB** - The normals of the ripples
        - **A** - A temporal offset for the ripples
    2. Apply the Temporal offset to the animation
        - We simply need to add `Time` by the Alpha Channel
    3. Apply the animation to the R channel
        - We need to only get the decimal portion of our Temporal Time, to simulate the alternating nature of ripples
        - Subtract it by 1 to make it increasing, and then add it to the R channel to get the first effects of an alternating expanding circle.
        - We can then manipulate the *contrast* of the circle by multiplying (by something like 20)
        - `Clamp` it to a reasonable range (like [0, 4])
        - The multiple ripple effect then comes from plugging in a `Sine`, which gives us an alternating value of multiple 0 to 1s.
            - The period (unique to UE) should be set to 2*pi to simulate a real mathematical sine, and multiplied by pi beforehand to get more believable ripples
    4. In order to make the ripples fade out when they reach the end. We need to subtract 1 minus the Red channel, which should decrease when we reach the end of our animation.Therefore, multiply our ripple effect with this "fade out"
        - This math can be further expanded to work with "weights" as shown in the image
    5. Extract the normals from the G & B channel, and convert them to a proper range for normals. Lastly, multiply it by the animated fading ripples effect, so that the normals "move" with the animation.
        - Also don't forget to append another float to make this 2D normal into a 3D normal

    ![Ripple Function](../images/Unreal%20Engine/Shaders/16%20-%20RippleFunction.png)
- A regular Ripple Effect like above is fine, but, in order to add variety like overlapping ripples that collide with each other We should create a function called `Ripples` that use the above `Ripple` effect. `Ripples` should feed different values into four `Ripple` effects, and then do some sort of **Combine** on those four `Ripple`s.
    1. **Ripples Function** -
    ![Ripples Function](../images/Unreal%20Engine/Shaders/16%20-%20RipplesFunction.png)
    2. **Combine Four Normals Helper Function** -
    ![Combine Four Normals Function](../images/Unreal%20Engine/Shaders/16%20-%20Combine%20Four%20Normals%20Function.png)
- **Conclusion** - Now we can call our **Ripples** function to return Normals of raining ripples. Here is an example of a basic material using it:
![Rain Ripples Test](../images/Unreal%20Engine/Shaders/16%20-%20Rain%20Ripples%20Test.png)

## 17 - Rain Puddle Shader
- We want to make puddles that dynamically grow. Just as in real life where rain accumulates and eventually dissipates.
- We want to encapsulate this shader into a Material Function, so that we could use it on any Material.
- 1 - Puddle Shader
    1. World Project UVs (from top to bottom) to the texture (and anything else that needs it)
    2. Define an Input parameter that will be the controller for a `Lerp` that makes puddles biger or smaller
    3. Multiply #2 by a "Top Mask" so the effect only appears at the top of surfaces (not the sides or bottom)
    4. We can out put this mask itself, in order to be used as a helper for materials to define something like a roughness.
- 2 - Add Previous Rain effects by using a `Lerp` that is controlled by the Puddle Mask, and interpolates between the Rain effects and the Normal Material normals
![Rain Puddle Function](../images/Unreal%20Engine/Shaders/17%20-%20Wind%20Ripples%20Function.png)
- **Conclusion** - We cam then use this function on any material. For example :
![Rain Puddle Material](../images/Unreal%20Engine/Shaders/17%20-%20Raing%20Puddle%20MAterial.png)

## 18 - Complete Rain Shader
- Finally, we can mix all of our Rain Material Functions into one Complete Rain function like so:
![Complete Rain Function](../images/Unreal%20Engine/Shaders/18%20-%20Complete%20Rain%20Function.png)
- The function can be used in any Material to simulate Rain!
![Complete Rain Shader](../images/Unreal%20Engine/Shaders/18%20-%20Complete%20Rain%20Material.png)

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
| CustomReflectionVector | Takes in a normal and |
| Sine | Given an ever increasing value. This alternates in the range [-1, 1] |
| Unsaturation | If fed a positive it shifts colors towards grayscale, otherwise if negative then it saturates colors|
| Saturation | Clamps values between [0, 1] |
| Absolute World Position | |
<h1 style="text-align: center;"> Unreal Engine Shaders </h1>

## 1 - What is a Shader?
- What is a shader?
    - Code that controls the color of each pixel on the screen
    - Usually runs on the GPU
    - Unreal Engine's **Material Editor** is a node based interface to create shaders. The engine writes the shader code for us under the hood based on the nodes we wire together.

- Shaders can do lots of things
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
- Introduction to Unreal Engine Materials
    - **To create a material** : *Content Drawer > Right-Click > Material*
        - The naming convention for materials is : *M_SomethingDescriptive*
    - **To open the material editor** : *Double-click a material*
    - **To connect nodes** : RMB-Drag from a Node's output to another node's input
    - **To remove connections** : Alt + LMB on a Node's Input

## Node Glossary
| Node | Description|
|---|---|
| Texture Sample | Contains a reference to a Texture Map |
| Constant | A single number ( 0 = Black and 1 = White) |
| Constant3Vector | A Vector that can signify a color via RGB representation

<h1 style="text-align: center;"> Unreal Engine VFX with Niagara </h1>

<h2 align="center">Table of Contents</h2>

1. [Line Trace Effect]()

<h2 align="center"> Line Trace Effect</h2>

### Laser Beam Emitter
1. Right-click on your Content Browser and select `FX -> Niagara Emitter -> New Emitter` and select the Template `Dynamic Beam` so we have a good starting place
    * From here we can pull the `Timeline`'s end point to the end of the particle effect (the green) so it's easier to work with
2. Change `Emitter Node -> Emitter Update -> Loop Behavior` from Infinite to `Once`, otherwise the particle would just loop when we fire it once
3. `Beam Emitter Setup` is what defines the start and end points, but we don't change anything here because we will dynamically do that via blueprints later
4. `Beam Width` lets you manipulate a curve that signifies the "thickness" of the beam. Here, specifically, to create a consistent non-disappearing beam we do the following -
    1. Select all points on the curve and right-click on one of them and select - `Linear`
    2. With all points still selected set the (right) "Multiple Values" value to `0.2`
5. `Color` changes the color of the beam
6. To get rid of the opacity being applied to the end of the particle system we have to change the default material being used in `Ribbon Renderer`
    * In this case I make my own called `M_LaserBase` w/ the following tweaks -
        1. `Root Node` - `Blend Mode` : Additive & `Shading Model` : Unlit
        2. ( `ParticleColor` * `ScalarParameter{Default Value : 1}`) &rarr; `RootNode[Emmisive Color]`
    * Although, the previous Material is a base for `Material Instance`s that we can create out of it. In this case we create a `Material Instance` called `M_LaserBeam_Inst` so that we can make the `EmissiveStrength` parameter we defined earlier higher. THIS IS THE MATERIAL WE REPLACE `Ribbon Renderer`'s default with
7. We need to set the end point of the emitter which is a `Vector3` in the World so we do the following -
    1. `Details` &rarr; `User Exposed` &rarr; `+` &rarr; `Vector` : "BeamEnd" 
    2. `Emitter Node` &rarr; `Beam Emitter Setup` &rarr; `Beam End (Dropdown)` &rarr; USER BeamEnd

### Laser Beam System
1. Right-click on the `Laser Beam Emitter` we created previously and select `Create Niagra System`
    * The System asset gets named after the Emitter, so if you want a compact name be sure to delete the "_EM"
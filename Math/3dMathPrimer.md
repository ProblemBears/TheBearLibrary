<h1 align="center"> 3D Math Primer </h1>

<h2 align="center"> Table of Contents </h2>

1. [Caresian Coordinate Systems](#cartesian-coordinate-systems)
2. [Vectors]()
3. [Multiple Coordinate Spaces]()

<h2 align="center" id="cartesian-coordinate-systems"> Cartesian Coordinate Systems </h2>

- The **Cartesian Coordinate System** was created by Rene Descartes
- 1D Mathematics originated by counting dead sheep
    * The First Law of Computer Graphics - *"If it looks right, it is right"*
- 2D Mathematics allowed map planning and involves **(x,y)** coordinates
    * No matter what orientation we choose for x and y, we can always rotate the coordinate space to get +x pointing right and +y up
- 3D Mathematics defines 3 axes - **x, y, z**
    * In 3D, any pair of axes defines a plane that contains those two axes and is perpendicular to the third axis
    * So if we make +x, +y, +z point right, up, and forward respectively, then the 2D coordinate space of the "ground" would be x and z, while y determines the "elevation"
    * We can pick one of two coordinate spaces - Left Handed or Right Handed
        * Unlike 2D spaces, we can't always "rotate" a 3D coordinate space to match our desired orientations
        * OpenGL uses a right-handed system while DirectX uses a left-handed system. For each, system you would have to do minor tweaks to your math for correctness.
        * **Throughout this reference we'll use a left-handed system**
- Angles, Degrees, and Radians
    * The most important unit of measure for angles are - **degrees** (&deg;) & **radians** (rad)
    * 1 degree = 1/360th of a revolution. So 360&deg; is a full revolution
    * However, most mathematicians use radians, a unit of measure based on circle properties
    * A
    * Conversions between Angles and Radians -
         $$
        \begin{align}
        2\pi rad = 360\degree \\
        \pi rad = 180\degree \\
        1 rad = (\frac{180}{\pi})^o \\
        \frac{\pi}{180} rad = 1\degree
        \end{align}
        $$
    * The (x,y) coordinates of the endpoints of a ray that were rotated have special properties called **Trig Functions** -
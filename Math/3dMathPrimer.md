<h1 align="center"> 3D Math Primer </h1>

<h2 align="center"> Table of Contents </h2>

1. [Caresian Coordinate Systems](#cartesian-coordinate-systems)
2. [Vectors](#vectors)
    - [Mathematical Definition of Vector, and Other Stuff](#mathematical-definition-of-vector-and-other-stuff)
    - [Geometric Definitions of Vectors](#geometric-definitions-of-vectors)
    - [Specifying Vectors with Cartesian Coordinates](#specifying-vectors-with-cartesian-coordinates)
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

<h2 align="center" id="vectors"> Vectors </h2>

### Mathematical Definition of Vector, and Other Stuff
- Mathematics distinguishes between `scalars` & `vectors`
- The `dimension` of a vector tells how many #'s it contains ( 1D = scalar )
- Vectors can be written as a row or column $ \begin{bmatrix} 1 & 2 & 3 \end{bmatrix} = \begin{bmatrix} 1 \\ 2 \\ 3 \end{bmatrix} $ 
- Subscript $ v_i $ (where i > 0) can be used to get certain values of a vector, but since we'll only be concerned up to the 4<sup>th</sup> dimension then
    * 2D = x,y
    * 3D = x, y, z
    * 4D = x, y, z, w
- Font Conventions -
    * Scalars = italics
    * Vectors = lowercase bold
    * Matrices = uppercase bold

### Geometric Definitions of Vectors
- Geometrically, a `vector` is a **directed line segment that has magnitude and direction**
    * The `magnitude` of a vector is it's **length. Which is a nonnegative**
    * The `direction` **describes which way the vector is pointing in space**
- We can't ask *"where is a vector?"* since it doesn't have a position. Correct examples of the significance of a vector can be the following examples -
    * Displacement - *"Take 3 steps forward"*
    * Velocity - *"I'm travelling NE at 25mph"*

### Specifying Vectors with Cartesian Coordinates
- When we use `Cartesian Coordinates` to describe a `vector`, each coordinate measures a signed displacement in the corresponding dimension
- A `vector` is just **a sequence of displacements**
- $ \boldsymbol{0} = \begin{bmatrix} 0 \\ 0\\ ... \\ 0  \end{bmatrix} $ `The Zero Vector`  is unique because it's the only vector w/out magnitude & direction.
    * Its usefulness is equivalent to a zero scalar in basic algebra

### Vector Versus Point
- A `point` specifies a location
- A `vector` specifies a displacement
- Since vectors can describe displacement, they can describe relative position
    * `Relative position` is **the position of something specified by describing where it is in relation to some other, known location**
<h1 align="center"> 3D Math Primer </h1>

<h2 align="center"> Table of Contents </h2>

1. [Caresian Coordinate Systems](#cartesian-coordinate-systems)
2. [Vectors](#vectors)
    - [2.1 - Mathematical Definition of Vector, and Other Stuff](#21---mathematical-definition-of-vector-and-other-stuff)
    - [2.2 - Geometric Definitions of Vectors](#22---geometric-definitions-of-vectors)
    - [2.3 - Specifying Vectors with Cartesian Coordinates](#23---specifying-vectors-with-cartesian-coordinates)
    - [2.4 - Vector Versus Point](#24---vector-versus-point)
    - [2.5 - Negating a Vector](#25---negating-a-vector)
    - [2.6 - Vector Multiplication by a Scalar](#26---vector-multiplication-by-a-scalar)
    - [2.7 - Vector Addition & Subtraction](#27---vector-addition--subtraction)
    - [2.8 - Vector Magnitude (Length)](#28---vector-magnitude-length)
    - [2.9 - Unit Vectors](#29---unit-vectors)
    - [2.10 - The Distance Formula](#210---the-distance-formula)
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

### 2.1 - Mathematical Definition of Vector, and Other Stuff
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

### 2.2 - Geometric Definitions of Vectors
- Geometrically, a `vector` is a **directed line segment that has magnitude and direction**
    * The `magnitude` of a vector is it's **length. Which is a nonnegative**
    * The `direction` **describes which way the vector is pointing in space**
- We can't ask *"where is a vector?"* since it doesn't have a position. Correct examples of the significance of a vector can be the following examples -
    * Displacement - *"Take 3 steps forward"*
    * Velocity - *"I'm travelling NE at 25mph"*

### 2.3 - Specifying Vectors with Cartesian Coordinates
- When we use `Cartesian Coordinates` to describe a `vector`, each coordinate measures a signed displacement in the corresponding dimension
- A `vector` is just **a sequence of displacements**
- $ \boldsymbol{0} = \begin{bmatrix} 0 \\ 0\\ ... \\ 0  \end{bmatrix} $ `The Zero Vector`  is unique because it's the only vector w/out magnitude & direction.
    * Its usefulness is equivalent to a zero scalar in basic algebra

### 2.4 - Vector Versus Point
- A `point` specifies a location
- A `vector` specifies a displacement
- Since vectors can describe displacement, they can describe relative position
    * `Relative position` is **the position of something specified by describing where it is in relation to some other, known location**

### 2.5 - Negating a Vector
- Additive inverse of a vector is a $ -\pmb{v} $ such that $$ v + (-\pmb{v}) = 0 $$
- To negate a vector. We do : $$ -\begin{bmatrix} a_1 \\ a_2 \\ ... \\ a_n\end{bmatrix} = \begin{bmatrix} -a_1 \\ -a_2 \\ ... \\ -a_n\end{bmatrix}$$ This results in a vector of the same magnitude but in the opposite direction

### 2.6 - Vector Multiplication by a Scalar
Where *k* is a scalar
- Multiplication
    $$
    k\begin{bmatrix}a_1 \\ a_2 \\ ... \\ a_n \end{bmatrix} =
    \begin{bmatrix}a_1 \\ a_2 \\ ... \\ a_n \end{bmatrix}k =
    \begin{bmatrix}ka_1 \\ ka_2 \\ ... \\ ka_n \end{bmatrix}
    $$
- Division
    $$
    \frac{ \pmb{v} }{k} =
    (\frac{1}{k}) \pmb{v} = 
    \begin{bmatrix} v_x/k \\ v_y/k \\ v_z/k \end{bmatrix}
    $$
- PEMDAS still applies for both cases
- Multiplying by a scalar has the effect of scaling a vectors magnitude by scalar *k*. If the scalar is negative it flips and scales

### 2.7 - Vector Addition & Subtraction
- Addition
    $$
    \begin{bmatrix}a_1 \\ a_2 \\ ... \\ a_n \end{bmatrix} +
    \begin{bmatrix}b_1 \\ b_2 \\ ... \\ b_n \end{bmatrix} =
    \begin{bmatrix}a_1 + b_1 \\ a_2 + b_2 \\ ... \\ a_n + b_n \end{bmatrix}
    $$
    - Communative : a+b = b+a
- Subtraction
    $$
    \begin{bmatrix}a_1 \\ a_2 \\ ... \\ a_n \end{bmatrix} -
    \begin{bmatrix}b_1 \\ b_2 \\ ... \\ b_n \end{bmatrix} =
    \begin{bmatrix}a_1 \\ a_2 \\ ... \\ a_n \end{bmatrix} +
    \left( -\begin{bmatrix}b_1 \\ b_2 \\ ... \\ b_n \end{bmatrix} \right) =
    \begin{bmatrix}a_1 - b_1 \\ a_2 - b_2 \\ ... \\ a_n - b_n \end{bmatrix}
    $$
    - Anticommunative : a-b = -(b-a)
- In a coordinate system, addition can be imagined as a sequence of displacements where the final endpoint is the result
- So a vector $ \begin{bmatrix} 1 \\ -3 \\  4 \end{bmatrix} $ can be interpreted as the addition of $ \begin{bmatrix} 1 \\ 0 \\  0 \end{bmatrix} + \begin{bmatrix} 0 \\ -3 \\  0 \end{bmatrix} + \begin{bmatrix} 0 \\ 0 \\  4 \end{bmatrix}$
- To calculate a vector from one point to another point we can think of two vectors from the origin to each of those points and then subtract those vectors to get the vector displacement a-b

### 2.8 - Vector Magnitude (Length)
- Also known as the length or norm of the vector
- `Magnitude` of a vector is **denoted by double vertical bars surrounding the vector**
    $$
    \left\lVert v \right\rVert =
    \sqrt{ \sum_{i=1}^n v_i^2 } =
    \sqrt{v_1^2 + v_2^2 + ... + v_{n-1}^2 + v_n^2}
    $$
    Thus, **the magnitude of a vector is the square root of the sum of the squares of the components of the vector**

### 2.9 - Unit Vectors
- For many vector quantities we're only concerned with *"which way am I facing"* (direction) and not the length. Therefore, it's more convenient to use `unit/normalized vectors` **which have a magnitude of one**
- Sometimes `unit vectors` are simply called `normals` 
    * Although, the term *"normal"* has a different commonly used meaning in which it means "perpendicular" to something and not simply "unit vector"
    * So `normalized` = "unit length" and `normal` = "perpendicular & by convention unit length"
- **For any nonzero vector $ v $ we can compute a unit vector that points in the same direction**. We call this `normalizing` a vector and it's done by **dividing a vector by its magnitude**
    $$
    \hat{v} = \frac{v}{\left\lVert v \right\rVert}
    $$
    * For any nonzero vector $ v $
    * The symbol $ \hat{v} $ represents a `unit vector`
    * Again, $ \left\lVert v \right\rVert $ represents the [magnitude of a vector](#28---vector-magnitude-length) $ v $

### 2.10 - The Distance Formula
- Since we know we can generate a vector from one point ot another ( by their individual vectors from the origin )
    $$
    d =
    b-a =
    \begin{bmatrix} 
        b_x - a_x \\
        b_y - a_y \\
        b_z - a_z 
    \end{bmatrix}
    $$
    We can get the distance between two points by calculating $ \left\lVert d \right\rVert $. Therefore, `the distance between two points` (in 3D) **can be calculated as follows -**
    $$
    distance(a,b) = 
    \left\lVert b-a \right\rVert =
    \sqrt{ (b_x - a_x)^2 + (b_y - a_y)^2 + (b_z - a_z)^2 }
    $$

### 2.11 - Vector Dot Product
- The name "dot product" comes from the dot symbol used in $ \pmb a \cdot \pmb v $
- The `dot product` of **two vectors is the sum of corresponding components resuling in a scalar** :
    $$
    \begin{bmatrix}
    a_1 \\
    a_2 \\
    ...\\
    a_n
    \end{bmatrix} \cdot
    \begin{bmatrix}
    b_1 \\
    b_2 \\
    ...\\
    b_n
    \end{bmatrix} =
    a_1b_1 + a_2b_2 + ... + a_nb_n
    $$
    or more succinctly
    $$
    a \cdot b = \sum_{i=1}^n a_ib_i
    $$

#### Two Geometric Interpretations of the Dot Product (IMPORTANT)
- `Interpretation 1 - Dot Product Performing a  "Projection"`
    - We can define $ \hat{a} \cdot b $ as the signed length of the projection of $ \pmb b $ onto this line. You can think of the projection of $ \pmb b $ onto $ \hat{a} $ as the "shadow" that $ \pmb b $ casts on  $ \hat{a} $ when the ray of lights are perpendicular to $ \hat{a} $
    - What does it mean for the dot product to measure a signed length?
        * It means  the value will be negative when ***b*** points opposite to $ \hat{a} $ and the projection has zero length if both vectors are perpendicular to each other
    - Dot Product Rules
        * Dot product is associative with multiplication by a scalar
        * Dot Product is associative w/ multiplication by a scalar for either vector
        * Dot product is commutative
        * Dot product distributes over addition and subtraction
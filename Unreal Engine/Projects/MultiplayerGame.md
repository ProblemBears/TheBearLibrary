<h1 align="center"> Multiplayer Games </h1>
<h2 align="center"> Table of Contents </h2>

- [Puzzle Platforms](#puzzle-platforms)
    * [Connecting Two Players](#connecting-two-players)
- [Menu System](#menu-system)
- [Online Multiplayer](#online-multiplayer)
- [Krazy Karts](#krazy-karts)

<!----------------------------------------------------------------------------------------------------------------->
<h2 align="center" id="puzzle-platforms"> Puzzle Platforms </h2>

### Connecting Two Players
- Create a new UE C++ Project with the Third Person Template (no Starter Content)
- To be able to play multiplayer in multiple windows do:  
    `Play Settings` &rarr; `Multiplayer Options` &rarr;
    * `Number of Players` : 2
    * `Net Mode` : Play as Listen Server
- Under the hood - 
    1. UE loads a `Map`
    2. The `Map` specifies a `GameMode`
    3. The `PlayerController` joins the `Map`
        * In our case 2 join
    4. The `PlayerController`s ask the `GameMode` to spawn a `Pawn`
    5. The `Pawn` is linked to the `PlayerController`
        * This "link" usually happens over the network when playing multiplayer. We'll worry about that later

### Surveying the Multiplayer Space
- When we say somethings are `Synchronous` then they "happen at the same time"
    | Game Type |  Synchronous? | Session Length | Indie Suitability | Unreal Support |
    | --- | ---| --- | --- | --- |
    | Turn-Based | &cross; | Variable | Excellent | Minimal |
    | Real-time / Session-based | &check; | < 1 hour | Good | Excellent |
    | MMO / Persistent World| &check; | Potentially Infinite | Poor | Minimal |
- Session-Based Stages (The three stages to make a connection)
    1. Discovery
        * Finding who else is online so that we can join and play with them
    2. Connection
        * Joining the session. Hooking up from a client to a server
    3. Synchronization
        * We need to make sure that what Player 1 sees is the same as what Player 2 sees

### Meet the Client-Server Model
- Input and State
    * `State` is the information that eventually is used to render the "state of the world"
    * It gets combined with some `Actions` the player(s) may want to do based on the current `State`, which in turn yields a new `State`. So on and so forth.
- A simple naive way to implement multiplayer is `Peer-to-Peer` -
    * A server has to wait for every players Input before it can change the state. This relies on waiting for the slowest person in terms of their connection speed (not their input speed)
- Instead of connecting directly to other players we can instead use the `Client-Server` model -
    * All players send their input to the server which has an `authorative state` that in turn sends the changes
    each client has to implement in their own internal `client state`
- Creating a Server and connecting with Clients using Command Prompt - 
    - To create a Server using your Command Prompt type something like :
        ```cmd
        "E:\Applications\Unreal Engine\Main\UE_5.0\Engine\Binaries\Win64\UnrealEditor.exe" "E:\Perforce\P4_Workspaces\jonathan_UE-PuzzlePlatforms\PuzzlePlatforms.uproject" /Game/ThirdPerson/Maps/ThirdPersonMap?listen -server -log
        ```
    - To connect to that server as a Client using your Command Prompt type something like (for as many players as you want) :
        ```cmd
        "E:\Applications\Unreal Engine\Main\UE_5.0\Engine\Binaries\Win64\UnrealEditor.exe" "E:\Perforce\P4_Workspaces\jonathan_UE-PuzzlePlatforms\PuzzlePlatforms.uproject" 127.0.0.1 -game
        ```
<!----------------------------------------------------------------------------------------------------------------->
<h2 align="center" id="menu-system"> Menu System </h2>

<!----------------------------------------------------------------------------------------------------------------->
<h2 align="center" id="online-multiplayer"> Online Multiplayer </h2>

<!----------------------------------------------------------------------------------------------------------------->
<h2 align="center" id="krazy-karts"> Krazy Karts </h2>
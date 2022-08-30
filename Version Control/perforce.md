<h1 style="text-align: center;"> Perforce - Helix Core Guide </h1>

## Why Helix Core?

- Helix Core's Version Control tools are used by most AAA game development studios such as Epic Games, the creators of
  Unreal Engine. Therefore, game engines like Unreal Engine have built-in support for Helix Core such as visualizations
  for each commit (changelist submission).

## Installation

1. **P4D** - The Helix Core Server is installed into your computer to make it act as the manager/storage for all your **depots**. It also sets up a port (by default 1666) in which **clients** using different computers (your computer can be a client too) can connect to. The computer that has P4D installed on it **should ideally be on at all times** in order to allow clients to connect at any time. Other useful installs are:
    - **P4Admin** - A visual tool for **Admin users** to create depots, add users, change permissions, assign groups, etc...
2. **P4V** - A GUI tool to access versioned files. Otherwise, we would have to just use the command line

## Unreal Engine Process

[Based On the Helix Core Docs](https://help.perforce.com/helix-core/quickstart-unreal/Content/quickstart/game-home-quickstart.html)

1. Create a Stream Depot
    - Open P4Admin
    - Go to the Depot Tab
    - File > New... > Depot... > Name the Depot
    - Ensure the **Depot Type** is set to **stream** (a writable depot that contains **streams** a type of branch)

2. Edit the **TypeMap**
    - In a new terminal, type `p4 typemap` and press Enter - A Text Editor will load with a File
    - Underneath the line that says **TypeMap:**, indent and then add lines for each file extension and/or path you want to specify. On each line, specify the file type and /or modifers, and the pattern to match. (More Info in the source above)
    - Sample Unreal engine TypeMap:
      ```
        Typemap:
          binary+w //....exe
          binary+w //....dll
          binary+w //....lib
          binary+w //....app
          binary+w //....dylib
          binary+w //....stub
          binary+w //....ipa
          binary+l //....uasset
          binary+l //....umap
          binary+l //....upk
          binary+l //....udk
          binary+l //....ubulk
          binary+wS //..._BuiltData.uasset
      ```
3. Create the Main Stream
    - In P4V, go to **View > Stream Graph**
    - Make sure your project's depot is select in the **Stream Graph > Depot: drop-down menu**
    - Right-click in an empty space in the Stream Graph on the right side and select **New Stream...**
    - Fill in the stream info:
      - Give your stream a name (ex: "main")
      - Make sure the *Stream type* is set to **mainline**
      - Double check that your **Depot:** is set correctly
      - (Optional) Add a description of the stream
      - Uncheck both the "Create a workspace..." and "Populate..." options at the bottom. We will do this manually in the next step.
        ![Streams Graph](https://help.perforce.com/helix-core/quickstart-unreal/Content/Resources/Images/game-create-main-stream-5_800x533.png)
    - Click **OK**. You will now see your stream in the Stream Graph
4. Create a workspace and associate it with a stream
    - In the main window of P4V, click the **Workspace** tab, open the drop-down list, and click **New Workspace...**. Alternatively, you can right-click on a stream and select **New Workspace...**.
    - Configure the *Basic* tab:

      ![Workspace Basic](https://help.perforce.com/helix-core/quickstart-unreal/Content/Resources/Images/game-create-workspace-2.png)
    - (Optional) Configure the Advanced tab to set up a few more helpful options:

      ![Workspace Advanced](https://help.perforce.com/helix-core/quickstart-unreal/Content/Resources/Images/game-create-workspace-3.png)
    - Click **OK** and you should see a new Workspace under your **Workspace tab**

5. Setup and create the ignore file
    - In order to use an ignore file, we need to tell our local Helix Core client where to look for an ignore file. This is done by setting the P4IGNORE environment variable or by using the `p4 set` command.\
    \
    Let's decide to call our ignore files **.p4ignore** by opening a terminal and typing:
      ```
      p4 set P4IGNORE=.p4ignore
      ```
    - Now that Helix Core knows to look for a file called *.p4ignore* in the workspace, we can create the ignore file.
    - Inside your workspace's root directory create a file called *.p4ignore*
    - In that file, write one line for each path you want ignored, and save:
      - Paths are case-sensitive
      - \* can be used as a wildcard to match any number of characters
      - Putting a / at the end of a path will only match directories. Otherwise it will match any files OR directories with the same characters.
      - A ! at the beginning of a line will make sure that matches files are not ignored, even if a previous rule matches them.
    - **Example Ignore file for Unreal Engine**
      ```
      # User-specific folders or temporary files that should not be versioned.
      Saved/
      Intermediate/
      DerivedDataCache/
      FileOpenOrder/
      obj/
      # Certain file types that should not be versioned.
      *.pdb
      *-Debug.*
      # Visual Studio user settings files that should be ignored.
      .vs/
      *.vcxproj
      *.sln
      ```
      [Link to a higly recommended p4Ignore](https://github.com/github/gitignore/blob/main/UnrealEngine.gitignore)

    - Finally, submit the *.p4ignore* in a **changelist**
6. Add Project Files
    - Create an Unreal Project and navigate to it's location in File Explorer
    - Copy all of the project files into your workspace root folder (in P4V click **Refresh**)
    - Add all of the files into a **Changelist** (Your previous *.gitignore* will remove all unescessary files when you submit the changelist)
    - Make sure the .p4ignore does its job. When you **Add**, your changelist should only contain files from your **Config, Content, and Source directories, as well as your .uproject file.** If this isn't the case, then your .p4ignore isn't working.
    - **Submit your Changelist**
    - Now all your files (that weren't **ignored**) are stored in the depot on your server (where you installed  **P4D**)
    - (Optional) Now that you moved your project it may be the case that the Epic Games Launcher won't conveniently detect it in **My Projects**. This means the only ways to open the project easily are through Perforce, or your file explorer. To fix this we can:
      - Navigate to:\
      `C:\Users[username]\AppData\Local\EpicGamesLauncher\Saved\Config\Windows directory`
      - Open:\
      `GameUserSettings.ini`
      - Under `[Launcher]` add a path to your Perforce Workspaces directory, in a form similar to the following:\
      `CreatedProjectPaths=E:/Perforce/P4_Workspaces`
      - Restart the Epic Games Launcher Process
7. Use source control extension in Unreal Engine
    - In UE5 click on the bottom left where it says "Source Control Off" (or **Tools>Connect to Source Control**)
    - Login with:\
    Provide = Perforce\
    Server = 1666 if the Helix Core Server is installed in your machine **OR** serverIPAddress:1666 if someone hosts the Helix Core server on their machine.
      - NOTE: 1666 is the default PORT #. It is possible this is different of the Server Admin changed it.
    - Then choose your username and the correct project Workspace
    - Unreal Engine now has the max benefit of Helix Core versioning!

## Working with Streams
[Based on the Helix Core Streams Guide](https://www.perforce.com/manuals/p4v/Content/P4V/chapter.streams.html)\
Helpful resources:\
[AAA Studio Tips](https://www.perforce.com/blog/vcs/ue5-update-perforce-streams)\
[Perforce Streams Quickstart Video](https://www.youtube.com/watch?v=ggty1lkL1gA)
- **Streams** are the branching mechanism used by Helix Core. The visual gateway for manipulating streams can be found in **P4V's Stream Graph Tab**. 
- The **mainline branching model** is ideal in **P4V** because the **Streams Graph** orders streams visually, by ordering the most stable streams higher up. This means "child" streams are less stable and therefore, they should always **merge down** from their "parent" before they **copy up**.

  ![Mainline Model](https://www.perforce.com/manuals/p4v/Content/Resources/Images/p4v-stream-organization.png)

- The **mainline** stream that we created in [Unreal Engine Process](#unreal-engine-process) is the parent of all streams, and all children can be parents of more children, in which "child" streams can **merge down** any changes that co workers may have submitted, and **Copy up** their own work when it is completed.
- **To Create a New Stream** - We can right-click on the mainline stream in **Stream Graph** and select **Create New Stream from 'main'...**\
This opens a dialog where you can specify:
  - **Stream Name**
  - **Stream Type** - Indicates the stability of the streams.
    - **Mainline** - A stream with no parent. Used as the stable trunk of a stream system.
    - **Release** - A stream that's more stable than it's parent. Expects merging down but doesn't expect copying up. This is used for stabilization, bug fixing, and release maintenance.
    - **Development** - A stream that's less stable than it's parent. Useful for long-term projects and major new features
    - **Task** - a short-term branch that reduces Server meta data
    - **Virtual** - a stream that acts like a filter
  - **Parent Stream**
  - **Parent View** - Almost any stream should be set to **inherit** except for **Release streams** which should have **noinherit** so that Parent "propogation" is disabled (since release stream only copy up)
  - **Create a workspace to use with this stream** - If you already have one, it might be convenient not to tick it since we can switch one workspace between many streams
  - You can also configure **Stream Views** in the **Advanced** tab. Read more from the reference above.
- **Using the Stream Graph** -
  - If you don't see your newly added Stream, then make sure you Select it on the left hierarchy and click **Apply** when you're on **Picker Mode**. Otherwise, go to **Filter Mode**. Picker Mode is ideal if your Graph gets too complicated, and you only want to see certain nodes
  - The arrows signifies whether you can **Merge Down/Copy up** and the wires become color-coded with the following meanings.
    - **Gray** - no merge or copy required
    - **Green** - a merge or copy is available
    - **Orange** - stream must be updated, after which merge or copy is available
  - The **Workspace Icon** that looks Like a TV indicates what stream you are currently working in. **You can drag it to another stream to switch your workspace to that stream**. The Stream field value in the workspace definition changes to the new stream. When you do this, if you have checked out files to the following:
    - **The default changelist** - P4V shelves them automatically in a numbered changelist with the description "Switched branch shelf" and unshelves them again to the default changelist when you switch back to that stream. However, note that the numbered changelist created for the interim shelf does not get deleted and remains in the system
    - **A numbered changelist** - P4V prompts you to shelve those files before switching. When you switch back to that stream, those files remain shelved in the numbered changelist. You have to unshelve them manually.

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

# The Overcomplicated Linux Haste Modding Template
This is an overcomplicated template for modding the game [Haste](https://store.steampowered.com/app/1796470/Haste/). The template only works on Linux.

It utilises MSBuild to the best of mine and its abilities, and bash for the rest of it.
The original intent was to get debugging working, but I still have not been able to get that to work properly :(.
However, it does manage to get the game running as a debug build, so debugging is theoretically just a step away. 


## What?
The build script included with this project will automatically:
- Download and extract DepotDownloader
- Download Haste through DepotDownloader
- Download and extract the debug executables from Unity
- Patch Haste with aforementioned debug executables
- Tell your IDE all about the local Haste installation just created
- Automatically update Haste whenever an update is available (theoretically - I have not tested this yet)
- Provides Rider run configurations for running the game directly through Rider. Note this **requires steam running in the background** and more importantly **DOES NOT BYPASS DRM**.
- Automatically track the player.log file when run through Rider - click on the player.log tab in the run tabs next to Console



## Should I use it?
Do you use Linux? Do you know a bit of bash? Do you have a line of contact for ChiefArug? Do you want to be able to collaborate with other people just like you easier? Do you have 12gb free disk space?

If you answered Yes to all of the above questions, then consider it. If you really want to.
Just make sure to read the [How to use it](#how-to-use-it) section!

## How to use it
Due to the limitations of Rider and MSBuild there are some caveats with this system, especially on the first setup.
- When you first open the project in Rider it will attempt to Sync. This will fail as you are not authenticated with Steam.
- Cancel the sync by killing the MSBuild process: `pkill -f MSBuild.dll`.
- Manually run build using the hammer icon. This will start showing stuff in your terminal as it downloads and runs DepotDownlaoder.
- When a QR code shows up, scan it with the Steam app on your phone. This will authenticate you with Steam.
- Make sure to also create a `.steamuser` file in the root of the project that contains your steam username, this will cause the build process to use your cached login details instead of asking for a new login each time.
- The game will then download. After the game is downloaded it will figure out the Unity version the game uses and download the installer for that
- Next the longest process begins, that of extracting the few files we need from the ~4gb Unity tarball. This will take a while.
- After this it should finish near instantly, with a lot of big red errors.
- These errors are because we only just downloaded the game dlls. A Project Sync is required to make the IDE recognise these files, so restart the IDE and it should resync.

## Troubleshooting
#### Stuck on Syncing:
- Run `pkill -f MSBuild.dll`, then manually run build so you get terminal output and can see what is happening.
#### The build is failing with lots of big red angry errors saying it can't find anything
- Restart Rider so that it syncs the project. If the sync isn't completing within a minute kill it (see above) and manually run build with the hammer so you get terminal output. Wait for that to finish then restart Rider.
#### Failing to build due to DepotDownloader timing out
- Edit `Timeout="300000"` in the .csproj file to a bigger number, the default is only 5 minutes.
#### I keep having to scan the QR code
- Make sure to create the `.steamuser` file with your username. Here is a command for doing that: `echo 'chiefarug' > .steamuser`. Make sure to replace `chiefarug` with your username!
#### It's asking for a password, but I cannot enter one (it says it's a read-only view)
This is a limitation of MSBuild and Rider and why this project uses qr code based authentication.
Provided you have not edited the .csproj file this will only show up when you entered the wrong username in the `.steamuser` file, or haven't run build **without** a `.steamuser` file present to set up authentication via the qr code.
#### Crashing due to some random error in a native executable
The direct run configuration provided here run it slightly differently to how Steam runs it.
Specifically this doesn't launch it through the Steam Runtime, which provides copies of common native executables so games can run in a predictable environment
If you have a wacky setup then this could be causing issues.
- In theory, you can solve it by running Rider through Steam by adding it as a Non-Steam game and forcing it to run with the Steam Runtime
- Properties -> Compatibility -> Force the use of a specific Steam Play compatibility tool -> Steam Linux Runtime 3.0 (sniper)
I have also found that running with vulkan can cause issues, so adding `-force-opengl` to the program arguments in the run configuration helps sometimes.
#### Stuck on black screen after launching
Check the player.log file, if it mentions that the SteamAPI couldn't initialise then make sure Steam is running in the background in the same user as Rider and the game.
Otherwise, it's possibly a random error in a native executable, see above.

#### Something else
- Contact ChiefArug for assistance, or figure it out yourself then let them know how you fixed it.


## Credits
Thanks to khyperia who made the orignal template this is based on:
https://github.com/Haste-Team/HastePlugins/tree/main/HelloWorld

Additional thanks to khyperia for answering some of my modding questions on the Haste Discord server.

Thank you to SteamRE who make DepotDownloader that this project uses.

## Contributing
Contributions would be welcome, especially to clean up my messy MSBuild script.
In particular getting this to work on Windows (or Mac, if you are that sort of person) would be awesome,
although would require rewriting a lot of the `.csproj` file.

Additional Troubleshooting steps are always welcome too.
And if you know how to get debugging working that would be greatly appreciated!!!
    

# Introduction #

The best way to get started with JNIBWAPI is to first browse the information available at the [BWAPI](https://code.google.com/p/bwapi/) project (note that the documentation relevant to JNIBWAPI is listed as "Old Documentation"). JNIBWAPI works as a BWAPI "AI Client", with the C++ code in `src/c` acting as a binding between the BWAPI and Java processes. The code in `src/java/jnibwapi` encapsulates the C++ code and tries to replicate the BWAPI interface.

JNIBWAPI assumes you have the following components :

  * Starcraft (v. 1.16.1)
  * Windows (Mac is unsupported)
  * Visual Studio 2008 (the dll requires Platform Toolset v90) - **not required** if you are not changing the C++ code
  * Java 1.7 (32-bit recommended)
  * BWAPI 3.7.4 ([available here](https://code.google.com/p/bwapi/downloads/list))
  * Chaoslauncher (included with BWAPI)
  * JNIBWAPI 0.4+ ([available here](https://drive.google.com/a/soe.ucsc.edu/folderview?id=0Bxdd3L2Qj5lxamZUSUpOLVZ6c2M))

The BWAPI installation guide is aimed predominantly at those intending to compile directly from the C++ source of BWAPI. It is possible to skip some of these steps if you don't intend to do this. This guide will assume that you will be developing solely in Java - follow the BWAPI guide if this is not the case.


# Setting Up JNIBWAPI #

  1. Install Starcraft and Brood War expansion
  1. Update to latest version (1.16.1)
  1. Download BWAPI, extract files from zip
  1. Follow the BWAPI Build Instruction steps in README in the BWAPI files
    * Note: If you do not have Visual Studio 2008 installed, you must run `vcredist_x86.exe` (included in the BWAPI files)
  1. Edit `<Starcraft dir>/bwapi-data/bwapi.ini`:
    * Change line 8 "`ai = bwapi-data\AI\ExampleAIModule.dll, bwapi-data\AI\TestAIModule.dll`" to "`ai = NULL`".
    * Remove line 9.
  1. Extract JNIBWAPI files.
  1. In the `release` directory of JNIBWAPI, right-click `run.bat` and select `Run as Administrator`
    * Expected output: "Loaded client bridge library." "Bridge: BWAPI Client launched!" "Bridge: Connecting..."
  1. Start Chaoslauncher as an Administrator and activate the BWAPI Release plugin
  1. Start the Starcraft game
    * Expected output: "Connected" "Bridge: waiting to enter match..."
  1. Begin a custom game as Zerg (the race the example AI is designed to play)
  1. The JNIBWAPI player should take control of the player's units and begin ordering units
    * Note: the game will freeze for one or two minutes the first time a new map is played as [BWTA](https://code.google.com/p/bwta/) has to analyze the map

# Your First JNIBWAPI Project #

_Assuming that you have followed the previous guide, you should have an extracted copy of JNIBWAPI_

  1. In your JNIBWAPI directory, copy `client-bridge-x86.dll` and `client-bridge-amd64.dll` from `release/` into the top level directory
    * Alternatively, you can change the Working Directory of any run configurations you create (eg. in step 3), to the `release` directory. This is preferable if you are changing the C++ code as it will output DLLs to the `release` directory.
  1. Run Eclipse as an Administrator, create a New Java Project, uncheck "Use default location" and set the location to be your JNIBWAPI folder. It should be able to determine the rest for itself. Click Finish.
  1. Expand the project and look in the package `jnibwapi`. Right click `ExampleAIClient.java`, select Run As, then select Java application.
  1. Ensure that the behaviour is as in the previous guide (you have just completed the equivalent of Step 7)
  1. You are now ready to duplicate `ExampleAIClient.java` to your own class and begin development. Take a look at the [documentation](documentation.md) to find the available commands.

Note that when using the provided .dll, certain files need to remain in the expected location. In particular `BWAPIEventListener.java` and `JNIBWAPI.java` should be left in the `jnibwapi` package. The core of your AI (equivalent to `ExampleAIClient.java`) can be anywhere, and is recommended to be outside the `jnibwapi` package.


# Troubleshooting #
## "Native code library failed to load" ##
If you receive the error "Native code library failed to load" when running your bot, it is likely a problem of using 32-bit or 64-bit Java. Try installing 32-bit Java and telling Eclipse or the batch file to use it.
To use a 32-bit JRE in Eclipse, go to: Run > Run Configurations > "JRE" tab > Installed JREs

## Freeze at the beginning of a match ##
If the game freezes for one or two minutes at the beginning of the match, it is because you're running a new map for the first time. [BWTA](https://code.google.com/p/bwta/) is used to analyze the map, which takes some time. This should only happen once on every map.

## "Failed to load AI module NULL" ##
Displayed inside StarCraft at the start of a match. This means that your  bot was unable to connect to the game. First check if it is
really running. Then try starting the game again a few times (this sometimes happens for no apparent reason).
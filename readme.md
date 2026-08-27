# About This Fork

## What?

This is version 1.006 Patch 3.1 (the same version that ships with NOLF Revival) with two things added:
- The "crouch toggle" feature from [the "crouch toggle" Modernizer on Mod DB](https://www.moddb.com/games/no-one-lives-forever/downloads/nolf-modernizer-with-crouch-toggle-v10)
- [Daniel Gibson's fixes](https://github.com/haekb/nolf1-modernizer/pull/55)

## Why?

The "crouch toggle" Modernizer on Mod DB doesn't work on some versions of Wine/Proton on Linux. (It blackscreens immediately.) That mod is based on Modernizer 1.006 Patch 4, which seems to be problematic. So I backported the crouch toggle on top of Patch 3.1, and it works.

## How?

The Modernizer source lacks good version control information, but we can deduce that version 1.006 Patch 3.1 probably corresponds to [commit 0bee38b](https://github.com/haekb/nolf1-modernizer/commit/0bee38bf0bf0debb884587ad9a8abc6886c3828c) by comparing the text files included in the released zip against their version histories on github.

The "crouch toggle" Modernizer on Mod DB comes with source code. `grep -nri crouch` gives us a list of lines that need changed.

Daniel Gibson's pull request is on github and can be cherry-picked.

## How to Use

- Put `MODERNIZERP3CROUCH.REZ` in your `<NOLF_install_directory>\Custom` directory.
- Run the launcher, go to "Advanced" > "Customize," remove `MODERNIZER.REZ`, and add `MODERNIZERP3CROUCH.REZ`. (If you have other mods, you'll need to remove and re-add all of them to preserve the ordering.)
- Edit `autoexec.cfg` by adding `AddAction CrouchLock 18` at the bottom of the block of lines that begin with `AddAction`.
- In game, you can now bind a key for crouch toggle under "Misc. Controls."

## Known Issue

This might cause trouble in multiplayer because it shares the same version number as 1.006 Patch 3.1.
Make sure not to start multiplayer games with people using other Modernizer forks.

(I don't want to number this "Patch 5," because it's based on Patch 3. And that would likely collide with some other fork anyway.)

## How to Build

Since the upstream build instructions are kinda lacking, I'm spelling the process out in better detail:

You need MS Visual Studio. Upstream was built with Visual Studio 2019, which is no longer available. I was able to build with Visual Studio 2026. Hopefully it will continue to build in future versions.

Add the following components to MS Visual Studio:
- C++/CLI support for v142 build tools
- MSVC v142 - VS2019 C++ x64/x86 build tools
- C++ ... ATL for v142 build tools (x86 & x64) (not sure if you really need this)
- C++ ... MFC for v142 build tools (x86 & x64)
- C++ Universal Windows Platform support for ARM64/ARM64EC build tools (Yes, you need this even though it says it's for ARM. Otherwise you will get “VC Project Cache error: Cannot establish connection to vcxprojreader.exe process: error code ‘80004005’")

Open `NOLF\NOLF.sln` in Visual Studio.

Select the "Final Release" build solution. (Per the original documentation, only "Release" and "Final Release" can be used in rez files. Per Modernizer's readme, only "Debug" and "Final Release" are set up to build.)

Build solution.

Copy these 3 files somewhere:
- `NOLF\ClientRes\Release\CRes.dll`
- `NOLF\ClientShellDLL\Final_Release\CShell.dll`
- `NOLF\ObjectDLL\Final_Release\Object.lto`

Use `TOOLS\lithrez.exe` to unpack the `MODERNIZER.REZ` that shipped with NOLF Revival.
The command is `lithrez.exe x MODERNIZER.REZ output_directory`.

Replace the binaries in the unpacked directory with your own.
(Since this is Windows, it shouldn't be case sensitive, but rename them to all caps just to be safe.)

Use `TOOLS\lithrez.exe` to pack your new rez file. The command is `lithrez.exe c NEWNAME.REZ input_directory`.

## TODO Someday...

Figure out what's wrong with Patch 4 and fix it, or cherry pick everything else that's not problematic.

## Original Readme starts now:


# No One Lives Forever Modernizer

(removed continuous integration badges)

The goal of NOLF Modernizer is to help fix some long standing bugs, and update some more outdated features of the game.

## Features

 - Working multiplayer out of the box
 - Cap framerate during menus and gameplay to 60fps
 - Fixed the mouse stutter
 - Optimized performance in select cases
 - Jukebox to play some of your favourite in-game tunes
 - Supports the Game of the Year edition

## Patch 1

 - Fixed a bug with defusing bombs and activating some switches.

## Patch 2

- Added `NoRawInput` to disable mouse raw input.
- Fixed missing continue button on mission summary screen.
- Improved loading screen time (...that I broke, oops!)
- Fixed some invisible impassible geometry.
- Added a experimental developer console, can be toggled on/off with tilde. (`~`)
- Fixed a crash in the weapons hotkey screen.

## Patch 3

- Bumped version to 1.006.
- Patched out GameSpy from dedicated/hosted servers and the server browser.
- Fixed a bug in ai path finding causing values to not always be accurate.
- Fixed a silent out of range bug that could cause enemies to disappear and travel to a nearby galaxy at FTL speed!
- Made the console key rebindable. (It's at the very bottom of the custom controls list.)
- Added Big Head Mode! It's currently a little buggy, but humourous. Check the console command list on how to enable it.
- Included some patched binaries to help improve compatibility. 
- Added a windowed mode toggle to the display options.
- Added anisotropic filtering to advanced performance options.
- Fixed shadows disappearing between cutscenes and saved games.
- Added a "Blackscreen Fix" work around for Intel HD graphics chips in Display options.

## Patch 4
- Re-worked jukebox into an attribute file. 
- Added missing ambient track for the Main Theme to the Jukebox.
- Added some jukebox strings to CRes.dll

## Additional Config/Console Commands

The following are new config/console commands:
  - `FramerateLock`           - INT - Locks the framerate if the value is 1. (Default is 1)
  - `ShowFramerate`           - INT - Displays the framerate if the value is 1. (Default is 0)
  - `OldMouseLook`            - INT - Uses the old mouse look code if the value is 1. (Default is 0)
  - `NoFunMenus`              - INT - Only displays the default main menu if the value is 1. (Default is 0)
  - `RestrictCinematicsTo4x3` - INT - Adds black bars onto the sides of cinematics on a non 4x3 resolution, if the value is 1. (Default is 0)
  - `QuickSwitch`             - INT - Instantly switch between weapons, if the value is 1. (Default is 0)
  - `UIScale`                 - FLOAT - Scales the in-game HUD. (Default is 0.5)
  - `UseGotyMenu`             - INT - Switch between the original main menu and the GOTY version. (Default is based on your version)
  - `NoRawInput`              - INT - Disables raw mouse input. (Default is 0)
  - `ConsoleBackdrop`         - INT - Swap between 3 different console backdrops: (0) demo, (1) blanktag, and (2) black. (Default is 0)
  - `BigHeadMode`             - INT - Enable or Disable BigHeadMode. (Default is 0)
  - `DisplayTriggers`         - INT - Shows or Hides a physical representation of level triggers. (Default is 0)
  - `EnableScreenTinting`     - INT - Enable or disable native screen tinting. Disabling will activate an alternate screen tinting. (Default is 1)
  - `EnableLightScale`        - INT - Enable or disable light scaling. (Default is 1)


Most of these commands are also available in their respective options menu.

## Building

You can now compile it using Visual Studio 2019 (Requires C++ and MFC), thanks to the NOLF2's sdk including some key files. They're all included and ready to compile.

The following build configurations are setup to build: 
 - Debug
 - Final Release

If you experience any issues, feel free to open an issue.

## Contributing

Simply fork and submit a PR (preferbly with a matching issue ticket!) 

Try to keep to the original coding style, with descriptive commit messages. (Unlike some of my original commits!)

## Localization

There have been community efforts to localize Modernizer into other languages. And while I don't have the time to directly help in these efforts, here are some steps you can do you to localize and distribute your localization patch!

First off modify the string table located in CRes.dll (Client Resource). This can be done with the latest version of Visual Studio 2019 and this source code. You may also attempt to use other programs to modify the string table directly in the dll. 

Secondly there are some additional strings in Jukebox.txt located here: https://github.com/haekb/nolf1-modernizer/blob/master/ASSETS/Attributes/Jukebox.txt) 

Finally compile your new CRes.dll and the modified attribute file into its own rez using LithRez.exe (from the SDK) and make sure it loads after Modernizer.rez.

## D3D and 2048 pixel limit

You can grab a patched d3d.ren, and d3dim700.dll from the ASSETS folder or from a release of NOLF Modernizer.

## DGVoodoo2

I don't recommend using this application. I've fixed the majority of the slowdowns caused by old d3d techniques. And (at least on my machine) DGVoodoo2 would cause dynamic lights to absolutely destroy my framerate!

## License

This code is still bound to the original EULA found in the NO ONE LIVES FOREVER Source Code v1.003. This can be viewed in the readme.txt file.

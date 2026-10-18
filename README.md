# Bunnymod XT (Mario cam fork)

This is [Bunnymod XT](https://github.com/YaLTeR/BunnymodXT) with the Mario cam controls for blindfolded runs added: turning the view exactly 45 degrees left or right, and a reset to the view you get when you spawn in c1a0 (looking into the door). Everything else is Bunnymod XT as it is.

The controls do nothing until you turn them on with `bxt_mariocam 1`, so with `bxt_mariocam 0` (the default) this is plain Bunnymod XT, for the Vanilla category. Bunnymod XT records the cvar and every command in your demos, so they show which way you played.

| Command or cvar | What it does |
| --- | --- |
| `bxt_mariocam 0/1` | Turns the Mario cam controls off (default) or on. |
| `bxt_mariocam_left` | Turns the view 45 degrees left (the pitch stays). |
| `bxt_mariocam_right` | Turns the view 45 degrees right (the pitch stays). |
| `bxt_mariocam_reset` | Sets the view to the home view. |
| `bxt_mariocam_home "<pitch> <yaw>"` | The home view, `"0 180"` by default: c1a0's spawn. |
| `bxt_mariocam_sethome` | Makes the current view the home view (and prints the line to keep it in your config). |

The turns use the same rounding the game uses for `+left` / `+right`, so 45 degree turns stay exact: eight of them bring you back to where you started.

For example, in a config you `exec` before the run:

    bxt_mariocam 1
    bind home bxt_mariocam_reset
    bind leftarrow bxt_mariocam_left
    bind rightarrow bxt_mariocam_right

Downloads: [Releases](../../releases/latest). Install it like Bunnymod XT (below), and keep the launcher from updating it back to the official build: in the `.bat` that starts the game, put `::` in front of the line that calls `update.bat`.

### VAC BAN WARNING: Do NOT connect to servers with this injected, or you might get VAC banned!

Bunnymod XT (BXT for short) is a cross-platform tool that provides speedrunning and TAS-related features for GoldSource games such as Half-Life and its modifications.

Used by hundreds if not thousands of speedrunners worldwide.

Bunnymod XT provides state-of-the-art speedrunning and TASing features and utilities, ranging from detailed HUDs to advanced autostrafing. It is a successor to the obsoleted hlspbunny and Bunnymod Pro.

## Why?

Despite the "mod" in Bunnymod XT, this is not a mod in the usual sense. Most Half-Life modifications work by modifying the Half-Life SDK and distributing the resulting DLLs. This includes the deprecated Bunnymod Pro. The downside is that while you can "mod" Half-Life itself, you cannot "mod" another Half-Life mod. This is a serious limitation for speedrunners intending to speedrun Half-Life mods and expansions.

To rectify this, Bunnymod XT injects into the Half-Life process while leaving every file on the disk intact. This means all modifications are done on-the-fly in RAM. This also means Bunnymod XT supports a wider range of Half-Life engines, from WON to the latest Steam.

## Installation

### Windows

The instructions are described [here](https://github.com/YaLTeR/BunnymodXT/wiki).

### Linux

Use the [Bunnymod XT Launcher](https://github.com/YaLTeR/bxt-launcher).

## Documentation

[On the Wiki pages](https://github.com/YaLTeR/BunnymodXT/wiki).

### Environment variables

- **BXT_SCRIPT** - if set to a filename of a hltas script, loads the non-shared RNG from that script on load.
- **BXT_LOGFILE** - if set, logs all Bunnymod XT messages into a file with that filename.
- **SPTLIB_DEBUG** - if set to 1, logs all dlopen, dlclose and dlsym calls.
- **BXT_DISABLE_DEBUG_CONSOLE** - if set, disables the Bunnymod XT debug console.
- **BXT_DISABLE_DISCORD_RPC** - if set, disables the Discord RPC module.
- **BXT_DISABLE_VSYNC** - if set, disables the V-Sync on initialization (Windows-only).

## Building

### Windows

Building on Windows requires

- Visual Studio 2019 or 2022
- [Boost](http://www.boost.org/)
- [Rust](https://rustup.rs/)
  - The `i686-pc-windows-msvc` target must be installed. You can do that using `rustup target add i686-pc-windows-msvc`.

Run the following commands, replacing `path\to\boost\base\dir` with path to the Boost base directory:

    git clone https://github.com/YaLTeR/BunnymodXT --recurse-submodules
    cd BunnymodXT
    cmake -A Win32 -B build -DBOOST_ROOT=path\to\boost\base\dir -Wno-dev

Then compile the `ALL_BUILD` project from the generated VS solution in the `build` folder.

If you want to make a release build, you need to specify `-DCMAKE_BUILD_TYPE=Release` in the `cmake` command line arguments. This is needed because the Rust CMake module uses that variable to determine whether to build the crate in release or debug mode.

### Linux

#### GNOME Builder
1. Set up Flathub by following the [guide](https://flatpak.org/setup/) for your distribution.
1. Install [GNOME Builder](https://flathub.org/apps/details/org.gnome.Builder).
1. Open GNOME Builder.
1. Press the Clone Repository button, enter `https://github.com/YaLTeR/BunnymodXT.git` and press Clone Project. Wait until it finishes.

   The cloning window should close, and a new window with the BunnymodXT project should open.
1. If Builder prompts you to install missing SDKs, press Install and wait for the process to finish. It will take a while. You can monitor the progress by pressing the circle in the top-right.
1. Click on the bar at the top-center which says BunnymodXT, and click the Build button.
1. Once the build finishes, in the same bar menu click the Export Bundle button. The file manager will open in a path that looks like `gnome-builder/projects/BunnymodXT/flatpak/staging/x86_64-master`. Navigate up to the `BunnymodXT` folder, then down to `builds/rs.bxt.BunnymodXT.json-...` where you will find the built `libBunnymodXT.so`.
1. Now you can make some changes to the code and press Build, then grab `libBunnymodXT.so` from the same folder.

#### Manually

Building on Linux requires

- A recent GCC or Clang toolchain
- Boost
- Rust: either from your distribution's packages, or from [rustup](https://rustup.rs/).
  - The `i686-unknown-linux-gnu` target must be installed. You can do that using `rustup target add i686-unknown-linux-gnu`.

Many of these dependencies can be installed from a package manager.

    git clone https://github.com/YaLTeR/BunnymodXT --recurse-submodules
    cd BunnymodXT
    cmake -B build -Wno-dev
    make -C build

Note that `-DBOOST_ROOT` is not required as CMake should be able to find its location in your system. In case it couldn't, you need to specify it manually like the case on Windows.

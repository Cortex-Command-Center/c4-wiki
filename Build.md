# General

These are things are platform independent.

You need to install [Git](https://git-scm.com/downloads) to be able to clone the repo as an alternative you could download it as zip.

 You need both the [Engine](https://github.com/Cortex-Command-Center/Cortex-Command-Community-Continuation-Engine) and [Data](https://github.com/Cortex-Command-Center/Cortex-Command-Community-Continuation-Data) to be be in the same directory.

**Note : don't change the name of the directories/folders**

You should have 2  directories/folders next to each others, one named `Cortex-Command-Community-Continuation-Data` containing the data and another `Cortex-Command-Community-Continuation-Engine` containing the engine.\

To clone the repos run the commands below :

```
git clone https://github.com/Cortex-Command-Center/Cortex-Command-Community-Continuation-Data.git
git clone https://github.com/Cortex-Command-Center/Cortex-Command-Community-Continuation-Engine.git
```

 After you've done that move to section of the platform you're currently on.



## Windows

1. Install the necessary tools:
   - You'll probably want [Visual Studio Community Edition](https://visualstudio.microsoft.com/downloads/) (build supports both 2017 and 2019 versions).
   
   - You also need to have both x86 and x64 versions of the [Visual C++ Redistributable for Visual Studio 2017](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) installed in order to run the compiled builds.
   
   - You may also want to check out the list of recommended Visual Studio plugins [here](https://github.com/Cortex-Command-Center/Cortex-Command-Community-Continuation-Engine/wiki).
     
     
2. Copy the following libraries from `Cortex-Command-Community-Continuation-Engine\external\lib\` into the **Data Repository**:
* `lua51.dll`
* `lua51-64.dll`
* `fmod.dll`
* `fmodL.dll`
  
  
  Now you're ready to build and launch the game.
  Simply open `RTEA.sln` with Visual Studio, choose your target platform (x86 or x64) and configuration, and run the project.
* Use `Debug Full` for debugging with all visual elements enabled (builds fast, runs very slow).
* Use `Debug Minimal` for debugging with all visual elements disabled (builds fast, runs slightly faster).
* Use `Debug Release` for a debugger-enabled release build (builds slow, runs almost as fast as Final).
* Use `Final` to build release executable.
  The first build will take a while, but future ones should be quicker.
  If you want to use an IDE other than Visual Studio, you will have to build using meson. Check the [Linux](#building) and [Installing Dependencies](#installing-dependencies) section for pointers.

**Windows 10 (64-bit) without Visual Studio**

- [Windows SDK](https://developer.microsoft.com/de-de/windows/downloads/windows-10-sdk/)
- [Clang Toolset](https://github.com/llvm/llvm-project/releases) (Grab the latest LLVM-...-win64.exe)
- [meson](https://github.com/mesonbuild/meson/releases) (documentation [here](https://www.mesonbuild.com))
- (optional) Visual Studio for the Developer Consoles since setup otherwise may be unnecessarily hard

## Linux

### Dependencies

You need to install the development (they may be suffixed with `-dev` or `-devel`) versions of the following libraries:

* `g++>=8.1` (needs to support c++17 filesystem)
* `allegro4`
* `loadpng`
* `flac`
* `luajit`
* `lua5.2` (may be `lua52` on some systems)
* `minizip`
* `lz4>=1.9.0`
* `libpng`
* `libX11`
* [`meson`](https://www.mesonbuild.com)`>= 0.53` (If your distro doesn't have a recent version of meson, use the pip version instead)
* `boost>=1.55`
* (optional) `xmessage`

#### Arch based distros:

```
sudo pacman -S allegro4 \
boost \
flac \
luajit \
lua52 \
minizip \
lz4 \
libpng \
libx11 \
xorg-xmessage \
meson \
ninja \
base-devel
```

#### Debian based distros:

```
sudo apt install -y build-essential \
 libboost-dev \
 liballegro4-dev \
 libloadpng4-dev \
 libflac++-dev \
 libluajit-5.1-dev \
 liblua5.2-dev \
 libminizip-dev \
 liblz4-dev \
 libpng++-dev \
 libx11-dev \
 ninja-build \
 meson
```

### Build

1. Open a terminal in the Engine Repository.
2. `meson build` or `meson --buildtype=debug build` for debug build (default is release build)
3. `ninja -C build`
4. (optional) `sudo ninja install -C build` (To uninstall later, keep the build directory intact. The game can then be uninstalled by `sudo ninja uninstall -C build`)
5. If you skipped step 4 go to [Running](#Running)
   
   
- If you want to change the build type afterwards, you can use `meson configure --buildtype {release or debug}` in the build directory or create a secondary build directory as in Step 4.

- There are also additional build options documented in the [wiki](https://github.com/Cortex-Command-Center/Cortex-Command-Community-Continuation-Engine/wiki) as well as through running `meson configure` in the build directory.

### Running

**Only follow if you haven't installed with ninja like in step 4 of [Build](#Build)**

#### Normal

1. Copy (or link, might be preferable for testing builds) `build/CortexCommand` or `build/CortexCommand_debug` (depending on if you made a debug build) and all `libfmod` files from `external/lib/linux/x86_64` into the **Data Repository**.
   
   ```
   cd Cortex-Command-Community-Continuation-Data
   ln -s ../Cortex-Command-Community-Continuation-Engine/build/CortexCommand .
   ln -s ../Cortex-Command-Community-Continuation-Engine/external/lib/linux/x86_64/libfmod.so* .
   ```

2. Copy `Scenes.rte` and `Metagames.rte` from your purchased copy of Cortex Command into **Data Repository**.

3. Run `./CortexCommand` or `./CortexCommand_debug` in the **Data Repository**.

#### Using deploy-tool

**deploy-tool can be used to create a portable zip file of the game**

1. Clone deploy-tool: 
   
   ```
   git clone https://github.com/Cortex-Command-Center/deploy-tool.git
   ```

2. Install dependencies:
   
   ```
   sudo apt install -y zip \
             zsync \
             unzip \
             patchelf
   sudo pip3 install appimage-builder
   wget https://github.com/vlang/v/releases/latest/download/v_linux.zip
   unzip v_linux.zip
   sudo ln -s $PWD/v/v /usr/bin/v
   ```

3. Build deploy-tool:
   
   ```
   cd deploy-tool
   ./build_deploy.sh
   ```

4. Copy (or link, might be preferable for testing builds) `build/CortexCommand` or `build/CortexCommand_debug` into the **binaries inside deploy-tool** :
   
   ```
   ln -s ../Cortex-Command-Community-Continuation-Engine/build/CortexCommand ./binaries/
   ```

5. Run deploy-tool:
   
   ```
   ./c4-deployment-tool
   ```
- A zip file should be created inside of it an `appimage` which you run the game with.

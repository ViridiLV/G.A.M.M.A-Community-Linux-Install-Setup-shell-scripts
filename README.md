# S.T.A.L.K.E.R G.A.M.M.A Community Linux Install/Setup shell scripts
(STILL EXPERIMENTAL AND WORK IN PROGRESS)

Community made scripts made to semi-automatically install gamma and  set up a bottle to play the game on Linux systems.

Includes a step-by-step guide.

# Guide
## Step 0 - Obtaining dependencies to run the scripts

For scripts to work it will need libraries and packages for funcitons that the scripts calls for.

- **For Ubuntu/Debian systems:**
```
apt install flatpak curl tar git
```

- **For Arch-based systems:**
```
sudo pacman -S flatpak curl tar git
```

## Step 1 - getting the scripts

Scripts are maintained in a rolling release manner, meaning you get them straight from the github repo.
Version numbers inside the scripts are informative for troubleshooting logs.

- **1.1** Open a terminal instance in the main directory where your gamma and anomaly files will be.

  Then make a folder and `cd` into it in your terminal.

  For example:
  ```
  cd /home/$USER/Games/GAMMA
  ```
- **1.2.** Download the script file:

  ```
  curl -O https://raw.githubusercontent.com/ViridiLV/gamma-scripts/refs/heads/main/gamma-scripts.sh
  ```

- **1.3.** Make it executable(this is optional if you choose to execute from already open terminal)

  ```
  chmod -x gamma-scripts.sh
  ```
- **1.4.** Run the script

  - **a)** 
  ```
  bash gamma-scripts.sh
  ```
  - **b)** via "Open in  terminal" of your distro's GUI file manager right click/open menu
    
## Step 2 - Installing the game

 - **INSTALL** - to install the game, type in the number that corresponds to the option "Install game", then press Enter.

  The script will download latest release from [https://github.com/FaithBeam/stalker-gamma-cli] 
  
  After that the script will proceed to install the game using it. 
  
  The folder structure of your chosen GAMMA main folder should look something like this:
  ```
  ~/Games/GAMMA$ tree -L 1
  .
  ├── Anomaly/
  ├── cache/
  ├── GAMMA/
  ├── gamma-scripts.sh
  ├── logs/
  └── stalker-gamma.AppImage
  ```
## Step 3 - Set up the game

Step 2 only lets you obtain game files, as if you installed the game on Windows, however this does not enable you to play GAMMA on Linux.
To play GAMMA on Linux you need to set up a working wine/proton compability layer with the needed dependencies.

This process often brings in a lot of user error and difficulty for newcomers.

The setup script aims to streamline this  process by scripting the set up of flatpak Bottles and making a bottle with dependencies for you with as little amount of user input as possible.

  - **3.1** With the script open and idle, type in the number that corresponds the option "setup GAMMA", then press Enter.
  
  - **3.2** If you don't have flatpak bottles installed, the script will attempt to install flatpak bottles.

      <img alt="image" src="https://github.com/ViridiLV/gamma-scripts/blob/main/Documentation/Pictures/bottles_install_ilu.png" />
  
      It will ask permission to do so, simply type `y` and hit enter to allow the installation of Bottles.
  
  - **3.3.** If Bottles does not have `filesystem=host` access, it may prompt you to **input sudo password**. 
  
    Not having flatpak permissions to acess game files may cause missing QT.dll errors later.
  
  - **3.4** Then, if the bottles just got installed and was not installed and set up previously, the script will **prompt you to open Bottles and complete initial setup**.
    
       <img width="661" height="121" alt="image" src="https://github.com/ViridiLV/gamma-scripts/blob/main/Documentation/Pictures/setup_bottles_ilustration.png" />
      
    - Open the flatpak Bottles and you will be greeted with a Welcome screen that guides you through the inital bottles setup
    
       <img alt="image" src="https://github.com/ViridiLV/gamma-scripts/blob/main/Documentation/Pictures/bottles_greet.png" />
       
    - Click next/continue until the intial setup is done:
      
       <img alt="image" src="https://github.com/ViridiLV/gamma-scripts/blob/main/Documentation/Pictures/bottles_init_finish.png" />
  
    - Once initial setup is done, Press `Start using Bottles`, press `X` to close bottles, then select the script terminal window and enter anything to continue
  
  - **3.5** When prompted, press `OK` on the small pop up screens.

    <img alt="image" src="https://github.com/ViridiLV/gamma-scripts/blob/main/Documentation/Pictures/dll1_ilu.png" />
  
    You may have a couple of small pop up windows saying ".dll registered succesfully".
  
    Simply press `OK`, or hit enter to continue.
  
  - **3.6.** The script will return to the action selection screen, at this point the bottle is set up.

    You can launch `ModOrganizer` from the `StalkerGAMMA` bottle in Bottles to start playing the game!
    
## Step 4 - Running the game


 - **a)** Open bottles "StalkerGAMMA", press play on Program "ModOrganizer"

 - **b)** Use a launcher, script or terminal command:
   ```
      flatpak run --command=bottles-cli com.usebottles.bottles run -b "StalkerGAMMA" -p "ModOrganizer"
   ```

## Step 4 - Troubleshooting

The `logs` folder contains a log file of the terminal output.

- **Bottle setup hangs during winetricks/dependency setup**

Seems to occur on Arch/Hyprland setups, for some reason the pop up window(regsvr32.exe, see step 3.5 of the install guide) appears in task process list(like `btop` or any other monitor sofware) but does not appear as a visual window, thus you can't hit OK, and continue on with the setup of the quartz .dll part of the winetricks command.
The issue seems to appear only when the command is ran via the script, so you should be able to run the winetricks manually.
To remedy, try copy pasting this in the a new terminal instance:
```
BOTTLE_NAME="StalkerGAMMA" &&
RUNNER_NAME="ge-proton9-20" &&
BOTTLES_RUNNER_PATH=".var/app/com.usebottles.bottles/data/bottles/runners" &&
BOTTLES_RUNNER_WINE="$BOTTLES_RUNNER_PATH/$RUNNER_NAME/files/bin/wine" &&
BOTTLES_PREFIX_PATH=".var/app/com.usebottles.bottles/data/bottles/bottles/$BOTTLE_NAME" &&
BOTTLES_RUNNER_WINETRICKS="$BOTTLES_RUNNER_PATH/$RUNNER_NAME/protonfixes/winetricks" &&
WINE=~/"$BOTTLES_RUNNER_WINE" WINEPREFIX=~/"$BOTTLES_PREFIX_PATH" ~/$BOTTLES_RUNNER_WINETRICKS cmd d3dx9 dx8vb d3dcompiler_42 d3dcompiler_43 d3dcompiler_46 d3dcompiler_47 d3dx10_43 d3dx10 d3dx11_42 d3dx11_43 dxvk quartz
```
This was the last step of the setup gamma scripted actions, so your bottle setup should be fine and you should be able to play the game.

## To Do list - planned changes and features

- ~~migrate wget to curl~~
- add feature - automated save and settings file backup via the script
- ~~add feature - update/download latest stalker-gamma-cli only~~
- add feature - stalker-gamma-cli update check and update apply via the script
- install game option available only if no game detected
- ~~detect and delete old bottle if "setup gamma" ran again~~
  
## Acknowledgements

Special thanks to:

- [Red007Master](https://github.com/Red007Master)
  (Code cleanup, log functionality, general code improvements)
- [FaithBeam](https://github.com/FaithBeam)
  (Collaboration - making a static latest build release for `stalker-gamma-cli`)


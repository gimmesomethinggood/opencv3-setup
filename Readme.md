# Disable ipv6 before going further

```bash
add lines to /etc/sysctl.conf

net.ipv6.conf.all.disable_ipv6=1
net.ipv6.conf.default.disable_ipv6=1
net.ipv6.conf.lo.disable_ipv6=1
net.ipv6.conf.eth0.disable_ipv6=1

Do sudo sysctl -p
```

## Raspberry Pi Menu Driven OpenCV 3 Compile from Source Script
#### Whiptail menu enabled script to help compile opencv3 from source



![cv3-install-menu](https://github.com/pageauc/opencv3-setup/blob/master/menu.png)

## Quick Install
For system requirements See [***Prerequisites***](https://github.com/pageauc/opencv3-setup#prerequisites)   
  
***Step1*** - Use mouse to highlight command below, Then right click copy on highlighted command     
***Step2*** - On a logged in RPI SSH terminal session right click paste then Enter to Run
[***setup.sh***](https://github.com/pageauc/opencv3-setup/blob/master/setup.sh) script

    curl -L https://raw.github.com/pageauc/opencv3-setup/master/setup.sh | bash 

The curl comand will run the GitHub [***setup.sh***](https://github.com/pageauc/opencv3-setup/blob/master/setup.sh)
script that will install files and configure into the ***~/opencv3-setup*** folder.
If you want to review code before running see [***Manual Install***](https://github.com/pageauc/opencv3-setup#manual-install)

## How To Run Menu

    cd ~/opencv3-setup
    ./cv3-install-menu.sh

Start at Step ***1 UPDATE*** menu pick and follow instructions. You will be prompted to optionally reboot.    
For details see [***How to Run Menu Picks***](https://github.com/pageauc/opencv3-setup#how-to-run-menu-picks)   
To change cv3_tmp folder location or opencv version see
[***How to Change Configuration***](https://github.com/pageauc/opencv3-setup#how-to-change-configuration)

## Operation
After update/upgrade and optional reboot is complete, Select **DEP** menu pick. If there are no problems,
you will be prompted with a simple y/n to continue without having to return to the main menu. If there are
issues that need to be resolved you can continue by selecting the appropriate main menu pick.

The [***cv3-install-menu.sh***](https://github.com/pageauc/opencv3-setup/blob/master/cv3-install-menu.sh) script will

* Online validate that ***OPENCV_VER*** variable setting is correct
* Check if ***INSTALL_DIR*** variable exists and points to a Non Fat File System.
* Creates ***cv3-log.txt*** file If It Does Not Exist. Records system information, date/times of steps including execution time.
* ***UPDATE*** menu pick to update/upgrade Raspbian. Prompts for optional reboot.
* ***DEP*** menu pick to Install build dependencies, Download opencv3 source zip files and unzip
* Auto Detect RPI3 and set NEON compile directive for cmake to enhance cv3 performance. Off for non RPI 3's
* ***COMPILE*** menu pick will Run ***cmake*** to check and configure build
* Temporarily Increase Swap memory to 1024 MB During make. Returned to original state after make.
* Auto Detect Total RAM memory and set compile cores. -j2 for 1 GB, -j1 for the Rest
* Run ***make*** to Compile opencv3 source code
* ***INSTALL*** Menu pick will Run ***make install*** to install new opencv python ***.so*** files to production.
* Optional run ***make clean*** to clear build directory to force full recompile.
* ***DELETE*** menu pick to optionally recover disk space by deleting the ***tmp_cv3*** folder containing
opencv source, build files and folders.
* ***SETTINGS*** menu pick to nano edit the [***cv3-install-menu.conf***](https://github.com/pageauc/opencv3-setup/blob/master/cv3-install-menu.conf) file

## Prerequisites
* Basic knowledge of unix terminal commands.
There are some optional configuration steps that
must be done manually using nano.
* Patience since this will take a few hours
* Recent Jessie or Stretch Raspbian Release
* Working RPI 
* Working Internet connected to RPI WIFI or RJ45 network cable
* Recommended min 16GB SD card with at least 6 GB Free.
If Free disk space is low or You have a smaller system SD card.
You can mount Non fat USB stick or hard disk see
[***How to Change Location of Temporary Working Folder***](https://github.com/pageauc/opencv3-setup#how-to-change-location-of-temporary-working-folder) 

## Manual Install
From a logged in RPI SSH session or console terminal perform the following.
This will allow you to review the code before installing.

    cd ~
    wget -o setup.sh https://raw.github.com/pageauc/opencv3-setup/master/setup.sh
    more setup.sh
    chmod +x setup.sh
    ./setup.sh
    rm setup.sh
    cd ~/opencv3-setup
    ./cv3-install-menu.sh

## How to Change Configuration
Run the ***SETTINGS*** menu pick or Edit ***cv3-install-menu.conf*** file using nano per the following commands.

    cd ~/opencv3-setup
    nano cv3-install-menu.conf
 
Press ctrl-x y to save changes and exit nano
 
### How to Change OpenCV Version
Edit variable OPENCV_VER='3.4.2' and change to a valid version per information at  
https://github.com/opencv/opencv/releases The version number will be verified at launch
against repo at https://github.com/Itseez/opencv/archive/    
See  https://github.com/opencv/opencv/releases    
and https://github.com/opencv/opencv_contrib/releases for valid zip versions    

### How to Change Location of Temporary Working Folder
***cv3-menu-install.sh*** will create a working folder per
***INSTALL_DIR*** variable. Default is ***/home/pi/tmp_cv3***. 
This folder will store downloaded opencv source and build files.
For a Full Build on a New OS, it is recommended you have a minimum 16GB SD card with at least 6GB free.
Less space may be needed depending on what dependencies are already installed.   
To check free disk space run

    df -h

You can recover most disk space after the build/install by running the DELETE menu pick. 
If there is not enough room on the system SD you can point the ***INSTALL_DIR*** to
USB Stick or disk drive media.  
***IMPORTANT:*** The USB memory stick or disk media must ***NOT be formatted as FAT***
since it does not support symbolic links that are needed to compile opencv.
Use a unix format like ext4 or microsoft NTFS format to avoid a failed make compile.   
For details see 
[***How To Mount External USB Storage***](https://github.com/pageauc/opencv3-setup#how-to-mount-external-usb-storage)

ctrl-x y to save changes and exit. Run ***cv3-install.menu.sh***.
The script will create the temporary working folder at the designated location. 
 
## RAM Memory
If RPI has 1GB of RAM memory make will use 2 cores -j2, otherwise 1 core -j1
will be used. Both will have Swap Memory temporarily set to 1024 MB during
the make compile process step. At the end of the compile the original Swap
config will be returned.

## How to Run Menu Picks
From the main menu select ***1 UPDATE* menu pick. Follow Instructions that
will guide you through the menu steps.  If there are errors you can
exit to the terminal to review output messages.  Problems will most likely
be related to Disk Space or Memory problems.    

You will be asked to reboot during some installation steps.
If you answer yes on successful completion of a step, you will be
sent to the next step otherwise you will be sent to the terminal
to review errors or back to the main menu as appropriate.
You may see warnings or not found messages and this is normal.  
 Eg tesseract (OCR) Not Found.

Users will be prompted to review output for errors and elect to continue.
You can repeat a particular step from the menu if required after
correcting or resolving any issues or errors.

Once compiling starts it will show you a percent progress.  ***Note*** If for some reason
the compile is interrupted, You can restart the compile Menu again and it will quickly
scan progress and will continue compile where it left off (was interrupted).
If a make clean is done (per menu prompt) then a full compile will restart from beginning again.

## Logging
A log file called ***cv3_log.txt*** will be created to record system
information and the date/time and details for each operation.  
Review the log to check how long various steps took to complete.
On exit you will be prompted if you want to clear the log file.
A backup copy will be saved to ***cv3_log.txt.bak***. A new log will be created
. The log can span multiple sessions. You must intentionally clear the log
from the LOG menu or per commands below to clear.

    cd ~/opencv3-setup
    cp cv3-log.txt cv3-log.txt.bak
    rm cv3-log.txt

## How To Mount External USB Storage
***IMPORTANT*** If there is limited space on the Raspbian SD card
you may want to change ***INSTALL_DIR*** variable to point to
an Non Fat external storage drive/device.

    cd ~/opencv3-setup
    nano cv3-install-menu.sh

Change the ***INSTALL_DIR*** variable to point to the desired mount location
or a symbolic link to the desired external storage device. ctrl-x y to save
and exit nano. See example mount commands below.

For more details on mounting external USB storage   
see https://www.htpcguides.com/properly-mount-usb-storage-raspberry-pi/

Sample commands to mount and use an external ntfs USB hard drive.
Plug ntfs formatted disk into a RPI USB slot.  From SSH or terminal session
use sample commands below. (Note) modify to suit your conditions.  
If you reboot you will need to redo sudo mount command unless you add
an entry to the /etc/fstab file (not covered here).

    cd ~
    sudo apt-get update
    sudo apt-get install ntfs-3g   # Make sure ntfs support installed
    sudo fdisk -l                  # will list drive if installed
    cd ~/
    mkdir /media/usb_1
    sudo mount -t ntfs-3g /dev/sda1 /media/usb_1
    df -h   #check space on usb device
    cd ~/opencv-setup
    nano cv3-install-menu.sh
    
In nano edit the variable per below. 

    INSTALL_DIR='/media/usb_1/tmp_cv3'
    
ctrl-x y to save changes and exit.   

## How to Test Build
To Test build for python or python3. See Example below
Start the required python interpreter by running the appropriate
command for python 2 or 3 below.

    python2

or

    python3    
    
At the >>> python prompt enter the following commands

    import cv2
    cv2.__version__

You should see output indicating the opencv version installed.
Press ctrl-d to exit python interpreter
Also check python3 opencv version using python3 interpreter.

If after a successful compile and install you see an older version of opencv
you may have to uninstall a previous apt-get version per the following

    sudo apt-get purge python-opencv
    
Check the opencv version again to see if the version is updated.  Otherwise
you can return the previous opencv per

    sudo apt-get install python-opencv    

See my other github repo at https://github.com/pageauc    
for various opencv motion and opencv camera projects.

## Credits
Script Steps Based on GitHub Repo
https://github.com/Tes3awy/OpenCV-3.2.0-Compiling-on-Raspberry-Pi

For Additional Details See https://github.com/pageauc/opencv3-setup

Have Fun
Claude Pageau   
YouTube Channel https://www.youtube.com/user/pageaucp   
GitHub Repo https://github.com/pageauc   


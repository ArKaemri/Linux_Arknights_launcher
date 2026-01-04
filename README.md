### Quick info before core
1. If you have other option than Android Studio to run Arknights, skip this project, don't be a masochist
2. For this to run you need:
  - Requirements? (not sure if have any impact): Linux Mint (wilma) specifically, Android 14.0 (device version in Android Studio), python-venv global
  - Requirements: regular color scheme (script works on color recognition, if you have color blind or custom color themes, it will most likely break), Android Studio, working Arknights (login once) (if getting 'out of storage' error when installing, "solution" is in 'Arknights Install' part), python global, Linux, Nvidia GPU (if you have AMD try Waydroid instead), MAA executable (how to get and use it will be discussed in 'MAA Setup' part, if you don't care about MAA, just ignore parts with it and ask ChatGPT to adapt script, just need to delete some parts)
3. Android emulators already finicky on Linux, playing Arknights on Linux using Android emulator is a total Karen, so even with all the setup it may not work, so no promises
4. This is not a good solution, just a crutches when nothing else works but you still want to play on PC (and don't want to dual boot), basically "best" out of the worst options
5. I will not discuss how to install Android Studio, adb or python
# Project Introduction
This project is a script that will open emulator, open Arknights, launch it when having 'data expired' problem and run MAA Assistant (terminal in headless mode). You will need to adapt some values in script, for MAA to work correctly, and the MAA files themselves to customize tasks. The only option that worked for me to run Arknights on Linux PC with Nvidia GPU was Android Studio with device 14.0 version (android-34/google_apis/x86_64/), it works quite reliably (crashed maybe 4 times in 5 months of everyday login twice, never during stage or IS, only when in base/menu's) and has easy MAA integration, the only problem is 'data expired' problem, that makes you click 'recover resources' (in Arknights) every time you launch emulator (sometimes twice in a row, sometimes this error doesn't happen, but 7/10 times it needs recover once, 2/10 twice, 1/10 just works), so I decided to make work-around to "fix" this problem to just automate everything -> press shortcut, leave and have Arknights to launch itself and do base automation with MAA. This project will go through: configuring Android emulator (needs same size while running automation before MAA, when you already logged in resize is fine, don't minimize, it is fine if other windows are in front of emulator + disabling on screen buttons so they won't appear when dragging operators gor general QoL), installing Arknights (with 'out of storage' error, even when you have 60GB left free), configuring MAA, binding to shortcut. 
# Android Setup
1. In Android Studio open 'Virtual Device Manager' (click on 3-dot in top right)
2. Click on '+' (Create Virtual Device), then press 'New Hardware Profile...'
3. Create profile as shown in 'device.png': Name - Arknights (if you choose different, you'll need to change it in the script), Type - Phone, Screen size - 5 inch, Resolution - 1920x1080, Ram - 8GB (may try less), Default skin - None, Only keep selected - Portrait/Landscape/Accelerometer/Gyroscope (for rotating device)
4. Create device with: API - 'API 34 "UpsideDownCake"; Android 14.0' (can try different, but this the only one that worked for me), Services - 'Google APIs', System Image - 'Google APIs Intel x86_64 Atom System Image', Internal storage - 64GB (may try less, adding more doesn't fix the problem, so 32GB should work), Expanded storage - dosen't change anything (so can choose None or leave 0.5GB), CPU cores - 4 or less, Ram - 8GB or less, VM heap size - 4GB or less, everything else just leave as it is
5. Launch emulator, go to System -> Navigation mode -> select '3-button navigation' (this with mainKeys=yes option will disable on screen buttons, so they won't appear when dragging operators from deployment zone on the map)
6. Install Arknights (now or after relaunch)
7. Open the 'config.ini' file at ~/.android/avd/Arknights.avd/ (or your name instead of Arknights) and check the values, for reference you can look at 'config.ini' that I use, the only parts you care about are: hw.lcd.height=1080, hw.lcd.width=1920, hw.mainKeys=yes
8. Save changes in config file, close the emulator and relaunch
# Arknights Install
1. Open default browser on emulator, install F-Droid, from it install Aurora Store, login as anonymous and search Arknights (if can't find/non-compatible, get another android version)
2. Then launch Arknights and let it to install the packages (it may close once, just reopen, if it keeps closing at same point, get another android version)
3. Once it will install everything and start decompressing files, you will get error 'ran out of storage', even when you have plenty left, just click ok and let it install resources (it will go back to about 95%)
4. Once it installs packages and starts decompressing resources again, click left corner 'clear cache' and select 'resources recover', it will recover about 1k resources and start decompressing again
5. Wait until it decompress about 20-30% and give same error, press ok -> wait to install packages, wait untill decompress 20%, click recover resources (it will recover about 3k resources)
6. Repeat step five but now it will decompress about 60%, so recover at about 55% (around 6k resources)
7. Repeat step five but now around 90%, this should be last time, it will start installing other packages again (about 1k instead starting 10k) and let you login (full recover is 9k)
- General idea - you want to 'recover resources' slightly before the storage error appears, increasing incrementally until there is no higher % to reach
# MAA Setup
1. Get MAA-cli executable from https://github.com/MaaAssistantArknights/maa-cli (get the release zip and extract maa file)
2. Give execution rights to the file (in folder where file is, in terminal run 'chmod +x ./maa)
3. Setup maa, instruction is at https://docs.maa.plus/en-us/manual/cli/usage.html, from what I remember you just need run in terminal 'maa install' and 'maa update' (or ./maa install if it doesn't work), setup file will be done manually
4. Open ~/.config/maa/profiles/default.json (create it, unless it already exist) and fill the info with example file (default.json) (check 'device' name, it shows on top bar of emulator when launched, if you play on EN, everything else can be the same)
5. Create the task files in ~/.config/maa/tasks/, name anything you want with extension .toml (can use JSON, but then you need to rewrite them) ('name' without .toml is used to launch the file with maa)
6. Fill in the .toml files, you can reuse the 'daily.toml' and 'recruit.toml' files I use, to customize files go to https://docs.maa.plus/en-us/protocol/integration.html#list-of-task-types
- Recruit file just does 3 recruits, using 7:40 to no guaranteed >3-stars and stops for robots/6-stars
- Daily file does same recruitment, visits friends, collect all base resources and spend drones on Combat Records, then gets credits from daily shop (it doesn't auto assigns operators to dormitories/facilities, doesn't touch clues and doesn't shop/fight, I prefer to do them manually, how to use those options just use url with tasks, but facility assign doesn't use presets)
# Bind Shortcut
Now when everything configured (have MAA .toml files, MAA executable, android emulator, Arknights launch atleast once):
1. Get the script from repo (pull or create .sh file and copy-paste the content, doesn't matter, you only need to have script, other files are change manually)
2. Give the autority to run the script (in folder where file is, in terminal run 'chmod +x ./auto_arknights.sh)
3. In 'auto_arknights.sh' change MAA_PATH to the path of 'maa' executable to where you put it, and the ANDROID_NAME to your android emulator name (if used the same as mine, skip the name change)
4. Go to Menu -> System Settings -> Keyboard -> Shortcuts -> Custom Shortcuts -> Add custom shortcut -> put in name, choose shortcut keys and give path to the './auto_arknights.sh'
- For example -> my script in ~/Scripts/ and in ~/.config/maa/tasks/ I have files 'daily.toml' and 'recruit.toml', so I create 2 shortcuts: 1) command - ~/Scripts/auto_arknights.sh daily, keys - CTRL+Super+1 2) command - ~/Scripts/auto_arknights.sh recruit, keys - CTRL+Super+2

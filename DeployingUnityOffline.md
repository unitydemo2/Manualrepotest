# Hello
## Hello
### Hello

#Hello
##Hello
###Hello


# Deploying Unity offline

The Unity Download Assistant supports offline deployment. This allows you to download all the necessary files for installing Unity, and to generate a script for repeating the same installation on other computers without internet access.

## Preparation

Run the Download Assistant, and install Unity as normal on one computer. This computer must have enough free disk space to download all the files.
Click the dropdown and select __Custom__, then choose the location you wish to download the files to.


![](../uploads/Main/DeployingUnityOffline-DownloadAssistant.png)

## Check you have everything you need

Open your PC's file manager, navigate to the custom location folder you specified earlier, and look for the `.sh` or `.bat` file inside that folder. Check the contents of this file. It should look similar to the following example:


![](../uploads/Main/DeployingUnityOffline-installsh.png)

## Deploying Unity to other computers

### Windows

1. Copy the entire folder to the target Windows PC, and run the supplied `.bat` file.
1. To avoid the Windows UAC prompt, run `install.bat` from the Administrator shell. In the __Start__ menu, search for `cmd.exe`, right-click, and select __Run as administrator__.
1. Navigate to the folder with the scripts. This will usually be in your Downloads folder (`cd C:\Users\[YourName]\Download\UnityPackages`).

###Mac
1. Copy the entire folder to the target Mac OS X machine, and run the supplied `.sh`  file.
Run `sudo install.sh`.
1. Navigate to the folder with the scripts. This will usually be in your Downloads folder (`cd ~/Downloads/UnityPackages`).
1. You can repeat these instructions as many times as you need to for each computer you wish to install Unity on.

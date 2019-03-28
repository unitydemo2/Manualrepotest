# Downloading and installing Unity

Download and install the Unity Editor from the [Unity download page](http://unity3d.com/download). 
The installer uses a Download Assistant and has detailed instructions to follow. If you want to download the Unity Editor using a torrent or install several versions of Unity at once, see [Torrent Download](#TorrentDownload), below.

## Unity Download Assistant

The Unity Download Assistant is a small executable program (approximately 1 MB in size) which lets you select which components of the Unity Editor you want to download and install.

If you're not sure which components you want to install, leave the default selections, click __Continue__, and follow the installer's instructions. 

![Unity Download Assistant (leave the default selections if you're not sure which to choose)](../uploads/Main/UnityDownloadAssistant_v52_75.png)

Note that on PC there is Microsoft Visual Studio Community 2015.

###


## Installing Unity without the Download Assistant

If you prefer, you can download and install all of the components separately, without using the Download Assistant. The components are normal installer executable programs and packages, so you may find it simpler, especially if you are a new Unity user, to use the Download Assistant. Some users, such as those wishing to automate deployment of Unity in an organization, may prefer to install from the command line.

### Installing Unity on Windows from command line

The following options can be used when installing the Unity Editor and other components from command line on Windows. Note that installer command line arguments are case-sensitive.

#### Unity Editor install

| | |
|:---|:---|
|__/S__|Performs a silent (no questions asked) install.|
|__/D=PATH__|Sets the default install directory. Useful when combined with the silent install option. Default folder is **C:\Program Files (x86)\Unity** (32-bit) or **C:\Program Files\Unity** (64-bit)|

**Example:**

````
UnitySetup64.exe /S /D=E:\Development\Unity
````

Install Unity silently to the folder E:\Development\Unity, which will be the root of the Unity installation. The Unity editor executable will in this case be installed in E:\Development\Unity\Editor\Unity.exe. "/D" argument must be last and without quotes, even if path contains spaces.

#### Unity Editor uninstall

To perform a silent uninstall, run `Uninstall.exe /S` (for example, from command line or a script).

Note that although the process will finish right away, it takes a few seconds before the files are actually removed. The reason for this is that the uninstaller is copied to a temporary location in order to be able to remove itself. Also, make sure the working directory is not inside the Unity install location, as it won't be able to remove the folder if this is the case.

#### Standard Assets install

Silently install Standard Assets. If specifying folder, use the Unity "root" folder (that is, the folder containing the Editor folder, and not where __Unity.exe__ is installed into).

````
UnityStandardAssetsSetup.exe /S /D=E:\Development\Unity
````

#### Example Project install

Silently install the Example Project. The default folder is **C:\Users\Public\Documentation\Unity Projects\Standard Assets Example Project**.

````
UnityExampleProjectSetup.exe /S /D=E:\Development\Unity
````


### Installing Unity on OS X from command line

The individual Unity installers are provided as .pkg files, which can be installed using `installer` as described below.

#### Unity Editor install

Installs into folder `/Applications/Unity` on the specified target volume:

````
sudo installer [-dumplog] -package Unity.pkg -target /
````

#### Standard Assets install

Installs into folder `/Applications/Unity/Standard Assets` on the specified volume:

````
sudo installer [-dumplog] -package StandardAssets.pkg -target /
````

#### Example Project install

Installs into folder `/Users/Shared/Unity/Standard-Assets` on the specified volume:

````
sudo installer [-dumplog] -package Examples.pkg -target /
````


<a name="#TorrentDownload"></a>
## Torrent Download

If you'd prefer to download Unity via a BitTorrent client, you can download get a torrent link from the [Unity download archive page](http://unity3d.com/get-unity/download/archive). Not all versions have a torrent download. If a version is available to download as a torrent, the option is presented as __Torrent download (Win+Mac)__ in the __Downloads__ dropdown menu.

![Downloading Unity via a Torrent](../uploads/Main/InstallingUnityTorrentDownload.png)


## Installing several versions at once

You can install as many versions of Unity as you like on the same computer. 

The installer on a Mac creates a folder called __Unity__, and overwrite any existing folder with this name -  however, if you rename the folder to something else before installing another versions, then both versions can exist on the same computer without problems. 

On a PC, the install folder is always named __Unity X.Y.Z[fp]W__, where the 'f' is for an official release, and 'p' is used to mark a patch release.

We strongly recommend that if you rename a Unity folder, you choose the new folder name logically (for example, add the version number to the end of the name). Note that any existing shortcuts, aliases and links to the offline docs may no longer point to the old version of Unity. This can be particularly confusing with the offline docs; if you suddenly find that browser bookmarks to the offline docs no longer work, then check that they have the right folder name in the URL.
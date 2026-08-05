# Void-gaming-tutorial
A tutorial for gaming on void Linux.

This tutorial aims to help those who have just installed void, and would like to game. For reference, I have an RTX 3060 and an i9 10900k, and this tutorial is primarily meant for NVIDIA users. This tutorial will assume that you have just completed installing void, so will go all the way from driver installation to playing games (but feel free to skip to the relevant bits for your install) A quick sidenote - I am using niri and noctalia with lightdm, and every game that I play works fine on this combo after following these steps.

# Repositories

Firstly, enable the multilib repository, non-free repository and multilib-nonfree repository. This will allow you to install 32bit packages, as well as "nonfree" packages (such as the nvidia driver). Here are the commands:

```shell
sudo xbps-install void-repo-nonfree void-repo-multilib void-repo-multilib-nonfree
```

```shell
sudo xbps-install -S
```

# Driver installation

For anybody who has an NVIDIA 16 series or newer card, install the following packages: **nvidia**, **nvidia-dkms**, **nvidia-libs** and **nvidia-libs-32bit**. The NVIDIA package will provide you with useful utilities such as nvidia-smi, and is essential for the driver to function. Nvidia-dkms will allow the nvidia driver to be built up against your kernel whenever it updates. Whilst this building can take a little time, it makes it much easier to update the system, and therefore I highly recommend it. The nvidia-libs package will provide OpenGL and Vulkan libraries, and is once again essential for the driver. Finally, nvidia-libs-32bit is the 32bit version, which is a requirement for 32bit applications (most games are 32bit).

Here is the command:

```shell
sudo xbps-install nvidia nvidia-dkms nvidia-libs nvidia-libs-32bit
```

For anybody who has an Nvidia card older than the 16 series, you will need to install the relevant driver packages for your card.

<img width="694" height="180" alt="image" src="https://github.com/user-attachments/assets/78973c47-f232-409a-b0db-4bed1b392f2c" />

For more information on Nvidia drivers, use the void handbook: https://docs.voidlinux.org/config/graphical-session/graphics-drivers/nvidia.html

# AMD/Intel users

As I mentioned earlier, since I don't have an Intel/AMD GPU, I am unable to provide reliable information. Here are the relevant void handbook pages:

AMD: https://docs.voidlinux.org/config/graphical-session/graphics-drivers/amd.html
Intel: https://docs.voidlinux.org/config/graphical-session/graphics-drivers/Intel.html

From what I know, you should only need these two packages for both - mesa and mesa-32bit. Here is the command:

```shell
sudo xbps-install mesa mesa-32bit
```

# Wayland users

Anybody using Wayland may have to do a couple more tweaks. Firstly, ensure Xwayland is installed, which can be done like this:

```shell
sudo xbps-install xorg-server-xwayland xorg-server-xwayland-32bit
```

Furthermore, if Xwayland refuses to work, you may need Xwayland satilite. Install like this:

```shell
sudo xbps-install xwayland-satellite
```

# Nvidia users

You will have to change a kernel parameter for Wayland + NVIDIA to work correctly (mostly for those running on a GPU older than the 16 series - newer drivers often enable this by default). The easiest way to do this is to install grub-customizer (if you are not using grub, you will have to look elsewhere on how to add the parameter):

```shell
sudo xbps-install grub-customizer
```

Then, go into grub-customizer (launching from a terminal is sometimes necessary), and under kernel parameters, add "nvidia-drm.modeset=1".

# Vulkan

For all GPU vendors, Vulkan has to be installed in some way. Almost every game will use Vulkan (and OpenGL is handled by the driver anyway), so we **must** install Vulkan.

We will need the following packages: vulkan-loader and vulkan-loader-32bit. If you are developing anything graphical, vulkan-loader-devel may be handy. vulkan-loader does exactly what it says: it loads Vulkan when it's needed. We also need the 32bit version (most games are 32bit).

Here is the command:

```shell
sudo xbps-install vulkan-loader vulkan-loader-32bit
```

# AMD/Intel users

I am not sure exactly what packages you need, but I believe it is "mesa-vulkan-intel" and "mesa-vulkan-radeon" for Intel and AMD respectively, aswell as 32-bit counterparts.

# Steam and other useful packages

At this point, we are ready to install steam. There are two ways to do this - native (xbps steam) and flatpak steam. Native xbps steam has the advantage that it will integrate into the void system more easily, however there may be more compatibility issues as it doesn't have flatpak runtime. I have not had issues with it though. Another advantage is if you have multiple drives, flatpak can become a hassle. See the later section on multiple drives.

If you have chosen xbps steam, simply install it as you would expect:
```shell
sudo xbps-install steam
```

If you have chosen flatpak steam, firstly install flatpak and add flathub as a repository. This can be done like this:

```shell
sudo xbps-install flatpak
```

```shell
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

(Note: flatpak will prove very useful later - I would recommend installing it and adding flathub anyway so it's ready)

Then, install steam:

```shell
flatpak install flathub com.valvesoftware.Steam
```

# More packages

Protontricks and winetricks can help compatibility, and are also necessary when installing some mods (like Geode for Geometry dash).

```shell
sudo xbps-install wine protontricks winetricks
```

MangoHud may be important for those wanting an RTSS-like way to monitor in-game stats. However, for whatever reason, it cannot monitor Nvidia statistics on specifically void (atleast for me). Therefore, I would only recommend it for Intel or AMD users.

```shell
sudo xbps-install MangoHud
```

ProtonUp-Qt is another essential application, as it allows us to install other compatibility tools, one of which is Glorious Eggroll's proton (called Proton-GE). This proton can sometimes improve performance, or even cause games that couldn't run before to run incredibly well.

This is a flatpak package, so after setting up flatpak, install like this:

```shell
flatpak install flathub net.davidotek.pupgui2
```

# Eync - IMPORTANT!!!

For whatever reason, esync is not configured on void, which leads to many games being unable to run. This is because it has been replaced by NTsync, which is now installed into the Linux kernel, however if it is incompatible, a fallback is necessary. To configure these, you must raise the vm.max_map_count, and the nofile limit.

Firstly, insert vm.max_map_count=262144 in the file /etc/sysctl.conf. Then, run sudo sysctl -p, which will update the config. Finally, run cat /proc/sys/vm/max_map_count, which should print 262144.

For the nofile limit, it's a little more complicated. Firstly, edit the file /etc/security/limits.conf, and add the two following lines.

```shell
* soft nofile 1048576
* hard nofile 1048576
```

Then add the following line to /etc/pam.d/login

```shell
session         required        pam_limits.so
```

Finally, add the following line to /etc/pam.d/lightdm (if you are not using lightdm, replace it with your login manager)

```shell
session   required pam_limits.so
```

After restarting, run both of the following commands:

```shell
ulimit -Hn
```
```shell
ulimit -n
```

If everything has worked, it should say 1048576 for both.

# So, what is this actually doing?
Obviously, that was alot of commands and stuff, so here is an explanation for those who want to know.

Esync, fsync and NTsync are all technologies used by proton to help reduce its overhead. Because proton is converting dx3d calls to OpenGL/Vulkan, there is some CPU overhead, aswell as any calls to windows functions that have to be translated.

Esync was the first of these to be created. Esync is designed to reduce the overhead of Windows synchronization primitives, by using Linux's eventfd system to manage threaded tasks more efficiently. Practically, this causes esync to need a high file descriptor limit. File descriptors are essentially Linux resources used for files and 
other IO objects. If this is too low, a esync cannot run.

This is what raising the nofile limit is doing. When we edit limits.conf, we are telling Linux to let us have 1048576 running file descriptors at once. A soft limit is the current practical limit, which cannot exceed the hard limit, and is used by everyone but root. A hard limit is the physical limit it can allow, and can only really be touched by root. They are set both the same as we don't need to worry about this kind of usage.

PAM is the library used for authentication on Linux. When we edit /etc/pam.d/login and add our line, we are telling it that whenever there is a login to TTY, automatically execute pam_limits.so, which is a session module that will cause limit.conf to be read and run, changing the limits. Similarly, by changing /etc/pam.d/lightdm, whenever lightdm authenticates us, pam_limits.so will be read, enforcing the new limits.

From my understanding, the increase of vm.max_map_count has to be done because each file descriptor is memory mapped area, so if there are alot of them, we also need to ensure that there is allowed to be alot of memory mapped areas. I cannot entirely validate if this is why, but proton logs will complain if this is not increased.

Fsync and NTsync are newer, more optimized ways of doing what Esync achieves, however it necessary to have a fallback, hence why we still setup esync (and fsync, as I believe it to benefits from these parameters).

Here are some websites for anybody interested on this topic:
https://www.cyberciti.biz/faq/linux-increase-the-maximum-number-of-open-files/
https://en.wikipedia.org/wiki/Linux_PAM/
https://wpsticky.com/proton-linux-gaming-explained-esync-fsync-and-performance-tweaks-for-steam-deck-and-pc/

# Final steps (for most people)

Open up ProtonUp-qt and click on steam and add version. Install the latest version of proton-GE to steam, and then launch steam. Go to compatibility, and install both proton hotfix and proton experimental (these will be useful later). Finally, install a game.

# Gaming

If everything went well, you should now be able to finally game!

Try to run the game with proton hotfix. If this doesn't work, try experimental. If that doesn't work, try proton-GE. If all of these don't work, go to the troubleshooting section.

# EasyAntiCheat

For EAC, you must install the EasyAntiCheat runtime. This can be found on the steam store like a game, so just install it and run the EAC game (the runtime will run when needed). Select proton-GE (as this generally works better for EAC games) and try to run the game. If it doesn't run, go to the troubleshooting section.

# Non-steam games

Most non-steam games can be found on heroic-launcher. This can be installed via xbps. After installation, log into your account and download the game you want to play. Before playing, go into compatibility, and select proton-GE (using ProtonUp-qt is unnecessary for this, as heroic has it built-in). Finally, if the game has EAC, select to enable the EAC runtime and try to play the game.

# Multiple drives

In the case that multiple drives are being used, setup depends on whether flatpak steam(or heroic) is in use, and whether the second drive is EXT4 or NTFS.

For all users, you will need to mount the drive on startup via /etc/fstab, and then point steam to the games library. To do this, firstly find your drive's UUID by running the following command:

```shell
lsblk -f
```

Then, copy your drive's UUID, and save it somewhere for later. A UUID is essentially a unique label given to each drive, that unlike other labels (like drive letters), will NEVER change, and are therefore the best way to talk to the drive.

Create a location to mount the drive:

```shell
sudo mkdir -p /mnt/games
```

(note that if you are doing more than 1 extra drive, you may need to make more folders under /mnt)

Now, if you are running an NTFS drive, edit your fstab (at /etc/fstab) and add the following new line:

```shell
UUID=YourUUID /mnt/games ntfs3 defaults,noatime,uid=1000,gid=1000,windows_names 0 0
```

(if when running the command "id", your user isn't id 1000, change the fstab entry "UID" to your user id, and "GID" to your group id)

For ext4 users, add this line:

```shell
UUID=YourUUID /mnt/games ext4 defaults,noatime, 0 2
```



# Troubleshooting

If launching a game is unsuccessful, firstly check if it works on any other distro. If it isn't Linux-compatible, sadly there is nothing you can do to play the game on void.

If you are running Nvidia, run nvidia-smi. If it gives no error, your driver is most likely installed correctly. Then, try run vkcube. If this fails, you most likely haven't installed Vulkan correctly, therefore you should refer back to the earlier section on installing Vulkan.

If you are on wayland, xwayland may not be working. The easiest step to check is by installing xterm, and trying to run it. If it does not run, xwayland is not working. This could be because xwayland-satilite is not installed. Refer back to the wayland section to learn how to install it.

Putting the launch option "PROTON_NO_NTSYNC=1 %command%" into the game may also help some games which refuse to launch, as this forces fsync or esync.

If your graphics card is too old that it does not support Vulkan, you may run into numerous issues. To check this, run lspci | grep "VGA", and search up your graphics card on TechPowerUp's database. Here, it will list if it supports Vulkan. If it doesn't, put the launch option "PROTON_USE_WINE3D=1 %command%" in your launch options (on heroic, just select to use wine3d instead of Vulkan). However, if your GPU doesn't support Vulkan, I wouldn't recommend Linux, as wine3D can be significantly slower than running native DX3D on windows (from my limited testing).

If it is an EAC game, check online to ensure the developers have enabled EAC for linux (if not, the game won't run). If they have, ensure esync has been setup properly, and add the launch option "PROTON_NO_NTSYNC=1".

# Void-gaming-tutorial
A tutorial for gaming on void Linux.

This tutorial aims to help those who have just installed void, and would like a quick startup to playing games. A quick disclaimer - this tutorial is aimed primarily at NVIDIA users primarily and I also can't account for all hardware combinations. For reference, I have an RTX 3060 and an i9 10900k. This tutorial will assume that you have just finished installing void, but feel free to skip ahead if you've done some of these steps. I am using niri + noctalia with lightdm. This tutorial also assumes that the multilib repository, multilib non-free and non-free repositories are enabled. A useful resource for this is the void handbook: https://docs.voidlinux.org/xbps/repositories/index.html

# Driver installation

For anybody who has an Nvidia 16 series or newer card, install the following packages: **nvidia**, **nvidia-dkms**, **nvidia-libs** and **nvidia-libs-32bit**. The nvidia package will provide you with useful utilities such as nvidia-smi, and is essential for the driver to function. Nvidia-dkms will allow the nvidia driver to be built up against your kernel whenever it updates. Whilst this building can take a little time, it makes it much easier to update the system, and therefore I highly recommend it. The nvidia-libs package will provide OpenGL and Vulkan libraries, and is once again essential for the driver. Finally, nvidia-libs-32bit is the 32bit version, which is a requirement for 32bit applications (most games are 32bit).

Here is the command:

sudo xbps-install nvidia nvidia-dkms nvidia-libs nvidia-libs-32bit

For anybody who has an Nvidia card older than the 16 series, you will need to install the relevant driver packages for your card.

<img width="694" height="180" alt="image" src="https://github.com/user-attachments/assets/78973c47-f232-409a-b0db-4bed1b392f2c" />

For more information on Nvidia drivers, use the void handbook: https://docs.voidlinux.org/config/graphical-session/graphics-drivers/nvidia.html

# AMD/Intel users

As I mentioned earlier, since I don't have an Intel/AMD GPU, I am unable to provide reliable information. Here are the relevant void handbook pages:

AMD: https://docs.voidlinux.org/config/graphical-session/graphics-drivers/amd.html
Intel: https://docs.voidlinux.org/config/graphical-session/graphics-drivers/intel.html

# Vulkan

For all GPU vendors, vulkan has to be installed in some way. Almost every game will use vulkan (and OpenGL is handled by the driver anyway), so we **must** install vulkan.

We will need the following packages: vulkan-loader and vulkan-loader-32bit. If you are developing anything graphical, vulkan-loader-devel may be handy. Vulkan-loader does exactly what it says: it loads vulkan when it's needed. We also need the 32bit version (most games are 32bit).

Here is the command:

sudo xbps-install vulkan-loader vulkan-loader-32bit

# AMD/Intel users

I am not sure exactly what packages you need, but in some way you will need to be able to load vulkan. I am also not certain if vulkan is already included in mesa. If it is, you won't need to do anything.

# Steam and other useful packages

At this point, we are ready to install steam. There are two ways to do this - native (xbps steam) and flatpak steam. Native xbps steam has the advantage that it will integrate into the void system more easily, however there may be more compatibility issues as it doesn't have flatpak runtime. I have not had issues with it though. Another advantage is if you have multiple drives, flatpak can become a hassle. See the later chapter on multiple drives.

If you have chosen xbps steam, simply install it as you would expect:

sudo xbps-install steam

If you have chosen flatpak steam, firstly install flatpak and add flathub as a repository. This can be done like this:

sudo xbps-install flatpak

flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

(Note: flatpak will prove very useful later - I would recommend installing it and adding flathub anyways so it is ready)

Then, install steam:

flatpak install flathub com.valvesoftware.Steam

# More packages

Protontricks and winetricks can help compatibility, and are also necessary when installing some mods (like Geode for Geometry dash).

sudo xbps-install wine protontricks winetricks

MangoHud may be important for those wanting an RTSS-like way to monitor in-game stats. However, for whatever reason, it cannot monitor Nvidia statistics (atleast for me). Therefore, I would only recommend it for intel or AMD users.

sudo xbps-install MangoHud

ProtonUp-Qt is another essential application, as it allows us to install other compatibility tools, one of which is Glorious Eggroll's proton (called Proton-GE). This proton can sometimes improve performance, or even cause games that couldn't run before to run incredibly well.

This is a flatpak package, so after setting up flatpak, install like this:

flatpak install flathub net.davidotek.pupgui2

#fsync and esync - IMPORTANT!!!

For whatever reason, fsync and esync are not configured on void, which leads to many games being unable to run. This is because they both replaced by NTsync, which is now installed into the linux kernel, however if it fails, a fallback is necessary. To configure these, you must raise the vm.max_map_count, and the nofile limit.

This can sometimes be system-dependent on how you do this. I will write how I did this below. It may necessary to look online for other solutions.

Firstly, insert vm.max_map_count=262144 in the file /etc/sysctl.conf. Then, run sudo sysctl -p, which will update the config. Finally, run cat /proc/sys/vm/max_map_count, which should print 262144.

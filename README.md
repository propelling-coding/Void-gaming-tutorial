# Void-gaming-tutorial
A tutorial for gaming on void Linux.

This tutorial aims to help those who have just installed void, and would like a quick startup to playing games. A quick disclaimer - this tutorial is aimed at NVIDIA users primarily and I also can't account for all hardware combinations. For reference, I have an RTX 3060 and an i9 10900k. This tutorial will assume that you have just finished installing void, and is for Wayland users (but most things should still be relevant on an x11 environment). I am using niri + noctalia with lightdm. This also assumes that the multilib repository, multilib non-free and non-free repositories are enabled. A useful resource for this is the void handbook: https://docs.voidlinux.org/xbps/repositories/index.html

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

# AMD/Intel users

I am not sure exactly what packages you need, but in some way you will need to be able to load vulkan. I am also not certain if vulkan is already included in mesa. If it is, you won't need to do anything.

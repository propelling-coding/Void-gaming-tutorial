# Void-gaming-tutorial
A tutorial for gaming on void linux.

This tutorial aims to help those who have just installed void, and would like a quick startup to playing games. A quick disclaimer - this tutorial is aimed at NVIDIA users primarily and I also can't account for all hardware combinations. For reference, I have an RTX 3060 and an i9 10900k. This tutorial will assume that you have just finished installing void, and is for Wayland users (but most things should still be relevant on an x11 environment). I am using niri + noctalia with lightdm. This also assumes that the multilib repository, multilib non-free and non-free repositories are enabled. A useful resource for this is the void handbook: https://docs.voidlinux.org/xbps/repositories/index.html

# Driver installation

For anybody who has an Nvidia 16 series or newer card, install the following packages: **nvidia**, **nvidia-dkms**, **nvidia-libs** and **nvidia-libs-32bit**. The nvidia package will provide you with useful utilities such as nvidia-smi, and is essential for the driver to function. Nvidia-dkms will allow the nvidia driver to be built up against your kernel whenever it updates. Whilst this building can take a little time, it makes it much easier to update the system, and therefore I highly recommend it. The nvidia-libs package will provide OpenGL and Vulkan libraries, and is once again essential for the driver. Finally, nvidia-libs-32bit is the 32bit version, which is a requirement for 32bit applications (most games are 32bit).

Here is the command:

sudo xbps-install nvidia nvidia-dkms nvidia-libs nvidia-libs-32bit

For anybody who has an Nvidia card older than the 16 series, you will need to install the relevant driver packages for your card.

<img width="621" height="126" alt="image" src="https://github.com/user-attachments/assets/e8ebab87-2d9c-4a86-85eb-946a83053e92" />



For more infomation on nvidia drivers, use the void handbook: https://docs.voidlinux.org/config/graphical-session/graphics-drivers/nvidia.html

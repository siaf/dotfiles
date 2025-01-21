# Arch Linux Setup

This repo contains a guide on how to setup Arch Linux with basic dev tools and Nord Theme using dotfiles.

## Setup Wifi on a minimal Arch Linux 

1. Install Required Tools
Ensure you have the necessary tools installed:

```bash
sudo pacman -Syu
sudo pacman -S iw wpa_supplicant dialog networkmanager
```
2. Check for Wireless Interface
Find your wireless network interface:

```bash
ip link
```
Look for an interface like ```wlan0``` or similar.

3. Persistent Configuration Using NetworkManager
Enable and start NetworkManager to ensure the connection persists after reboot:

```bash
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager
```
To reconnect after reboot, NetworkManager will automatically handle it.


4. Connect to Wi-Fi Temporarily (Interactive Way)
You can use ```nmtui``` (NetworkManager's terminal interface) for a quick setup:

1.Start ```nmtui```

```bash
nmtui
```
2.Select "Activate a Connection".
3.Choose your wireless network and enter the password.


5. Verify Connection
```bash
ping -c 3 archlinux.org
```
## Preparing basic software

```bash

# Install Git
sudo pacman -S git

# Install yay
cd ~
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

# Setup SSH

yay -S openssh
sudo systemctl start sshd
sudo systemctl enable sshd

# Configure git ssh and global user name
ssh-keygen -t ed25519 -C "youremail"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub #upload key to github and verify using command below.
ssh -T git@github.com

git config --global user.email "Your Email"
git config --global user.name "Your Name"

# Install kitty, starship, code nerd fonts
yay -S kitty
yay -S starship
yay -S ttf-firacode-nerd

# Install hyprland, hyprlock, hypridle, hyprpaper, waybar, wofi, font awesome
yay -S hyprland
yay -S hyprlock
yay -S hypridle
yay -S hyprpaper

yay -S waybar
yay -S wofi

yay -S ttf-font-awesome

# Install yazi, fzf
yay -S yazi
yay -S fzf

# Install neovim, sublime text
yay -S neovim
yay -S sublime-text

# Install btop
yay -S btop

# Install dust - for viewing storage usage
yay -S dust

# Install tldr - an cure minal replacement for man
yay -S tldr

# Install stow
yay -S stow

# fetch dotfiles and stow them
cd ~
git clone https://github.com/siaf/dotfiles.git
cd dotfiles
stow . --adopt #need to find a better way for doing this currently this requires restoring the git changes after this command.

# Install Google Chrome
yay -S google-chrome


```

# These are specific to Intel
```bash
# Install powertop - by Intel for power management 
yay -S powertop
```

# Lenovo W520 Specific Setup:
If starting from a minimal Arch Linux setup, ensuring your Lenovo ThinkPad W520 has all necessary drivers installed on your Arch Linux setup involves the following steps:

## 1. Graphics Drivers
The W520 typically comes with NVIDIA Optimus (Intel + NVIDIA graphics):

Install the drivers for both:

Intel Graphics:
```bash
sudo pacman -S mesa libva-mesa-driver lib32-mesa vulkan-intel
```
NVIDIA Graphics: For proprietary NVIDIA drivers (recommended):
```bash
sudo pacman -S nvidia nvidia-utils lib32-nvidia-utils nvidia-settings
```
For open-source Nouveau drivers (less performant but may suffice for general use):

```bash
sudo pacman -S xf86-video-nouveau
```
Enable NVIDIA Optimus (Hybrid Graphics):
Install and configure PRIME:

```bash
sudo pacman -S nvidia-prime
```

Use NVIDIA GPU for specific apps:

```bash
prime-run <application>
```

## 2. Wireless and Bluetooth
The W520 uses Intel wireless adapters, which are well-supported by the iwlwifi driver included in the kernel.

For Wi-Fi:
Install tools for managing Wi-Fi:

```bash
sudo pacman -S networkmanager

sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager
```
For Bluetooth:

Install Bluetooth support, then Enable and start the Bluetooth service:


```bash
sudo pacman -S bluez bluez-utils
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```
## 3. Audio
The W520 uses Intel HDA audio, which should work out of the box with either PulseAudio or PipeWire:

For PulseAudio:
```bash
sudo pacman -S pulseaudio pulseaudio-alsa alsa-utils
```
For PipeWire (recommended):
```bash
sudo pacman -S pipewire pipewire-pulse pipewire-alsa pipewire-jack
```

Test audio:

```bash
aplay -l
```
Use ```pavucontrol``` or ```pipewire-pulse``` to manage audio devices.

## 4. Trackpad and TrackPoint
The W520's trackpad and TrackPoint are supported by the libinput driver:

```bash
sudo pacman -S xf86-input-libinput
```
You can customize the sensitivity and behavior using configuration files in /etc/X11/xorg.conf.d/.

## 5. Fingerprint Reader
If your W520 has a fingerprint reader:

Install fprintd:
```bash
sudo pacman -S fprintd
```
Enable the service:

```bash
sudo systemctl enable --now fprintd
```
Enroll fingerprints:
```bash
fprintd-enroll
```
## 6. Power Management
For better battery life and power management:

Install tlp:

```bash
sudo pacman -S tlp
```
Enable and start tlp:
```bash
sudo systemctl enable tlp
sudo systemctl start tlp
```
For NVIDIA Optimus power savings:

```bash
sudo pacman -S bbswitch
```

## 7. Docking Station and External Monitors
For external monitor support, use xrandr:

```bash
xrandr
```

For advanced setups (e.g., multiple external monitors), configure the NVIDIA driver or use Wayland with PipeWire.

## 8. Firmware Updates
Keep your firmware updated using fwupd:

```bash
sudo pacman -S fwupd
sudo fwupdmgr refresh
sudo fwupdmgr update
```

## Verify Everything Works
Graphics: Test NVIDIA and Intel GPU with:
```bash
glxinfo | grep "OpenGL renderer"
```
Wi-Fi: Verify with:
```bash
nmcli device status
```
Audio: Test with:
```bash
speaker-test -c 2
```
Power Management: Check battery savings with tlp-stat:

```bash
sudo tlp-stat
```
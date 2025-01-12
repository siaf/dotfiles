# Arch Linux Setup

This repo contains a guide on how to setup Arch Linux with basic dev tools and Nord Theme using dotfiles.

## Prepare

```bash

# Install Git
sudo pacman -S git

# Install yay
cd ~
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

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

# Install stow
yay -S stow

# fetch dotfiles and stow them
cd ~
git clone https://github.com/siaf/dotfiles.git
cd yay
stow . --adopt

# Install Google Chrome
yay -S google-chrome

# Configure git ssh and global user name
ssh-keygen -t ed25519 -C "youremail"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
ssh -T git@github.com

git config --global user.email "Your Email"
git config --global user.name "Your Name"

```

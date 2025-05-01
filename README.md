# temp

#!/bin/bash

echo "=== Updating System ==="
sudo apt update && sudo apt upgrade -y

echo "=== Installing Basic Packages ==="
sudo apt install -y curl wget git flatpak gnome-software-plugin-flatpak tlp gamemode timeshift mint-meta-codecs software-properties-common

echo "=== Setting up Flatpak ==="
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

echo "=== Installing Apps ==="

# Chrome
echo "--- Installing Google Chrome ---"
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install -y ./google-chrome-stable_current_amd64.deb
rm google-chrome-stable_current_amd64.deb

# VS Code
echo "--- Installing Visual Studio Code ---"
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install -y code
rm packages.microsoft.gpg

# Node.js (LTS version) and npm
echo "--- Installing Node.js and npm ---"
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs

# Python 3 and pip
echo "--- Installing Python and pip ---"
sudo apt install -y python3 python3-pip

# Flatpak Apps
echo "--- Installing Flatpak apps (Discord, Spotify, Prism Launcher, DaVinci Resolve, Postman, Obsidian, Parsec) ---"
flatpak install -y flathub com.discordapp.Discord
flatpak install -y flathub com.spotify.Client
flatpak install -y flathub org.prismlauncher.PrismLauncher
flatpak install -y flathub com.blackmagicdesign.DaVinciResolve
flatpak install -y flathub com.getpostman.Postman
flatpak install -y flathub md.obsidian.Obsidian
flatpak install -y flathub com.parsecgaming.parsec

# Steam
echo "--- Installing Steam ---"
sudo apt install -y steam

# Ulauncher
echo "--- Installing Ulauncher ---"
sudo add-apt-repository -y ppa:agornostal/ulauncher
sudo apt update
sudo apt install -y ulauncher

# PIA VPN (Private Internet Access)
echo "--- Installing PIA VPN Client ---"
wget https://installers.privateinternetaccess.com/download/pia-linux-3.5.2-06924.run
chmod +x pia-linux-3.5.2-06924.run
sudo ./pia-linux-3.5.2-06924.run
rm pia-linux-3.5.2-06924.run

# Redshift (Flux alternative)
echo "--- Installing Redshift (for eye comfort) ---"
sudo apt install -y redshift redshift-gtk

# Transmission (torrent client)
echo "--- Installing Transmission ---"
sudo apt install -y transmission-gtk

# VLC Media Player
echo "--- Installing VLC ---"
sudo apt install -y vlc

# Zen Browser (Edge-based browser)
echo "--- Installing Zen Browser ---"
flatpak install -y flathub io.gitlab.Zen_Browser.Zen

# Nvidia Drivers and PRIME support
echo "=== Installing Nvidia Drivers and PRIME support ==="
sudo apt install -y nvidia-driver-535 nvidia-prime

echo "=== Enabling TLP Power Saving ==="
sudo systemctl enable tlp
sudo systemctl start tlp

echo "=== Enabling Gamemode for Performance Boost ==="
sudo systemctl enable gamemoded
sudo systemctl start gamemoded

echo "=== Final Cleanup ==="
sudo apt autoremove -y
sudo apt clean

echo "=== System Setup Complete! ==="
echo "Optional next steps:"
echo " - Open 'Driver Manager' to double-check Nvidia driver installation (should show 535 or newer)"
echo " - Reboot your system once to fully load Nvidia modules"
echo " - Use Nvidia PRIME app to switch between Intel and Nvidia GPU"
echo " - Set up Timeshift for system restore snapshots"
echo " - Log in to Chrome, VS Code, Discord, Spotify, Postman, Obsidian"
echo " - Open Steam Settings -> Compatibility -> Enable Proton for all games"
echo " - Configure Prism Launcher for Minecraft mods"
echo " - Launch DaVinci Resolve (should use Nvidia GPU)"
echo " - Configure Redshift for night light settings"
echo " - Start using Parsec for remote gaming if needed"

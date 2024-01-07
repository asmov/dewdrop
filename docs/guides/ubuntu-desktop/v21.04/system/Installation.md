Installation Notes for Ubuntu Desktop 23.04
================================================================

Settings
----------------------------------------------------------------
# Ubuntu Desktop
## Dock
- Icon size: 32
## Search / Search Results
- All: Off
- Settings: On
# Multitasking / Workspaces
- Fixed number of workspaces: Selected
- Number of Workspaces: 2
# Privacy
## Screen Lock
- Blank Screen Delay: 15 minutes
## File History & Trash
### File History
- File History: On
- File History Duration: 7 days
### Trash & Temporary Files
- Automatically Delete Trash Content: On
- Automatically Delete Temporary Files: On
- Automatically Delete Period: 14 days
# Sharing
**BUG:** Freeze when first clicking on Menu
# Sound / Sounds
- Alert Sound: None
# Power
- Power Mode: Performance
# Removable Media
- Software: Ask what to do
# Date & Time
- Time Format: AM / PM

System Configuration
----------------------------------------------------------------
# Public Share
## Create a system-wide Public directory in /var/local
```bash
run() {
    set -e -o pipefail
    sudo mkdir /var/local/public
    sudo chown root:staff /var/local/public
    sudo chmod 755 /var/local/public
}
run
```
## Move a user's Public folder to the system-wide directory
```bash
run() {
    set -e -o pipefail
    sudo mv ~/Public /var/local/public/$USER
    ln -s /var/local/public/$USER ~/Public
}
run
```

Software Packages
----------------------------------------------------------------
# Packages
- `git`
- `nemo` (Files)
- `curl`
- `flatpak`
- `gocryptfs`
- `gnome-shell-extension-prefs`
- `gpa` (GNU Privacy Assistant)
## apt install
```bash
run() {
    set -e -o pipefail
    sudo apt install  \
        git  \
        nemo  \
        curl  \
        flatpak  \
        gocryptfs \
        gpa \
        gnome-shell-extension-prefs \
        build-essential
}
run
```
# Brave Browser Extensions
- GNOME Shell Integration
# GNOME Shell Extensions
- Workspace Indicator
    [https://extensions.gnome.org/extension/21/workspace-indicator/]
- Simple Timer
    [https://extensions.gnome.org/extension/5115/simple-timer/]
- Task Widget
    [https://extensions.gnome.org/extension/3569/task-widget/]
Widgets
----------------------------------------------------------------
# Panel
- Pin:
  + Nemo (Files)
  + Brave Web Browser
  + Text Editor
  + Terminal
  + LibreOffice Writer
  + Keeper Password Manager
  + Signal
  + Calculator
  + Document Scanner
  + Thunderbird

Applications
----------------------------------------------------------------
# Terminal
## Menu / Preferences / Profiles / Unnamed
### Text
- Initial terminal size: 132 x 43
- Custom font: Ubuntu Mono 10 pt
### Colors
#### Text and Background Color
- Use colors from system theme: Off
- Built-in schemes: Gray on black
- Default color
  + Text: #D5D5D5
- Use transparency from system theme: Off
- Use transparent background: 33%
#### Palette
- Built-in schemes: Tango
- Show bold text in bright colors: On

# Nemo (Files)
## Configure Nemo as the default file manager
```bash
run() {
    set -e -o pipefail
    xdg-mime default nemo.desktop inode/directory  \
        application/x-gnome-saved-search

    gsettings set org.gnome.desktop.background  \
        show-desktop-icons false

    gsettings set org.nemo.desktop  \
        show-desktop-icons true
}
run
```
Source: 
[Prakash, A. (2023). How to Install and Make Nemo the Default File Manager in Ubuntu. It's FOSS.](https://itsfoss.com/install-nemo-file-manager-ubuntu)
## Edit / Preferences
(Right click on top-panel to access menu.)
### Views
#### Default View
- View new folders using: List View
- Inherit view type from parent: On
### Behavior
#### Behavior
- Single click to open items: Selected
#### Trash
- Include a Delete command that bypasses Trash: Off
### Display
#### Date
- Format: Yesterday
### List Columns
1. Name
2. Type
3. Date Modified
### Toolbar / Visible Buttons
- Icon view: Off
- Compact view: Off
- Open in terminal: On
- List View: Off
- Show Thumbnails: On
### Context Menus / Visible Entries
#### Selection
- Open in New Window: Off
- Pin: Off
- Make Link: On
- Copy to: On
- Move to: On
- Open in Terminal: Off
- Open as Root: Off

# Brave (Browser)
## Install using Apt
Software Installer app only provides Snap packages for Brave.

```bash
run() {
    set -e -o pipefail
    sudo curl -fsSLo  \
        /usr/share/keyrings/brave-browser-archive-keyring.gpg  \
        https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg

    echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"  \
    | sudo tee  \
        /etc/apt/sources.list.d/brave-browser-release.list

    sudo apt update
    sudo apt install brave-browser
}
run
```

Source: 
[Brave. (May 2023). Installing Brave on Linux.](https://brave.com/linux/)
## Default App for browsing
Must be done manually:
- Settings / Default Apps / Web: Brave Web Browser

# Text Editor
##  Config Icon
- Spaces (indentation): Selected
- Spaces per Tab: 4
## Preferences
- Custom Font: Ubuntu Mono Regular 10pt


Asmov IT
----------------------------------------------------------------
# Overview
- Install Printer / Scanner drivers
 

Asmov Developer IT
----------------------------------------------------------------
# Overview
- Install VS Code

Personal IT
----------------------------------------------------------------
# Overview
- Install Zoom
- Settings / Mouse & Touchpad / Touchpad : Off

Migration
----------------------------------------------------------------
# Overview
- Bash
 + .bashrc source from home-config
- Neovim
 + .config/nvim link to home-config
- Hosts
 + append from home-config
- Dewdrop ~/ Files
 + copy/merge
 + update xdg user dirs config
 + create nemo bookmarks
- Brave Browser
- SSH
 + Config
 + Keys
- Test Git projects
- Test Tote / Cabinet

# SSH
- Link the `~/data/keys/ssh` directory relatively to `~/.ssh/keys`
- Copy the _$HOMECFG/ssh/config_ file to `~/.ssh/config`
- For each host in `~/.ssh/config`:
  + Link the referred public and private SSH keyfiles from `~/.ssh/keys` relatively into `~/.ssh`
- Verify that Seahorse (Passowrds & Keys) shows these keys in the OpenSSH Keys section.

# GPG
- Mount `~/.data/keys/gpg/gpg-private-keys.gocryptfs` via `gocryptfs` to `~/.data/keys/gpg/gpg-private-keys`
- Using **Seahorse**, import each `.keypair.asc`
- Unmount the gocryptfs mount.

# VM Management

```bash
sudo apt install virt-manager
sudo reboot
```

Add virt-manager to GNOME Favorites.

# Setting up an Ubuntu Desktop VM

1. Copy the ISO to: `/var/lib/libvirt/images`
2. Installation config
- Minimal packages
- Hostname: `ubuntu-desktop-vm`
- Username / Name: `vmuser`
- Passowrd: `P4ssw0rd!vmuser`
- No login required
- Default CPU / RAM / Storage, etc.
3. Install packages
```bash
sudo apt install \
    openssh-server \
    gnome-shell-extension-prefs \
    neovim
```
## SSH

1. Log in to localhost to init ~/.ssh:
```bash
ssh localhost
```
2. On the WORKSTATION: Copy local workstation SSH key to VM
```bash
# FROM WORKSTATION, NOT THE VM
scp ~/.ssh/{local key file.pub} vmuser@ubuntu-desktop-vm.local:~/.ssh
```
3. Install the SSH key on the VM
```bash
cat ~/.ssh/{local key file.pub} >> ~/.ssh/authorized_keys
```
4. On the WORKSTATION: Config SH client to use key for VM by adding the following to `~/.ssh/config`:
```
Host ubuntu.desktop.vm.local
    Hostname ubuntu-desktop-vm.local
    User vmuser
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/{local key filename}
```

## OSX KVM

[README](https://github.com/kholia/OSX-KVM)

1. Create KVM config `/etc/modprobe.d/kvm.conf`:
```
options kvm_intel nested=1
options kvm_intel emulate_invalid_guest_state=0
options kvm ignore_msrs=1 report_ignored_msrs=0
```

2. Create the VM config:
`virsh --connect qemu:///system define macOS.xml`


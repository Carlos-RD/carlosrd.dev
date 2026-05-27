+++
date = "2026-05-27"
lastmod = "2026-05-27"
draft = false
title = "Installing Fedora Sway"
categories = ["technology"]
tags = ["linux"]
+++
<div style="text-align: justify">

This is a similar post to [Rebuilding my MacBook](/Posts/260318_rebuilding_mbp/), but this time I'll keep track of how I've installed Fedora Sway on my T480s. I was [running Pop!_OS before](/posts/231004_first_post/) and although pretty much everything worked out of the box, I decided to complicate my life a little bit more in the name of science and *"fuckraroundery"*. I chose Fedora mainly out of nostalgia. If memory serves me well, Fedora Core 2 was the distro I was taught on during my Telecom diploma circa 2004. It was the first time I ever used Linux and I had no idea what I was doing. I guess that hasn't changed…  but I’m working on it!

I'm not sure going the atomic route was the right call, as it introduces other layers of complexity. But so far I'm happy, and I've particularly enjoyed the workflow Sway provides, even if it comes out of the box with a very minimalistic configuration. I'm basically on my own trying to make things work. Fun!

I'll keep this post as a living document and update it as things change.

## Adding packages to the base

This is the first time I've run an [atomic OS](https://fedoraproject.org/atomic-desktops/), so I'm not too familiar with how things should be set up. The way I understand it is that the base system should be kept as clean as possible, with everything managed via Flatpaks for GUI apps or Toolbox for CLI tools.  So I decided to install [Alacrity](https://alacritty.org/) (my terminal of choice for now) along with tmux, C compilers and the tools needed to install Mullvad and Brew, which apparently doesn't mess up much with the “atomic way of doing things”.

```Shell
rpm-ostree install gcc gcc-c++ glibc-langpack-en tmux alacritty
```

### Mullvad

Mullvad installation is a bit more finicky, the repo needs to be added manually and the app needs to be run as a daemon after installation.

```Shell
sudo -s
cd /etc/yum.repos.d/
wget https://repository.mullvad.net/rpm/stable/mullvad.repo
rpm-ostree install mullvad-vpn
systemctl reboot
systemctl enable --now mullvad-daemon
```

### Brew

Brew can then be installed by fetching it:

```Shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

adding the environment variables to the shell configuration:

```Shell
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.bash_profile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

### GNU Stow

GNU Stow can then be installed via brew.

```Shell
❯ brew install stow
✔︎ JSON API formula.jws.json                                   Downloaded   32.8MB/ 32.8MB
✔︎ JSON API cask.jws.json                                      Downloaded   16.8MB/ 16.8MB
```

## Make the shell look prettier

Sway is very minimalistic out of the box. Going down the rabbit hole of ricing and making things look better is an odyssey in itself. After a quick research session, I decided to go with [Starship](https://starship.rs/), [Iosevka](https://typeof.net/Iosevka/) fonts, and [Gruvbox material](https://github.com/sainnhe/gruvbox-material) colour theme.

### Configure nerd fonts

```Shell
mkdir -p ~/.local/share/fonts
curl -s 'https://api.github.com/repos/be5invis/Iosevka/releases/latest' | jq -r ".assets[] | .browser_download_url" | grep PkgTTC-Iosevka | xargs -n 1 curl -L -O --fail --silent --show-error
unzip PkgTTC-Iosevka-34.6.1.zip -d ~/.local/share/fonts
sudo fc-cache
```

Or I guess one can just install them using brew

```Shell
brew install --cask font-iosevka-nerd-font
```

### Install theme for alacritty

In search of a more earthy tone, I changed my color scheme to Gruvbox Material, which has been very pleasant on the eyes in both dark and light versions.

```Shell
# We use Alacritty's default Linux config directory as our storage location here.
mkdir -p ~/.config/alacritty/themes
git clone https://github.com/alacritty/alacritty-theme ~/.config/alacritty/themes
```

The theme has to be added to Alacritty’s config file ~/.config/alacritty/.config

```Shell
[general]
import = [
    "~/.config/alacritty/themes/themes/gruvbox_material.toml"]

[env]
TERM = "xterm-256color"

[window]
padding.x = 10
padding.y = 10

decorations = "none"

opacity = 0.8
blur = true 

option_as_alt = "Both"

[font]
size = 14.0

[font.bold]
family = "Iosevka Nerd Font"
style = "Bold"

[font.bold_italic]
family = "Iosevka Nerd Font"
style = "Bold Italic"

[font.italic]
family = "Iosevka Nerd Font"
style = "Italic"

[font.normal]
family = "Iosevka Nerd Font"
style = "Regular"
```

### Starship

Minimalistic shell prompt written in Rust!

```Shell
curl -sS https://starship.rs/install.sh | sh
# ~/.bashrc

eval "$(starship init bash)"
```

### mise-en-place

Mise-en-Place is used for dev environment version control. So far I've used it to install bash autocompletion and LazyVim.

```Shell
curl https://mise.run | sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 10390  100 10390    0     0  21841      0 --:--:-- --:--:-- --:--:-- 21873
mise: installing mise...
######################################################################## 100.0%

mise: run `mise doctor` to verify this is set up correctly
~/.local/bin/mise --version
              _                                        __
   ____ ___  (_)_______        ___  ____        ____  / /___ _________
  / __ `__ \/ / ___/ _ \______/ _ \/ __ \______/ __ \/ / __ `/ ___/ _ \
 / / / / / / (__  )  __/_____/  __/ / / /_____/ /_/ / / /_/ / /__/  __/
/_/ /_/ /_/_/____/\___/      \___/_/ /_/     / .___/_/\__,_/\___/\___/
                                            /_/                 by @jdx
2026.2.22 linux-x64 (2026-02-27)

echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc
source ~/.bashrc
```

### Bash autocompletion

```Shell
mise completion bash --include-bash-completion-lib
echo 'eval "$(mise completion bash --include-bash-completion-lib)"' >> ~/.bashrc
```

### Lazyvim

```Shell
mise use -g neovim@nightly
mise use -g git
mise use -g ripgrep
mise use -g fd
mise use -g lazygit  # Optional: for built-in lazygit support
mise use -g nodejs   # Recommended: for Copilot and other plugins
git clone https://github.com/LazyVim/starter ~/.config/nvim
```

### Bat

Cat files in a fancy way!!

```Shell
mise use --global bat@latest
```

Now that everything looks pretty we can start configuring and ricing

## Sway config (WIP)

```Shell

output eDP-1 mode 3840x2160@60Hz scale 2
output *allow_tearing yes
output* max_render_time off
Move windows to monitor when lid is closed.
bindswitch lid:on output eDP-1 disable
bindswitch lid:off output eDP-1 enable

# Start Thunar file manager

bindsym Mod4+p exec thunar
```

## Waybar (WIP)

## Flatpak

GUI apps are installed via Flatpak.

```Shell
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

### Obsidian

```Shell
flatpak install flathub md.obsidian.Obsidian
```

### Solaar

I use Solaar to configure my logitech MX mouse and personalize gestures

```Shell
flatpak install flathub io.github.pwr_solaar.solaar
sudo vi /etc/udev/rules.d/42-logitech-unify-permissions.rules
```

```Shell
# This rule was added by Solaar.
#
# Allows non-root users to have raw access to Logitech devices.
# Allowing users to write to the device is potentially dangerous
# because they could perform firmware updates.
KERNEL=="uinput", SUBSYSTEM=="misc", TAG+="uaccess", OPTIONS+="static_node=uinput"

ACTION == "remove", GOTO="solaar_end"
SUBSYSTEM != "hidraw", GOTO="solaar_end"

# USB-connected Logitech receivers and devices
ATTRS{idVendor}=="046d", GOTO="solaar_apply"

# Lenovo nano receiver
ATTRS{idVendor}=="17ef", ATTRS{idProduct}=="6042", GOTO="solaar_apply"

# Bluetooth-connected Logitech devices
KERNELS == "0005:046D:*", GOTO="solaar_apply"

GOTO="solaar_end"

LABEL="solaar_apply"

# Allow any seated user to access the receiver.
# uaccess: modern ACL-enabled udev
TAG+="uaccess"

# Grant members of the "plugdev" group access to receiver (useful for SSH users)
#MODE="0660", GROUP="plugdev"

LABEL="solaar_end"
# vim: ft=udevrules
```

Add your user to the plugdev group so it can manage connected peripherals.

```Shell
sudo groupadd plugdev
sudo usermod -a -G plugdev $USER
groups $USER

sudo udevadm control --reload-rules
```

Now the mouse should show up in the device list and gestures can be configured.

If natural scrolling is all you need to configure, you can just update sway config file with this which may be way simpler

```Shell
input "<device_identifier>" {
    natural_scroll enabled
}
```

## Things to work on

- Stow the damn dot files!
- Make the fingerprint reader work
- Configure noise alerts when the battery is below 25% and 10%

</div>

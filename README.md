<div align=center>

![stars] ![forks] ![repo-size]
</div>

## Showcase :

| | |   
|:------------------------------:|:------------------------------:| 
|<img width="100%" src="https://github.com/cybergaz/void_repo/blob/master/screenshots/1676185887.png" alt="ss1">|<img width="100%" src="https://github.com/cybergaz/void_repo/blob/master/screenshots/1676186035.png" alt="ss2">|
|<img width="100%" src="https://github.com/cybergaz/void_repo/blob/master/screenshots/1676186789.png" alt="ss3">|<img width="100%" src="https://github.com/cybergaz/void_repo/blob/master/screenshots/1676202951.png" alt="ss4">|


Main purpose of this README is , I usually just forget what changes I've done , so ...    

<br>

___
<div align="center">

quick setup
===========

**To setup most of the things at ones :** (errors may raise out of nowhere)
<br>
    
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/cybergaz/scripts/refs/heads/main/system/hypr_setup.sh)"

</div>

___


<br>

<br>

## Essential packages (afaik) :

```
make wlroots wayland-protocols pkgconf ninja patch catch2 waybar brightnessctl pipewire pipewire-alsa pipewire-pulse pipewire-jack wireplumber xdg-desktop-portal-wlr hyprland

```
<br>

## Additional packages :
    wofi alacritty nemo ranger mako neofetch btop viewnior swaybg swayidle swaylock-effects waylogout-git swww zoxide cliphist wtype wl-clipboard polkit-gnome grim slurp satty-bin

<br>


### Screen sharing / recording issue :
1. remove every package named `xdg-desktop-portal-*` , check'em by :  
```
pacman -Q | grep xdg-desktop-portal-
```
2. now just install : 
```
yay -S pipewire wireplumber xdg-desktop-portal-hyprland-git
```
3. more detailed docs : [someone's notes](https://gist.github.com/PowerBall253/2dea6ddf6974ba4e5d26c3139ffb7580)

    
### Setup a polkit agent :
    yay -S polkit-gnome
* and then just initiate it your `hyprland.conf`        
```
exec-once = /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1
```


<br>

<div align="center">

## Theming
</div>

### GTK theme 
1. Download your fav theme and place the theme folder at : `/usr/share/themes` or `.local/share/themes`    
* i just use        
```
yay -S layan-gtk-theme-git
```
2. to set it up , just run        
```
gsettings set org.gnome.desktop.interface gtk-theme Layan-Dark
```
* for dark theme , you have to make changes at your : `~/.config/gtk-3.0/settings.ini`         
* * settings.ini -->  `gtk-application-prefer-dark-theme=true`  &  `gtk-theme-name=Layan-Dark`         

### GTK icons 

1. Download and place in `/usr/share/icon`   ( i use Kora btw )    
```
yay -S kora-icon-theme
```
2. and then just run          
```
gsettings set org.gnome.desktop.interface icon-theme {theme-folder}
```

### cursor theme 
1. Download and put them in : `/usr/share/icons/`          
* i use          
```
yay -S catppuccin-mocha-dark-cursors 
```
2. and then edit `/usr/share/icons/default/index.theme`           
3. also  
```
gsettings set org.gnome.desktop.interface cursor-theme {theme-folder}
```
 
### Fonts 
1.  Download ttf files and put them in : `/usr/local/share/fonts`     
2.  then give'em necesarry permissions like : `chmod 444 and 555`    

>  refer to powerlevel10k github readme font section for mesloLGS-nf  fonts

### SDDM theme 
1. Download sddm theme you want , and place them at : `/usr/share/sddm/themes/`         
2. set the theme , at `/usr/lib/sddm/sddm.conf.d/default.conf`        
* if that doesn't work , check out this place : `/etc/sddm.conf.d/`

### GRUB theme
1. Download and place at : `/usr/share/grub/themes/`            
2. then just edit `/etc/default/grub` : GRUB_THEME=`/usr/share/grub/themes/{theme_name}/theme.txt`      
3. a lil `update-grub` and you are good to go           

<br>

<div align="center">

## Extras
</div>

### for emoji's stuff

    yay -S wofi wtype wl-clipboard ttf-twemoji-color noto-fonts-emoji
    
    git clone https://github.com/cybergaz/scripts
    cd scripts/wofi-emoji
    sh wofi-emoji.sh


### android mount pkgs

    mtpfs   --> for media transfer protocol
    jmtpfs --> for later version supports
    gvfs-mtp --> for automount and all ( usb android file transfer )
    gvfs-gphoto2  --> for picture transfer protocol



<div align="center">

## Tips
</div>

* Alter power key,lid, idle related actions at `/etc/systemd/logind.conf`
---

* Install `powertop and tlp`   --> for power management in laptops    
---

* If you're facing pulseaudio & bluetooth related issue then, uninstall everything related to pulseaudio , pipewire and bluetooth ( bluez and all.. ) and then install : 
```
pulseaudio pulseaudio-bluetooth pulseaudio-ctl bluez bluez-utils blueman-git
```
---

* For nix packages search :    
```
nix-channel --add https://nixos.org/channels/nixpkgs-unstable
sudo nix search nixpkgs --extra-experimental-features nix-command --extra-experimental-features flakes
nix-channel --update
nix-env -qaP --description | awk '{$2 = ":"; print $0;}' > "nps.cache"
```
---

* To get rid of that **90s shutdown issue** :
* * head to the `/etc/systemd/system.conf` and overwrite these line -> 
```
DefaultTimeoutStartSec=1s
DefaultTimeoutStopSec=1s
DefaultTimeoutAbortSec=
DefaultDeviceTimeoutSec=1s
```
---

#### switching from `pulseaudio` to `pipewire`  
* install necessary packages   
```
yay -S pipewire pipewire-alsa pipewire-pulse pipewire-jack
```
* Stop PulseAudio services and enable pipewire services: 
```
systemctl --user stop pulseaudio.socket
systemctl --user stop pulseaudio.service
systemctl --user disable pulseaudio.socket
systemctl --user disable pulseaudio.service

systemctl --user enable pipewire.socket
systemctl --user enable pipewire.service
systemctl --user start pipewire.socket
systemctl --user start pipewire.service
```
---

* If you are facing bluetooth headset's `mic` related issue , then `try changing profile to HSP/HFP` using pavucontrol or pactl
---

* To adjust the `ownership and permissions of drives` :    
* * mount the partition at desired location say /mnt and then the following commands   
```
sudo chown -R yourusername:yourusername /mnt
sudo chmod -R u+w /mnt
sudo usermod -aG dis,storage,uuidd yourusername
```
---

#### iwd setup

1. create `/etc/iwd/main.conf` with following content :
```
[General]
EnableNetworkConfiguration=true

[Network]
EnableIPv6=true
```
2. enable following services 
```
sudo systemctl enable --now systemd-networkd
sudo systemctl enable --now systemd-resolved
sudo systemctl enable --now iwd
# important for some packages that relies only on resolv.conf (which is a NetworkManager thing)
sudo ln -rsf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```
> NOTE :
> To provide DNS for software that reads `/etc/resolv.conf` (which is created and used by NetworkManager) , such as `Go-lang softwares` like github-cli , some web-browsers etc, we need to link `/run/systemd/resolve/stub-resolv.conf` (which is created and used by IWD) to resolv.conf     
> more detailed docs : [arch wiki systemd-resolved](https://wiki.archlinux.org/title/Systemd-resolved#DNS) 

#### systemd-networkd for ethernet connections
+ create `/etc/systemd/network/20-wired.network` with following content (**change the name of ethernet interface to yours**)
```
[Match]
Name=enp0s20f0u5

[Network]
DHCP=yes
```
___

#### PGP keys importing error
+ run following cmds
```
sudo rm -rf /etc/pacman.d/gnupg
sudo pacman-key --init
sudo pacman-key --populate
sudo pacman -S archlinux-keyring
```

---
#### 'incorrect password' limit
+ open `/etc/security/faillock.conf` file and look for variable **deny and unlock-time** and change them accordingly








<!----------------------------------{ Labels }--------------------------------->

[repo-size]: https://img.shields.io/github/repo-size/cybergaz/hyprland_rice
[forks]: https://img.shields.io/github/forks/cybergaz/hyprland_rice?style=flat
[stars]: https://img.shields.io/github/stars/cybergaz/hyprland_rice

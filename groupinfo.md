
yum grouplist hidden
yum groupinfo

## xfce group info

```txt
Group: Xfce
 Group-Id: xfce-desktop
 Description: A lightweight desktop environment that works well on low end machines.
 Mandatory Packages:
   +Thunar
   +xfce4-panel
   +xfce4-session
   +xfce4-settings
   +xfconf
   +xfdesktop
   +xfwm4
 Default Packages:
   +NetworkManager-gnome
   +gdm
   +leafpad
   +openssh-askpass
   +orage
   +polkit-gnome
   +thunar-archive-plugin
   +thunar-volman
   +tumbler
   +xfce4-appfinder
   +xfce4-icon-theme
   +xfce4-power-manager
   +xfce4-pulseaudio-plugin
   +xfce4-session-engines
   +xfce4-terminal
   +xfwm4-theme-nodoka
 Optional Packages:
   xfwm4-themes
 Conditional Packages:
   +pinentry-gtk
```

## X Window System

```
Group: X 視窗系統
 Group-Id: x11
 Description: X Window 系統支援。
 Mandatory Packages:
   +glx-utils
   +initial-setup-gui
   +mesa-dri-drivers
   +plymouth-system-theme
   +spice-vdagent
   +xorg-x11-drivers
   +xorg-x11-server-Xorg
   +xorg-x11-utils
   +xorg-x11-xauth
   +xorg-x11-xinit
   +xvattr
 Optional Packages:
   mesa-libGLES
   tigervnc-server
   wayland-protocols-devel
   xorg-x11-drv-evdev
   xorg-x11-drv-keyboard
   xorg-x11-drv-libinput
   xorg-x11-drv-mouse
   xorg-x11-drv-openchrome
```

## GNOME Desktop

```
Environment Group: GNOME Desktop
 Environment-Id: gnome-desktop-environment
 Description: GNOME is a highly intuitive and user friendly desktop environment.
 Mandatory Groups:
   +base
   +core
   +desktop-debugging
   +dial-up
   +directory-client
   +fonts
   +gnome-desktop
   +guest-agents
   +guest-desktop-agents
   +input-methods
   +internet-browser
   +java-platform
   +multimedia
   +network-file-system-client
   +networkmanager-submodules
   +print-client
   +x11
 Optional Groups:
   +backup-client
   +gnome-apps
   +internet-applications
   +legacy-x
   +office-suite
   +remote-desktop-clients
   +smart-card
```




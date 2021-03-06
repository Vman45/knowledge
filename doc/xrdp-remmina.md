# xrdp remmina

*[networking](../README.md#networking)*

## prerequisites

```sh
apt install -y xrdp remmina
```

## gnome session

### tune ~/.xsessionrc

*behavior*: without follow taskbar and panel not appears

```
export GNOME_SHELL_SESSION_MODE=ubuntu
export XDG_CURRENT_DESKTOP=ubuntu:GNOME
export XDG_DATA_DIRS=${D}
export XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg

gnome-extensions enable desktop-icons@csoriano
gnome-extensions enable ubuntu-appindicators@ubuntu.com
gnome-extensions enable ubuntu-dock@ubuntu.com
```

- [reference](https://www.hiroom2.com/2018/04/29/ubuntu-1804-xrdp-gnome-en/)

### avoid color profile auth

*behavior*: without this authentication required for color profile

create `/etc/polkit-1/localauthority/50-local.d/45-allow-colord.pkla`

```
[Allow Colord all Users]
Identity=unix-user:*
Action=org.freedesktop.color-manager.create-device;org.freedesktop.color-manager.create-profile;org.freedesktop.color-manager.delete-device;org.freedesktop.color-manager.delete-profile;org.freedesktop.color-manager.modify-device;org.freedesktop.color-manager.modify-profile
ResultAny=no
ResultInactive=no
ResultActive=yes
```

```
systemctl restart polkit
systemctl restart xrdp
```

- [reference](http://c-nergy.be/blog/?p=12043)

## lxde session

```
apt install -y lxde
```

### tune ~/.xsession

```
cat > ~/.xsession <<EOF
startlxde
EOF
chmod +x ~/.xsession
```

### remmina connection

- Server : localhost
- User name : xxx
- User password : xxx
- Color depth : RemoteFX (32bpp)

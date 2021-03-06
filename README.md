# Container sandbox for GUI applications 
Script uses podman to create and run aplications inside container

#### Installing

```shell script
sudo dnf copr enable zirix/Podbox
sudo dnf install podbox
```
or download and use podbox.sh

#### Examples

```shell script
# create container with "ContainerName" name
# run as user, no root/sudo required
podbox create ContainerName --gui --net --ipc --audio

# then run bash
podbox bash ContainerName
# (use --root option to run bash as root)

# run command inside container
podbox exec ContainerName Command
# (use --root option to run command as root)

# Create desktop icon for command inside container
podbox desktop create ContainerName Command 'Desktop icon title'
# (use --icon /path/to/icon/ option or --cont_icon /path/to/icon/inside/container)

# add(share) path to container
podbox volume add ContainerName /path
```

#### Install Firefox inside container

```shell script
podbox create firefox --gui --net --ipc --audio
podbox exec firefox --root dnf install firefox libXt dbus-glib gtk3 pulseaudio-libs -y
podbox desktop create firefox firefox 'Firefox Inside Podbox' --icon firefox

Now you can run browser with desktop icon or:
podbox exec firefox firefox 

```

#### Install Tor browser inside container

```shell script
podbox create torbrowser --gui --net --ipc --audio
podbox exec torbrowser --root dnf install torbrowser-launcher libXt dbus-glib gtk3 pulseaudio-libs -y
podbox exec torbrowser torbrowser-launcher
podbox exec torbrowser --root cp -s /home/user/.local/share/torbrowser/tbb/x86_64/tor-browser_en-US/Browser/start-tor-browser /usr/bin/torbrowser
podbox read-only torbrowser on
podbox desktop create torbrowser torbrowser 'TorBrowser in PodBox' --icon torbrowser

Now you can run browser with desktop icon or:
podbox exec torbrowser torbrowser

```

```
Usage: 
  podbox command
Available Commands:
  create Name [OPTIONS]                   Create new container
    Available Options:
      --gui                                 Add X11 permission to run programs with gui
      --ipc                                 Add ipc permission. Should be used with gui option
      --audio                               Add PulseAudio permission to play audio
      --net                                 Add network permission
      --security on|off|unconfined          Enable/Disable SELinux permissions for container
      --map-user                            Map host user to guest user
      --volume /host/path[:/cont/path]      Mount path to container
  bash Name [--root]                      Run shell inside container
  exec Name command                       Run command inside container
  remove Name                             Remove container
  volume add Name /host/path [OPTIONS]    Add volume to container
    Available Options:
      --to [/container/path]                Set container path
      --type ro|rsync                       Moutn type
  volume rm Name /host/path               Remove volume from container
  read-only Name on|off                   Set container as read-only. All changes in container file system will be cleared on stop
  net Name on|off                         Add/Remove network permission
  ipc Name on|off                         Add/Remove ipc permission. Should be used with gui option
  audio Name on|off                       Add/Remove PulseAudio permission to play audio
  net Name on|off                         Add/Remove network permission
  security Name on|off|unconfined         Enable/Disable SELinux permissions for container
  map-user Name on|off                    Map/Unmap host user to guest user
  desktop create Name AppCmd AppName      Create desktop entry for container program
    Available Options:
      --icon /path/to/icon                  Set icon for desktop entry
      --cont_icon /path/to/icon             Set icon from container for desktop entry
      --categories /path/to/icon            Set categories for desktop entry
  desktop rm Name AppCmd                  Remove desktop entry
```

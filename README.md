# ChromeOS_Crouton_Debian_Buster_VSCode

### The following instructions are written and depend on the repositories files being found in the ~/Downloads folder.

### Put Chromebook in developer mode.
For my model it was `Esc + Refresh` and once you are at the recovery screen press `Ctrl + D` and `enter` to confirm.

#### Open terminal and enter shell.

Press `Ctrl+Alt+T` to enter terminal.

Type `shell` hit enter.

#### Download crouton and install to /usr/local/bin

```
sudo mkdir /usr/local/bin
```


```
sudo curl -L -# -o /usr/local/bin/crouton https://github.com/dnschneid/crouton/raw/master/installer/crouton && sudo chmod +x /usr/local/bin/crouton
```

#### Download and save Debian 10 release name Buster and create a bootstrap to save time on reinstalls.

```
sudo crouton -r buster -d -f ~/Downloads/mybuster.tar.bz2
```

#### Install enough targets to boot.

```
sudo crouton -f ~/Downloads/mybuster.tar.bz2 -t core,audio,cli-extra
```

*When prompted create a username and password.*

#### Enter Crouton.

```
sudo enter-chroot -n buster
```

#### Install some basic software while in crouton and then exit.

```
sudo apt install xterm xinit wget curl git -y
```

#### Add repositories for code-oss.

```
curl -s https://packagecloud.io/install/repositories/headmelted/codebuilds/script.deb.sh | sudo bash
```

#### Retrieve code-oss with wget.

```
wget --content-disposition https://packagecloud.io/headmelted/codebuilds/packages/debian/stretch/code-oss_1.43.0-1581920809_armhf.deb/download.deb
```

#### Manually install code-oss.

```
sudo apt install code-oss_1.43.0-1581920809_armhf.deb
```

#### Exit to chrosh.

`exit`

#### Install additional targets.

```
sudo crouton -n buster -t xiwi,extension,keyboard -u
```

#### Quickstart code-oss.

```
sudo startxiwi -n buster -T code-oss
```

#### Download Script to make rootfs rewriteable.(Only neccessary if you want to autostart Crouton.)

```
curl -Lk --connect-timeout 60 -m 300 --retry 2 "https://gist.githubusercontent.com/DennisLfromGA/6690677/raw/817787b7a6d2ca5d8993b8d4c2311cfc2ae86ae4/rw-rootfs" -o ~/Downloads/rw-rootfs
```

#### Make rootfs rewriteable. (Only neccessary if you want to autostart Crouton.)

```
sudo sh ~/Downloads/rw-rootfs
```

#### Autostart Crouton when Chromebook starts by copying these files into ChromeOS.

```
sudo cp ~/Downloads/crouton.conf /etc/init/
```

```
sudo cp ~/Downloads/crouton.init /usr/local/
```

#### Install Chromebrew and add nano utility.

```
curl -Ls https://raw.github.com/skycocker/chromebrew/master/install.sh | bash
```

```
crew update
```

```
crew upgrade
```

```
crew install nano
```

#### Add Common Alias to bash file.

```
sudo nano ~/.bashrc
```

```
alias buster="sudo enter-chroot -n buster"
```

```
alias code="sudo startxiwi -n buster -T code-oss"
```

#### Backup

```
sudo edit-chroot -b buster -f ~/Downloads/
```

#### Restore

```
sudo edit-chroot -f ~/Downloads/ -r buster
```

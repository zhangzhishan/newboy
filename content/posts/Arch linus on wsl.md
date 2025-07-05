---
title: Arch linus on wsl
tag: Arch, wsl
date: 2021-01-18 14:34:54
---
## update after install
+ `pacman-key --init`
+ `pacman-key --populate archlinux`
+ `pacman -Syu`


## new user
```
useradd -m zz
su zz
```

Use visudo to add your user to the sudoers.

## install yaourt
https://itsfoss.com/best-aur-helpers/

Open `/etc/pacman.conf` file and add these lines at the bottom:

```
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch
```

Save the change. Install Yaourt with the below command

```
sudo pacman -Syu yaourt
```

Use the command below to sync Yaourt with AUR:

```
yaourt -Syy
```
To install AUR packages, you can the below commands :

```
yaourt -S package-name
```

## yay
### install yay

```
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### use yay

| Command                         | Description                                                                                                                                         |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `yay <Search Term>`             | Present package-installation selection menu.      
|`yay -S package` | Install package using yay
| `yay -Rns package`|remove a package
| `yay -Ps`                       | Print system statistics.                                                                                                                            |
| `yay -Yc`                       | Clean unneeded dependencies.                                                                                                                        |
| `yay -G <AUR Package>`          | Download PKGBUILD from ABS or AUR.                                                                                                                  |
| `yay -Y --gendb`                | Generate development package database used for devel update.                                                                                        |
| `yay -Syu --devel --timeupdate` | Perform system upgrade, but also check for development package updates and use PKGBUILD modification time (not version number) to determine update. |



> [呼呼呼山]()(http://code4fun.me)
> 2021-01-18 14:34:54

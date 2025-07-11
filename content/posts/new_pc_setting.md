---
title: Windows新机设置
tag: windows
date: 2018-10-08 21:15:59
---
由于最近入职，多了一台Windows笔记本，一台台式机以及数台虚拟机，由于这些环境都是Windows，而以后难免要长期混迹再这个平台上，所以就总结一下该如何配置环境。

## install dependences
当然，第一步是安装一些必须的软件，主要是大平台的软件，开箱即用，没有什么配置的要求，比较简单，列个列表足矣。

利用Scoop安装相应的软件并进行配置，在powershell中执行下述命令

```
Set-ExecutionPolicy Unrestricted
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

添加额外的库，该库里面包含一些常用的软件。 [fork](https://git-fork.com/)
```
Mouse Without Borders
```

```bat
scoop install git
scoop bucket add extras
scoop bucket add Versions
scoop bucket add Ash258 'https://github.com/Ash258/scoop-Ash258.git'
scoop bucket add nerd-fonts
; picgo
; scoop bucket add helbing https://github.com/helbing/scoop-bucket
```

```cmd
scoop install sudo
sudo scoop install 3270-NF
scoop install ack
scoop install Ag
scoop install autohotkey
scoop install bandzip
scoop install bat
scoop install beyondcompare
; scoop install brave
scoop install caffeine
scoop install calibre-normal
; scoop install chromium
scoop install cmake
scoop install cmake-rc
scoop install copyq
scoop install ditto
scoop install dropit
scoop install everything
scoop install firefox
scoop install fork
scoop install fzf
; scoop install gitkraken
scoop install graphviz
scoop install gdrive
scoop install keypirinha
scoop install lepton
scoop install less
scoop install listen1desktop
scoop install llvm
scoop install megasync
scoop install neovim
scoop install nodejs
scoop install oh-my-posh
scoop install pandoc
scoop install perl
scoop install picgo
scoop install postman
scoop install PotPlayer
scoop install powertoys
scoop install python27
; scoop install python
scoop install rescuetime
scoop install ripgrep
scoop install rustup
scoop install ruby
scoop install sharex
scoop install sharpkeys
scoop install spotify
; scoop install steam
scoop install sublime-merge
scoop install sumatrapdf
scoop install SyncTrayzor
scoop install telegram
; scoop install terminus
scoop install typora
scoop install vim
scoop install vnote
scoop install vscode
; scoop install weasel
scoop install wiztree
; scoop install workflowy
scoop install zeal

curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```
## mklink 相关配置

有些软件只有一个主配置文件存在于用户目录，对于这种只需要直接进行一次HardLink，可以借助`mklink`命令。

```
mklink /H c:\Users\zhizha\vimrc.bundles "c:\Users\zhizha\Dropbox (Personal)\settingsync\dotfiles\vim\winvim\vimrc.bundles"
del c:\Users\zhizha\_vimrc
mklink /H c:\Users\zhizha\_vimrc "c:\Users\zhizha\Dropbox (Personal)\settingsync\dotfiles\vim\_vimrc"
mklink /J C:\Users\zhizha\vimfiles "C:\Users\zhizha\Dropbox (Personal)\settingsync\dotfiles\vim\vimfiles"
mklink /J C:\Users\zhizha\AppData\Local\nvim "C:\Users\zhizha\Dropbox (Personal)\settingsync\dotfiles\vim\nvim"
mklink /J C:\Users\zhizha\vimconfigs "c:\Users\zhizha\localdropbox\Dropbox (Personal)\settingsync\dotfiles\vim\vimconfigs"
mklink /J C:\Users\zhizha\AppData\Roaming\vnote\snippets "C:\Users\zhizha\Dropbox (Personal)\settingsync\vnote\snippets"
mklink /J C:\Users\zhizha\AppData\Roaming\vnote\templates "C:\Users\zhizha\Dropbox (Personal)\settingsync\vnote\templates"
del C:\Users\zhizha\AppData\Local\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\profiles.json
mklink /H C:\Users\zhizha\AppData\Local\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\profiles.json "C:\Users\zhizha\localdropbox\Dropbox (Personal)\settingsync\dotfiles\profiles.json"
```
这里我们从简单到复杂一步步来，
## Visual Studio Code

对于visual studio code 也可也直接利用scoop进行安装，`scoop install vscode`。
作为目前使用的几乎最多的一个编辑器，自然装了很多插件以及主题、字体等进行了一番设置，这时候我们可以借助一个神奇的插件 `Settings Sync`， 其利用Github Gist进行设置的配置。只需要输入正确的Token和Gist id，就轻松的完成了同步，并且可以设置自动更新与下载，这样以后某一天电脑上做一个改动，另一台电脑上也会随之改动。

### install cquery
主要参考[安装指南](https://github.com/cquery-project/cquery/wiki/Visual-Studio-Code)

```
git clone --recursive https://github.com/cquery-project/cquery.git
cd cquery
mkdir build && cd build
cmake -DCMAKE_GENERATOR_PLATFORM=x64 .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=release -DCMAKE_EXPORT_COMPILE_COMMANDS=YES
cmake --build . --config release
cmake --build . --target install --config release
```
安装后需要根据文档，进行相关配置，提供高亮。
## 输入法 Rime
在Mac 上一直用鼠须管，所有到了Windows下输入法也就相应的选择了小狼毫，虽然在词库方面和主流的像搜狗之类的存在一些差距，但是毕竟省心，不会动不动弹什么又有垃圾需要清理了或者浏览器不快了之类的东西。

![](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1539004595_4563.png)


这就是一个输入法，并且是一个几乎没有任何卡顿的输入法。他不会干任何其余的事情，而词库方面你也可以自己添加一些词库，网友也做好了很多扩展词库，这个就可以根据自己的需要添加就好了。

安利完之后，就是设置，其实这个也比较简单，右键打开输入法菜单，选择用户文件夹，所有的配置和你手动添加的词库都在这个地方，直接复制到另外的电脑上就好了，当然 `installation.yaml` 中，你可以修改同步的位置，然后点击菜单中的同步个人文件夹，你就可以在指定的位置看到这些设置了。利用这种方式也可以共享多台设备之间的用户词典，也就是那些你自己经常输入的词汇。这个在不同的sync id 的用户词典也会进行合并。

## ConEmu
由于其设置在文件夹目录下，所以直接在官网下载，然后将其放到dropbox同步文件夹内，这样就可以在多台设备上同步了。

## Powershell setting

```
scoop innstall oh-my-posh
Install-Module PSColors -Scope CurrentUser -AllowClobber
Install-Module TabExpansionPlusPlus -Scope CurrentUser -AllowClobber
Install-Module Find-String
Install-Module Jump.Location
Install-Module ZLocation
Install-Module Get-ChildItemColor
Install-Module posh-git
Install-Module PSReadline
Import-Module oh-my-posh
```
### install poweline fonts
[meslo lg m](https://github.com/powerline/fonts/blob/master/Meslo%20Slashed/Meslo%20LG%20M%20Bold%20for%20Powerline.ttf)
## Vim
对于Vim来说，这是一个既有可能简单，也有可能复杂的问题，其实在大部分情况下，直接同步`.vimrc`或者`_vimrc`就可以，以及可能根据你的插件依赖选择一个插件管理器放到指定的文件夹下面，然后启动vim，运行相关的plugin install命令既可。
但是这个过程对于不同的操作系统会有各种不同的配置，需要特别注意。
## 字体设置
目前在用的编程相关的字体有两份，更纱黑体和pragmatapro，直接利用powershell脚本安装。

```
$Path="C:\Users\zhizha\Dropbox (Personal)\settingsync\font"
$FONTS = 0x14
$objShell = New-Object -ComObject Shell.Application
$objFolder = $objShell.Namespace($FONTS)
$Fontdir = dir $Path
foreach($File in $Fontdir) {
    If (Test-Path "C:\Users\zhizha\AppData\Local\Microsoft\Windows\Fonts\$($File.name)")
    { }
    Else
    {
        $objFolder.CopyHere($File.fullname)
    }
}
```

### install nerd-fonts
```
scoop bucket add nerd-fonts
sudo scoop install 3270-NF
```
## blog setting
`hexo clean && hexo deploy`


> [呼呼呼山](http://code4fun.me)
> 2018-10-08 21:15:59

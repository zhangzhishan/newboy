---
title: backup method
tags:
  - backup
date: 2025-06-23 22:13
---

个人备份设置.

tools and services:

* rsync
* syncthing
* duplicacy-cli
* dropbox
* onedrive


基本策略:

* Most files are synced between several machines through `syncthing`.
* Use `rsync` to sync imortant files from `syncthing` to `dropbox`
* Use `duplicacy-cli` to backup given folders to `onedrive`

dropbox ignore files:

```
Set-Content -Path "C:\Users\zhizha\Dropbox (Personal)\code\tele2dropbox\target" -Stream com.dropbox.ignored -Value 1
Set-Content -Path "C:\Users\zhizha\Dropbox (Personal)\code\poddownload\target" -Stream com.dropbox.ignored -Value 1
```


stignore file

```
// Windows image file caches
Thumbs.db
ehthumbs.db

// Folder config file
Desktop.ini

// Recycle Bin used on file shares
$RECYCLE.BIN/

// OSX
.DS_Store
.AppleDouble
.LSOverride

// Icon must end with two \r
Icon


// Thumbnails
._*

// Files that might appear on external disk
.Spotlight-V100
.Trashes

// Directories potentially created on remote AFP share
.AppleDB
.AppleDesktop
Network Trash Folder
Temporary Items
.apdisk


// Linux
*~

// KDE directory preferences
.directory

// Linux trash folder which might appear on any partition or disk
.Trash-*

// Peazip temp directory
.pmtp*

// Ignore directory exclude for large items
/exclude
```

## rsync from syncthing to dropbox
As there are many rust code in my folder, to avoid sync the large built binary. 不去 sync 所有名字是 target 的文件夹. 虽然有些暴力, 但是总比每次去添加来的好.

based on [this](https://arshaw.com/exclude-node-modules-dropbox-google-drive)

Create a new bash script with a .sh extension:

```sh
#!/usr/bin/env bash

set -e # always immediately exit upon error

# directory config. ending slashes are important!
src_dir="$HOME/Projects/"
dest_dir="$HOME/Dropbox/Projects-Backup/"

# run the sync
rsync -ar --delete \
  --filter=':- .gitignore' \
  --exclude='node_modules' \
  --exclude='.git' \
  --exclude='.DS_Store' \
  --chmod='F-w' \
  "$src_dir" "$dest_dir"
```
Make sure it has execution permission. You can run `chmod +x <path-to-file>` to do this.

Change the `src_dir` and `dest_dir` to the directories you’d like. Make sure to include trailing slashes! Our rsync setup needs this.

Please add or remove the `--exclude` args to your liking.

The a flags is a compound flag telling rsync to do a bunch of things that you don’t need to worry about. The r flags tells rsync to work recursively. The --delete flag tells rsync to delete files in dest_dir when they no longer exist in src_dir. I always do this because I don’t want dest_dir to accumulate old deleted files. If I want access to old deleted files, I can use Dropbox’s or Google Drive’s history.

The --filter=':- .gitignore' portion of the rsync command is really neat. It tells rsync to exclude the files that are listed in the .gitignore file in each directory. These are files you already don’t care about, such as vendor files or temp files, so you won’t be forced to create another list!

The --chmod='F-w' portion tells rsync that the copied files (but not directories) should have their write permissions removed. This is a good idea because it prevents us from accidentally going into the dest_dir instead of the authoritative src_dir and making edits. We unfortunately can’t do this for directories because we need to allow rsync to add new files and delete old ones.
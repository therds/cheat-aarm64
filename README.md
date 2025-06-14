# cheat-aarm64
cheat for android arm 64 on termux (googl play)
## Install request goland and make  
```bash
$ apt update && upgrade
$ apt install golang make git
Download size: 130 MB
Space needed: 801 MB
...
$ go version
go version go1.24.2 android/arm64**
make -v -q
GNU Make 4.4.1
Built for aarch64-unknown-linux-android
...
$echo $TERMUX_VERSION
googleplay.2025.01.18
```
## Install tool for build Cheat
### option add path local-bin for run software
$ cd $HOME
$ mkdir local-bin
Add path to .bashrc or .zshrc
$ export PATH=~/bin-local:$PATH
...login again for new  path
### buide "revive" for Cheat requests during compilation
```bash
$ mkdir golang
$ cd golang
$ git clone https://github.com/mgechev/revive
$ cd revive
$ make
...
$ file revive
revive: ELF shared object, 64-bit LSB arm64, dynamic (/system/bin/linker64), for Android 29, built by NDK r27c (12479018),..

$ cd revive ~/bin-local
$ chmode +x ~/bin-local/revive
$ revive -version
Version:        v1.10.0-4-g426a27a
Commit:         426a27ac0d6cbe7f3dba4ce0dd9e645a913a3a5a
Built           2025-06-13 13:55 UTC by Makefile
```
### Build Cheat for andriod arm64 (termux)

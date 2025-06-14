# cheat-aarm64
[Cheat](https://github.com/cheat/cheat) for android arm 64 on termux (googl play)
## Install request goland and make  
```bash
$ apt update && upgrade
$ apt install golang make git
Download size: 130 MB
Space needed: 801 MB
```
```
```bash
$ go version
go version go1.24.2 android/arm64**
$ make -v
GNU Make 4.4.1
Built for aarch64-unknown-linux-android
...
```
```bash
$ echo $TERMUX_VERSION
googleplay.2025.01.18
```
## Install tool for build Cheat
### option add path local-bin for run software
```bash
$ cd $HOME
$ mkdir local-bin
Add path to .bashrc or .zshrc
$ export PATH=~/bin-local:$PATH
...login again for new  path
```
### go install "revive" and "cs"  for Cheat requests during compilation (make setup > go get)
```bash
$ go install github.com/boyter/cs@latest
$ cd ~/go/bin
$ file cs
revive: ELF shared object, 64-bit LSB arm64, dynamic (/system/bin/linker64), for Android 29, built by NDK r27c (12479018), B...
$ cp cs ~/bin-local/
$ chmode +x ~/bin-local/cs
$ cs --version
cs version 1.4.0
```
```bash
$ go install github.com/mgechev/revive@latest
$ cd ~/go/bin
$ file revive
revive: ELF shared object, 64-bit LSB arm64, dynamic (/system/bin/linker64), for Android 29, built by NDK r27c (12479018), B...
$ cp revive ~/bin-local/
$ chmode +x ~/bin-local/revive
$ revive -version
Version:        v1.10.0-4-g426a27a
Commit:         426a27ac0d6cbe7f3dba4ce0dd9e645a913a3a5a
Built           2025-06-13 13:55 UTC by Makefile
```
### Build Cheat for andriod arm64 (termux)
```bash
$ git clone https://github.com/cheat/cheat
...
$ cd cheat
$ make help
$ make check
go mod vendor && go mod tidy && go mod verify
all modules verified
go fmt ./...
revive -exclude vendor/... ./...
go vet ./...
go: creating work dir: stat /tmp: no such file or directory
make: *** [Makefile:181: vet] Error 
```
Modifi Makefile for termux andriod arm64
```bash
$ cp Makefile Makefile.org
$ nano Makefile
...
GZIP   := gzip
...
TMPDIR       :=
...
# release binaries
releases :=                        \
    $(dist_dir)/cheat-android-arm64 \                                                                                                                                          $(dist_dir)/cheat-darwin-amd64 \
...
## build-release: build release executables                                                                                                                           .PHONY: build-release
build-release: $(releases)

# cheat-android-arm64
$(dist_dir)/cheat-android-arm64: prepare
    GOARCH=arm64 GOOS=android \                                                                                                                                           $(GO) build $(BUILD_FLAGS) -o $@ $(cmd_dir) && $(GZIP) $@ && chmod -x $@.gz
```
Make check again
```bash
$ make check
go mod vendor && go mod tidy && go mod verify
all modules verified
go fmt ./...
revive -exclude vendor/... ./...
go vet ./...
go test ./...
?       github.com/cheat/cheat/cmd/cheat        [no test files]
ok      github.com/cheat/cheat/internal/cheatpath       0.023s
ok      github.com/cheat/cheat/internal/config  0.030s
ok      github.com/cheat/cheat/internal/display 0.032s
?       github.com/cheat/cheat/internal/installer       [no test files]
?       github.com/cheat/cheat/internal/mock    [no test files]
?       github.com/cheat/cheat/internal/repo    [no test files]
ok      github.com/cheat/cheat/internal/sheet   0.069s
ok      github.com/cheat/cheat/internal/sheets  0.043s
```
If not error. Make it possible to run on current machines.  
```bash
$ make
rm -f ./dist/* ./cmd/cheat/str_config.go ./cmd/cheat/str_usage.go
mkdir -p ./dist
go generate ./cmd/cheat
go fmt ./...
go mod vendor && go mod tidy && go mod verify
all modules verified
revive -exclude vendor/... ./...
go vet ./...
pandoc -s -t man doc/cheat.1.md -o doc/cheat.1
make: pandoc: Permission denied
make: [Makefile:163: man] Error 127 (ignored)
go build -ldflags="-s -w" -mod vendor -trimpath -o ./dist/cheat ./cmd/cheat
```
test run and copy to bin-local
```bash
~/golang/cheat > ./dist/cheat -v
4.4.2
~/golang/cheat > cp ./dist/cheat ~/bin-local/
~/golang/cheat > chmod +x ~/bin-local/cheat
~/golang/cheat > cd
~/ > cheat -h
Usage:
  cheat [options] [<cheatsheet>]
```
config Cheat run local termux android
```bash
~/ > cheat
A config file was not found. Would you like to create one now? [Y/n]: y
Would you like to download the community cheatsheets? [Y/n]: y
Cloning community cheatsheets to /data/data/com.termux/files/home/.config/cheat/cheatsheets/community.
Enumerating objects: 335, done.
Counting objects: 100% (335/335), done.
Compressing objects: 100% (310/310), done.
Total 335 (delta 43), reused 213 (delta 23), pack-reused 0 (from 0)
Cloning personal cheatsheets to /data/data/com.termux/files/home/.config/cheat/cheatsheets/personal.
Created config file: /data/data/com.termux/files/home/.config/cheat/conf.yml
Please read this file for advanced configuration information.
~/ > cheat chate
```
test otherd command is work
```bash
~/ > cheat -d
community: /data/data/com.termux/files/home/.config/cheat/cheatsheets/community
personal:  /data/data/com.termux/files/home/.config/cheat/cheatsheets/personal
~/ > cheat -T
apache
audio
...
```
```bash
~/ > cheat -e mycheat
---
tags: [ mycheat ]
---
# My First cheat ,
echo "My First cheat on termux andriod aarm64"
```
```bash
~/ > cheat mycheat
# My First cheat ,
echo "My First cheat on termux andriod aarm64"
```

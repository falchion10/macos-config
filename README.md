# Disable Various System Restrictions

Disable SIP: 

```
csrutil disable
```

Disable SSV: 

```
csrutil authenticated-root disable
```

Disable Gatekeeper (Sonoma and below):

```
sudo spctl --master-disable
```

Best Bootargs: 

```
sudo nvram boot-args="-v -arm64e_preview_abi amfi_get_out_of_my_way=1 ipc_control_port_options=0"
```

Permissive security, disable kernel kext signing, disable ktrr, disable boot arg restrictions, disable ssv, allow remote mdm:

```
bputil -nkcasm
```

# Essential Defaults

Directly under the first command is it's reverse counterpart to undo any changes done.

Faster Dock Hiding:

```
defaults write com.apple.dock autohide-delay -float 0 && defaults write com.apple.dock autohide-time-modifier -int 0 && killall Dock
```

```
defaults write com.apple.dock autohide-delay -float 0.5 && defaults write com.apple.dock autohide-time-modifier -int 0.5 && killall Dock
```

Disk Warning Disable:

```
sudo defaults write /Library/Preferences/SystemConfiguration/com.apple.DiskArbitration.diskarbitrationd.plist DADisableEjectNotification -bool true && sudo pkill diskarbitrationd
```

```
sudo defaults delete /Library/Preferences/SystemConfiguration/com.apple.DiskArbitration.diskarbitrationd.plist DADisableEjectNotification && sudo pkill diskarbitrationd
```

Show Hidden Files Always:

```
defaults write com.apple.finder AppleShowAllFiles -bool true && killall Finder
```

```
defaults delete com.apple.finder AppleShowAllFiles && killall Finder
```

Tahoe Beta 3 White Screenshots Fix:

```
defaults write com.apple.screencapture disable-shadow -bool true
```

```
defaults delete com.apple.screencapture disable-shadow
```

Notification for Crash Reports:

```
defaults write com.apple.CrashReporter UseUNC 1
```

```
defaults delete com.apple.CrashReporter UseUNC
```

Edit Launchpad Layout:

```
defaults write com.apple.dock springboard-rows -int 5
```
```
defaults write com.apple.dock springboard-columns -int 10
```
```
killall Dock
```

Stock Launchpad Layout:

```
defaults delete com.apple.dock springboard-rows && defaults delete com.apple.dock springboard-columns && killall Dock
```

Disable Font Smoothing: 

```
defaults -currentHost write -g AppleFontSmoothing -int 0
```

```
defaults -currentHost delete -g AppleFontSmoothing
```

Enable Metal HUD (does not persist through reboots): 

```
launchctl setenv MTL_HUD_ENABLED 1
```

```
launchctl unsetenv MTL_HUD_ENABLED
```

## Parallels Flags:

```
vm.efi.secureboot=0
video.hw_pointer=0
```

## zshrc stuff

```
alias ls="eza --icons --group-directories-first -a"
alias ll="eza --icons --group-directories-first -al"
alias cafd="caffeinate -dism"
alias ff="fastfetch"
```

## Folders to check when reducing System Data: (Please don't do this lol)

```
~/Libary/Caches
sudo rm -rf /private/var/db/diagnostics/*
sudo rm -rf /private/var/folders/_p/*
sudo rm -rf /Libary/Logs/*
sudo rm -rf /private/var/db/uuidtext/*
```

Custom Screen Saver (semi-broken): 

```
/Library/Application Support/com.apple.idleassetsd/Customer/4KSDR240FPS
```

# Development Kernel Stuff:

https://developer.apple.com/download/all/?q=Kernel%20Debug%20Kit

https://kernelshaman.blogspot.com/2021/02/building-xnu-for-macos-112-intel-apple.html?m=1

```
kmutil create -a arm64e -z -V development -n boot \
-B DevKernel.kc \
-k /Library/Developer/KDKs/KDK_26.0_25A5279m.kdk/System/Library/Kernels/kernel.development.t6041 \
-r /System/Library/Extensions \
-r /System/Library/DriverExtensions \
-x \
$(kmutil inspect -V release --no-header | awk '{print " -b "$1; }')
```

macOS Boot Logo File:

```
/System/Library/PrivateFrameworks/LoginUIKit.framework/Versions/A/Resources/Assets.car
```

## macOS Jailbreak Guide:

https://github.com/falchion10/macOS-Jailbreak

# Bypass apps not opening

```
xattr -cr Application.app
```

```
sudo codesign --force --deep --sign - Application.app
```

adhoc sign with Sentinel: https://github.com/alienator88/Sentinel

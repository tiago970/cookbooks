# macOS (a.k.a. Darwin)

<!--
https://matthewhoelter.com/2019/06/03/flush-and-reset-dns-cache-on-macos.html

sudo spctl --master-disable
-->

## Applications

- [Digital Color Meter](https://support.apple.com/guide/digital-color-meter/welcome/mac)

## References

- [How to create a bootable installer for macOS](https://support.apple.com/en-us/HT201372)

<!--
https://eshop.macsales.com/guides/Mac_OS_X_Compatibility
-->

## Commands

```sh
#
system_profiler SPSoftwareDataType

#
sw_vers

#
softwareupdate --all --install --force
```

## Configuration

### Desktop Services Store

```sh
# Git ignore
echo '/**/.DS_Store' >> ~/.gitignore_global
```

### Desktop & Documents Folder

1. System Preferences
2. Apple ID
3. iCloud
4. iCloud Drive -> Options...
   - Check: Desktop & Documents Folder
   - Done

## Issues

### True Tone

1. System Preferences
2. Displays
3. Display Tab -> Uncheck "True Tone"

### 802.1X Network

```log
Select the certificate or enter the name and password for this 802.1X network
```

1. System Preferences
2. Network
3. Select: Thunderbolt Ethernet
4. Button: Advanced...
5. Tab: 802.1X
6. Uncheck: Enable automatic connection

## Docs

### Stop Rearrange Spaces

1. System Preferences
2. Mission Control
3. Automatically rearrange Spaces based on most recent use

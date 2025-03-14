# Visual Studio Code

<!--
https://code.visualstudio.com/docs/remote/ssh
https://github.com/datalayer-examples/vscode-extension-examples
-->

## Guide

- [Debugging](https://code.visualstudio.com/docs/editor/debugging)

## App

### Dependencies

- [cURL](/curl.md)
- [jq](/jq.md)
- [moreutils](/moreutils.md)

#### APT

```sh
sudo apt update
sudo apt -y install software-properties-common apt-transport-https wget
```

### Installation

#### Homebrew

```sh
brew install --cask visual-studio-code
```

#### APT

```sh
wget -q https://packages.microsoft.com/keys/microsoft.asc -O - | \
  sudo apt-key add -

sudo add-apt-repository 'deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main'
```

```sh
sudo apt update
sudo apt -y install code
```

#### Zypper

```sh
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[vscode]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'

sudo zypper refresh
sudo zypper --non-interactive install code
```

#### Chocolatey

```sh
choco install -y vscode
```

### Configuration

```sh
# Darwin
mkdir -p "$HOME/.config/Code/User"
ln -s "$HOME/Library/Application Support/Code/User/settings.json" "$HOME/.config/Code/User/settings.json"

# Git Ignore Global
cat << EOF >> ~/.gitignore_global
/**/.settings
/**/.vscode
EOF
```

### Commands

```sh
code -h
```

### Usage

```sh
#
code ./[filename]

# Open current
code ./

# List installed
code --list-extensions

# Install extension
code --install-extension [extension]

# Disable
code --disable-extension [extension]
```

### Tips

#### Unsaved Files

```sh
# Darwin
cat ~/Library/Application\ Support/Code/Backups
```

#### Proxy

```sh
# Add
jq '."http.proxy" |= "'$http_proxy'"' "$HOME/.config/Code/User/settings.json" | sponge "$HOME/.config/Code/User/settings.json"
jq '."http.proxyAuthorization" |= "null"' "$HOME/.config/Code/User/settings.json" | sponge "$HOME/.config/Code/User/settings.json"

# Delete
jq 'del(."http.proxy")' "$HOME/.config/Code/User/settings.json" | sponge "$HOME/.config/Code/User/settings.json"
jq 'del(."http.proxyAuthorization")' "$HOME/.config/Code/User/settings.json" | sponge "$HOME/.config/Code/User/settings.json"
```

#### Useful Plugins

```sh
#
code --install-extension mkxml.vscode-filesize

#
code --install-extension shardulm94.trailing-spaces

#
code --install-extension Tyriar.sort-lines

#
# code --install-extension wmaurer.change-case

#
# code --install-extension ephoton.indent-switcher
```

<!-- #### GlassIt

```sh
code --install-extension s-nlf-fh.glassit
```

TODO -->

#### Command-line completion

```sh
# Using Oh My Zsh
sed -ri 's/^plugins=\((.*)\)/plugins=\(\1 vscode\)/g' ~/.zshrc

source ~/.zshrc

rm ~/.zcompdump*
```

#### Close from CLI

```sh
# Darwin
osascript -e 'quit app "Visual Studio Code"'
```

#### Shortcuts

- `⌘` Command
- `⇧` Shift
- `⌥` Option
- `⌃` Control
- `↩︎` Return/Enter

- `Ctrl + Shift + l` Delete line

### Issues

#### Import Failed

```log
error: https://packages.microsoft.com/keys/microsoft.asc: import read failed(2).
```

Install [cURL](/curl.md).

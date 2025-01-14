# Git Submodules

## Usage

```sh
# Add
git submodule add [repo] ./[destination]

# Remove folder/module from git repository
git rm -r --cached [destination]

# Update from submodule folder
cd [destination] && git pull

# Update all submodules
git submodule foreach git pull origin master

# Close with submodules
git clone --recursive [repo]

git submodule foreach git checkout master
git submodule foreach git checkout main

# Get submodules from cloned repo
git submodule update --init --recursive
git submodule foreach git pull origin master
git submodule foreach git checkout master

# Sync
git submodule sync --recursive

# Update
git submodule update --init --recursive
```

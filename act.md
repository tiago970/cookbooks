# act

## Links

- [Code Repository](https://github.com/nektos/act)
- [GitHub / Settings / Personal Access Tokens](https://github.com/settings/tokens)

## Guides

- [Webhook events and payloads / push](https://docs.github.com/en/developers/webhooks-and-events/webhooks/webhook-events-and-payloads#push)

## CLI

### Installation

#### Homebrew

```sh
brew install act
```

#### Linux Binary

```sh
curl \
  -L \
  'https://github.com/nektos/act/releases/download/v0.2.23/act_Linux_x86_64.tar.gz' | \
    tar -xzC /usr/local/bin act
```

#### Chocolatey

```sh
choco install act-cli
```

### Commands

```sh
act -h
```

### Configuration

```sh
#
cat << EOP > ~/.actrc
-P ubuntu-latest=catthehacker/ubuntu:act-latest
-P ubuntu-20.04=catthehacker/ubuntu:act-20.04
-P ubuntu-18.04=catthehacker/ubuntu:act-18.04
ubuntu-16.04=catthehacker/ubuntu:act-16.04
EOP

# Git ignore
echo '/.secrets' >> ~/.gitignore_global
```

### Usage

```sh
# List actions
act -l

act \
  -l \
  -W ./.github/workflows/[name].yml

#
act -g

# Run on push
act push \
  --rm \
  -v

# Run on pull request
act pull_request \
  --rm \
  -v

# Run on release
act release \
  --rm \
  -v

# Run on workflow dispatch
act workflow_dispatch \
  --rm \
  -v

# Run specific job
act \
  -j [name] \
  -W ./.github/workflows/[name].yml \
  --rm \
  -v

# Cache Folder
ls ~/.cache/act
```

### Tips

### Inputs

```sh
#
act workflow_dispatch \
  -j [name] \
  -e <(cat << EOF
{
  "action": "workflow_dispatch",
  "inputs": {
    "name": "Mr. Bill"
  }
}
EOF
)
```

### Visual Studio Code

**Dependencies:** [dotENV](/dotenv.md#visual-studio-code) VSCode plugin.

```sh
jq '."files.associations".".secrets" |= "dotenv"' "$HOME/.config/Code/User/settings.json" | \
  sponge "$HOME/.config/Code/User/settings.json"
```

### Issues

#### Missing GitHub Token

```log
Error: Error undefined: Parameter token or opts.auth is required
```

```sh
export GITHUB_TOKEN=''
```

#### Update Token

```log
DEBU[0003] authentication required
```

Update your `GITHUB_TOKEN` in `.secrets` file.

#### Missing Language Environment

```log
Warning: the environment variable LANG is not set!
```

```yaml
jobs:
  test:
    steps:
    - name: # ...
      env:
        LC_ALL: C.UTF-8
        LANG: C.UTF-8
      run: # ...
```

#### Missing Events

```log
ERRO[0000] Unable to interpolate string '${{!contains(github['event']['pull_request']['title'], '[SKIP CI]')}}' - [TypeError: Cannot access member 'title' of undefined]
```

```sh
# Using local-act.event.json
act \
  -W ./.github/workflows/ci.yml \
  -e <(cat << EOF
{
  "pull_request": {
    "title": "[SKIP CI] My Awesome Contribution"
  }
}
EOF
) \
  -v
```

<!-- ####

```log
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```

TODO -->

<!-- ####

```log
FATA[0000] Unable to build dependency graph!
```

TODO -->

<!-- ####

```log
Error: unable to find api field in struct HorizontalPodAutoscalerSpec for the json field "behavior"
```

TODO -->

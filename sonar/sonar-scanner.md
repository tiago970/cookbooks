# SonarQube Scanner

<!--
https://github.com/josephjsf2/josephjsf2.github.io/blob/b4780c2e99bd5b69b1d32ae3f8f7d9665360f758/_posts/others/2019/03/20/2019-03-20-SonarQube-1.md
https://github.com/Stashchenko/sonarqube/blob/dc356fb8550c1c8605ae49c5c10a2b0e53d105b4/sonar-analyze.sh
-->

## CLI

### Plugins

- [SonarQube PHP Plugin](/sonar-php-plugin.md)

### Dependencies

- [SonarQube](/sonarqube.md#docker)

#### Unix-like

- [GNU Wget](/gnu-wget.md)
- [UnZip](/unzip.md)

### Installation

#### Homebrew

```sh
brew install sonar-scanner
```

#### Unix-like

```sh
wget \
  -O /tmp/sonar-scanner-cli.zip \
  'https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472.zip'

( cd /tmp && unzip ./sonar-scanner-cli.zip -d /opt && rm ./sonar-scanner-cli.zip )

ln -s /opt/sonar-scanner-4.6.2.2472 /opt/sonar-scanner
```

### Environment

#### Unix-like

For Bash or Zsh, put something like this in your `$HOME/.bashrc` or `$HOME/.zshrc`:

```sh
# SonarQube Scanner
export PATH="/opt/sonar-scanner/bin:$PATH"
```

### Properties

```sh
# Darwin
export SONARQUBE_HOME="$(brew --cellar sonar-scanner)/$(brew info --json sonar-scanner | jq -r '.[0].installed[0].version')/libexec"

sed -i '/^#sonar.host.url=.*/ s/#//' "$SONARQUBE_HOME/conf/sonar-scanner.properties"
sed -i '/^#sonar.sourceEncoding=.*/ s/#//' "$SONARQUBE_HOME/conf/sonar-scanner.properties"
```

### Commands

```sh
sonar-scanner -h
```

### Configuration

```sh
echo -e '[INFO]\thttp://127.0.0.1:9000'
```

1. Projects -> Create new project
   - Project key: com.example.app:master
   - Display name: My App Production
   - Set Up
2. Provide a token
   - Generate a token: [token]
   - Generate
   - Continue
3. Run analysis on your project
   - What is your project's main language?: Other (JS, TS, Go, Python, PHP, ...)
   - What is your OS?: macOS
   - Download and unzip the Scanner for macOS: Click Download
   - Execute the Scanner from your computer

   ```sh
   sonar-scanner \
     -Dsonar.projectKey='com.example.app:master' \
     -Dsonar.projectName='My App Production' \
     -Dsonar.projectVersion='0.1.0' \
     -Dsonar.sources='./' \
     -Dsonar.exclusions='./vendor/**,./tests/**' \
     -Dsonar.language='php' \
     -Dsonar.host.url='http://127.0.0.1:9000' \
     -Dsonar.sourceEncoding='UTF-8' \
     -Dsonar.dynamicAnalysis='reuseReports' \
     -Dsonar.login='[token]'
   ```

   ***Or***

   ```sh
   cat << EOF > ./sonar-project.properties
   sonar.projectKey=com.example.app:master
   sonar.projectName=My App Production
   sonar.projectVersion=0.1.0

   sonar.sources=./
   sonar.exclusions=./vendor/**,./tests/**

   sonar.language=php

   sonar.host.url=http://127.0.0.1:9000

   sonar.sourceEncoding=UTF-8
   sonar.dynamicAnalysis=reuseReports
   EOF
   ```

   ```sh
   sonar-scanner \
     -Dproject.settings=./sonar-project.properties \
     -Dsonar.login='[token]'
   ```

```sh
# Git ignore
echo '/.scannerwork' >> ~/.gitignore_global
```

## Docker

### Running

```sh
#
docker run -it --rm \
  $(echo "$DOCKER_RUN_OPTS") \
  -h sonar-scanner \
  --name sonar-scanner \
  docker.io/sonarsource/sonar-scanner-cli:4.6
```

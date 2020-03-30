# My Setup - MacOS from scratch (March 2020)

My **very personal** setup (backup/format/restore) sequence

## install Catalina

* boot from usb stick with Catalina
* disk utility, erase
* install
* set main folder equals to the other macs (if any)
* use a temporary password, different from icloud one
* filevault on, no reset from icloud, save your key

## after install
* open terminal, `passwd` and write your new password, **equal** to the icloud one (to overcome the icloud request to keep password different)
* run software update
* test your recovery key: `sudo fdesetup validaterecovery`

## basic configuration

* finder, preferences, enable all in show items on desktop
* system preferences, if trackpad, enable right click lower bottom right
* system preferences, dock, uncheck "show recent applications in dock"
* system preferences, keyboard, check "use F1, F2, as standard ..."
* system preferences, general, uncheck "Close windows when quitting an app"
* basic bash settings

```
# revert back to bash
chsh -s /bin/bash

cat <<'EOT' > ~/.bash_profile
export BASH_SILENCE_DEPRECATION_WARNING=1
export COPYFILE_DISABLE=true

export VISUAL=vim
export EDITOR="$VISUAL"

alias ll='ls -la'

#### prompt
BLACK="\[\033[0;38m\]"
RED="\[\033[0;31m\]"
RED_BOLD="\[\033[01;31m\]"
BLUE="\[\033[01;34m\]"
GREEN="\[\033[0;32m\]"

export PS1="$BLACK\u@$RED\h $GREEN\w$RED_BOLD$BLACK: "
EOT

cat <<'EOT' > ~/.vimrc
set number
EOT
```

## software to install

* install xcode
	* accept terms: `sudo xcodebuild -license`
	* install command line tools`xcode-select --install`
* install some good developer fonts: [inconsolata font](https://fonts.google.com/specimen/Inconsolata)  and [fira font](https://github.com/tonsky/FiraCode)
* install homebrew

```
brew update && brew upgrade
brew install cmake
brew install rsync
brew install jq
brew install neofetch
brew install tree
brew install grep
brew install python3
brew install openssl@1.1
brew install gettext
brew install sqlite
brew install tig
brew install htop
brew cask install iterm2
brew cask install google-chrome
brew cask install firefox

cat <<'EOT' >> ~/.bash_profile

# brew
export PATH="/usr/local/opt/grep/libexec/gnubin:$PATH"
export PATH="/usr/local/sbin:$PATH"

alias python=/usr/local/bin/python3
alias pip=/usr/local/bin/pip3

export PATH="/usr/local/opt/gettext/bin:$PATH"
export PATH="/usr/local/opt/openssl@1.1/bin:$PATH"
export PATH="/usr/local/opt/sqlite/bin:$PATH"
export PATH="/usr/local/opt/ncurses/bin:$PATH"
EOT
```

* chrome: set it as default, disable "offer to save passwords"
* chrome: plugins and enable sync
* firefox: enable sync
* install 1password
* install dropbox
* install alfred4 and advanced -> set preferences to ~Dropbox/AlfredPreferences
* install pcloud
* iterm2
	* preferences, preferences, load preferences from custom folder (/Users/pd/Dropbox/iTerm2)
	* make iterm2 default term
	* install shell integration

### java

* install jdk 7 (*old customers ... sigh*)
	* [from here](https://www.oracle.com/java/technologies/javase/javase7-archive-downloads.html#license-lightbox)

#### option 1 - install oracle jdk

* install jdk 8
	* [from here](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
* install jdk 11
	* [from here](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)

#### option 2 - install openjdk

**best but giving problems with the latest eclipse versions**

```
brew tap AdoptOpenJDK/openjdk

brew cask install adoptopenjdk8
brew cask install adoptopenjdk11
```

#### update bash profile

```
cat <<'EOT' >> ~/.bash_profile

#### java
export JAVA_7_HOME=$(/usr/libexec/java_home -v1.7)
export JAVA_8_HOME=$(/usr/libexec/java_home -v1.8)
export JAVA_11_HOME=$(/usr/libexec/java_home -v11)

alias java7='export JAVA_HOME=$JAVA_7_HOME'
alias java8='export JAVA_HOME=$JAVA_8_HOME'
alias java11='export JAVA_HOME=$JAVA_11_HOME'

# defaults to jdk 11
java11
EOT
```

### tools

```
brew install maven
```

### eclipse

* install eclipse from eclipse.org, download standard package, jee version
* add plugins
	* jboss tools (only jboss AS, Wildfly ...), (*old customers ... sigh*)
	* anyedit tools

### jet brains / intellij

* install toolbox app
* log in and set options
	* update all tools automatically
	* keep only last version
* install intellij
* install clion
* install webstorm

### visual studio code

* install visual studio code
* install "settings sync" plugin
* login with github
* select visual studio code gist
* `Shift + Option + D` to download settings

### node

```
brew install nvm
mkdir ~/.nvm
cat <<'EOT' >> ~/.bash_profile

#### nvm
export NVM_DIR="$HOME/.nvm"
[ -s "/usr/local/opt/nvm/nvm.sh" ] && . "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
[ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && . "/usr/local/opt/nvm/etc/bash_completion.d/nvm"  # 
EOT

nvm install v8.17.0
nvm install --lts
nvm use --lts
nvm alias default 'lts/*'
```

#### vue cli

```
npm install -g @vue/cli
```

#### local ssl proxy

```
npm install -g local-ssl-proxy
```

copy pem key and bundle to `~/.nvm/versions/node/<node version>/lib/node_modules/local-ssl-proxy/resources`

#### other command line tools

```
npm install -g is-up-cli
npm install -g pageres-cli
npm install -g surge
npm install -g netlify-cli
npm install -g loadtest
npm install -g npm-home
npm install -g caniuse-cmd
npm install -g imgur-uploader-cli
npm install -g svgo
brew install httpie
brew install icdiff
brew install pandoc
brew install ranger
brew install ttygif
brew install youtube-dl
brew tap heroku/brew && brew install heroku

mkdir -P ~/.config/ranger
cat <<'EOT' > ~/.config/ranger/rc.conf
set show_hidden true
EOT
```

#### taskwarrior

```
brew install task tasksh
task version
```

```
[ -d ~/Dropbox/taskwarrior ] || mkdir ~/Dropbox/taskwarrior
sed -i '' 's/^data\.location=.*$/data.location=~\/Dropbox\/taskwarrior/g' ~/.taskrc
rm -r ~/.task
```

### ssh keys

* restore all your ssh keys into `.ssh`

### gpg

```
brew install gnupg
```

and restore all the keys under `~/.gnupg`

### git

```
brew install git-crypt
```

restore `~/.gitconfig` if available.

### gcloud

* download [gcloud sdk](https://cloud.google.com/sdk/docs/quickstart-macos)
* unzip under /Application
* /Applications/google-cloud-sdk/install.sh
* `gcloud init`

## other sw to install

### brew install

```
brew cask install postman
brew cask install gimp
brew cask install macdown
brew cask install zoomus
brew cask install telegram
brew cask install viscosity
brew cask install firefox
brew cask install cryptomator
brew cask install textmate
brew cask install balenaetcher
brew cask install docker
brew cask install authy
```

### app store install

* amphetamine
* magnet

### manual install

* carbon copy cloner
* parallels
* *email clients* (I change my mind every day ...)
* MS office suite
* little snitch

## final restore (valid only for my pd account)

set restore source from another mac

```
RESTORE_FROM="ssh <user>@<another mac ip>:~"
```

or a path

```
RESTORE_FROM="/Volumes/MyBackupHD"
```

and restore

```
cd ~
rsync -avhe ${RESTORE_FROM}/bin .
rsync -avhe ${RESTORE_FROM}/Develop .
rsync -avhe ${RESTORE_FROM}/Parallels .
[ -d ~/.m2 ] || mkdir ~/.m2
rsync -avhe ${RESTORE_FROM}/.m2/settings.xml .m2/.

cat <<'EOT' >> ~/.bash_profile

# my path
export PATH="~/bin:$PATH"
EOT
```

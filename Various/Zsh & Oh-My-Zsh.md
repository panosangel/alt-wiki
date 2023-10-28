# Zsh & Oh-My-Zsh!

## Install Zsh

**Linux:**
```bash
sudo apt-get install zsh
```
**macOS:**
```bash
brew install zsh zsh-completions
```

## Make Zsh the default shell

**Linux:**
```bash
chsh -s $(which zsh)
```
**macOS:**
```bash
chsh -s /bin/zsh
```

## Install Oh-My-Zsh!

```bash

sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
or
```bash

sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

## Install Extras

```bash
sudo apt-get install powerline fonts-powerline
```
```shell
https://github.com/lxbrtsch/Menlo-for-Powerline
```

## Basic Configuration

Edit config file:
```bash
nano ~/.zshrc
```
Set a new theme:
```bash
ZSH_THEME="agnoster"
```
Save and EXIT.

## Manual Update

```bash
cd ~/.oh-my-zsh && ./upgrade_oh_my_zsh
```

## Plugins

All plugins listed on the [plugins Github page](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins) are pre-installed with Oh-My-Zsh at `~/.oh-my-zsh/plugins`. Custom plugins can be installed at `~/.oh-my-zsh/custom/plugins`.

### Manage installed plugins

Add/edit on `~/.zshrc` the plugins names from the following list to activate/deactivate them.
```
plugins=(
  git
  npm
)
```

### Show only the username in the prompt

Add/edit on `~/.zshrc`
```bash
prompt_context() {
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment black default "%(!.%{%F{yellow}%}.)$USER"
  fi
}
```

### Install and enable zsh-autosuggestions

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
Add to `~/.zshrc`
```
plugins=(
  [...]
  zsh-autosuggestions
  [...]
)
```

### Install and enable  zsh-syntax-highlighting

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
Add to `~/.zshrc`
```
plugins=(
  [...]
  zsh-syntax-highlighting
)
```

## Revert to default Shell

```bash
chsh -s /bin/bash
```

## Sources

- [Oh My Zsh](https://ohmyz.sh/)
- [zsh-users](https://github.com/zsh-users)
- [agnoster.zsh-theme](https://github.com/agnoster/agnoster-zsh-theme)
- [ZSH: Hide computer name in terminal](https://stackoverflow.com/questions/31848957/zsh-hide-computer-name-in-terminal)
- [Oh My Zsh “agnoster” theme not showing correct font on VSCode ? (ubuntu)](https://cloverinks.medium.com/oh-my-zsh-agnoster-theme-not-showing-correct-font-on-vscode-ubuntu-47b5e8dcbada)

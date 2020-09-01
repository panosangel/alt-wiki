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
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
or
```bash
sh -c "$(wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## Install Extras
```bash
sudo apt-get install powerline fonts-powerline
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
All plugins listed on the [plugins Github page](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins) are pre-installed with Oh-My-Zsh at `~/.oh-my-zsh/plugins`. Custom plugins can be installed at `~/.oh-my-zsh/custom/plugins`.

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
cd ~/.oh-my-zsh/custom/plugins && git clone https://github.com/zsh-users/zsh-autosuggestions.git
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
cd ~/.oh-my-zsh/custom/plugins && git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
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


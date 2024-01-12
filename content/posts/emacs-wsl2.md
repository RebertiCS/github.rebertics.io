+++
authors = ["RebertiCS"]
title = "Emacs no WSL2"
date = "2023-12-11"
description = "Guia para Emacs no WSL2"
+++

# Emacs+Spacemacs(Ubuntu 22.04 no WSL2)
  Essas configurações tem as soluções de tudo o que deu errado
enquanto eu configurei o emacs no wsl2, deve funcionar com qualquer
distribuição, e.g, Doom Emacs e Spacemacs, sendo assim a minha
configuração pessoal(init.el) pode ser ignorada mas vale ressaltar
que as opções de fullscreen são fundamentais no caso Spacemacs.

---

## ZSH + OhMyZsh
``` bash
sudo apt update
sudo apt upgrade
sudo apt install zsh git
chsh -s $(which zsh)
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1
ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
```

### Tema
Local do arquivo `~/.zshrc`

``` bash
ZSH_THEME="spaceship"
```

# Git
Referência:
  - [GPG](https://docs.github.com/pt/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
  - [SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
---
### Usuario
Sempre usar o mesmo email do Github no gpg
``` bash
git config --global user.name "USUARIO"
git config --global user.email "EMAIL"
git config --global commit.gpgsign true
git config --global user.signingkey "CHAVE"
```

### GPG
Correção do IOCTL
``` bash
echo 'GPG_TTY=$(tty)' >> ~/.zshrc
echo 'export GPG_TTY' >> ~/.zshrc

gpgconf --kill gpg-agent
echo "test" | gpg --clearsign
```

### SSH
Correção de permissões
``` bash
chmod 700 .ssh
chmod 600 ~/.ssh/id_rsa
chmod 600 ~/.ssh/id_rsa.pub
```

# WSL2
---

### Fontes
Corrige problemas com fontes no GUI do Emacs.
``` bash
sudo apt update
sudo apt upgrade
sudo apt install fontconfig fonts-hack-ttf

ln -s /mnt/c/Windows/Fonts ~/.fonts
fc-cache -fv
```

### Teclado ABNT2
Sempre iniciar o terminal do Ubuntu antes do Emacs,
essa é a única solução que funciona no momento
``` bash
sudo dpkg-reconfigure locales #Descer a lista e ativar pt_BR.UTF8 e pt_BR.ISO
sudo update-locale LANG=pt_BR.UTF8

echo "setxkbmap -model abnt2 -layout br -variant abnt2" >> ~/.profile
echo "setxkbmap -model abnt2 -layout br -variant abnt2" >> ~/.zshrc
```

## Emacs
``` bash
sudo apt update
sudo apt upgrade

sudo apt install git emacs sqlite3 libsqlite3-dev
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d

# Minha configuração do spacemacs
git clone https://github.com/RebertiCS/spacemacs-dotfiles.git ~/.spacemacs.d
```

### Python(PyEnv + LSP)
``` bash
sudo apt update
sudo apt upgrade
sudo apt install python3-pip python3-tk liblzma-dev

git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc

pyenv install 3.10
pyenv global 3.10

pip install -U pip
pip install flake8 isort yapf importmagic autoflake 'python-lsp-server[all]'
```

dotfiles
========

[sloria's dotfiles](https://github.com/sloria/dotfiles-old), rewritten as Ansible roles. Sets up a full local development environment with a **single command.**

Fully supports macOS

a few neat features
-------------------

- zsh configured with [prezto](https://github.com/sorin-ionescu/prezto).
- nice fonts for the terminal and coding.
- python2, python3, pipx (for managing python CLIs), pyenv (for managing Python versions), and pyenv-virtualenv (for managing virtualenvs)
- a tmux.conf that's pretty neat.
- vim with [vim-plug](https://github.com/junegunn/vim-plug) for plugin management. All configuration in a single file [.vimrc](https://github.com/sloria/dotfiles/blob/master/roles/vim/files/vimrc).
- pluggable. Everything is optional. Fork this. Remove what you don't use. Configure what you do use.
- Mac packages installed with [homebrew][]. Mac apps installed with [homebrew-cask][] and [mas][].
- Useful git aliases
- Optional git commit signing with GPG

prerequisites
-------------

- homebrew (If on macOS) - **Install this first**
- git: `brew install git`
- ansible >= 1.6: `brew install ansible`

install
-------

- [Fork](https://github.com/sloria/dotfiles/fork) this repo.
- Clone your fork.

```bash
# Replace git url with your fork
# NOTE: It is important that you clone to ~/dotfiles
git clone https://github.com/YOU/dotfiles.git ~/dotfiles
cd ~/dotfiles
```

- Update the following variables in `group_vars/local` (at a minimum)
    - `full_name`: Your name, which will be attached to commit messages, e.g. "Steven Loria"
    - `git_user`: Your Github username.
    - `git_email`: Your git email address.
- Optional, but recommended: Update `group_vars/local` with the programs you want installed by [homebrew][], [homebrew-cask][], and npm.
    - `mac_homebrew_packages`:  Utilities that don't get installed by the roles.
    - `mac_cask_packages`: Mac Apps you want installed with [homebrew-cask][].
- Edit `local_env.yml` as you see fit. Remove any roles you don't use. Edit roles that you do use.
- Optional: If you want to sign Git commits with a GPG key, follow the
    instructions [here](https://github.com/pstadler/keybase-gpg-github)
    to generate a GPG key from Keybase. Then set the
    `GIT_SIGNING_KEY_ID` environment variable before running the
    `dot-bootstrap` script.

```
export GIT_SIGNING_KEY_ID=631262B829DDB506
```

Note: After running the dot-bootstrap script, you should put the above
line in `~/.localrc`.

- Run the installation script.

```bash
./bin/dot-bootstrap
```

updating your local environment
-------------------------------

Once you have the dotfiles installed you can run the following command to rerun the ansible playbook:

```bash
dot-update
```

You can optionally pass role names

```bash
dot-update git python
```

updating your dotfiles repo
---------------------------

To keep your fork up to date with the `sloria` fork:

```
git remote add sloria https://github.com/sloria/dotfiles.git
git pull sloria master
```

commands
--------

There are three main commands in the `bin` directory for setting up and updating development environments:

- `dot-bootstrap`: sets up local environment by executing all roles in `local_env.yml`.
- `dot-update`: updates local environment by executing all roles in `local_env.yml` except for the ones tagged with "bootstrap".

special files
-------------

All configuration is done in `~/dotfiles`. Each role may contain (in addition to the typical ansible directories and files) a number of special files

- **role/\*.zsh**: Any files ending in `.zsh` get loaded into your environment.
- **bin/**: Anything in `bin/` will get added to your `$PATH` and be made available everywhere.

notes
-----

**vscode**

I use the [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) extension to sync my VSCode setup.
My settings are uploaded here: https://gist.github.com/sloria/1256526bc7e1a52d3c14954d1d26740b

**macOS keyboard settings**

There are a few keyboard customizations that must be done manually:

- Turning repeat speed up to 11.

![Keyboard settings](https://user-images.githubusercontent.com/2379650/34223505-91f95072-e58d-11e7-9b36-78aec4203b0d.png "Key repeat settings")


- Mapping Caps Lock to Ctrl.

![Modifier keys](https://user-images.githubusercontent.com/2379650/34223523-a2c8e4e4-e58d-11e7-9532-d74b95d8408a.png)

**login message**

You can add a message to the login screen using the following command:

```
sudo defaults write /Library/Preferences/com.apple.loginwindow LoginwindowText "This laptop is connected to an iCloud account and is valueless if lost. Contact (123) 456-7890 if found. Reward included."
```

what if I only want your vim?
-----------------------------

First make sure you have a sane vim compiled. On macOS, the following will do:

```
brew install macvim
```

The following commands will install vim-plug and download my `.vimrc`.

After backing up your `~/.vim` directory and `~/.vimrc`:

```
mkdir -p ~/.vim/autoload
curl -fLo ~/.vim/autoload/plug.vim https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
curl -fLo ~/.vimrc https://raw.githubusercontent.com/sloria/dotfiles/master/roles/vim/files/vimrc
```

You will now be able to open vim and run `:PlugInstall` to install all plugins.


troubleshooting
---------------

If you get an error about Xcode command-line tools, you may need to run

```
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

If pyenv versions fail to install, try the steps here:

https://github.com/pyenv/pyenv/issues/1219#issuecomment-459333831

If python environments break after upgrading brew-installed python, rehash pyenv and re-install pipx environments

```
pyenv rehash


mv ~/.local/bin ~/.local/bin.bak
mv ~/.local/pipx ~/.local/pipx.bak

dot-update pipx
```


license
-------

[MIT Licensed](http://sloria.mit-license.org/).

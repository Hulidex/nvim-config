# nvim-config

## Introduction

I forked the repository [nvim-lua/kickstart](https://github.com/nvim-lua/kickstart.nvim) for having an started point for my customized neovim config.
So here you will see an attempt of configuring nvim for my personal needs,
feel free to have a look at it

> [!IMPORTANT]
> The nvim tool was configured and tested for debian distributions. It remains uncertain whether the commands described herein will function as intended on Windows-based systems.

## Features

- Copilot

## TODO

- [ ] Install Github Copilot extension
- [ ] YAML linting
- [ ] Split config file to several files easier to manage
- [ ] Spell check
- [ ] Markdown preview and capabilities

## Installation

### Install Neovim

Kickstart.nvim targets *only* the latest
['stable'](https://github.com/neovim/neovim/releases/tag/stable) and latest
['nightly'](https://github.com/neovim/neovim/releases/tag/nightly) of Neovim.
If you are experiencing issues, please make sure you have the latest versions.

### Install External Dependencies

External Requirements:

#### Basic utils

- Basic utils: `git`, `make`, `unzip`, C Compiler (`gcc`)

#### ripgrep

You can check at the official documentation [ripgrep](https://github.com/BurntSushi/ripgrep#installation)

For debian based distributions:

```sh
sudo apt install ripgrep
```

#### NerdFont

You'll need to install a [Nerd Font](https://github.com/ryanoasis/nerd-fonts),
which offers a wide selection for personalization. To install a Nerd Font of
your choice, you may adhere to the following steps. In this instance, I have
opted for the Hack Nerd Font as an example:


```sh
git clone --filter=blob:none --sparse git@github.com:ryanoasis/nerd-fonts ~/.config/nerd-fonts
cd ~/.config/nerd-fonts
git sparse-checkout add patched-fonts/Hack
```

Finally install the font:

```sh
cd  ~/.config/nerd-fonts
./install.sh Hack
```

> [!NOTE]
> In the first command, we downloaded the repository via a ```git sparse clone```.
> This method entails cloning only the repository's metadata .
> Then with the third command we specifically indicated git
> to checkout completely the folder ```patched-fonts/Hack```.
> You might wonder why such an approach was taken. The rationale is
> straightforward: the repository's extensive size, due to its inclusion of
> numerous fonts and files, becomes impractical when our intention is to
> install merely a select few fonts.

### Copilot

Official copilot getting started guide can be found [here](https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot?tool=vimneovim#prerequisites-3)

- We require an active Github Copilot subscription
- And Node.js version 18 or higher

#### Install node for debian based distributions

Official node installation [here](https://nodejs.org/en/download/package-manager)

```bash
# installs NVM (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# download and install Node.js
nvm install 20

# verifies the right Node.js version is in the environment
node -v # should print `v20.12.1`

# verifies the right NPM version is in the environment
npm -v # should print `10.5.0`
```

#### Laguage setup

- If want to write Typescript, you need `npm`
- If want to write Golang, you will need `go`
- etc.

> **NOTE**
> See [Install Recipes](#Install-Recipes) for additional Windows and Linux specific notes
> and quick install snippets

### Install Kickstart

> **NOTE**
> [Backup](#FAQ) your previous configuration (if any exists)

Neovim's configurations are located under the following paths, depending on your OS:

| OS | PATH |
| :- | :--- |
| Linux, MacOS | `$XDG_CONFIG_HOME/nvim`, `~/.config/nvim` |
| Windows (cmd)| `%userprofile%\AppData\Local\nvim\` |
| Windows (powershell)| `$env:USERPROFILE\AppData\Local\nvim\` |

#### Configure neovim


<details><summary> Linux </summary>

For those indifferent to the repository's storage location, direct cloning
into the designated configuration space is feasible:

```sh
git clone https://github.com/Hulidex/nvim-config ~/.config/nvim
```

If you rather prefer to allocate the repository to an alternative directory, you should
create a symbolic link from the repository to the nvim configuration directory,
thus ensuring any modifications within the repository are automatically reflected

```sh
git clone https://github.com/Hulidex/nvim-config <your_path>/nvim
ln -s <your_path>/nvim ~/.config
```

</details>


### Post Installation

Start Neovim

```sh
nvim
```

That's it! Lazy will install all the plugins you have. Use `:Lazy` to view
current plugin status. Hit `q` to close the window.

Read through the `init.lua` file in your configuration folder for more
information about extending and exploring Neovim.


#### Examples of adding popularly requested plugins

NOTE: You'll need to uncomment the line in the init.lua that turns on loading custom plugins.

<details>
  <summary>Adding autopairs</summary>

This will automatically install [windwp/nvim-autopairs](https://github.com/windwp/nvim-autopairs)
and enable it on startup. For more information, see documentation for
[lazy.nvim](https://github.com/folke/lazy.nvim).

In the file: `lua/custom/plugins/autopairs.lua`, add:

```lua
-- File: lua/custom/plugins/autopairs.lua

return {
  "windwp/nvim-autopairs",
  -- Optional dependency
  dependencies = { 'hrsh7th/nvim-cmp' },
  config = function()
    require("nvim-autopairs").setup {}
    -- If you want to automatically add `(` after selecting a function or method
    local cmp_autopairs = require('nvim-autopairs.completion.cmp')
    local cmp = require('cmp')
    cmp.event:on(
      'confirm_done',
      cmp_autopairs.on_confirm_done()
    )
  end,
}
```

</details>
<details>
  <summary>Adding a file tree plugin</summary>

This will install the tree plugin and add the command `:Neotree` for you.
For more information, see the documentation at
[neo-tree.nvim](https://github.com/nvim-neo-tree/neo-tree.nvim).

In the file: `lua/custom/plugins/filetree.lua`, add:

```lua
-- File: lua/custom/plugins/filetree.lua

return {
  "nvim-neo-tree/neo-tree.nvim",
  version = "*",
  dependencies = {
    "nvim-lua/plenary.nvim",
    "nvim-tree/nvim-web-devicons", -- not strictly required, but recommended
    "MunifTanjim/nui.nvim",
  },
  config = function ()
    require('neo-tree').setup {}
  end,
}
```

</details>

### Getting Started

[The Only Video You Need to Get Started with Neovim](https://youtu.be/m8C0Cq9Uv9o)

### FAQ

* What if I want to "uninstall" this configuration:
  * See [lazy.nvim uninstall](https://github.com/folke/lazy.nvim#-uninstalling) information
* Why is the kickstart `init.lua` a single file? Wouldn't it make sense to split it into multiple files?
  * The main purpose of kickstart is to serve as a teaching tool and a reference
    configuration that someone can easily use to `git clone` as a basis for their own.
    As you progress in learning Neovim and Lua, you might consider splitting `init.lua`
    into smaller parts. A fork of kickstart that does this while maintaining the 
    same functionality is available here:
    * [kickstart-modular.nvim](https://github.com/dam9000/kickstart-modular.nvim)
  * Discussions on this topic can be found here:
    * [Restructure the configuration](https://github.com/nvim-lua/kickstart.nvim/issues/218)
    * [Reorganize init.lua into a multi-file setup](https://github.com/nvim-lua/kickstart.nvim/pull/473)

### Install Recipes

Below you can find OS specific install instructions for Neovim and dependencies.

After installing all the dependencies continue with the [Install Kickstart](#Install-Kickstart) step.

#### Windows Installation

<details><summary>Windows with Microsoft C++ Build Tools and CMake</summary>
Installation may require installing build tools and updating the run command for `telescope-fzf-native`

See `telescope-fzf-native` documentation for [more details](https://github.com/nvim-telescope/telescope-fzf-native.nvim#installation)

This requires:

- Install CMake and the Microsoft C++ Build Tools on Windows

```lua
{'nvim-telescope/telescope-fzf-native.nvim', build = 'cmake -S. -Bbuild -DCMAKE_BUILD_TYPE=Release && cmake --build build --config Release && cmake --install build --prefix build' }
```
</details>
<details><summary>Windows with gcc/make using chocolatey</summary>
Alternatively, one can install gcc and make which don't require changing the config,
the easiest way is to use choco:

1. install [chocolatey](https://chocolatey.org/install)
either follow the instructions on the page or use winget,
run in cmd as **admin**:
```
winget install --accept-source-agreements chocolatey.chocolatey
```

2. install all requirements using choco, exit previous cmd and
open a new one so that choco path is set, and run in cmd as **admin**:
```
choco install -y neovim git ripgrep wget fd unzip gzip mingw make
```
</details>
<details><summary>WSL (Windows Subsystem for Linux)</summary>

```
wsl --install
wsl
sudo add-apt-repository ppa:neovim-ppa/unstable -y
sudo apt update
sudo apt install make gcc ripgrep unzip neovim
```
</details>

#### Linux Install
<details><summary>Ubuntu Install Steps</summary>

```
sudo add-apt-repository ppa:neovim-ppa/unstable -y
sudo apt update
sudo apt install make gcc ripgrep unzip neovim
```
</details>
<details><summary>Debian Install Steps</summary>

```
sudo apt update
sudo apt install make gcc ripgrep unzip git
echo "deb https://deb.debian.org/debian unstable main" | sudo tee -a /etc/apt/sources.list
sudo apt update
sudo apt install -t unstable neovim
```
</details>
<details><summary>Fedora Install Steps</summary>

```
sudo dnf install -y gcc make git ripgrep fd-find neovim
```
</details>

## Additional notes

- Vim tutorial: run ```:Tutor```

### Hot shortcuts 

- Space s h  -> Look for help, similar to ```:help``` but more advanced


NvChad Goland

Clean existing installation

Linux / MacOS (unix) 

```
$ rm -rf ~/.config/nvim 
$ rm -rf ~/.local/share/nvim
```

## Install Nerd fonts
Download and install **JetBrainsMono.zip** from  https://github.com/ryanoasis/nerd-fonts/releases/ 

Set **JetBrainsMono NF** as the font for non-ASCII text, also select the options for Use ligatures and Anti-aliased

￼![JetBrainsMono NF](https://github.com/m0nadicph0/notes/assets/123083726/dda09362-f186-4c39-a1c6-b39207e129ea)


## Install NvChad

```
$ git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1 && nvim
```

Select **No** for `Do you want to install example custom config?`

## Install `gopls`  
Create a file ~/.config/nvim/lua/custom/plugins.lua wit the following content

```lua
local plugins = {
  {
    "williamboman/mason.nvim",
    opts = {
      ensure_installed = {
        "gopls",
      },
    },
  },
}
return plugins
```

edit the file  ~/.config/nvim/lua/custom/chadrc.lua

```lua
---@type ChadrcConfig
local M = {}

M.ui = { theme = 'catppuccin' }

M.plugins = "custom.plugins"

return M
```


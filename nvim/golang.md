# NvChad Goland

## Clean existing installation

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
Create a file ~/.config/nvim/lua/custom/plugins.lua with the following content

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

Save and close the files, restart nvim and run the **MasonInstallAll** command from the colon prompt.

Add the following section in the `~/.config/nvim/lua/custom/plugins.lua` file

```
local plugins = {
  ...
  {
    "neovim/nvim-lspconfig",
    config = function ()
      require "plugins.configs.lspconfig"
      require "custom.configs.lspconfig"
    end,
  },
}
return plugins
```
Create a file `~/.config/nvim/lua/custom/configs/lspconfig.lua`

```
local on_attach = require("plugins.configs.lspconfig").on_attach
local capabilities = require("plugins.configs.lspconfig").capabilities

local lspconfig = require("lspconfig")
local util = require "lspconfig/util"

lspconfig.gopls.setup {
  on_attach = on_attach,
  capabilities = capabilities,
  cmd = {"gopls"},
  filetypes = { "go", "gomod", "gowork", "gotmpl" },
  root_dir = util.root_pattern("go.work", "go.mod", ".git"),
  settings = {
    gopls = {
      completeUnimported = true,
      usePlaceholders = true,
      analyses = {
        unusedparams = true,
      },
    },
  },
}

```
add the following code snippet to `~/.config/nvim/lua/core/init.lua`

```lua
autocmd("BufWritePre", {
  pattern = "*.go",
  callback = function()
    local params = vim.lsp.util.make_range_params()
    params.context = {only = {"source.organizeImports"}}
    -- buf_request_sync defaults to a 1000ms timeout. Depending on your
    -- machine and codebase, you may want longer. Add an additional
    -- argument after params if you find that you have to write the file
    -- twice for changes to be saved.
    -- E.g., vim.lsp.buf_request_sync(0, "textDocument/codeAction", params, 3000)
    local result = vim.lsp.buf_request_sync(0, "textDocument/codeAction", params)
    for cid, res in pairs(result or {}) do
      for _, r in pairs(res.result or {}) do
        if r.edit then
          local enc = (vim.lsp.get_client_by_id(cid) or {}).offset_encoding or "utf-16"
          vim.lsp.util.apply_workspace_edit(r.edit, enc)
        end
      end
    end
    vim.lsp.buf.format({async = false})
  end
})
```

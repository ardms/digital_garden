---
title: "Neovim setup"
date: 2022-08-17T15:47:16+01:00
lastmod: 2022-08-23-T17:55:51se 
tags: ["vim", "Notes"]
---

Notes to help with seting up Neovim with the use of lua config files.

## For installation under WSL2
Under WSL2 you have to do the following:

### Install pyright 

[pyright](https://github.com/Microsoft/pyright) is a type checker for python managed by Microsoft. 
Probbaly the best way to install it is under a virtual env

### Install nodejs

The default nodejs package under Ubuntu gave me an error so I had to manualy upgrade that. 
use
`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`
to add the latest repositories to Ubuntu then 
`sudo apt-get install -y nodejs`

### Install latest nvim instance

If going for the appimage option then first install [FUSE](https://docs.appimage.org/user-guide/troubleshooting/fuse.html)

`sudo apt-get install fuse libfuse2`

Then based on the information [here](https://github.com/neovim/neovim/wiki/Installing-Neovim)
do the following:

```
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim.appimage
chmod u+x nvim.appimage
./nvim.appimage
```

## Configuration with lua

### Packer
First start with plugin manager. For now I use [Packer](https://github.com/wbthomason/packer.nvim).
Main instruction are in the github repository.  

### Language Server Protocol (LSP)

Nvim supports the Language Server Protocol (LSP), which means it acts as
a client to LSP servers and includes a Lua framework `vim.lsp` for building
enhanced LSP tools.

It is considered a good practice to install [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig).
This plugin is only a collection of LSP configurations. 

We also need to install a language server for the language we need:
* For **Python**. Using [pyright](https://github.com/microsoft/pyright). Just create a venv and `pip install pyright`
Before lunching nvim we will need to activate the environment. 

### Auto-completion

For now I use [nvim-cmp](https://github.com/hrsh7th/nvim-cmp)

This was a bit tricky to set up. 

```
local capabilities = require('cmp_nvim_lsp').update_capabilities(vim.lsp.protocol.make_client_capabilities())
```

We then need the `capabilities` variable inside our call in **LSP**
```
require'lspconfig'.pyright.setup{
    capabilities = capabilities,
}
```

Finally we need to set up cmp.

```
vim.opt.completeopt={"menu","menuone","noselect"}

local has_words_before = function()
  local line, col = unpack(vim.api.nvim_win_get_cursor(0))
  return col ~= 0 and vim.api.nvim_buf_get_lines(0, line - 1, line, true)[1]:sub(col, col):match("%s") == nil
end

local luasnip = require("luasnip")
local cmp = require("cmp")

cmp.setup({
 snippet = {
      -- REQUIRED - you must specify a snippet engine
      expand = function(args)
        -- vim.fn["vsnip#anonymous"](args.body) -- For `vsnip` users.
        require('luasnip').lsp_expand(args.body) -- For `luasnip` users.
        -- require('snippy').expand_snippet(args.body) -- For `snippy` users.
        -- vim.fn["UltiSnips#Anon"](args.body) -- For `ultisnips` users.
      end,
    },
  mapping = {

    -- ... Your other mappings ...

    ["<Tab>"] = cmp.mapping(function(fallback)
      if cmp.visible() then
        cmp.select_next_item()
      elseif luasnip.expand_or_jumpable() then
        luasnip.expand_or_jump()
      elseif has_words_before() then
        cmp.complete()
      else
        fallback()
      end
    end, { "i", "s" }),

    ["<S-Tab>"] = cmp.mapping(function(fallback)
      if cmp.visible() then
        cmp.select_prev_item()
      elseif luasnip.jumpable(-1) then
        luasnip.jump(-1)
      else
        fallback()
      end
    end, { "i", "s" }),

    -- ... Your other mappings ...
  },
  sources = {
      {name = "nvim_lua"},
      {name = "nvim_lsp"},
      {name = "luasnip"},
      {name = "buffer"},
  },
  experimental = {
      native_menu = false

  },
  formatting = {
    format = function(entry,vim_item)
        vim_item.menu = ({
            buffer = "[buf]",
            nvim_lsp = "[LSP]",
            nvim_lua = "[api]",
            path = "[path]",
            luasnip = "[snip]",
            gh_issues = "[issues]",
            tn = "[TabNine]",
      })[entry.source.name]

      return vim_item
    end
    },
 -- ... Your other configuration ...
})
```


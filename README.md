# Neovim Config

My custom Neovim config, along with general instructions for porting it to remote SSH servers.

## Functionalities:
- LSP support
- Treesitter highlighting
- Packer
- Custom remaps for netrw and harpoon

## Steps to configure:
1) Clone this repository to dirA.
2) Add the following script to `neovim_config/nvim` (the script will be able to find `init.lua`, and source all of the plugin files as a result):

```bash
#!/bin/bash

# change SCRIPT_DIR to where this script is located
SCRIPT_DIR="dirA/neovim_config/nvim/"
export XDG_CONFIG_HOME=$SCRIPT_DIR
nvim "$@"
```

3) Run the following command (or just simply add the alias to the file):
```bash
echo `alias [nvim_command_name]="dirA/neovim_config/nvim/[nvim_script.sh]"` >> ~/.bashrc
```

5) Add the following function to packer.lua ([https://stackoverflow.com/questions/76645160/nvim-not-sourcing-packer](https://stackoverflow.com/questions/76645160/nvim-not-sourcing-packer)):
```lua
local ensure_packer = function()
    local fn = vim.fn
    local install_path = fn.stdpath('data')..'/site/pack/packer/start/packer.nvim'
    if fn.empty(fn.glob(install_path)) > 0 then
        fn.system({'git', 'clone', '--depth', '1', 'https://github.com/wbthomason/packer.nvim', install_path})
        vim.cmd [[packadd packer.nvim]]
        return true
    end
    return false
end

local packer_bootstrap = ensure_packer()
```

5) Run `:so` in packer, restart nvim, and check that `:PackerSync` runs.
6) Check that custom remaps, tree-sitter, harpoon, and packer are indeed integrated.

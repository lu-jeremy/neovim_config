# Neovim Config

My custom Neovim config, along with general instructions for porting it to remote SSH servers.

## Functionalities:
- LSP support
- Treesitter highlighting
- Packer
- Custom remaps for netrw and harpoon

## Steps to configure:
1) Clone this repository to dir A.
2) Add the following script to dir B:

```bash
#!/bin/bash

# change SCRIPT_DIR to dir A
SCRIPT_DIR="$HOME/../../media/corallab-s1/2tbhdd/Jeremy/neovim_config/\[nvim_script.sh\]"
export XDG_CONFIG_HOME=$SCRIPT_DIR
nvim "$@"
```

3) Run `echo ``alias [nvim_command_name]="dir/[nvim_script.sh]"`` >> ~/.bashrc`, or just simply add the alias to the file.
4) Add the following function to packer.lua:
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

5) Run `:so` in packer, restart nvim, and make sure `:PackerSync` works.
6) Make sure that custom remaps work. 

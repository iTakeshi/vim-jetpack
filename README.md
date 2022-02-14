# 🚀 JETPACK.vim

The **lightning-fast** minimalist plugin manager for Vim/ Neovim.

## Benchmark

In the simple cases, JETPACK.vim is the fastest plugin manager.

|  name   | time (ms) |
| :-----: | --------: |
| jetpack |        77 |
|  dein   |        97 |
|  plug   |       155 |
| packer  |       190 |

You can run the benchmarks in your local environment.
See the `benchmark` directory for more detail

## Installation

Download pack.vim and put it in the `autoload` directory.

### Vim

```
curl -fLo ~/.vim/autoload/pack.vim --create-dirs https://raw.githubusercontent.com/tani/jetpack/master/autoload/pack.vim
```

### Neovim

```
curl -fLo ~/.config/nvim/autoload/pack.vim --create-dirs https://raw.githubusercontent.com/tani/jetpack/master/autoload/pack.vim
```

## Supported options

JETPACK.vim is 90% compatible with vim-plug.

|      name       |        type        | description                           |
| :-------------: | :----------------: | :------------------------------------ |
| `branch`/ `tag` |      `sring`       | Branch/ tag of the repository to use  |
|      `rtp`      |      `string`      | Subdirectory that contains Vim plugin |
|      `dir`      |      `string`      | Custom directory for the plugin       |
|      `as`       |      `string`      | Use different name for plugin         |
|      `do`       | `string` or `func` | Post-updat hook                       |
|      `on`       | `string` or `list` | On-demand loading: Commands           |
|      `for`      | `string` or `list` | On-demand loading: File types         |
|      `opt`      |     `boolean`      | On-demand loading: `packadd {name}`   |
|    `frozen`     |     `boolean`      | Do not update                         |

We welcome to merge a new option if you create a pull request to add the `commit` option.

## Configuration

- `g:pack#optimization` -- The optimization level for the bundle algorithm.

  |  speed  |    0    |   1    |    2    |
  | :-----: | :-----: | :----: | :-----: |
  | install | fastest |  slow  | faster  |
  | startup |  slow   | faster | fastest |
  - `0` -- Bundle nothing. This is the same as vim-plug and is the safest level.
  - `1` -- Bundle if there are no conflicts. It tries to bundle plgins as
    possible. This is default and is safer than `3`.
  - `2` -- Bundle everything. This may be the same as dein.vim, and is the
    fastest level. It overwrites some duplicated files.

## Commands

- `:PackSync` -- Synchronize configuration and state. It performs to install,
  update, and bundle.

## Example Usage

### vim-plug style

The most of vim-plug users can migrate to JETPACK.vim by `:%s/plug/pack/g` and `:%s/Plug/Pack/g`.

```vim
call pack#begin()
Pack 'junegunn/fzf.vim'
Pack 'junegunn/fzf', { 'do': {-> fzf#install()} }
Pack 'neoclide/coc.nvim', { 'branch': 'release' }
Pack 'neoclide/coc.nvim', { 'branch': 'master', 'do': 'yarn install --frozen-lockfile' }
Pack 'vlime/vlime', { 'rtp': 'vim' }
Pack 'dracula/vim', { 'as': 'dracula' }
Pack 'tpope/vim-fireplace', { 'for': 'clojure' }
call pack#end()
```

### dein/ minpac style

```vim
call pack#begin()
call pack#add('junegunn/fzf.vim')
call pack#add('junegunn/fzf', { 'do': {-> fzf#install()} })
call pack#add('neoclide/coc.nvim', { 'branch': 'release' })
call pack#add('neoclide/coc.nvim', { 'branch': 'master', 'do': 'yarn install --frozen-lockfile' })
call pack#add('vlime/vlime', { 'rtp': 'vim' })
call pack#add('dracula/vim', { 'as': 'dracula' })
call pack#add('tpope/vim-fireplace', { 'for': 'clojure' })
call pack#end()
```

### packer style

```lua
require('pack').setup(function(use)
  use 'junegunn/fzf.vim'
  use {'junegunn/fzf', do = 'call fzf#install()' }
  use {'neoclide/coc.nvim', branch = 'release'}
  use {'neoclide/coc.nvim', branch = 'master', do = 'yarn install --frozen-lockfile'}
  use {'vlime/vlime', rtp = 'vim' }
  use {'dracula/vim', as = 'dracula' }
  use {'tpope/vim-fireplace', for = 'clojure' }
end)
```

You additionally need to download the lua extension and put it in the `lua`
directory as follows.

```
curl -fLo ~/.config/nvim/lua/pack.lua --create-dirs \
    https://raw.githubusercontent.com/tani/jetpack/master/pack.lua
```

## Q & A

### Why is this plugin so fast?

Because we bundle the all plugins as possible to reduce runtimepath, which takes
a long time at startup. This is the same algorithm of the plugin manager
dein.vim.

### Is this plugin faster than dein?

No if you are vim-wizard. Dein provides many option to tune the startup. Thus,
dein takes milli-seconds to do many things. Our plugin does as the same as
vim-plug, i.e., this plugin provides less options than dein.

## Copyright and License

Copyright (c) 2022 TANIGUCHI Masaya. All rights reserved.

This software is licensed under the MIT License.

# Vim Resources

Vim is a highly configurable text editor built to make creating and changing any kind of text very efficient. It is included as "vi" with most UNIX systems and with Apple macOS.

&nbsp;


Table of Contents
--

- [Getting Started](#getting-started)
  - [Verify the installation](#verify-the-installation)
- [Install vim-plug](#install-vim-plug)
- [Install COC](#install-coc)
  - [COC Extensions](#coc-extension)
  - [Custom Language servers](#custom-language-servers)
  - [Configuration](#configuration)
  - [Key mapping](#key-mapping)
  - [More COC Extension](#more-coc-extension)
- [Example](#example)
- [Troubleshooting](#troubleshooting)
- [Switch between VIM and Terminal quickly](#switch-between-vim-and-terminal-quickly)


&nbsp;



Getting Started
--

```
sudo pacman -S vim
```

### Verify the installation
```
pacman -Si vim
```
```
Repository      : extra
Name            : vim
Version         : 8.1.1073-1
Description     : Vi Improved, a highly configurable, improved version of the vi text editor
Architecture    : x86_64
URL             : https://www.vim.org
Licenses        : custom:vim
Groups          : None
Provides        : xxd  vim-minimal  vim-python3
Depends On      : vim-runtime=8.1.1073-1  gpm  acl  glibc  libgcrypt  pcre  zlib  libffi
Optional Deps   : python2: Python 2 language support
                  python: Python 3 language support
                  ruby: Ruby language support
                  lua: Lua language support
                  perl: Perl language support
                  tcl: Tcl language support
Conflicts With  : gvim  vim-minimal  vim-python3
Replaces        : vim-python3  vim-minimal
Download Size   : 1445.23 KiB
Installed Size  : 3532.00 KiB
Packager        : Levente Polyak <anthraxx@archlinux.org>
Build Date      : Fri 29 Mar 2019 04:08:59 PM EDT
Validated By    : MD5 Sum  SHA-256 Sum  Signature
```

### Verify it by calling ```vim``` command. 
```
vim
```

&nbsp;


Install vim-plug
--

Download [vim-plug](https://github.com/junegunn/vim-plug) and move  `plug.vim`  to  `~/.vim/autoload`  directory
```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

&nbsp;


Install COC
--
You can follow the guide in the [official github page](https://github.com/neoclide/coc.nvim/wiki/Install-coc.nvim) for the latest instruction, but, to save your time, i’ll give it below. Although, it will not be guaranteed for future releases.

1.  Make sure you have nodeJS ≥ 8.0, or else download it via the command below.
	```
	sudo pacman -S nodejs
	```
2. Install yarn.
	```
	sudo pacman -S yarn
	```
3. Create directory for COC’s installation.
	```
	mkdir -p ~/.vim/pack/coc/start
	```
4. Clone COC’s repo.
	```
	cd ~/.vim/pack/coc/start  
	git clone https://github.com/neoclide/coc.nvim.git --depth=1
	```
5. Install COC.
	```
	cd ~/.vim/pack/coc/start/coc.nvim  
	yarn install
	```
6. Add Plug for COC. Open  `~/.vimrc`  in editor and add the lines below.
	```
	call plug#begin()
	
	Plug 'neoclide/coc.nvim', {'do': { -> coc#util#install()}}
	
	call plug#end()
	```
7. Open vim and execute `:PlugInstall` in the command-line mode.

&nbsp;

### COC Extension

The last steps is the installation of the language server you want to use with the code-completion.

You can see the list of available extensions  [here](https://www.npmjs.com/search?q=keywords%3Acoc.nvim). Make sure to follow the requirements of the extensions you want to install as they’re different with each other.

For this example, I will use ```Python``` language server.

1. Execute `:CocInstall coc-python` in the vim’s command-line mode.
2. Open Vim with any Python code: `vim test.py` and see if the code-completion works. 

To manage the extensions you can read the guide in the  [official page](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions).


&nbsp;


## Custom Language Servers
If your favorite language server is not available yet. You can add custom language server in your [`coc-settings.json`](#configuration) for more information click [here](https://github.com/neoclide/coc.nvim/wiki/Language-servers#register-custom-language-servers).
```
{
  "languageserver": {
    "bash": {
      "command": "bash-language-server",
      "args": ["start"],
      "filetypes": ["sh"],
      "ignoredRootPaths": ["~"]
    },
    "dockerfile": {
      "command": "docker-langserver",
      "filetypes": ["dockerfile"],
      "args": ["--stdio"]
    },
    "golang": {
      "command": "gopls",
      "rootPatterns": ["go.mod", ".vim/", ".git/", ".hg/"],
      "filetypes": ["go"]
    }
  }
}
```


&nbsp;

## Configuration 

There're two types of user [configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file).

-   The user configuration is named as  `coc-settings.json`  and placed inside folder  `$XDG_CONFIG_HOME/nvim`  or  `$HOME/.config/nvim`  by default（or  `$HOME/.vim`  for vim）. Run command  `:CocConfig`  to open your user configuration file.
    
-   The workspace configuration should be named with  `coc-settings.json`  and would be resolve in directory  `.vim`. After a file opened in vim, this directory would be resolved from parent directories of that file.
    

The active configuration would be a merged result from 'default', 'user' and 'workspace' configuration file,  **the later one have higher priority**.

To enable intellisense of  `coc-settings.json`, install json language extension  [coc-json](https://github.com/neoclide/coc-json)  by:

```
:CocInstall coc-json
```

&nbsp;

### Key mapping
`coc.nvim`  doesn't come with a default key mapping commands, you need to configure it yourself.

Here's a recommended configuration:
```vim
" ~/.vimrc
" Configuration for coc.nvim

" Smaller updatetime for CursorHold & CursorHoldI
set updatetime=300

" don't give |ins-completion-menu| messages.
set shortmess+=c

" always show signcolumns
set signcolumn=yes

" Some server have issues with backup files, see #649
set nobackup
set nowritebackup

" Better display for messages
set cmdheight=2

" Use <c-space> for trigger completion.
inoremap <silent><expr> <c-space> coc#refresh()

" Use <cr> for confirm completion, `<C-g>u` means break undo chain at current position.
" Coc only does snippet and additional edit on confirm.
inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"

" Use `[c` and `]c` for navigate diagnostics
nmap <silent> [c <Plug>(coc-diagnostic-prev)
nmap <silent> ]c <Plug>(coc-diagnostic-next)

" Remap keys for gotos
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)

" Remap for do codeAction of current line
nmap <leader>ac <Plug>(coc-codeaction)

" Remap for do action format
nnoremap <silent> F :call CocAction('format')<CR>

" Use K for show documentation in preview window
nnoremap <silent> K :call <SID>show_documentation()<CR>

function! s:show_documentation()
  if &filetype == 'vim'
    execute 'h '.expand('<cword>')
  else
    call CocAction('doHover')
  endif
endfunction

" Highlight symbol under cursor on CursorHold
autocmd CursorHold * silent call CocActionAsync('highlight')

" Remap for rename current word
nmap <leader>rn <Plug>(coc-rename)

" Show all diagnostics
nnoremap <silent> <space>a  :<C-u>CocList diagnostics<cr>
" Find symbol of current document
nnoremap <silent> <space>o  :<C-u>CocList outline<cr>
" Search workspace symbols
nnoremap <silent> <space>s  :<C-u>CocList -I symbols<cr>
" Do default action for next item.
nnoremap <silent> <space>j  :<C-u>CocNext<CR>
" Do default action for previous item.
nnoremap <silent> <space>k  :<C-u>CocPrev<CR>
" Resume latest coc list
nnoremap <silent> <space>p  :<C-u>CocListResume<CR>
```

&nbsp;


Example
--

```vim
" Basic configs
set nocompatible
filetype plugin indent on
syntax enable
set encoding=utf-8
set ruler
set tabstop=4
set softtabstop=4
set shiftwidth=4
set textwidth=79
set expandtab
set autoindent
set smartindent
set fileformat=unix
set backspace=indent,eol,start
set history=50
set showcmd
set incsearch
set hidden
set nobackup
set nowritebackup
set cmdheight=2
set updatetime=300
set shortmess+=c
set signcolumn=yes

" Enableing mouse
if has('mouse')
    set mouse=a
endif

" VIM Plugin
call plug#begin()

" Intellisense Engine
Plug 'neoclide/coc.nvim', {'do': { -> coc#util#install()}}

" === Colorscheme === "
Plug 'NLKNguyen/papercolor-theme'

" === Git Plugins === "
Plug 'airblade/vim-gitgutter'
Plug 'itchyny/lightline.vim'

" Customized vim status line
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'

" Icons
Plug 'ryanoasis/vim-devicons'

call plug#end()


" Airline configs
let g:airline_theme='papercolor'
let g:airline#extensions#tabline#enabled = 1

" GitGutter configs
let g:gitgutter_max_signs = 500

" Syntax configs
set background=dark
colorscheme PaperColor
set number
set laststatus=2
let python_highlight_all = 1

" GVIM configs
set guifont=Monaco\ 12
if has('gui_running')
    set lines=999 columns=999
endif


" Use tab for trigger completion with characters ahead and navigate.
" Use command ':verbose imap <tab>' to make sure tab is not mapped by other plugin.
inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

" Use <c-space> to trigger completion.
inoremap <silent><expr> <c-space> coc#refresh()

" Use <cr> to confirm completion, `<C-g>u` means break undo chain at current position.
" Coc only does snippet and additional edit on confirm.
inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"

" Use `[c` and `]c` to navigate diagnostics
nmap <silent> [c <Plug>(coc-diagnostic-prev)
nmap <silent> ]c <Plug>(coc-diagnostic-next)

" Remap keys for gotos
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)

" Use K to show documentation in preview window
nnoremap <silent> K :call <SID>show_documentation()<CR>

function! s:show_documentation()
  if (index(['vim','help'], &filetype) >= 0)
    execute 'h '.expand('<cword>')
  else
    call CocAction('doHover')
  endif
endfunction

" Highlight symbol under cursor on CursorHold
autocmd CursorHold * silent call CocActionAsync('highlight')

" Remap for rename current word
nmap <leader>rn <Plug>(coc-rename)

" Remap for format selected region
vmap <leader>f  <Plug>(coc-format-selected)
nmap <leader>f  <Plug>(coc-format-selected)

augroup mygroup
  autocmd!
  " Setup formatexpr specified filetype(s).
  autocmd FileType typescript,json setl formatexpr=CocAction('formatSelected')
  " Update signature help on jump placeholder
  autocmd User CocJumpPlaceholder call CocActionAsync('showSignatureHelp')
augroup end

" Remap for do codeAction of selected region, ex: `<leader>aap` for current paragraph
vmap <leader>a  <Plug>(coc-codeaction-selected)
nmap <leader>a  <Plug>(coc-codeaction-selected)

" Remap for do codeAction of current line
nmap <leader>ac  <Plug>(coc-codeaction)
" Fix autofix problem of current line
nmap <leader>qf  <Plug>(coc-fix-current)

" Use `:Format` to format current buffer
command! -nargs=0 Format :call CocAction('format')

" Use `:Fold` to fold current buffer
command! -nargs=? Fold :call     CocAction('fold', <f-args>)


" Add diagnostic info for https://github.com/itchyny/lightline.vim
let g:lightline = {
      \ 'colorscheme': 'wombat',
      \ 'active': {
      \   'left': [ [ 'mode', 'paste' ],
      \             [ 'cocstatus', 'readonly', 'filename', 'modified' ] ]
      \ },
      \ 'component_function': {
      \   'cocstatus': 'coc#status'
      \ },
      \ }


" Using CocList
" Show all diagnostics
nnoremap <silent> <space>a  :<C-u>CocList diagnostics<cr>
" Manage extensions
nnoremap <silent> <space>e  :<C-u>CocList extensions<cr>
" Show commands
nnoremap <silent> <space>c  :<C-u>CocList commands<cr>
" Find symbol of current document
nnoremap <silent> <space>o  :<C-u>CocList outline<cr>
" Search workspace symbols
nnoremap <silent> <space>s  :<C-u>CocList -I symbols<cr>
" Do default action for next item.
nnoremap <silent> <space>j  :<C-u>CocNext<CR>
" Do default action for previous item.
nnoremap <silent> <space>k  :<C-u>CocPrev<CR>
" Resume latest coc list
nnoremap <silent> <space>p  :<C-u>CocListResume<CR>
" Python shortcut
nnoremap <silent> <F5> <Esc>:w<CR>:!clear;python %<CR>
```


&nbsp;

### More COC Extension
If you want you can install the following COC Extension:
- `:CocInstall coc-syntax`
- `:CocInstall coc-pairs`
- `:CocInstall coc-highlight`
- `:CocInstall coc-yaml`
- `:CocInstall coc-json`
- `:CocInstall coc-snippets`
- `:CocInstall coc-prettier`
- `:CocInstall coc-docker`
- `:CocInstall coc-go`


&nbsp;

Troubleshooting
--

If you get any error message similar to [this one](https://github.com/mads-hartmann/bash-language-server#installation) `Server bash failed to start`, run the following command:

```
sudo npm -i install --unsafe-perm -g  bash-language-server
```

and then, add the following configuration to ```.vimrc```:
```
if executable('bash-language-server')
  au User lsp_setup call lsp#register_server({
        \ 'name': 'bash-language-server',
        \ 'cmd': {server_info->[&shell, &shellcmdflag, 'bash-language-server start']},
        \ 'whitelist': ['sh'],
        \ })
endif
```
Now remove `bash-language-server` configuration from `coc-settings.json`:
```
  "languageserver": {
    "bash": {
      "command": "bash-language-server",
      "args": ["start"],
      "filetypes": ["sh"],
      "ignoredRootPaths": ["~"]
    }
  }
```
Navigate to ```~/.vim/pack/coc/start/coc.nvim``` directory and run the following command:
```
$ cd ~/.vim/pack/coc/start/coc.nvim
$ sudo rm -r node_modules
$ yarn install
```

&nbsp;

Switch between VIM and Terminal quickly
--

### To suspend your running vim
```Ctrl```+```Z```

will suspend the process and get back to your shell
```
fg
```
will resume (bring to foreground) your suspended vim

&nbsp;

### To start a new shell
start a subshell using:
```
:sh
```
(as configured by)
```
:set shell?
```
### or
```
:!bash
```
followed by:

```Ctrl```+```D``` (or ```exit```, but why type so much?)

to kill the shell and return to vim


[top](#table-of-contents)
&nbsp;


&nbsp;

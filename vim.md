## vim || emacs
https://wiki.python.org/moin/Vim

## 靜態解析類插件
neobundle.vim
vim-flake8

## vim 配置
~/.vimrc

```
syntax on
filetype plugin indent on
NeoBundle 'nvie/vim-flake8'
```

~/.vim/ftplugin/python.vim
```
setl expandtab
setl tabstop=4
setl shiftwidth=4
setl softtabstop=0
autocmd BufWritePre * :%s/\s\+$//ge
selocal textwidth=80
```

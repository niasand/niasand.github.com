title: vimrc小配置
id: 610
date: 2012-04-05 22:16:27
tags:
---

set nocompatible
source $VIMRUNTIME/vimrc_example.vim
source $VIMRUNTIME/mswin.vim
behave mswin
cd d:wampwww
syntax on
set nu
set guifont=Courier New:h10
colorscheme murphy
set tags=tags;
"设置不生成备份文件
set nobackup
set smartindent
set tabstop=4
set ignorecase
set history=400
set shortmess=atI
set ai!
set cindent shiftwidth=4
set ambiwidth=double
set lines=35 columns=138

set diffexpr=MyDiff()
function MyDiff()
  let opt = '-a --binary '
  if &amp;diffopt =~ 'icase' | let opt = opt . '-i ' | endif
  if &amp;diffopt =~ 'iwhite' | let opt = opt . '-b ' | endif
  let arg1 = v:fname_in
  if arg1 =~ ' ' | let arg1 = '"' . arg1 . '"' | endif
  let arg2 = v:fname_new
  if arg2 =~ ' ' | let arg2 = '"' . arg2 . '"' | endif
  let arg3 = v:fname_out
  if arg3 =~ ' ' | let arg3 = '"' . arg3 . '"' | endif
  let eq = ''
  if $VIMRUNTIME =~ ' '
    if &amp;sh =~ ' ' . arg3 . eq
endfunction
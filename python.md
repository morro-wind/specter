## pip
导入 IPython 后,交互模式会实现下述功能。
·TAB 键补全代码
·使用通常的 Shell 命令
·与 pdb(调试器)联动
pip install ipython  

\(檢查代碼是否符合PEP編碼格式規範，找出語法錯誤，導入未被使用的模塊）  
pip install flake8
// 查看当前安装的版本
pip freeze

## virtualenv
// 创建virtualenv 环境目录
virtualenv venv  
// 启动virtualenv 环境
source venv/bin/activate  
// 关闭virtualenv 环境
deactivate  
virtualenv --python=/usr/local/python2.7.16/bin/python venv2  
source venv2/bin/activate

## vim python 格式化設置
touch ~/.vim/ftplugin/python.vim

```
" 语法高亮的设置“
syntax on
" 自动缩进的设置”
filetype plugin indent on
" 将“Tab”替换为“空格”
setl expandtab
" 将“Tab”的“缩进幅度”改为 4
setl tabstop=4
" 自动缩进时的“缩进幅度”改为 4
setl shiftwidth=4
" 按下键盘“Tab”键时插入的空格数
" 这里设置为 0 就可以插入“tabstop”中设置的空格数了
setl softtabstop=0
" 保存时删除行尾的空格
autocmd BufWritePre * :%s/\s\+$//ge
"80 个字符换行
setlocal textwidth=80
```

## 執行DEBUG

vim 編輯中的腳本啓動調試工具  
!/path/to/virtualenv/python -m pdb %

## pdb

import pdb; pdb.set\_trace\(\)


## pip

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

## 執行DEBUG

vim 編輯中的腳本啓動調試工具  
!/path/to/virtualenv/python -m pdb %

## pdb

import pdb; pdb.set\_trace\(\)


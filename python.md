## pip
pip install ipython
(檢查代碼是否符合PEP編碼格式規範，找出語法錯誤，導入未被使用的模塊）
pip install flake8
pip freeze

## virtualenv
virtualenv venv
source venv/bin/activate
deactivate
virtualenv --python=/usr/local/python2.7.16/bin/python venv2
source venv2/bin/activate

## 執行DEBUG
vim 編輯中的腳本啓動調試工具
!/path/to/virtualenv/python -m pdb %

## pdb
import pdb; pdb.set_trace()
## pip
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

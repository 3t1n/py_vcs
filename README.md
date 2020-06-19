# Versionar arquivos do Power BI com GIT

Esse diretório possue um compilado de todos arquivos necessários para realizar o versionamento dos arquivos do Power BI com o GIT
<br>
Repositório Original para refência - https://github.com/awaregroup/powerbi-vcs

# Vídeo Demonstrativo

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/EVx9HTGd6OU/0.jpg)](https://www.youtube.com/watch?v=EVx9HTGd6OU)

## Configurações para esteira de Entrega Contínua do Azure Dev Ops

Vocês podem utilizar o arquivo YAML ``` config.yml ``` e importar no Azure Dev Ops , ou realizar os procedimentos abaixo:

## Dependências

```python
python -m pip install --upgrade pip
pip install ConfigArgParse
pip install lxml
pip install requests-toolbelt
pip install json
pip install requests

```

## Step de compactação da esteira
```python
cd ../
git clone https://github.com/3t1n/py_vcs.git 
ls -lia
cd py_vcs
python pbivcs.py -c /home/vsts/work/1/s f1.pbix
ls -lia
```
Para mais informações sobre esse arquivo acesse esse repositório : https://github.com/3t1n/build_pbi

## Step de Build

```python
# -*- coding: utf-8 -*-
"""
@author: Tadeu Mansi
"""

import requests
from requests_toolbelt import MultipartEncoder
import json

url = "https://login.microsoftonline.com/common/oauth2/token"

payload = {
'client_id': 'SEU_CLIENT_ID',
'grant_type': 'password',
'resource': 'https://analysis.windows.net/powerbi/api',
'username': 'SEU_EMAIL',
'password': 'SUA_SENHA',
'client_secret': 'SEU_CLIENT_SECRET'
}

response = requests.request("POST", url, headers={}, data = payload)

json_encode = json.loads(response.text.encode('utf8'))
accessToken = json_encode['access_token']

groupId = "ID_DO_WORKSPACE"
reportName = "NOME_DO_REPORT"

url = 'https://api.powerbi.com/v1.0/myorg/groups/' + groupId + '/imports?datasetDisplayName=' + reportName + '&nameConflict=CreateOrOverwrite'

headers = {
    'Content-Type': 'multipart/form-data',
    'authorization': 'Bearer ' + accessToken
}

file_location = 'SEUARQUIVO.pbix'

files = {'value': (None, open(file_location, 'rb'), 'multipart/form-data')}
mp_encoder = MultipartEncoder(fields=files)

r = requests.post(
    url=url,
    data=mp_encoder, 
    headers=headers
)

```

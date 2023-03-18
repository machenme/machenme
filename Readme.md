## Python for windows([mamba](https://mamba.readthedocs.io/en/latest/installation.html)/[conda](https://docs.conda.io/en/latest/miniconda.html))  
### change conda sources
- open mamba/conda then type `conda config --set show_channel_urls yes`(the application you open is named like xxx Prompt)
- open `%HOMEPATH%\.condarc` with editor whatever you like
- replace below code into .condarc file
  ```powershell
  channels:
  - defaults
  show_channel_urls: true
  default_channels:
    - https://mirrors.bfsu.edu.cn/anaconda/pkgs/main
    - https://mirrors.bfsu.edu.cn/anaconda/pkgs/r
    - https://mirrors.bfsu.edu.cn/anaconda/pkgs/msys2
  custom_channels:
    conda-forge: https://mirrors.bfsu.edu.cn/anaconda/cloud
    msys2: https://mirrors.bfsu.edu.cn/anaconda/cloud
    bioconda: https://mirrors.bfsu.edu.cn/anaconda/cloud
    menpo: https://mirrors.bfsu.edu.cn/anaconda/cloud
    pytorch: https://mirrors.bfsu.edu.cn/anaconda/cloud
    pytorch-lts: https://mirrors.bfsu.edu.cn/anaconda/cloud
    simpleitk: https://mirrors.bfsu.edu.cn/anaconda/cloud
  ```
- clean conda cache with `conda clean -i`
- create py ervironment with `mamba create py311 python==3.11`(py311 is your environment name, I like use python version)
- activate your python env with `conda/mamba activate py311)
- change your pip sources with `pip config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple`
---
## Python use persional profile on selenium (edge)
```python
from selenium import webdriver
from selenium.webdriver.edge.service import Service as EdgeService
from webdriver_manager.microsoft import EdgeChromiumDriverManager

option = webdriver.EdgeOptions()
# make sure change C:/Users/chen/AppData/Local/Microsoft/Edge/User Data to your own profile address,you can find it with edge in edge://version
option.add_argument("user-data-dir=C:/Users/chen/AppData/Local/Microsoft/Edge/User Data")
option.add_argument("--start-maximized")
driver = webdriver.Edge(service=EdgeService(EdgeChromiumDriverManager().install()),options=option)
```

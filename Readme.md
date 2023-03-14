## Create python environment on wsl2
---
- install wsl2(windows 11 should be wsl2 by default)
  ```powershell
  wsl --install
  ```
- restart and open ubuntu then create a user
- install miniconda
  ```ubuntu
  wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
  bash Miniconda3-latest-Linux-x86_64.sh
  ```
- create python envs(you can change python `python==<version>` version if you want)
  ```ubuntu
  conda create -n py311 python==3.11
  ```
- change pip sources
  ```powershell
  pip config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple
  ```
---
- Nolsp.exe
  ```powershell
  NoLsp.exe C:\Windows\System32\wsl.exe
  NoLsp.exe "C:\Program Files\WindowsApps\MicrosoftCorporationII.WindowsSubsystemForLinux_1.1.3.0_x64__8wekyb3d8bbwe\wslhost.exe"
  NoLsp.exe "C:\Program Files\WindowsApps\MicrosoftCorporationII.WindowsSubsystemForLinux_1.1.3.0_x64__8wekyb3d8bbwe\wslg.exe"
  NoLsp.exe "C:\Program Files\WindowsApps\MicrosoftCorporationII.WindowsSubsystemForLinux_1.1.3.0_x64__8wekyb3d8bbwe\wslservice.exe"
  NoLsp.exe "C:\Program Files\WindowsApps\MicrosoftCorporationII.WindowsSubsystemForLinux_1.1.3.0_x64__8wekyb3d8bbwe\wsl.exe"
  NoLsp.exe "C:\Program Files\WindowsApps\MicrosoftCorporationII.WindowsSubsystemForLinux_1.1.3.0_x64__8wekyb3d8bbwe\msrdc.exe"
  pause
  ```

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

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


---
### FAQ
Q: Error code: Wsl/Service/0x800706f7  
A: 
- reset winsock
  ```powershell
  wsl --shutdown
  netsh winsock reset
  ```
- download [Nolsp.exe](https://wtto00.github.io/cdn/windows/nolsp.exe)
- get wsl location
  ```powershell
  Get-AppxPackage MicrosoftCorporationII.WindowsSubsystemForLinux | Select-Object -expand InstallLocation
  ```
- go to Nolsp.exe location with administrator powershell(remember use your location replace them)
  ```powershell
  .\NoLsp.exe "C:\Program Files\WindowsApps\MicrosoftCorporationII.WindowsSubsystemForLinux_1.0.3.0_x64__8wekyb3d8bbwe\wsl.exe"
  .\NoLsp.exe "C:\Program Files\WindowsApps\MicrosoftCorporationII.WindowsSubsystemForLinux_1.0.3.0_x64__8wekyb3d8bbwe\wslg.exe"
  .\NoLsp.exe "C:\Program Files\WindowsApps\MicrosoftCorporationII.WindowsSubsystemForLinux_1.0.3.0_x64__8wekyb3d8bbwe\wslservice.exe"
  ```

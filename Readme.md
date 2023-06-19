## fix Error code: Wsl/Service/0x800706f7
solution page:https://github.com/microsoft/WSL/issues/4177#issuecomment-1359200646

save code like fix_wsl.ps1, then run it with powershell administrator .\fix_wsl.ps1

```powershell
#Requires -RunAsAdministrator
# Fix for https://github.com/microsoft/WSL/issues/4177

$MethodDefinition = @'
[DllImport("ws2_32.dll", CharSet = CharSet.Unicode)]
public static extern int WSCSetApplicationCategory([MarshalAs(UnmanagedType.LPWStr)] string Path, uint PathLength, [MarshalAs(UnmanagedType.LPWStr)] string Extra, uint ExtraLength, uint PermittedLspCategories, out uint pPrevPermLspCat, out int lpErrno);
'@

$Ws2Spi = Add-Type -MemberDefinition $MethodDefinition -Name 'Ws2Spi' -PassThru

$WslLocation = Get-AppxPackage MicrosoftCorporationII.WindowsSubsystemForLinux | Select-Object -expand InstallLocation

$Executables = ("wsl.exe", "wslservice.exe");

foreach ($Exe in $Executables) {
    $ExePath = "${WslLocation}\${Exe}";
    $ExePathLength = $ExePath.Length;

    $PrevCat = $null;
    $ErrNo = $null;
    if ($Ws2Spi::WSCSetApplicationCategory($ExePath, $ExePathLength, $null, 0, [uint32]"0x80000000", [ref] $PrevCat, [ref] $ErrNo) -eq 0) {
        Write-Output "Added $ExePath!";
    }
}
```
then
``` powershell
taskkill -IM "wslservice.exe" /F
```


## MySQL for wsl2
- install MySQL(check version)
```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.25-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.25-1_all.deb
sudo apt update
sudo apt install mysql-server
```




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
- create py ervironment with `mamba create -n py311 python==3.11`(py311 is your environment name, I like use python version)
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

## python 连接sqlServer
- 下载`OOBC`驱动 [https://learn.microsoft.com/en-us/sql/connect/odbc/download-odbc-driver-for-sql-server?view=sql-server-ver16](https://learn.microsoft.com/en-us/sql/connect/odbc/download-odbc-driver-for-sql-server?view=sql-server-ver16)
- Sql Server 配置管理器-SQL SERVER 网络配置-SQLEXPRESS的协议-TCP/IP属性-IP地址-IPALL-TCP动态端口留空 TCP端口对应下面的1433-重启sql服务
```python
import pyodbc
import pandas as pd

# 数据库连接配置
conn_str = (
    "DRIVER={ODBC Driver 18 for SQL Server};"
    "SERVER=localhost\\SQLEXPRESS,1433;"
    "DATABASE=BlockchainPolicyDB;"
    "Trusted_Connection=yes;"
    "TrustServerCertificate=yes;"
)
conn = pyodbc.connect(conn_str)
cursor = conn.cursor()
```


## EDGE修改用户数据路径到脚本位置.bat
```bat
chcp 65001
@echo off
setlocal enabledelayedexpansion
:: 获取批处理文件所在目录（带引号处理）
set "scriptDir=%~dp0"
set "edgeDir=%scriptDir%EdgeData"
set "regPath=HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge"

:: 创建目标目录结构
if not exist "%edgeDir%" (
    mkdir "%edgeDir%\User Data"
    mkdir "%edgeDir%\Cache"
    echo 已创建目录结构：%edgeDir%
)

:: 关闭所有Edge进程
taskkill /f /im msedge.exe >nul 2>&1
echo 已终止所有Edge进程...
timeout /t 2 >nul

:: 迁移现有用户数据（首次运行有效）
if exist "%LocalAppData%\Microsoft\Edge\User Data" (
    robocopy "%LocalAppData%\Microsoft\Edge\User Data" "%edgeDir%\User Data" /MIR /R:3 /W:5
    echo 已迁移历史数据...
)

:: 写入注册表配置
reg add "%regPath%" /v "UserDataDir" /d "%scriptDir%profile" /f >nul
reg add "%regPath%" /v "DiskCacheDir" /d "%edgeDir%\Cache" /f >nul
echo 已更新注册表配置...

pause
```

## uv添加国内源
`~/.config/uv/uv.toml` or `APPDATA\uv\uv.toml`
```bash
python-install-mirror = "https://registry.npmmirror.com/-/binary/python-build-standalone"
[[index]]
url = "https://mirrors.bfsu.edu.cn/pypi/web/simple"
# url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
# url = "https://mirrors.cernet.edu.cn/pypi/web/simple"
default = true
```


### 安装UV
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
```bash
# On macOS and Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 复现uv项目，确保项目根目录有`uv.lock`,`pyproject.toml`
```bash
git clone <your-repo-url>
pip install uv
uv sync
```


## 移动Public\Desktop 所有文件到当前bat目录
```bat
@echo off
setlocal
:: get current bat path
set currentDir=%~dp0
set publicDesktop=%PUBLIC%\Desktop
set movedFiles=0
if exist "%publicDesktop%" (
    echo Public Desktop found.
    for %%f in ("%publicDesktop%\*") do (
        echo Moving %%f
        move "%%f" "%currentDir%"
        if not errorlevel 1 (
            set /a movedFiles+=1
            echo Moved: %%f
        )
    )
    if %movedFiles% equ 0 (
        echo no file
    )
) else (
    echo Public Desktop not found.
)
pause
endlocal
```





## 解决图标快捷方式变白的问题
另存为bat格式运行即可
```bat
taskkill /f /im explorer.exe
attrib -h -i %userprofile%\AppData\Local\IconCache.db
del %userprofile%\AppData\Local\IconCache.db /a
start explorer
```

## PLT显示中文和负号
```py
import matplotlib.pyplot as plt
plt.rcParams["font.sans-serif"] = ["SimHei"]  # 用来正常显示中文标签
plt.rcParams["axes.unicode_minus"] = False  # 用来正常显示负号
```

## LSTM network 多X特征反归一化解决方法
```py
from sklearn.preprocessing import MinMaxScaler
# 归一化
scaler = MinMaxScaler(feature_range=(0, 1))
data_normalized = scaler.fit_transform(data)
# 反归一化预测结果  6 是X的特征维度
prediction_copies_array = np.repeat(predicted.detach().cpu().numpy(), 6, axis=-1)

predicted_cpu = predicted.detach().cpu().numpy()
predicted_np = scaler.inverse_transform(
    np.reshape(prediction_copies_array, (len(predicted_cpu), 6))
)[:, 0]

y_test_copies_array = np.repeat(y_test_tensor.detach().cpu().numpy(), 6, axis=-1)

y_test_cpu = y_test_tensor.detach().cpu().numpy()
y_test_np = scaler.inverse_transform(
    np.reshape(y_test_copies_array, (len(y_test_cpu), 6))
)[:, 0]
```



## Pytorch 2.x with cuda
```bash
conda install pytorch torchvision torchaudio pytorch-cuda=12.4 -c pytorch -c nvidia
```

check
```bash
import torch
print(torch.cuda.is_available())
```


## Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/
```bash
.\vs_buildtools.exe --norestart --passive --downloadThenInstall --includeRecommended --add Microsoft.VisualStudio.Workload.NativeDesktop --add Microsoft.VisualStudio.Workload.VCTools --add Microsoft.VisualStudio.Workload.MSBuildTools
```

## install ohmyzsh in china
```bash
git clone https://mirrors4.tuna.tsinghua.edu.cn/git/ohmyzsh.git
cd ohmyzsh/tools
REMOTE=https://mirrors4.tuna.tsinghua.edu.cn/git/ohmyzsh.git sh install.sh
```

### change conda source
```bash
cd ~
vim .condarc
```

```bash
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirror.bjtu.edu.cn/anaconda/pkgs/main
  - https://mirror.bjtu.edu.cn/anaconda/pkgs/r
  - https://mirror.bjtu.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirror.bjtu.edu.cn/anaconda/cloud
  pytorch: https://mirror.bjtu.edu.cn/anaconda/cloud
```
### change pip source
```bash
pip config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple
```

### more
`https://mirrors.bfsu.edu.cn/help`


## wsl2 use proxy,default port 7897
need add to last with .bashrc or .zshrc
```bash
# wsl2 proxy
# export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
export hostip=127.0.0.1
#export https_proxy="http://${hostip}:7897"
#export http_proxy="http://${hostip}:7897"
#export all_proxy="socks5://${hostip}:7897"
#export ALL_PROXY="socks5://${hostip}:7897"
# 默认不开启代理，可通过unproxy 和proxy 别名命令 关闭或开启代理
alias proxy='export https_proxy="http://${hostip}:7897";export http_proxy="http://${hostip}:7897";export all_proxy="socks5://${hostip}:7897";export ALL_PROXY="socks5://${hostip}:7897";'
alias unproxy='unset https_proxy; unset http_proxy; unset all_proxy; unset ALL_PROXY;'
```
if you use `.wslconfig` with `experimental` you need change `hostip=127.0.0.1`
```bash
[wsl2]

[experimental]
networkingMode=mirrored
```

### install miniconda3
```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh

~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
```

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


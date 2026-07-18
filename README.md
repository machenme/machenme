# 👋 Chen Ma's Tech Notes

> Personal knowledge base for Linux / Python / AI / DevOps / Reverse Engineering.

这里记录我的技术实践、环境配置以及踩坑笔记。

主要方向：

- 🐍 Python / PyTorch / LLM
- 🖥️ Linux / WSL / Windows Optimization
- 🚀 CUDA / Deep Learning Environment
- 🗄️ Database / Big Data
- 🔧 Reverse Engineering & Hardware

---

# 📚 Contents

## 🤖 AI / Deep Learning

- [🚀 uv + PyTorch CUDA 国内加速配置](#-uv--pytorch-cuda-国内加速配置)
- [📊 LSTM 多特征反归一化解决方案](#-lstm-多特征反归一化解决方案)
- [📈 Matplotlib 中文字体和负号显示](#-matplotlib-中文字体和负号显示)

## 🐍 Python / Development

- [📦 uv 配置国内源](#-uv-配置国内源)
- [🔗 Python 连接 SQL Server](#-python-连接-sql-server)
- [🔧 解决 Microsoft Visual C++ Build Tools 缺失](#-解决-microsoft-visual-c-build-tools-缺失)
- [🐍 修改 conda 源](#-修改-conda-源)
- [📦 修改 pip 源](#-修改-pip-源)

## 🖥️ Windows / Linux

- [🌐 OpenClaw WSL 代理启动](#-openclaw-wsl-代理启动)
- [🎮 LTSC 删除 Xbox Gaming Overlay](#-ltsc-删除-xbox-gaming-overlay)
- [💻 Oh My Zsh 国内安装](#-oh-my-zsh-国内安装)

## 🗄️ Database

- [Oracle 11gR2 11.2.0.4 + Oracle Linux 7.9](#-oracle-11gr2-112040)

## 🔧 Hardware / Reverse Engineering

- [📱 黑域 Brevent 启动命令](#-黑域-brevent)
- [📡 TEWA-1000E 超级管理员密码](#-tewa-1000e-超级管理员密码)
- [🔐 Redmi AX6S SSH 密码计算](#-redmi-ax6s-ssh-密码计算)

---

# 🤖 AI / Deep Learning

## 🚀 uv + PyTorch CUDA 国内加速配置

使用 uv 管理 PyTorch CUDA 环境：

```toml
[project]
dependencies = [
    "torch",
    "torchvision",
    "torchaudio",
]

[[tool.uv.index]]
name = "pytorch-cu130"
url = "https://mirrors.nju.edu.cn/pytorch/whl/cu130/"
explicit = true

[tool.uv.sources]
torch = [
    { index = "pytorch-cu130" }
]
torchvision = [
    { index = "pytorch-cu130" }
]
torchaudio = [
    { index = "pytorch-cu130" }
]
```

---

## 📊 LSTM 多特征反归一化解决方案

> sklearn MinMaxScaler 在多变量预测中，需要恢复原始维度。

```python
from sklearn.preprocessing import MinMaxScaler
import numpy as np

# 归一化
scaler = MinMaxScaler(feature_range=(0, 1))
data_normalized = scaler.fit_transform(data)

divi = 6  # X 的特征维度

# 反归一化预测结果
prediction_copies_array = np.repeat(predicted.detach().cpu().numpy(), divi, axis=-1)
predicted_cpu = predicted.detach().cpu().numpy()
predicted_np = scaler.inverse_transform(
    np.reshape(prediction_copies_array, (len(predicted_cpu), divi))
)[:, 0]

# 反归一化真实值
y_test_copies_array = np.repeat(y_test_tensor.detach().cpu().numpy(), divi, axis=-1)
y_test_cpu = y_test_tensor.detach().cpu().numpy()
y_test_np = scaler.inverse_transform(
    np.reshape(y_test_copies_array, (len(y_test_cpu), divi))
)[:, 0]
```

---

## 📈 Matplotlib 中文字体和负号显示

```python
import matplotlib.pyplot as plt

plt.rcParams["font.sans-serif"] = ["SimHei"]  # 用来正常显示中文标签
plt.rcParams["axes.unicode_minus"] = False    # 用来正常显示负号
```

---

# 🐍 Python / Development

## 📦 uv 配置国内源

Windows PowerShell:

```powershell
$filePath = Join-Path $env:APPDATA 'uv\uv.toml'
$dir = Split-Path $filePath -Parent
New-Item -Path $dir -ItemType Directory -Force | Out-Null
@"
python-install-mirror = "https://registry.npmmirror.com/-/binary/python-build-standalone"
[[index]]
url = "https://mirrors.bfsu.edu.cn/pypi/web/simple"
# url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
# url = "https://mirrors.cernet.edu.cn/pypi/web/simple"
default = true
"@ | Set-Content -Path $filePath
```

---

## 🔗 Python 连接 SQL Server

- 下载 ODBC 驱动：[Download ODBC Driver for SQL Server](https://learn.microsoft.com/en-us/sql/connect/odbc/download-odbc-driver-for-sql-server?view=sql-server-ver16)
- SQL Server 配置管理器 → SQL Server 网络配置 → SQLEXPRESS 的协议 → TCP/IP 属性 → IP 地址 → IPALL → TCP 动态端口留空，TCP 端口填 `1433` → 重启 SQL 服务

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

---

## 🔧 解决 Microsoft Visual C++ Build Tools 缺失

> Microsoft Visual C++ 14.0 or greater is required.

参考下载：
- [VS 2022 Community](https://github.com/machenme/download/blob/main/vs_2022_community.exe)
- [Microsoft C++ Build Tools 官方](https://visualstudio.microsoft.com/visual-cpp-build-tools/)

```bash
.\vs_buildtools.exe --norestart --passive --downloadThenInstall --includeRecommended --add Microsoft.VisualStudio.Workload.NativeDesktop --add Microsoft.VisualStudio.Workload.VCTools --add Microsoft.VisualStudio.Workload.MSBuildTools
```

---

## 🐍 修改 conda 源

```bash
cd ~
vim .condarc
```

```yaml
channels:
  - defaults
show_channel_urls: true
default_channels:
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

---

## 📦 修改 pip 源

```bash
pip config set global.index-url http://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple
```

---

# 🖥️ Windows / Linux

## 🌐 OpenClaw WSL 代理启动

```bash
HTTP_PROXY=http://127.0.0.1:7897 \
HTTPS_PROXY=http://127.0.0.1:7897 \
NODE_TLS_REJECT_UNAUTHORIZED=0 \
DEBUG=openclaw:tools:* \
openclaw gateway run
```

---

## 🎮 LTSC 删除 Xbox Gaming Overlay

Windows LTSC 默认没有完整 Xbox 组件。以管理员身份运行 PowerShell：

```powershell
# --- 1. 权限检查：确保以管理员身份运行 ---
if (-not ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)) {
    Write-Host "请以管理员身份运行此脚本！" -ForegroundColor Red
    return
}

Write-Host "正在开始清理 Xbox Game Bar 相关组件..." -ForegroundColor Cyan

# --- 2. 卸载 Xbox 相关组件 ---
Write-Host "[1/3] 正在卸载 Appx 软件包..." -ForegroundColor Yellow
$packages = @(
    "Microsoft.XboxGamingOverlay",
    "Microsoft.XboxGameOverlay",
    "Microsoft.XboxSpeechToTextOverlay"
)
foreach ($pkg in $packages) {
    Get-AppxPackage $pkg -AllUsers | Remove-AppxPackage -ErrorAction SilentlyContinue
}

# --- 3. 禁用 GameDVR 注册表配置 ---
Write-Host "[2/3] 正在修改 GameDVR 策略..." -ForegroundColor Yellow
$regConfig = @(
    @{ Path = "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR"; Name = "AppCaptureEnabled"; Value = 0 },
    @{ Path = "HKCU:\System\GameConfigStore"; Name = "GameDVR_Enabled"; Value = 0 }
)

foreach ($item in $regConfig) {
    if (-not (Test-Path $item.Path)) { New-Item -Path $item.Path -Force | Out-Null }
    Set-ItemProperty -Path $item.Path -Name $item.Name -Value $item.Value -Type DWord -Force
}

# --- 4. 彻底禁用协议关联 (防止弹出"需要新应用以打开此 ms-gamebar 链接") ---
Write-Host "[3/3] 正在劫持 ms-gamebar 协议关联..." -ForegroundColor Yellow
$protPaths = @("Registry::HKEY_CLASSES_ROOT\ms-gamebar", "Registry::HKEY_CLASSES_ROOT\ms-gamebarservices")

foreach ($path in $protPaths) {
    if (-not (Test-Path $path)) { New-Item -Path $path -Force | Out-Null }

    Set-Item -Path $path -Value "URL:ms-gamebar" -Force
    Set-ItemProperty -Path $path -Name "URL Protocol" -Value "" -Force
    Set-ItemProperty -Path $path -Name "NoOpenWith" -Value "" -Force

    $cmdPath = "$path\shell\open\command"
    if (-not (Test-Path $cmdPath)) { New-Item -Path $cmdPath -Force | Out-Null }
    Set-Item -Path $cmdPath -Value "$env:SystemRoot\System32\systray.exe" -Force
}

Write-Host "-------------------------------------------"
Write-Host "操作完成！Xbox Game Bar 已被深度禁用。" -ForegroundColor Green
Write-Host "提示：建议重启资源管理器 (explorer.exe) 或重启电脑以生效。" -ForegroundColor Gray
```

---

## 💻 Oh My Zsh 国内安装

```bash
git clone https://mirrors4.tuna.tsinghua.edu.cn/git/ohmyzsh.git
cd ohmyzsh/tools
REMOTE=https://mirrors4.tuna.tsinghua.edu.cn/git/ohmyzsh.git sh install.sh
```

---

# 🗄️ Database

## Oracle 11gR2 11.2.0.4

兼容环境：

```
Oracle Linux 7.9
Oracle Database 11.2.0.4
```

```text
magnet:?xt=urn:btih:4FA33857F956EBCB3FE630753B59DEF49EF76445
```

---

# 🔧 Hardware / Reverse Engineering

## 📱 黑域 Brevent

```cmd
adb -d shell 'output=$(pm path me.piebridge.brevent); export CLASSPATH=${output#*:}; app_process /system/bin me.piebridge.brevent.server.BreventServer bootstrap; /system/bin/sh /data/local/tmp/brevent.sh'
```

---

## 📡 TEWA-1000E 超级管理员密码

> 适用于硬件版本 V1.0

下载 [天邑软件.exe](https://github.com/machenme/download/blob/main/%E5%A4%A9%E9%82%91%E8%BD%AF%E4%BB%B6.exe) 后打开，选择"大悦me"连接后手动输入相关登录命令，每次输入一次按一下回车。

```
telnetadmin
telnetadmin
su
BwcNuaFS
exit
telecomadmin get
```

---

## 🔐 Redmi AX6S SSH 密码计算

打开默认 `192.168.31.1` 并登录，查看 SN 码。

将下面代码另存为 `calc_passwd.py`，然后运行 `python calc_passwd.py 12345/A1BC23456`（将 `12345/A1BC23456` 替换为你的 SN 码）。

```python
import sys
import hashlib

def calc_passwd(sn):
    passwd = sn + '6d2df50a-250f-4a30-a5e6-d44fb0960aa0'
    md5_value = hashlib.md5(passwd.encode())
    return md5_value.hexdigest()[:8]

if __name__ == "__main__":
    if len(sys.argv) > 1:
        serial = sys.argv[1]
        print(calc_passwd(serial))
    else:
        serial = input('input your SN eg: 12345/A1BC23456 \n')
        print(calc_passwd(serial))
```

---

# 📦 Environment

常用工具：

| Category | Tools                      |
| -------- | -------------------------- |
| Language | Python / Rust / Java       |
| AI       | PyTorch / CUDA / LLM       |
| Package  | uv / pip / conda           |
| OS       | Windows / Linux / WSL2     |
| Database | Oracle / SQL Server / Hive |
| Dev      | Git / Docker               |

---

⭐ If these notes helped you, consider giving a star.

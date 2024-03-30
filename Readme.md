## LSTM network 多X特征反归一化解决方法
```py
from sklearn.preprocessing import MinMaxScaler
# 归一化
scaler = MinMaxScaler(feature_range=(0, 1))
data_normalized = scaler.fit_transform(data)
# 反归一化预测结果  6 是lookback
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
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```
or
```bash
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```
### cuda
windows
```bash
 https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda_12.1.0_531.14_windows.exe
```
wsl2.0
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda-repo-wsl-ubuntu-12-1-local_12.1.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-12-1-local_12.1.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-12-1-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
```

ubuntu
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda-repo-ubuntu2204-12-1-local_12.1.0-530.30.02-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2204-12-1-local_12.1.0-530.30.02-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2204-12-1-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
```
cudnn
```bash
https://developer.nvidia.com/rdp/cudnn-archive
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

## Ubuntu-22.04 LTS change source to bfsu
```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bac
sudo vim /etc/apt/source.list
```

```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors4.bfsu.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors4.bfsu.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors4.bfsu.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors4.bfsu.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors4.bfsu.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors4.bfsu.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

deb https://mirrors4.bfsu.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors4.bfsu.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors4.bfsu.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# # deb-src https://mirrors4.bfsu.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
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
  deepmodeling: https://mirrors.bfsu.edu.cn/anaconda/cloud/
```
### change pip source
```bash
pip config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple
```

### more
`https://mirrors.bfsu.edu.cn/help`


## wsl2 use proxy,default port 7890
need add to last with .bashrc or .zshrc
```bash
# wsl2 proxy
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
#export https_proxy="http://${hostip}:7890"
#export http_proxy="http://${hostip}:7890"
#export all_proxy="socks5://${hostip}:7890"
#export ALL_PROXY="socks5://${hostip}:7890"
# 默认不开启代理，可通过unproxy 和proxy 别名命令 关闭或开启代理
alias proxy='export https_proxy="http://${hostip}:7890";export http_proxy="http://${hostip}:7890";export all_proxy="socks5://${hostip}:7890";export ALL_PROXY="socks5://${hostip}:7890";'
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


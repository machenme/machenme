### change source(if work slow) and update 
```ubuntu
sudo update && sudo upgrade
```

### install nvidia display driver
```ubuntu
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda
```

### copy your fonts to /usr/share/fonts then rebuild fonts cache
```ubuntu
fc-cache -f -v
```

### install 3rd broswer(edge)
```ubuntu
wget https://packages.microsoft.com/repos/edge/pool/main/m/microsoft-edge-stable/microsoft-edge-stable_107.0.1418.35-1_amd64.deb
sudo dpkg -i microsoft-edge-stable_107.0.1418.35-1_amd64.deb
```

### set edge to chinese(optional)
```ubuntu
sudo nano /usr/bin/microsoft-edge
```
### add this at top line(line 6)
```ubuntu
export LANGUAGE=ZH-CN.UTF-8
```

### uninstall snap firefox
```ubuntu
sudo snap remove firefox
```
### get ubuntu pro for free(optional)
https://ubuntu.com/pro/dashboard

### open click-action to minisize on bar
```ubuntu
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```
### undo
```ubuntu
gsettings reset org.gnome.shell.extensions.dash-to-dock click-action
```

### update snap program
```ubuntu
sudo snap refresh
```

### install imwheel
```ubuntu
sudo apt install imwheel
sudo nano ~/.imwheelrc
```

```ubuntu
".*"
None,      Up,      Button4, 2
None,      Down,    Button5, 2
Control_L, Up,      Control_L|Button4
Control_L, Down,    Control_L|Button5
Shift_L,   Up,      Shift_L|Button4
Shift_L,   Down,    Shift_L|Button5
None,      Thumb1,  Alt_L|Left
None,      Thumb2,  Alt_L|Right
```
### install miniconda3
```ubuntu
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

### change conda source(restart terminal should see (base) in terminal)
```ubuntu
nano .condarc
```
```ubuntu
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  # 如果需要使用其他第三方源（参考上方完整列表）
  # 例如 conda install -c pytorch-test，则可以添加
  #pytorch-test: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

### change pipy source
```ubuntu
python3 -m pip config set global.index-url https://pypi.douban.com/simple
```

### toolbox(optional)
```ubuntu
sudo apt install fuse
wget https://download.jetbrains.com/toolbox/jetbrains-toolbox-1.26.5.13419.tar.gz
chmod +x jetbrains-toolbox-1.26.5.13419.tar.gz
./jetbrains-toolbox-1.26.5.13419.tar.gz
```
### netease-cloud-music
```ubuntu
sudo nano /opt/netease/netease-cloud-music/netease-cloud-music.bash
```

```ubuntu
#!/bin/sh
HERE="$(dirname "$(readlink -f "${0}")")"
export LD_LIBRARY_PATH="${HERE}"/libs
export QT_PLUGIN_PATH="${HERE}"/plugins 
export QT_QPA_PLATFORM_PLUGIN_PATH="${HERE}"/plugins/platforms
# start
cd /lib/x86_64-linux-gnu/ 
# end
exec "${HERE}"/netease-cloud-music $@
```



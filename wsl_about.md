### install wsl cuda with cudnn
```bash
sudo apt-key del 7fa2af80
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda
wget https://developer.nvidia.com/downloads/compute/cudnn/secure/8.9.1/local_installers/12.x/cudnn-local-repo-ubuntu2204-8.9.1.23_1.0-1_amd64.deb/
sudo dpkg -i cudnn-local-repo-ubuntu2204-8.9.1.23_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2204-8.9.1.23/cudnn-local-E7A7D88D-keyring.gpg /usr/share/keyrings/
```

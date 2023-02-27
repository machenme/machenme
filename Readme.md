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

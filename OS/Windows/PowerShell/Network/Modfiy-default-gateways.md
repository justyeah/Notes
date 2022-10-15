# PowerShell change default gateway (PowerShell 修改默认网关)
```powershell
$adapt=Get-WmiObject win32_networkadapterconfiguration -filter 'IPEnabled=True'
$adapt.SetGateways(@('10.0.0.1','10.0.0.100'),@(100.200))
```
# Simple nginx proxy server on windows vm

Powershell script ot run:
```
#Install folder - C:\nginx\
New-Item -Path "C:\" -Name "nginx" -ItemType "directory"

#File links
$install_ps1_link    = "https://raw.githubusercontent.com/GuardNexusGN/nginx_for_windows_proxy/main/Install-Nginx.ps1"
$get_winsw_link      = "https://raw.githubusercontent.com/GuardNexusGN/nginx_for_windows_proxy/main/winsw-2.11.0.exe" #or x86 version
$get_nginx_conf_link = "https://raw.githubusercontent.com/GuardNexusGN/nginx_for_windows_proxy/main/nginx.conf"
#$get_nginx_conf_inf2_link = "https://raw.githubusercontent.com/GuardNexusGN/nginx_for_windows_proxy/main/nginx_inf2.conf"

#nginx install
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
(new-object net.webclient).DownloadFile($get_winsw_link,'C:\nginx\winsw-2.11.0.exe')
(new-object net.webclient).DownloadFile($install_ps1_link,'C:\nginx\Install-Nginx.ps1')
powershell -File C:\nginx\Install-Nginx.ps1 -Version 1.21.1 -InstallPath C:\nginx;

#Firewall rule
New-NetFirewallRule -DisplayName 'nginx_server' -Direction Inbound -Action Allow -Protocol TCP -LocalPort 80

#new nginx conf for proxy
(new-object net.webclient).DownloadFile($get_nginx_conf_link,'C:\nginx\conf\nginx.conf')
Restart-Service -Name nginx

```

Install-Nginx.ps1 script by: https://github.com/guanwei

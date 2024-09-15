```ps1

# Variables
$WazuhVersion = "4.3.10"
$WazuhServerIP = "YOUR_WAZUH_SERVER_IP"

# Descargar el instalador
$InstallerUrl = "https://packages.wazuh.com/4.x/windows/wazuh-agent-$WazuhVersion-1.msi"
$InstallerPath = "$env:TEMP\wazuh-agent-$WazuhVersion.msi"
Invoke-WebRequest -Uri $InstallerUrl -OutFile $InstallerPath

# Instalar el agente
Start-Process msiexec.exe -ArgumentList "/i $InstallerPath /qn" -Wait

# Configurar el agente
$ConfigPath = "C:\Program Files (x86)\ossec-agent\ossec.conf"
(Get-Content $ConfigPath) -replace '<address>0.0.0.0</address>', "<address>$WazuhServerIP</address>" | Set-Content $ConfigPath

# Iniciar el servicio
Start-Service -Name "Wazuh"

Write-Host "La instalación del agente Wazuh se ha completado."

```
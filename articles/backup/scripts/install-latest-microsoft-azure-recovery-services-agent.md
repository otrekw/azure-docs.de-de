---
title: 'Skriptbeispiel: Installieren Sie den neuesten MARS-Agenten auf Ihrem lokalen Windows Server.'
description: Hier erfahren Sie, wie Sie mithilfe eines Skripts den neuesten MARS-Agent auf Ihren lokalen Windows-Servern in einem Speicherkonto installieren.
ms.topic: sample
ms.date: 06/23/2021
ms.openlocfilehash: 1080149d33eebed160449865d58f422e6eba36b1
ms.sourcegitcommit: f0168d80eb396ce27032aa02fe9da5a0c10b5af3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2021
ms.locfileid: "112560840"
---
# <a name="powershell-script-to-install-the-latest-mars-agent-on-an-on-premises-windows-server"></a>PowerShell-Skript zum Installieren des neuesten MARS-Agents auf einem lokalen Windows-Server

Mit diesem Skript können Sie den neuesten MARS-Agent auf Ihrem lokalen Windows-Server installieren.

## <a name="sample-script"></a>Beispielskript

```azurepowershell
<#

.SYNOPSIS
Sets MARS agent

.DESCRIPTION
Sets MARS agent

.ROLE
Administrators

#>
Set-StrictMode -Version 5.0
$ErrorActionPreference = "Stop"
Try {
    $agentPath = $env:TEMP + '\MARSAgentInstaller.exe'
    Invoke-WebRequest -Uri 'https://aka.ms/azurebackup_agent' -OutFile $agentPath
    & $agentPath /q | out-null

    $env:PSModulePath = (Get-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Session Manager\Environment' -Name PSModulePath).PSModulePath
    $azureBackupModuleName = 'MSOnlineBackup'
    $azureBackupModule = Get-Module -ListAvailable -Name $azureBackupModuleName
    if ($azureBackupModule) {
        $true
    }
    else {
        $false
    }
}
Catch {
    if ($error[0].ErrorDetails) {
        throw $error[0].ErrorDetails
    }
    throw $error[0]
}

```

## <a name="next-steps"></a>Nächste Schritte

[Informieren Sie sich](../backup-client-automation.md) über die Verwendung von PowerShell zum Bereitstellen und Verwalten von lokalen Sicherungen mit einem MARS-Agent.
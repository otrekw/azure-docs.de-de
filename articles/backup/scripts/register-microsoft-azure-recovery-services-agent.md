---
title: 'Skriptbeispiel: Registrierung eines lokalen Windows Servers oder Clientcomputers bei einem Recovery Services-Tresor'
description: Hier erfahren Sie, wie Sie ein Skript für die Registrierung eines lokalen Windows Servers oder Clientcomputers bei einem Recovery Services-Tresor nutzen können.
ms.topic: sample
ms.date: 06/23/2021
ms.openlocfilehash: a35d241ba86868c666916e284fb0700b59034eee
ms.sourcegitcommit: f0168d80eb396ce27032aa02fe9da5a0c10b5af3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2021
ms.locfileid: "112560822"
---
# <a name="powershell-script-to-register-an-on-premises-windows-server-or-a-client-machine-with-recovery-services-vault"></a>PowerShell-Skript für die Registrierung eines lokalen Windows Servers oder Clientcomputers bei einem Recovery Services-Tresor

Dieses Skript unterstützt Sie bei der Registrierung Ihres lokalen Windows Servers oder Clientcomputers bei einem Recovery Services-Tresor. 

## <a name="sample-script"></a>Beispielskript

```azurepowershell
<#

.SYNOPSIS
Registers MARS agent

.DESCRIPTION
Registers MARS agent

.ROLE
Administrators

#>
param (
    [Parameter(Mandatory = $true)]
    [String]
    $vaultcredPath,
    [Parameter(Mandatory = $true)]
    [String]
    $passphrase
)
Set-StrictMode -Version 5.0
$env:PSModulePath = (Get-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Session Manager\Environment' -Name PSModulePath).PSModulePath
Import-Module MSOnlineBackup
$ErrorActionPreference = "Stop"
Try {
    $date = Get-Date
    Start-OBRegistration -VaultCredentials $vaultcredPath -Confirm:$false
    $securePassphrase = ConvertTo-SecureString -String $passphrase -AsPlainText -Force
    Set-OBMachineSetting -EncryptionPassphrase $securePassphrase -SecurityPIN " "
}
Catch {
    if ($error[0].ErrorDetails) {
        throw $error[0].ErrorDetails
    }
    throw $error[0]
}

```

## <a name="how-to-execute-the-script"></a>Ausführen des Skripts

1. Speichern Sie das obige Skript auf Ihrem Computer mit einem Namen Ihrer Wahl und einer .ps1-Erweiterung.
1. Führen Sie das Skript unter Angabe folgender Parameter aus:
   - – vaultcredPath – Vollständiger Pfad der heruntergeladenen Datei mit den Tresoranmeldeinformationen
   - – passphrase – Klartextzeichenfolge, die mithilfe des Cmdlets [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-7.1&preserve-view=true) in eine sichere Zeichenfolge konvertiert wird

>[!Note]
>Sie müssen auch die Sicherheits-PIN angeben, die im Azure-Portal generiert wurde. Navigieren Sie zum Generieren der PIN auf dem Blatt „Recovery Services-Tresor“ zu den **Einstellungen** -> **Eigenschaften** -> **Sicherheits-PIN** und wählen Sie dann **Generieren** aus.

## <a name="next-steps"></a>Nächste Schritte

Hier [erfahren Sie mehr](../backup-client-automation.md) über die Verwendung von PowerShell zum Bereitstellen und Verwalten von lokalen Sicherungen mit dem MARS-Agent.


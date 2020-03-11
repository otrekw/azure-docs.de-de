---
title: Problembehandlung bei Problemen durch Anwendungen, die TLS 1.2 nicht unterstützen | Microsoft-Dokumentation
description: Problembehandlung bei Problemen durch Anwendungen, die TLS 1.2 nicht unterstützen
services: cloud-services
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: top-support-issue
ms.assetid: ''
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 01/17/2020
ms.author: tagore
ms.openlocfilehash: a9d15a94421694583562f433c20413fcc84697c7
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2020
ms.locfileid: "78270861"
---
# <a name="troubleshooting-applications-that-dont-support-tls-12"></a>Problembehandlung für Anwendungen, die TLS 1.2 nicht unterstützen
In diesem Artikel wird beschrieben, wie Sie die älteren TLS-Protokolle (TLS 1.0 und 1.1) aktivieren und Legacyverschlüsselungssammlungen anwenden, um die zusätzlichen Protokolle für die Web- und Workerrollen des Windows Server 2019-Clouddiensts zu unterstützen. 

Obwohl TLS 1.0 und TLS 1.1 als veraltet eingestuft werden, müssen Kunden die älteren Protokolle und Verschlüsselungssammlungen möglicherweise unterstützen, bis sie ihre Veraltung selbst planen können.  Das Aktivieren dieser Legacyprotokolle wird zwar nicht empfohlen, es werden aber dennoch Anleitungen für Kunden bereitgestellt. Es wird empfohlen, dass Kunden das Risiko dieser Regression abwägen, bevor sie die in diesem Artikel beschriebenen Änderungen vornehmen. 

> [!NOTE]
> Das Release „Guest OS Family 6“ (Gastbetriebssystemfamilie) erzwingt TLS 1.2, indem 1.0/1.1-Verschlüsselungen deaktiviert werden. 


## <a name="dropping-support-for-tls-10-tls-11-and-older-cipher-suites"></a>Einstellen der Unterstützung für TLS 1.0, TLS 1.1 und ältere Verschlüsselungssammlungen 
Zur Unterstützung des Ziels, die bestmögliche Verschlüsselung zu verwenden, kündigte Microsoft im Juni 2017 die Migration von TLS 1.0 und 1.1 an.   Seit dieser Ankündigung kündigte Microsoft die Absicht an, TLS 1.0 und 1.1 in unterstützten Versionen von Microsoft Edge und Internet Explorer 11 in der ersten Hälfte des Jahres 2020 standardmäßig zu deaktivieren.  Ähnliche Ankündigungen von Apple, Google und Mozilla geben die Richtung an, in die sich die Branche entwickelt.   

## <a name="tls-configuration"></a>TLS-Konfiguration  
In der Konfiguration des Windows Server 2019-Cloudserverimages sind TLS 1.0 und TLS 1.1 auf der Registrierungsebene deaktiviert. Das heißt, dass Anwendungen, die auf dieser Version von Windows bereitgestellt werden und den Windows-Stapel für TLS-Aushandlung verwenden, die TLS 1.0- und TLS 1.1-Kommunikation nicht zulassen.   

Außerdem verfügt der Server standardmäßig über eine begrenzte Anzahl von Verschlüsselungssammlungen: 

```
    TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 
    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 
    TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
    TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 
    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 
    TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 
    TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 
    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 
```

## <a name="step-1-create-the-powershell-script-to-enable-tls-10-and-tls-11"></a>Schritt 1: Erstellen eines PowerShell-Skripts zum Aktivieren von TLS 1.0 und TLS 1.1 

Verwenden Sie das folgende Codebeispiel, um ein Skript zu erstellen, das die älteren Protokolle und Verschlüsselungssammlungen aktiviert. Für die Zwecke dieser Dokumentation wird das Skript folgendermaßen benannt: **TLSsettings.ps1**. Speichern Sie dieses Skript auf Ihrem lokalen Desktop für den einfachen Zugriff in späteren Schritten. 


```Powershell
#******************* FUNCTION THAT ACTUALLY UPDATES KEYS; WILL RETURN REBOOT FLAG IF CHANGES *********************** 
 
Function Set-CryptoSetting {  
    param (  
        $regKeyName,  
        $value,  
        $valuedata,  
        $valuetype       
    )  
    
    $restart = $false 
  
    # Check for existence of registry key, and create if it does not exist  
    If (!(Test-Path -Path $regKeyName)) {  
        New-Item $regKeyName | Out-Null  
    }  
 
    # Get data of registry value, or null if it does not exist  
    $val = (Get-ItemProperty -Path $regKeyName -Name $value -ErrorAction SilentlyContinue).$value  
 
    If ($val -eq $null) {  
        # Value does not exist - create and set to desired value  
        New-ItemProperty -Path $regKeyName -Name $value -Value $valuedata -PropertyType $valuetype | Out-Null  
        $restart = $true 
    } 
 
    Else {  
        # Value does exist - if not equal to desired value, change it  
        If ($val -ne $valuedata) {  
            Set-ItemProperty -Path $regKeyName -Name $value -Value $valuedata  
            $restart = $true  
        }  
    }  
 
    $restart  
}  
 
#*************************************************************************************************************** 
 
#****************************** CIPHERSUITES FOR OS VERSIONS WINDOWS 10 AND ABOVE ****************************** 
 
function Get-BaseCipherSuitesWin10Above() 
{ 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384," 
 
# Legacy cipher suites 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256," 
        $cipherorder += "TLS_RSA_WITH_AES_256_GCM_SHA384,"  
        $cipherorder += "TLS_RSA_WITH_AES_128_GCM_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_256_CBC_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_128_CBC_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_256_CBC_SHA," 
        $cipherorder += "TLS_RSA_WITH_AES_128_CBC_SHA" 
 
 return $cipherorder 
} 
 

#*************************************************************************************************************** 
 
 
#********************************************** REGISTRY KEYS **************************************************** 
 
 
function Get-RegKeyPathToEnable() 
{ 
    $regKeyPath = @( 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2",         
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" , 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client", 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" , 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" 
    ) 
    return $regKeyPath 
} 
 
#*************************************************************************************************************** 
 
$localRegistryPath = @() 
 
# Enable TLS 1.2, TLS 1.1 and TLS 1.0 
$localRegistryPath += Get-RegKeyPathToEnable 
 
#******************* CREATE THE REGISTRY KEYS IF THEY DON'T EXIST******************************** 
 
# Check for existence of the registry keys, and create if they do not exist  
For ($i = 0; $i -lt $localRegistryPath.Length; $i = $i + 1) {  
   Write-Host "Checking for existing of key: $($localRegistryPath[$i]) Severity Level: Information"
   If (!(Test-Path -Path $localRegistryPath[$i])) {
        New-Item $localRegistryPath [$i] | Out-Null
    Write-Host "Creating key: $($localRegistryPath[$i]) Severity Level: Information"
    }
}  
 
#********************************* EXPLICITLY Enable TLS12,  TLS11 and TLS10********************************* 
 
For ($i = 0; $i -lt $localRegistryPath.Length; $i = $i + 1) { 
    if ($localRegistryPath[$i].Contains("Client") -Or $localRegistryPath[$i].Contains("Server")) { 
      Write-Host "Enabling this key: $($localRegistryPath[$i]) Severity: Information "
        $result = Set-CryptoSetting $localRegistryPath[$i].ToString() Enabled 1 DWord   
        $result = Set-CryptoSetting $localRegistryPath[$i].ToString() DisabledByDefault 0 DWord  
        $reboot = $reboot -or $result 
    } 
} 
 
#**************************************** SET THE CIPHER SUITE ORDER******************************** 
 
$cipherlist = @() 
 
# Set cipher suite order 
$cipherlist += Get-BaseCipherSuitesWin10Above 
$CipherSuiteRegKey = "HKLM:\SOFTWARE\Policies\Microsoft\Cryptography\Configuration\SSL\00010002"  
 
if (!(Test-Path -Path $CipherSuiteRegKey))  
{  
    New-Item $CipherSuiteRegKey | Out-Null  
    $reboot = $True  
    Write-Host "Creating key: $($CipherSuiteRegKey) Severity: Information "
}  
 
#Set-ItemProperty -Path $CipherSuiteRegKey -Name Functions -Value $cipherorder  
Set-ItemProperty -Path $CipherSuiteRegKey -Name Functions -Value $cipherlist  
#********************************************* REBOOT ******************************************* 
 
Write-Host "A reboot is required in order for changes to effect"  
Write-Host "Rebooting now..."  
shutdown.exe /r /t 5 /c "Crypto settings changed" /f /d p:2:4
```

## <a name="step-2-create-a-command-file"></a>Schritt 2: Erstellen einer Befehlsdatei 

Erstellen Sie eine CMD-Datei namens **RunTLSSettings.cmd** unter Verwendung der nachfolgenden Angaben. Speichern Sie dieses Skript auf Ihrem lokalen Desktop für den einfachen Zugriff in späteren Schritten. 

```cmd
PowerShell -ExecutionPolicy Unrestricted %~dp0TLSsettings.ps1
REM This line is required to ensure the startup tasks does not block the role from starting in case of error.  DO NOT REMOVE!!!! 
EXIT /B 0
```

## <a name="step-3-add-the-startup-task-to-the-roles-service-definition-csdef"></a>Schritt 3: Hinzufügen eines Starttasks zur Dienstdefinition der Rolle (csdef) 

Fügen Sie den folgenden Codeausschnitt zu Ihrer vorhandenen Dienstdefinitionsdatei hinzu. 


```
    <Startup> 
        <Task executionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
        </Task> 
    </Startup> 
```

Im folgenden Beispiel werden sowohl die Worker- als auch die Webrolle veranschaulicht. 

```
<?xmlversion="1.0"encoding="utf-8"?> 
<ServiceDefinitionname="CloudServiceName"xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition"schemaVersion="2015-04.2.6"> 
    <WebRolename="WebRole1"vmsize="Standard_D1_v2"> 
        <Sites> 
            <Sitename="Web"> 
                <Bindings> 
                    <Bindingname="Endpoint1"endpointName="Endpoint1"/> 
                </Bindings> 
            </Site> 
        </Sites> 
        <Startup> 
            <Taske xecutionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
            </Task> 
        </Startup> 
        <Endpoints> 
            <InputEndpointname="Endpoint1"protocol="http"port="80"/> 
        </Endpoints> 
    </WebRole> 
<WorkerRolename="WorkerRole1"vmsize="Standard_D1_v2"> 
    <Startup> 
        <Task executionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
        </Task> 
    </Startup> 
</WorkerRole> 
</ServiceDefinition> 
```

## <a name="step-5-add-the-scripts-to-your-cloud-service"></a>Schritt 5: Fügen Sie die Skripts zu Ihrem Clouddienst hinzu. 

1) Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihre WebRole.
2) Wählen Sie **Hinzufügen** aus.
3) Wählen Sie **Vorhandenes Element** aus.
4) Navigieren Sie im Datei-Explorer zu Ihrem Desktop, wo Sie die Dateien **TLSsettings.ps1** und **RunTLSSettings.cmd** gespeichert haben. 
5) Wählen Sie die beiden Dateien aus, um sie zu Ihrem Clouddienstprojekt hinzuzufügen.

## <a name="step-6-enable-copy-to-output-directory"></a>Schritt 6: Aktivieren Sie „In Ausgabeverzeichnis kopieren“.

Um sicherzustellen, dass die Skripts bei jedem Update, das von Visual Studio aus gepusht wird, hochgeladen werden, muss die Einstellung *In Ausgabeverzeichnis kopieren* auf *Immer kopieren* festgelegt werden.

1) Klicken Sie unter Ihrer WebRole mit der rechten Maustaste auf „RunTLSSettings.cmd“.
2) Wählen Sie **Eigenschaften** aus.
3) Ändern Sie auf der Registerkarte „Eigenschaften“ die Option *In Ausgabeverzeichnis kopieren* in *Immer kopieren*.
4) Wiederholen Sie die Schritte für **TLSsettings.ps1**.

## <a name="step-7-publish--validate"></a>Schritt 7: Veröffentlichen und Überprüfen

Nachdem die oben genannten Schritte abgeschlossen sind, veröffentlichen Sie nun das Update für Ihren bestehenden Clouddienst. 

Sie können [SSLLabs](https://www.ssllabs.com/) verwenden, um den TLS-Status Ihrer Endpunkte zu überprüfen. 

 

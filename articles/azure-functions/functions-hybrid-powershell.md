---
title: Remoteverwaltung von lokalen Ressourcen mithilfe von PowerShell-Funktionen
description: Erfahren Sie, wie Sie Hybrid Connections in Azure Relay konfigurieren können, um eine PowerShell-Funktions-App mit lokalen Ressourcen zu verbinden, die dann zur Remoteverwaltung der lokalen Ressource verwendet werden kann.
author: eamono
ms.topic: conceptual
ms.date: 04/26/2020
ms.author: eamono
ms.openlocfilehash: 5e01ffd8e17fda9113c7ec0fdb2c7f436b39c810
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97936905"
---
# <a name="managing-hybrid-environments-with-powershell-in-azure-functions-and-app-service-hybrid-connections"></a>Verwalten von Hybridumgebungen mit PowerShell in Azure Functions und App Service Hybrid Connections

Die Funktion Azure App Service Hybrid Connections ermöglicht den Zugriff auf Ressourcen in anderen Netzwerken. Weitere Informationen zu dieser Funktion finden Sie in der Dokumentation zu [Hybrid Connections](../app-service/app-service-hybrid-connections.md). In diesem Artikel wird beschrieben, wie Sie diese Funktion zum Ausführen von PowerShell-Funktionen verwenden, die auf einen lokalen Server abzielen. Dieser Server kann dann verwendet werden, um alle Ressourcen in der lokalen Umgebung über eine Azure PowerShell-Funktion zu verwalten.


## <a name="configure-an-on-premises-server-for-powershell-remoting"></a>Konfigurieren eines lokalen Servers für PowerShell-Remoting

Das folgende Skript ermöglicht PowerShell-Remoting und erstellt eine neue Firewallregel und einen WinRM-HTTPS-Listener. Zu Testzwecken wird ein selbstsigniertes Zertifikat verwendet. In einer Produktionsumgebung wird die Verwendung eines signierten Zertifikats empfohlen.

```powershell
# For configuration of WinRM, see
# https://docs.microsoft.com/windows/win32/winrm/installation-and-configuration-for-windows-remote-management.

# Enable PowerShell remoting.
Enable-PSRemoting -Force

# Create firewall rule for WinRM. The default HTTPS port is 5986.
New-NetFirewallRule -Name "WinRM HTTPS" `
                    -DisplayName "WinRM HTTPS" `
                    -Enabled True `
                    -Profile "Any" `
                    -Action "Allow" `
                    -Direction "Inbound" `
                    -LocalPort 5986 `
                    -Protocol "TCP"

# Create new self-signed-certificate to be used by WinRM.
$Thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME  -CertStoreLocation Cert:\LocalMachine\My).Thumbprint

# Create WinRM HTTPS listener.
$Cmd = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:COMPUTERNAME ""; CertificateThumbprint=""$Thumbprint""}"
cmd.exe /C $Cmd
```

## <a name="create-a-powershell-function-app-in-the-portal"></a>Erstellen einer PowerShell-Funktions-App im Portal

Die Funktion App Service Hybrid Connections ist nur in den Tarifen Basic, Standard und Isolated verfügbar. Wenn Sie die Funktions-App mit PowerShell erstellen, erstellen oder wählen Sie einen dieser Tarife aus.

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Wählen Sie auf der Seite **Neu** die Option **Compute** > **Funktions-App** aus.

1. Verwenden Sie auf der Seite **Grundlagen** die Funktions-App-Einstellungen, die in der folgenden Tabelle angegeben sind.

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Funktions-App erstellt wird. |
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Der Name der neuen Ressourcengruppe, in der die Funktionen-App erstellt wird |
    | **Name der Funktions-App** | Global eindeutiger Name | Der Name, der Ihre neue Funktionen-App bezeichnet Gültige Zeichen sind `a-z` (Groß-/Kleinschreibung nicht beachtet), `0-9` und `-`.  |
    |**Veröffentlichen**| Code | Option zum Veröffentlichen von Codedateien oder eines Docker-Containers. |
    | **Laufzeitstapel** | Bevorzugte Sprache | Wählen Sie die PowerShell Core aus. |
    |**Version**| Versionsnummer | Wählen Sie die Version der installierten Runtime aus.  |
    |**Region**| Bevorzugte Region | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-basics.png" alt-text="Erstellen einer Funktions-App: Grundlagen." border="true":::

1. Wählen Sie **Weiter: Hosting** aus. Geben Sie auf der Seite **Hosting** die folgenden Einstellungen ein.

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **[Speicherkonto](../storage/common/storage-account-create.md)** |  Global eindeutiger Name |  Erstellen Sie ein Speicherkonto, das von Ihrer Funktions-App verwendet wird. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Sie können auch ein vorhandenes Konto verwenden, das die [Anforderungen an das Speicherkonto](../azure-functions/storage-considerations.md#storage-account-requirements) erfüllen muss. |
    |**Betriebssystem**| Bevorzugtes Betriebssystem | Ein Betriebssystem ist für Sie basierend auf Ihrer Runtimestapelauswahl vorab ausgewählt, aber Sie können die Einstellung ggf. ändern. |
    | **[Plantyp](../azure-functions/functions-scale.md)** | **App Service-Plan** | Wählen Sie **App Service-Plan** aus. Bei der Ausführung in einem App Service-Plan müssen Sie die [Skalierung Ihrer Funktions-App](../azure-functions/functions-scale.md) verwalten.  |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-hosting.png" alt-text="Erstellen einer Funktions-App: Hosting." border="true":::

1. Wählen Sie **Weiter: Überwachung** aus. Geben Sie auf der Seite **Überwachung** die folgenden Einstellungen ein.

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../azure-functions/functions-monitoring.md)** | Standard | Erstellt eine Application Insights-Ressource mit dem gleichen *App-Namen* in der nächstgelegenen unterstützten Region. Durch Erweitern dieser Einstellung oder Auswählen von **Neu erstellen** können Sie den Application Insights-Namen ändern oder eine andere Region in einer [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) auswählen, in der Sie Ihre Daten speichern möchten. |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-monitoring.png" alt-text="Erstellen einer Funktions-App: Überwachung." border="true":::

1. Wählen Sie **Bewerten + erstellen** aus, um die App-Konfigurationsauswahl zu überprüfen.

1. Überprüfen Sie auf der Seite **Bewerten + erstellen** Ihre Einstellungen, und wählen Sie dann **Erstellen** aus, um die Funktions-App bereitzustellen.

1. Wählen Sie oben rechts im Portal das **Benachrichtigungssymbol** aus, und achten Sie auf die Meldung **Bereitstellung erfolgreich**.

1. Wählen Sie **Zu Ressource wechseln**, um Ihre neue Funktionen-App anzuzeigen. Sie können auch die Option **An Dashboard anheften** auswählen. Wenn Sie die Funktions-App anheften, können Sie einfacher über das Dashboard auf sie zugreifen.

## <a name="create-a-hybrid-connection-for-the-function-app"></a>Erstellen einer Hybridverbindung für die Funktions-App

Hybridverbindungen werden im Abschnitt „Netzwerk“ der Funktions-App konfiguriert:

1. Wählen Sie unter **Einstellungen** in der soeben erstellten Funktions-App **Netzwerk** aus. 
1. Wählen Sie **Endpunkte der Hybridverbindung konfigurieren** aus.
   
    :::image type="content" source="./media/functions-hybrid-powershell/configure-hybrid-connection-endpoint.png" alt-text="Konfigurieren der Endpunkte der Hybridverbindung" border="true":::.

1. Wählen Sie **Hybridverbindung hinzufügen** aus.
   
    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-connection-overview.png" alt-text="Hinzufügen einer Hybridverbindung." border="true":::

1. Geben Sie Informationen zu der Hybridverbindung ein, wie nach dem Screenshot angegeben. Sie haben die Möglichkeit, die Einstellung **Endpunkthost** so festzulegen, dass sie mit dem Hostnamen des lokalen Servers übereinstimmt, damit Sie sich den Server später beim Ausführen von Remotebefehlen leichter merken können. Der Port stimmt mit dem Standardport für den Windows-Remoteverwaltungsdienst überein, der zuvor auf dem Server definiert wurde.
  
      :::image type="content" source="./media/functions-hybrid-powershell/add-hybrid-connection.png" alt-text="Hinzufügen einer Hybridverbindung." border="true":::

    | Einstellung      | Vorgeschlagener Wert  |
    | ------------ | ---------------- |
    | **Hybridverbindungsname** | ContosoHybridOnPremisesServer |
    | **Endpunkthost** | finance1 |
    | **Endpunktport** | 5986 |
    | **Service Bus-Namespace** | Create New |
    | **Location** | Wählen Sie einen verfügbaren Standort aus. |
    | **Name** | contosopowershellhybrid | 

1. Wählen Sie **OK** aus, um die Hybridverbindung zu erstellen.

## <a name="download-and-install-the-hybrid-connection"></a>Herunterladen und Installieren der Hybridverbindung

1. Wählen Sie **Verbindungs-Manager herunterladen** aus, um die *MSI*-Datei lokal auf Ihrem Computer zu speichern.

    :::image type="content" source="./media/functions-hybrid-powershell/download-hybrid-connection-installer.png" alt-text="Herunterladen des Installationsprogramms." border="true":::

1. Kopieren Sie die *MSI*-Datei von Ihrem lokalen Computer auf den lokalen Server.
1. Führen Sie den Installer des Hybridverbindungs-Managers aus, um den Dienst auf dem lokalen Server zu installieren.

    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-installation.png" alt-text="Installieren der Hybridverbindung." border="true":::

1. Öffnen Sie im Portal die Hybridverbindung, und kopieren Sie die Gatewayverbindungszeichenfolge in die Zwischenablage.

    :::image type="content" source="./media/functions-hybrid-powershell/copy-hybrid-connection.png" alt-text="Kopieren der Hybridverbindungszeichenfolge." border="true":::

1. Öffnen Sie die Benutzeroberfläche des Hybrid Connection-Managers auf dem lokalen Server.

    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-connection-ui.png" alt-text="Öffnen der Benutzeroberfläche der Hybridverbindung." border="true":::

1. Wählen Sie **Manuell eingeben** aus, und fügen Sie die Verbindungszeichenfolge aus der Zwischenablage ein.

    :::image type="content" source="./media/functions-hybrid-powershell/enter-manual-connection.png" alt-text="Einfügen der Hybridverbindung." border="true":::

1. Wenn der Hybridverbindungs-Manager nicht als verbunden angezeigt wird, starten Sie ihn mit PowerShell neu.
    ```powershell
    Restart-Service HybridConnectionManager
    ```

## <a name="create-an-app-setting-for-the-password-of-an-administrator-account"></a>Erstellen einer App-Einstellung für das Kennwort eines Administratorkontos

1. Wählen Sie unter **Einstellungen** für ihre Funktions-App **Konfiguration** aus. 
1. Wählen Sie **+ Neue Anwendungseinstellung** aus.

    :::image type="content" source="./media/functions-hybrid-powershell/select-configuration.png" alt-text="Konfigurieren eines Kennworts für das Administratorkonto." border="true":::

1. Geben Sie der Einstellung den Namen **ContosoUserPassword**, und geben Sie das Kennwort ein. Klicken Sie auf **OK**.
1. Wählen Sie **Speichern**  aus, um das Kennwort in der Funktionsanwendung zu speichern.

    :::image type="content" source="./media/functions-hybrid-powershell/save-administrator-password.png" alt-text="Speichern des Kennworts für das Administratorkonto." border="true":::

## <a name="create-a-function-http-trigger"></a>Erstellen eines HTTP-Funktionstriggers

1. Wählen Sie in Ihrer Funktions-App **Funktionen** und dann **+ Hinzufügen** aus.

    :::image type="content" source="./media/functions-hybrid-powershell/create-http-trigger-function.png" alt-text="Erstellen eines neuen HTTP-Triggers." border="true":::

1. Wählen Sie die Vorlage **HTTP-Trigger** aus.

    :::image type="content" source="./media/functions-hybrid-powershell/select-http-trigger-template.png" alt-text="Auswählen der Vorlage „HTTP-Trigger“." border="true":::

1. Benennen Sie die neue Funktion, und wählen Sie dann **Funktion erstellen** aus.

    :::image type="content" source="./media/functions-hybrid-powershell/create-new-http-function.png" alt-text="Benennen und Erstellen der neuen HTTP-Triggerfunktion." border="true":::

## <a name="test-the-function"></a>Testen der Funktion

1. Wählen Sie in der neuen Funktion **Programmieren und testen** aus. Ersetzen Sie den PowerShell-Code aus der Vorlage durch den folgenden Code:

    ```powershell
    # Input bindings are passed in via param block.
    param($Request, $TriggerMetadata)
    
    # Write to the Azure Functions log stream.
    Write-Output "PowerShell HTTP trigger function processed a request."
    
    # Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword.
    $UserName = "ContosoUser"
    $securedPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
    $Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)
    
    # This is the name of the hybrid connection Endpoint.
    $HybridEndpoint = "finance1"
    
    $Script = {
        Param(
            [Parameter(Mandatory=$True)]
            [String] $Service
        )
        Get-Service $Service
    }
    
    Write-Output "Scenario 1: Running command via Invoke-Command"
    Invoke-Command -ComputerName $HybridEndpoint `
                   -Credential $Credential `
                   -Port 5986 `
                   -UseSSL `
                   -ScriptBlock $Script `
                   -ArgumentList "*" `
                   -SessionOption (New-PSSessionOption -SkipCACheck)
    ```

1. Wählen Sie **Speichern** aus.

    :::image type="content" source="./media/functions-hybrid-powershell/save-http-function.png" alt-text="Ändern des PowerShell-Codes und Speichern der HTTP-Triggerfunktion." border="true":::

 1. Wählen Sie **Testen** aus, und wählen Sie dann **Ausführen** aus, um die Funktion zu testen. Überprüfen Sie die Protokolle, um sicherzustellen, dass der Test erfolgreich war.

     :::image type="content" source="./media/functions-hybrid-powershell/test-function-hybrid.png" alt-text="Testen der HTTP-Triggerfunktion." border="true":::

## <a name="managing-other-systems-on-premises"></a>Lokale Verwaltung anderer Systeme

Sie können den verbundenen lokalen Server verwenden, um eine Verbindung mit anderen Servern und Verwaltungssystemen in der lokalen Umgebung herzustellen. Auf diese Weise können Sie Ihre Rechenzentrumsvorgänge in Azure mithilfe von PowerShell-Funktionen verwalten. Mit dem folgenden Skript wird eine PowerShell-Konfigurationssitzung registriert, die unter den angegebenen Anmeldeinformationen ausgeführt wird. Dabei muss es sich um die Anmeldeinformationen eines Administrators für die Remoteserver handeln. Anschließend können Sie diese Konfiguration verwenden, um auf andere Endpunkte auf dem lokalen Server oder im Rechenzentrum zuzugreifen.

```powershell
# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Write to the Azure Functions log stream.
Write-Host "PowerShell HTTP trigger function processed a request."

# Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword.
$UserName = "ContosoUser"
$SecuredPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
$Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)

# This is the name of the hybrid connection Endpoint.
$HybridEndpoint = "finance1"

# The remote server that will be connected to run remote PowerShell commands on
$RemoteServer = "finance2".

Write-Output "Use hybrid connection server as a jump box to connect to a remote machine"

# We are registering an endpoint that runs under credentials ($Credential) that has access to the remote server.
$SessionName = "HybridSession"
$ScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $SessionName)

    if (-not (Get-PSSessionConfiguration -Name $SessionName -ErrorAction SilentlyContinue))
    {
        Register-PSSessionConfiguration -Name $SessionName -RunAsCredential $Using:Credential
    }
}

Write-Output "Registering session on hybrid connection jumpbox"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $ScriptCommand `
               -ArgumentList $SessionName `
               -SessionOption (New-PSSessionOption -SkipCACheck)

# Script to run on the jump box to run against the second machine.
$RemoteScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $ComputerName)
        # Write out the hostname of the hybrid connection server.
        hostname
        # Write out the hostname of the remote server.
        Invoke-Command -ComputerName $ComputerName -Credential $Using:Credential -ScriptBlock {hostname} `
                        -UseSSL -Port 5986 -SessionOption (New-PSSessionOption -SkipCACheck)
}

Write-Output "Running command against remote machine via jumpbox by connecting to the PowerShell configuration session"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $RemoteScriptCommand `
               -ArgumentList $RemoteServer `
               -SessionOption (New-PSSessionOption -SkipCACheck) `
               -ConfigurationName $SessionName
```

Ersetzen Sie die folgenden Variablen in diesem Skript durch die entsprechenden Werte in Ihrer Umgebung:
* $HybridEndpoint
* $RemoteServer

In den beiden vorherigen Szenarien können Sie mithilfe von PowerShell in Azure Functions und Hybrid Connections eine Verbindung mit lokalen Umgebungen herstellen und diese verwalten. Weitere Informationen finden Sie unter [Hybrid Connections](../app-service/app-service-hybrid-connections.md) und [PowerShell in Functions](./functions-reference-powershell.md).

Sie können auch [virtuelle Netzwerke](./functions-create-vnet.md) in Azure verwenden, um über Azure Functions eine Verbindung mit der lokalen Umgebung herzustellen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Weitere Informationen zum Arbeiten mit PowerShell-Funktionen](functions-reference-powershell.md)

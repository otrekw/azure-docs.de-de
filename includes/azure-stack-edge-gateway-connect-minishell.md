---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/30/2021
ms.author: alkohli
ms.openlocfilehash: 89e648099a5ac6d905f475319cc108dd0d05a6e9
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106081084"
---
Je nach Betriebssystem des Clients unterscheiden sich die Verfahren für Remoteverbindungen mit dem Gerät.

### <a name="remotely-connect-from-a-windows-client"></a>Herstellen einer Remoteverbindung von einem Windows-Client


#### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

- Auf dem Windows-Client wird Windows PowerShell 5.0 oder höher ausgeführt.
- Der Windows-Client verfügt über die Signaturkette (Stammzertifikat), die dem auf dem Gerät installierten Knotenzertifikat entspricht. Ausführliche Anweisungen finden Sie unter [Installieren von Zertifikaten auf Ihrem Windows-Client](../articles/databox-online/azure-stack-edge-gpu-manage-certificates.md#import-certificates-on-the-client-accessing-the-device).
- Die `hosts`-Datei unter `C:\Windows\System32\drivers\etc` für den Windows-Client enthält einen Eintrag für das Knotenzertifikat im folgenden Format:

    `<Device IP>    <Node serial number>.<DNS domain of the device>`

    Dies ist ein Beispieleintrag in der Datei `hosts`:
 
    `10.100.10.10    1HXQG13.wdshcsso.com`
  

#### <a name="detailed-steps"></a>Ausführliche Schritte

Befolgen Sie die folgenden Schritte, um eine Remoteverbindung von einem Windows-Client aus herzustellen.

1. Führen Sie eine Windows PowerShell-Sitzung als Administrator aus.
2. Stellen Sie sicher, dass der Dienst Windows-Remoteverwaltung auf dem Client ausgeführt wird. Geben Sie an der Eingabeaufforderung Folgendes ein:

    `winrm quickconfig`

    Weitere Informationen finden Sie unter [Installation und Konfiguration für die Windows-Remoteverwaltung](/windows/win32/winrm/installation-and-configuration-for-windows-remote-management#quick-default-configuration).

3. Weisen Sie der IP-Adresse des Geräts eine Variable zu.

    $ip = "<device_ip>"

    Ersetzen Sie `<device_ip>` durch die IP-Adresse des Geräts.

4. Geben Sie den folgenden Befehl ein, um der Liste der vertrauenswürdigen Hosts des Clients die IP-Adresse Ihres Geräts hinzuzufügen:

    `Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force`

5. Starten Sie eine Windows PowerShell-Sitzung auf dem Gerät:

    `Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell -UseSSL`

    Wenn ein Fehler im Zusammenhang mit der Vertrauensstellung auftritt, überprüfen Sie, ob die Signaturkette des Knotenzertifikats, das auf Ihr Gerät hochgeladen wurde, auch auf dem Client installiert ist, der auf Ihr Gerät zugreift.

    Wenn Sie die Zertifikate nicht verwenden (es wird empfohlen, dass Sie die Zertifikate verwenden!), können Sie diese Überprüfung überspringen, indem Sie die Sitzungsoptionen verwenden: `-SkipCACheck -SkipCNCheck -SkipRevocationCheck`.

    ```powershell
    $sessOptions = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck 
    Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell -UseSSL -SessionOption $sessOptions    
    ```

    > [!NOTE] 
    > Wenn Sie die Option `-UseSSL` verwenden, führen Sie Remoting über PowerShell per *HTTPS* aus. Sie sollten immer *HTTPS* für Remoteverbindungen über PowerShell verwenden. 

6. Geben Sie das Kennwort an, wenn Sie dazu aufgefordert werden. Verwenden Sie dasselbe Kennwort wie für die Anmeldung bei der lokalen Webbenutzeroberfläche. Das Standardkennwort für die lokale Webbenutzeroberfläche lautet *Password1*. Wenn Sie mithilfe von PowerShell eine Verbindung mit dem Gerät herstellen konnten, wird die folgende Beispielausgabe angezeigt:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $ip = "10.100.10.10"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell -UseSSL

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [10.100.10.10]: PS>
    ```

> [!IMPORTANT]
> In der aktuellen Version können Sie eine Verbindung mit der PowerShell-Schnittstelle des Geräts nur über einen Windows-Client herstellen. Die `-UseSSL` Option funktioniert nicht mit den Linux-Clients.

<!--### Remotely connect from a Linux client-->

<!--On the Linux client that you'll use to connect:

- [Install the latest PowerShell Core for Linux](/powershell/scripting/install/installing-powershell-core-on-linux) from GitHub to get the SSH remoting feature. 
- [Install only the `gss-ntlmssp` package from the NTLM module](https://github.com/Microsoft/omi/blob/master/Unix/doc/setup-ntlm-omi.md). For Ubuntu clients, use the following command:
    - `sudo apt-get install gss-ntlmssp`

For more information, go to [PowerShell remoting over SSH](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core).

Follow these steps to remotely connect from an NFS client.

1. To open PowerShell session, type:

    `pwsh`
 
2. For connecting using the remote client, type:

    `Enter-PSSession -ComputerName $ip -Authentication Negotiate -ConfigurationName Minishell -Credential ~\EdgeUser`

    When prompted, provide the password used to sign into your device.
 
> [!NOTE]
> This procedure does not work on Mac OS.-->
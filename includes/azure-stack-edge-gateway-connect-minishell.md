---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/15/2021
ms.author: alkohli
ms.openlocfilehash: 6f732f7c59f76c99d82d5c4f7b062532c60a5a78
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110164026"
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

    ```powershell
    winrm quickconfig
    ```

    Weitere Informationen finden Sie unter [Installation und Konfiguration für die Windows-Remoteverwaltung](/windows/win32/winrm/installation-and-configuration-for-windows-remote-management#quick-default-configuration).

3. Weisen Sie der in der Datei `hosts` verwendeten Verbindungszeichenfolge eine Variable zu.

    ```powershell
    $Name = "<Node serial number>.<DNS domain of the device>"
    ``` 

    Ersetzen Sie `<Node serial number>` und `<DNS domain of the device>` durch die Seriennummer des Knotens und die DNS-Domäne Ihres Geräts. Sie können die Werte für die Seriennummer des Knotens von der Seite **Zertifikate** und die DNS-Domäne von der Seite **Gerät** in der lokalen Webbenutzeroberfläche Ihres Geräts abrufen.

4. Geben Sie den folgenden Befehl ein, um der Liste der vertrauenswürdigen Hosts des Clients die Verbindungszeichenfolge für Ihr Gerät hinzuzufügen:

    ```powershell
    Set-Item WSMan:\localhost\Client\TrustedHosts $Name -Concatenate -Force
    ```

5. Starten Sie eine Windows PowerShell-Sitzung auf dem Gerät:

    ```powershell
    Enter-PSSession -ComputerName $Name -Credential ~\EdgeUser -ConfigurationName Minishell -UseSSL
    ```

    Wenn ein Fehler im Zusammenhang mit der Vertrauensstellung auftritt, überprüfen Sie, ob die Signaturkette des Knotenzertifikats, das auf Ihr Gerät hochgeladen wurde, auch auf dem Client installiert ist, der auf Ihr Gerät zugreift.

6. Geben Sie das Kennwort an, wenn Sie dazu aufgefordert werden. Verwenden Sie dasselbe Kennwort wie für die Anmeldung bei der lokalen Webbenutzeroberfläche. Das Standardkennwort für die lokale Webbenutzeroberfläche lautet *Password1*. Wenn Sie mithilfe von PowerShell eine Verbindung mit dem Gerät herstellen konnten, wird die folgende Beispielausgabe angezeigt:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $Name = "1HXQG13.wdshcsso.com"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $Name -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $Name -Credential ~\EdgeUser -ConfigurationName Minishell -UseSSL

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [1HXQG13.wdshcsso.com]: PS>
    ```

Wenn Sie die Option `-UseSSL` verwenden, führen Sie Remoting über PowerShell per *HTTPS* aus. Sie sollten immer *HTTPS* für Remoteverbindungen über PowerShell verwenden. In vertrauenswürdigen Netzwerken sind PowerShell-Remotesitzungen über HTTP zulässig. Sie müssen PowerShell-Remotesitzungen über HTTP jedoch zunächst über die lokale Benutzeroberfläche aktivieren. Dann können Sie eine Verbindung mit der PowerShell-Schnittstelle des Geräts herstellen, indem Sie die zuvor beschriebene Prozedur ohne die Option `-UseSSL` befolgen.

Wenn Sie die Zertifikate nicht verwenden (nicht empfohlen), können Sie die Zertifikatvalidierung überspringen, indem Sie die Sitzungsoptionen verwenden: `-SkipCACheck -SkipCNCheck -SkipRevocationCheck`.

```powershell
$sessOptions = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck 
Enter-PSSession -ComputerName $Name -Credential ~\EdgeUser -ConfigurationName Minishell -UseSSL -SessionOption $sessOptions    
```
Hier sehen Sie eine Beispielausgabe, wenn die Zertifikatvalidierung übersprungen wird:

```powershell
PS C:\WINDOWS\system32> $Name = "1HXQG13.wdshcsso.com"
PS C:\WINDOWS\system32> $sessOptions = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
PS C:\WINDOWS\system32> $sessOptions

MaximumConnectionRedirectionCount : 5
NoCompression                     : False
NoMachineProfile                  : False
ProxyAccessType                   : None
ProxyAuthentication               : Negotiate
ProxyCredential                   :
SkipCACheck                       : True
SkipCNCheck                       : True
SkipRevocationCheck               : True
OperationTimeout                  : 00:03:00
NoEncryption                      : False
UseUTF16                          : False
IncludePortInSPN                  : False
OutputBufferingMode               : None
MaxConnectionRetryCount           : 0
Culture                           :
UICulture                         :
MaximumReceivedDataSizePerCommand :
MaximumReceivedObjectSize         :
ApplicationArguments              :
OpenTimeout                       : 00:03:00
CancelTimeout                     : 00:01:00
IdleTimeout                       : -00:00:00.0010000

PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $Name -Credential ~\EdgeUser -ConfigurationName Minishell -UseSSL -SessionOption $sessOptions
WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please
engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing.
Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
[1HXQG13.wdshcsso.com]: PS>
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
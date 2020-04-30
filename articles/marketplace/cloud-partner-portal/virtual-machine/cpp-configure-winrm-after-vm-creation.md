---
title: Konfigurieren von WinRM nach Erstellung eines virtuellen Azure-Computers | Azure Marketplace
description: Erläutert, wie Windows-Remoteverwaltung (WinRM) zu konfigurieren ist, nachdem ein in Azure gehosteter virtueller Computer erstellt wurde.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: dsindona
ms.openlocfilehash: 3a67371ce6f951a9e446ab639ea5b59248b79565
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82144140"
---
# <a name="configure-winrm-after-virtual-machine-creation"></a>Konfigurieren von WinRM nach Erstellung eines virtuellen Computers

> [!IMPORTANT]
> Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer Azure Virtual Machines-Angebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote im Partner Center. Folgen Sie den Anweisungen unter [Erstellen eines Azure Virtual Machines-Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer), um Ihre migrierten Angebote zu verwalten.

In diesem Artikel wird erläutert, wie ein vorhandener in Azure gehosteter virtueller Computer konfiguriert wird, um WinRM über HTTPS zu aktivieren.  Diese Konfiguration gilt nur für Windows-basierte virtuelle Computer und erfordert die beiden folgenden Schritte:

1. Aktivieren des Portdatenverkehrs für das WinRM-über-HTTPS-Protokoll.  Sie konfigurieren diese Einstellung für den virtuellen Computer im Azure-Portal.
2. Konfigurieren Sie den virtuellen Computer, um WinRM zu aktivieren. Führen Sie dazu das bereitgestellte PowerShell-Skript aus.


## <a name="enabling-port-traffic"></a>Aktivieren des Portsatenverkehrs

Für das WinRM-über-HTTPS-Protokoll wird Port 5986 verwendet. Dieser Port ist auf den im Azure Marketplace angebotenen vorkonfigurierten virtuellen Windows-Computern standardmäßig nicht aktiviert. Um dieses Protokoll zu aktivieren, führen Sie die folgenden Schritte aus, um der Netzwerksicherheitsgruppe (NSG) über das [Azure-Portal](https://portal.azure.com) eine neue Regel hinzuzufügen.  Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Sicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview).

1. Navigieren Sie zu dem Blatt **Virtuelle Computer >**   <*Name des virtuellen Computers*>   **> Einstellungen/Netzwerk**.
2. Klicken Sie auf den NSG-Namen (in diesem Beispiel **testvm11002**), um die zugehörigen Eigenschaften anzuzeigen:

    ![Eigenschaften einer Netzwerksicherheitsgruppe](./media/nsg-properties.png)
 
3. Wählen Sie unter **Einstellungen** die Option **Eingangssicherheitsregeln** aus, damit dieses Blatt angezeigt wird.
4. Klicken Sie auf **+ Hinzufügen**, um eine neue Regel namens `WinRM_HTTPS` für den TCP-Port 5986 zu erstellen.

    ![Eingangssicherheitsregel für Netzwerk hinzufügen](./media/nsg-new-rule.png)

5. Klicken Sie auf **OK**, wenn Sie die entsprechenden Werte eingegeben haben.  Die Liste der Eingangssicherheitsregeln sollte die folgenden neuen Einträge enthalten.

    ![Liste der Eingangssicherheitsregeln für das Netzwerk](./media/nsg-new-inbound-listing.png)


## <a name="configure-vm-to-enable-winrm"></a>Konfigurieren des virtuellen Computers, um WinRM zu aktivieren 

Gehen Sie wie folgt vor, um die Windows-Remoteverwaltung auf Ihrem virtuellen Windows-Computer zu aktivieren und zu konfigurieren.   

1. Stellen Sie eine Remotedesktopverbindung mit Ihrem in Azure gehosteten virtuellen Computer her.  Weitere Informationen hierzu finden Sie unter [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).  Die restlichen Schritte werden auf Ihrem virtuellen Computer ausgeführt.
2. Laden Sie die folgenden Dateien herunter, und speichern Sie diese in einem Ordner auf Ihrem virtuellen Computer:
    - [ConfigureWinRM.ps1](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1)
    - [makecert.exe](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe)
    - [winrmconf.cmd](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd)
3. Öffnen Sie die **PowerShell-Konsole** mit erhöhten Rechten (**Als Administrator ausführen**). 
4. Führen Sie den folgenden Befehl aus, wozu Sie den erforderlichen Parameter angeben: der vollqualifizierte Domänenname (FQDN) für Ihren virtuellen Computer: <br/>
   `ConfigureWinRM.ps1 <vm-domain-name>`

    ![PowerShell-Konfigurationsskript 1](./media/powershell-file1.png)

    Dieses Skript erfordert, dass sich die beiden anderen Dateien im selben Ordner befinden.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie WinRM konfiguriert haben, können Sie [Ihren virtuellen Computer auf Basis seiner zugehörigen VHDs bereitstellen](./cpp-deploy-vm-vhd.md).

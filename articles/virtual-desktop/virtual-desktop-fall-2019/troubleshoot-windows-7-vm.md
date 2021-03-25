---
title: Virtuelle Windows 7-Computer in Windows Virtual Desktop (klassisch) – Azure
description: Vorgehensweise beim Beheben von Problemen mit virtuellen Windows 7-Computern (VMs) in einer Windows Virtual Desktop-Umgebung (klassisch).
author: Heidilohr
ms.topic: troubleshooting
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 1d71b4472b16372927db56b20eca4fcbde0cc625
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88005413"
---
# <a name="troubleshoot-windows-7-virtual-machines-in-windows-virtual-desktop-classic"></a>Problembehandlung für virtuelle Windows 7-Computer in Windows Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop (klassisch). Der Dienst unterstützt keine Windows Virtual Desktop-Objekte in Azure Resource Manager.

Verwenden Sie diesen Artikel, um Probleme zu behandeln, die bei der Konfiguration von virtuellen Computern (VMs) der Windows Virtual Desktop-Sitzung auftreten.

## <a name="known-issues"></a>Bekannte Probleme

Windows 7 unter Windows Virtual Desktop bietet keine Unterstützung für die folgenden Features:

- Virtualisierte Anwendungen (RemoteApps)
- Zeitzonenumleitung
- Automatische DPI-Skalierung

Windows Virtual Desktop kann nur vollständige Desktops für Windows 7 virtualisieren.

Auch wenn automatische DPI-Skalierung nicht unterstützt wird, können Sie die Auflösung auf dem virtuellen Computer manuell ändern, indem Sie mit der rechten Maustaste auf das Symbol im Remote Desktop-Client klicken und **Auflösung** auswählen.

## <a name="error-cant-access-the-remote-desktop-user-group"></a>Error: Auf die Remote Desktop-Benutzergruppe kann nicht zugegriffen werden

Wenn Windows Virtual Desktop Sie oder die Anmeldeinformationen Ihrer Benutzer in der Remote Desktop-Benutzergruppe nicht finden kann, wird möglicherweise eine der folgenden Fehlermeldungen angezeigt:

- „Dieser Benutzer ist kein Mitglied der Remote Desktop-Benutzergruppe“
- „Ihnen müssen Berechtigungen zum Anmelden über Remote Desktop-Dienste erteilt werden“

Fügen Sie den Benutzer der Remote Desktop Benutzergruppe hinzu, um diesen Fehler zu beheben:

1. Öffnen Sie das Azure-Portal.
2. Wählen Sie den virtuellen Computer aus, für den die Fehlermeldung angezeigt wurde.
3. Wählen Sie **Befehl ausführen** aus.
4. Führen Sie den folgenden Befehl aus, wobei `<username>` durch den Namen des Benutzers ersetzt wird, den Sie hinzufügen möchten:

   ```cmd
   net localgroup "Remote Desktop Users" <username> /add
   ```
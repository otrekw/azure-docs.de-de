---
title: Microsoft Teams in Windows Virtual Desktop – Azure
description: Hier erfahren Sie, wie Sie Microsoft Teams in Windows Virtual Desktop verwenden.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a860b005457c6e02187423a3ffbbc63fe7c758b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187527"
---
# <a name="use-microsoft-teams-on-windows-virtual-desktop"></a>Verwenden von Microsoft Teams in Windows Virtual Desktop

> Gilt für: Windows 10 und Windows 10 IoT Enterprise

Bei virtualisierten Umgebungen gilt es im Zusammenhang mit Apps für die Zusammenarbeit wie Microsoft Teams viele Herausforderungen zu meistern. Dazu gehören beispielsweise die erhöhte Latenz, die hohe Host-CPU-Auslastung und unzureichende Audio- und Videoleistungen. Weitere Informationen zur Verwendung von Microsoft Teams in VDI-Umgebungen finden Sie unter [Teams für virtualisierte Desktopinfrastrukturen](https://docs.microsoft.com/microsoftteams/teams-for-vdi).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Microsoft Teams in Windows Virtual Desktop verwenden können, müssen Sie Folgendes tun:

- Installieren Sie den [Windows Desktop-Client](connect-windows-7-and-10.md) auf einem Windows 10-Gerät, das die [Hardwareanforderungen](https://docs.microsoft.com/microsoftteams/hardware-requirements-for-the-teams-app) von Microsoft Teams erfüllt.
- Stellen Sie eine Verbindung mit einer Windows 10-VM mit mehreren Sitzungen oder einer virtuellen Windows 10 Enterprise-VM her.
- [Bereiten Sie Ihr Netzwerk](https://docs.microsoft.com/microsoftteams/prepare-network) für Microsoft Teams vor.

## <a name="use-unoptimized-microsoft-teams"></a>Verwenden der nicht optimierten Microsoft Teams-Version

Sie können Microsoft Teams in Ihrer Windows Virtual Desktop-Umgebung verwenden, um die Chat- und Zusammenarbeitsfeatures von Microsoft Teams zu nutzen. Windows Virtual Desktop unterstützt Teams für VDI-Audio-/Videooptimierungen nicht. Anrufe und Besprechungen werden nicht unterstützt. Wenn die Richtlinien Ihrer Organisation dies zulassen, können Sie trotzdem Audioanrufe durchführen und an Besprechungen mit Audiodaten teilnehmen. Die nicht optimierte Audioqualität in den Anrufen variiert jedoch je nach Hostkonfiguration und ist möglicherweise nicht zuverlässig. Weitere Informationen finden Sie unter [Teams und VDI-Leistungsüberlegungen](https://docs.microsoft.com/microsoftteams/teams-for-vdi#teams-on-vdi-performance-considerations).

### <a name="prepare-your-image-for-teams"></a>Vorbereiten des Images für Teams

Um die computerspezifische Installation von Teams zu aktivieren, legen Sie den folgenden Registrierungsschlüssel auf dem Host fest:

```shell
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Teams\IsWVDEnvironment]
  Type: REG_DWORD
  Value: 1
```

### <a name="install-microsoft-teams"></a>Installieren von Microsoft Teams

Sie können die Teams-Desktop-App mit einer computerspezifischen Installation bereitstellen. So installieren Sie Microsoft Teams in Ihrer Windows Virtual Desktop-Umgebung:

1. Laden Sie das [Teams-MSI-Paket](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm) herunter, das Ihrer Umgebung entspricht. Es wird empfohlen, das 64-Bit-Installationsprogramm auf einem 64-Bit-Betriebssystem zu verwenden.
2. Führen Sie diesen Befehl aus, um das MSI-Paket auf der Host-VM zu installieren.

      ```shell
      msiexec /i <msi_name> /l*v < install_logfile_name> ALLUSER=1
      ```

      Dadurch wird Teams entweder in Programmdateien oder in Programmdateien (x86) installiert. Wenn Sie sich das nächste Mal anmelden und Teams starten, werden Sie von der App aufgefordert, Ihre Anmeldeinformationen einzugeben.

      > [!NOTE]
      > Benutzer und Administratoren können den automatischen Start für Teams während der Anmeldung zu diesem Zeitpunkt nicht deaktivieren.

      Führen Sie den folgenden Befehl aus, um MSI auf der Host-VM zu deinstallieren:

      ```shell
      msiexec /passive /x <msi_name> /l*v <uninstall_logfile_name>
      ```

      > [!NOTE]
      > Wenn Sie Teams mit der MSI-Einstellung „ALLUSER=1“ installieren, werden automatische Updates deaktiviert. Es wird empfohlen, Teams mindestens einmal im Monat zu aktualisieren. Weitere Informationen zum Bereitstellen der Teams-Desktop-App finden Sie unter [Bereitstellen der Teams-Desktop-App auf der VM](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm).

## <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Anpassen der Remotedesktopprotokoll-Eigenschaften für einen Hostpool
Durch Anpassen der RDP-Eigenschaften (Remotedesktopprotokoll) eines Hostpools, z. B. Unterstützung mehrerer Monitore, Aktivieren des Mikrofons und Audioumleitung, können Sie für Benutzer eine optimale Bedienumgebung bereitstellen, die ihren Anforderungen entspricht. RDP-Eigenschaften können Sie in Windows Virtual Desktop über den **-CustomRdpProperty**-Parameter im **Set-RdsHostPool**-Cmdlet anpassen.
Eine vollständige Liste der unterstützten Eigenschaften samt deren Standardwerten finden Sie unter [Unterstützte RDP-Dateieinstellungen](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context).

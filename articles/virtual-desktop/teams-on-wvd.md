---
title: Microsoft Teams in Windows Virtual Desktop – Azure
description: Hier erfahren Sie, wie Sie Microsoft Teams in Windows Virtual Desktop verwenden.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/29/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3a14ffc9f103e58681418eacbb35b72b704f2d61
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267136"
---
# <a name="use-microsoft-teams-on-windows-virtual-desktop"></a>Verwenden von Microsoft Teams in Windows Virtual Desktop

>[!IMPORTANT]
>Die Medienoptimierung für Microsoft Teams befindet sich derzeit in der Phase der öffentlichen Vorschau. Sie sollten die optimierte Benutzerfreundlichkeit von Teams evaluieren, bevor Sie Teams für Produktionsworkloads bereitstellen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.

>[!NOTE]
>Die Medienoptimierung für Microsoft Teams steht nur für den Windows Desktop-Client auf Windows 10-Computern zur Verfügung. Medienoptimierungen erfordern Windows Desktop-Clientversion 1.2.1026.0 oder höher.

Microsoft Teams auf Windows Virtual Desktop unterstützt Chat und Zusammenarbeit. Mit Medienoptimierungen werden auch Anruf- und Besprechungsfunktionen unterstützt. Weitere Informationen zur Verwendung von Microsoft Teams in VDI-Umgebungen (Virtual Desktop Infrastructure, virtualisierte Desktopinfrastruktur) finden Sie unter [Teams für virtualisierte Desktopinfrastruktur](/microsoftteams/teams-for-vdi/).

Mit der Medienoptimierung für Microsoft Teams verarbeitet der Windows Desktop-Client Audio- und Videodaten lokal für Anrufe und Besprechungen von Teams. Sie können Microsoft Teams weiterhin auf Windows Virtual Desktop mit anderen Clients ohne Optimierung von Anrufen und Besprechungen verwenden. Die Chat- und Zusammenarbeitsfunktionen von Teams werden auf allen Plattformen unterstützt. Wenn Sie lokale Geräte in Ihre Remotesitzung umleiten möchten, lesen Sie [Anpassen der Remotedesktopprotokoll-Eigenschaften für einen Hostpool](#customize-remote-desktop-protocol-properties-for-a-host-pool).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Microsoft Teams in Windows Virtual Desktop verwenden können, müssen Sie Folgendes tun:

- [Bereiten Sie Ihr Netzwerk](/microsoftteams/prepare-network/) für Microsoft Teams vor.
- Installieren Sie den [Windows Desktop-Client](connect-windows-7-and-10.md) auf einem Windows 10-Gerät, das die [Hardwareanforderungen für Microsoft Teams auf einem Windows-PC](/microsoftteams/hardware-requirements-for-the-teams-app#hardware-requirements-for-teams-on-a-windows-pc/) erfüllt.
- Stellen Sie eine Verbindung mit einer Windows 10-VM mit mehreren Sitzungen oder einer virtuellen Windows 10 Enterprise-VM her.
- Sie können die Teams-Desktop-App mit einer computerspezifischen Installation auf dem Host installieren. Die Medienoptimierung für Microsoft Teams erfordert die Teams-Desktop-App-Version 1.3.00.4461 oder höher.

## <a name="install-the-teams-desktop-app"></a>Installieren der Teams-Desktop-App

In diesem Abschnitt erfahren Sie, wie Sie die Teams-Desktop-App auf Ihrem Windows 10-Multisession- oder Windows 10-Enterprise-VM-Image installieren. Weitere Informationen finden Sie unter [Installieren oder Aktualisieren der Desktop-App für Teams in VDI](/microsoftteams/teams-for-vdi#install-or-update-the-teams-desktop-app-on-vdi/).

### <a name="prepare-your-image-for-teams"></a>Vorbereiten des Images für Teams

Um die computerspezifische Installation von Teams zu aktivieren, legen Sie den folgenden Registrierungsschlüssel auf dem Host fest:

1. Führen Sie im Startmenü **RegEdit** als Administrator aus. Navigieren Sie zu **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Teams**.
2. Erstellen Sie den folgenden Wert für den Teams-Schlüssel:

| Name             | type   | Daten/Wert  |
|------------------|--------|-------------|
| IsWVDEnvironment | DWORD  | 1           |

### <a name="install-the-teams-websocket-service"></a>Installieren des Teams-WebSocket-Diensts

Installieren Sie den [WebSocket-Dienst](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4vkL6) auf Ihrem VM-Image. Wenn ein Installationsfehler auftritt, installieren Sie das [aktuelle Microsoft Visual C++ Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads), und versuchen Sie es erneut.

### <a name="install-microsoft-teams"></a>Installieren von Microsoft Teams

Sie können die Teams-Desktop-App mit einer computerspezifischen Installation bereitstellen. So installieren Sie Microsoft Teams in Ihrer Windows Virtual Desktop-Umgebung:

1. Laden Sie das [Teams-MSI-Paket](/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm/) herunter, das Ihrer Umgebung entspricht. Es wird empfohlen, das 64-Bit-Installationsprogramm auf einem 64-Bit-Betriebssystem zu verwenden.

      > [!NOTE]
      > Die Medienoptimierung für Microsoft Teams erfordert die Teams-Desktop-App-Version 1.3.00.4461 oder höher.

2. Führen Sie diesen Befehl aus, um das MSI-Paket auf der Host-VM zu installieren.

      ```console
      msiexec /i <path_to_msi> /l*v <install_logfile_name> ALLUSER=1 ALLUSERS=1
      ```

      Dadurch wird Teams auf einem 64-Bit-Betriebssystem im Ordner „Programme (x86)“ und auf einem 32-Bit-Betriebssystem im Ordner „Programme“ installiert. An diesem Punkt ist das Golden Image-Setup fertiggestellt. Die Installation von Teams pro Computer ist für nicht beständige Setups erforderlich.

      Wenn Sie Teams das nächste Mal in einer Sitzung öffnen, werden Sie aufgefordert, Ihre Anmeldeinformationen einzugeben.

      > [!NOTE]
      > Benutzer und Administratoren können den automatischen Start für Teams während der Anmeldung zu diesem Zeitpunkt nicht deaktivieren.

      Führen Sie den folgenden Befehl aus, um MSI auf der Host-VM zu deinstallieren:

      ```console
      msiexec /passive /x <msi_name> /l*v <uninstall_logfile_name>
      ```

      Hierdurch wird Teams je nach Betriebssystemumgebung aus dem Ordner „Programme (x86)“ oder „Programme“ deinstalliert.

      > [!NOTE]
      > Wenn Sie Teams mit der MSI-Einstellung „ALLUSER=1“ installieren, werden automatische Updates deaktiviert. Es wird empfohlen, Teams mindestens einmal im Monat zu aktualisieren. Weitere Informationen zum Bereitstellen der Teams-Desktop-App finden Sie unter [Bereitstellen der Teams-Desktop-App auf der VM](/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm/).

### <a name="verify-media-optimizations-loaded"></a>Überprüfen, ob Medienoptimierungen geladen wurden

Nachdem Sie den WebSocket-Dienst und die Teams-Desktop-App installiert haben, führen Sie die folgenden Schritte aus, um zu überprüfen, ob die Teams-Medienoptimierungen geladen wurden:

1. Wählen Sie Ihr Benutzerprofilbild und dann **Informationen zu** aus.
2. Wählen Sie **Version** aus.

      Wenn Medienoptimierungen geladen wurden, zeigt das Banner **WVD-Medien optimiert** an. Wenn das Banner **WVD-Medien nicht verbunden** anzeigt, beenden Sie die Teams-App, und versuchen Sie es erneut.

3. Wählen Sie Ihr Benutzerprofilbild und dann **Einstellungen** aus.

      Wenn Medienoptimierungen geladen wurden, werden die lokal verfügbaren Audiogeräte und Kameras im Gerätemenü aufgelistet. Wenn im Menü **Remoteaudio** angezeigt wird, beenden Sie die Teams-App, und versuchen Sie es erneut. Wenn die Geräte immer noch nicht im Menü angezeigt werden, wechseln Sie zurück zu [Microsoft Teams installieren](#install-microsoft-teams), und stellen Sie sicher, dass Sie den Installationsvorgang abgeschlossen haben.

## <a name="known-issues-and-limitations"></a>Einschränkungen und bekannte Probleme

Die Verwendung von Teams in einer virtualisierten Umgebung unterscheidet sich von der Verwendung von Teams in einer nicht virtualisierten Umgebung. Weitere Informationen zu den Einschränkungen von Teams in virtualisierten Umgebungen finden Sie unter [Bekannte Probleme und Einschränkungen](/microsoftteams/teams-for-vdi#known-issues-and-limitations/).

### <a name="client-deployment-installation-and-setup"></a>Clientbereitstellung, Installation und Setup

- Bei der Installation pro Computer wird Teams auf VDI nicht auf dieselbe Weise automatisch aktualisiert wie Nicht-VDI-Teams-Clients. Um den Client zu aktualisieren, müssen Sie das VM-Image aktualisieren, indem Sie eine neue MSI-Datei installieren.
- Teams zeigt derzeit nur die UTC-Zeitzone in Chat, Kanälen und Kalender an.
- Die Medienoptimierung für Teams wird nur für den Windows Desktop-Client auf Windows 10-Computern unterstützt.
- Die Verwendung expliziter, auf einem Endpunkt definierter HTTP-Proxys wird nicht unterstützt.

### <a name="calls-and-meetings"></a>Anrufe und Besprechungen

- Der Desktopclient für Teams in Windows Virtual Desktop-Umgebungen unterstützt keine Liveereignisse. Sie sollten derzeit stattdessen über den [Teams-Webclient](https://teams.microsoft.com) an Liveereignissen in Ihrer Remotesitzung teilnehmen.
- Das Minimieren der Teams-App während eines Anrufes oder einer Besprechung kann dazu führen, dass der eingehende Videofeed beim Erweitern der App verschwindet.
- Anrufe oder Besprechungen unterstützen derzeit keine Anwendungsfreigabe. Desktopsitzungen unterstützen die Desktopfreigabe.
- Wenn die Desktopfreigabe in einem Mehrfachmonitor-Setup erfolgt, werden alle Monitore freigegeben.
- Übergabe und Übernahme der Steuerung werden derzeit nicht unterstützt.
- Auf Windows Virtual Desktop unterstützt Teams nur jeweils eine eingehende Videoeingabe. Dies bedeutet, dass immer dann, wenn jemand versucht, seinen Bildschirm zu teilen, der Bildschirm desjenigen anstelle des Bildschirms des Besprechungsleiters angezeigt wird.
- Aufgrund von WebRTC-Einschränkungen ist die Auflösung eingehender und ausgehender Videostreams auf 720p beschränkt.
- Die Teams-App unterstützt keine HID-Schaltflächen oder LED-Steuerelemente in Verbindung mit anderen Geräten.

Informationen zu bekannten Problemen bei Teams, die sich nicht auf virtualisierte Umgebungen beziehen, finden Sie unter [Unterstützung von Microsoft Teams in Ihrer Organisation](/microsoftteams/known-issues/).

## <a name="feedback"></a>Feedback

Geben Sie auf der [UserVoice-Website](https://microsoftteams.uservoice.com/) von Teams Ihr Feedback zu Microsoft Teams auf Windows Virtual Desktop.

## <a name="collect-teams-logs"></a>Sammeln von Teams-Protokollen

Wenn bei der Teams-Desktop-App in Ihrer Windows Virtual Desktop-Umgebung Probleme auftreten, sammeln Sie Clientprotokolle auf der Host-VM unter **%appdata%\Microsoft\Teams\logs.txt**.

Wenn Probleme bei Anrufen und Besprechungen auftreten, sammeln Sie Teams-Webclientprotokolle mit der Tastenkombination **STRG** + **ALT** + **UMSCHALT** + **1**. Die Protokolle werden auf der Host-VM in **%userprofile%\Downloads\MSTeams Diagnostics Log DATE_TIME.txt** geschrieben.

## <a name="contact-microsoft-teams-support"></a>Kontaktieren des Microsoft Teams-Supports

Wenn Sie sich an den Microsoft Teams-Support wenden möchten, besuchen Sie das [Microsoft 365-Admin Center](https://docs.microsoft.com/microsoft-365/admin/contact-support-for-business-products?view=o365-worldwide&tabs=online).

## <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Anpassen der Remotedesktopprotokoll-Eigenschaften für einen Hostpool

Durch Anpassen der RDP-Eigenschaften (Remotedesktopprotokoll) eines Hostpools, z. B. Unterstützung mehrerer Monitore, Aktivieren des Mikrofons und Audioumleitung, können Sie für Benutzer eine optimale Bedienumgebung bereitstellen, die ihren Anforderungen entspricht.

Das Aktivieren von Geräteumleitungen ist nicht erforderlich, wenn Teams mit Medienoptimierung verwendet wird. Wenn Sie Teams ohne Medienoptimierung verwenden, legen Sie die folgenden RDP-Eigenschaften fest, um die Mikrofon- und Kameraumleitung zu aktivieren:

- `audiocapturemode:i:1` aktiviert die Audioerfassung vom lokalen Gerät und leitet Audioanwendungen in die Remotesitzung um.
- `audiomode:i:0` legt die Audiowiedergabe auf dem lokalen Computer fest.
- `camerastoredirect:s:*` leitet alle Kameras um.

Weitere Informationen finden Sie unter [Anpassen der Remotedesktopprotokoll-Eigenschaften für einen Hostpool](customize-rdp-properties.md).

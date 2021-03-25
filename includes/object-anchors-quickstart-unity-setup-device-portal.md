---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 04/03/2020
ms.author: crtreasu
ms.openlocfilehash: 44fd3b2bc89ac53e7a7c0293961098509d3f5c76
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102044581"
---
### <a name="set-up-the-windows-device-portal"></a>Einrichten des Windows-Geräteportals

Führen Sie die folgenden Schritte aus, um über das WLAN eine Verbindung zu Ihrem HoloLens-Gerät herzustellen:

1. [Verbinden Sie zunächst Ihr HoloLens-Gerät mit dem WLAN](https://docs.microsoft.com/hololens/hololens-network).

2. Rufen Sie dann auf dem Gerät unter **Einstellungen > Netzwerk und Internet > WLAN > Erweiterte Optionen** die IP-Adresse auf.

3. Rufen Sie in einem Webbrowser auf dem PC „`https://<YOUR_HOLOLENS_IP_ADDRESS>`“ auf. Im Browser wird die folgende Meldung angezeigt: „Es besteht ein Problem mit dem Sicherheitszertifikat dieser Website“. Diese Nachricht tritt auf, weil das für das Geräteportal ausgestellte Zertifikat ein selbstsigniertes Zertifikat ist. Sie können diesen Zertifikatfehler ignorieren und fortfahren.

Weitere Informationen zum Einrichten des Windows-Geräteportals finden Sie [unter diesem Link](https://docs.microsoft.com/windows/mixed-reality/using-the-windows-device-portal).

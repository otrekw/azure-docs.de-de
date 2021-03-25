---
title: 'Azure Communication Services: Häufig gestellte Fragen/bekannte Probleme'
description: Informationen zu Azure Communication Services
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: troubleshooting
ms.service: azure-communication-services
ms.openlocfilehash: 2c6ac34d8daf00578cb1d03833a28eb8535708b7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103493643"
---
# <a name="faq--known-issues"></a>Häufig gestellte Fragen/bekannte Probleme
Dieser Artikel enthält Informationen zu bekannten Problemen und häufig gestellten Fragen zu Azure Communication Services.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="why-is-the-quality-of-my-video-degraded"></a>Warum ist die Qualität meines Videos beeinträchtigt?

Die Qualität von Videostreams richtet sich nach der Größe des clientseitigen Renderers, der zum Initiieren dieses Streams verwendet wurde. Wenn ein Empfänger einen Remotestream abonniert, bestimmt er seine eigene Auflösung basierend auf den clientseitigen Rendererdimensionen des Senders.

### <a name="why-is-it-not-possible-to-enumerateselect-micspeaker-devices-on-safari"></a>Warum lassen sich Mikrofon-/Lautsprechergeräte in Safari nicht auflisten/auswählen?

Anwendungen können keine Mikrofon-/Lautsprechergeräte (z. B. Bluetooth) in Safari für iOS/iPad auflisten/auswählen. Dies ist eine Einschränkung des Betriebssystems. Es ist immer nur ein Gerät vorhanden.

Für Safari unter MacOS: Die Anwendung kann den Lautsprecher nicht über den Communication Services-Geräte-Manager auflisten/auswählen. Dieser muss über das Betriebssystem ausgewählt werden. Wenn Sie Chrome unter MacOS verwenden, kann die App Geräte über den Communication Services-Geräte-Manager auflisten/auswählen.

## <a name="known-issues"></a>Bekannte Probleme

Dieser Abschnitt enthält Informationen zu bekannten Problemen mit Azure Communication Services.

### <a name="repeatedly-switching-video-devices-may-cause-video-streaming-to-temporarily-stop"></a>Ein wiederholter Wechsel von Videogeräten kann dazu führen, dass das Videostreaming vorübergehend angehalten wird.

Das Wechseln zwischen Videogeräten kann dazu führen, dass Ihr Videostream angehalten wird, während der Stream vom ausgewählten Gerät erfasst wird.

#### <a name="possible-causes"></a>Mögliche Ursachen
Das Streaming von und Wechseln zwischen Mediengeräten ist rechenintensiv. Ein häufiger Wechsel kann zu Leistungsbeeinträchtigungen führen. Entwicklern wird empfohlen, einen Gerätestream zu beenden, bevor sie einen anderen starten.

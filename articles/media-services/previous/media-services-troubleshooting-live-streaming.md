---
title: Anleitung zur Behandlung von Problemen bei Livestreaming | Microsoft Docs
description: Dieser Artikel bietet Vorschläge zum Behandeln von Problemen beim Livestreaming mit Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2021
ms.author: inhenkel
ms.openlocfilehash: 1b7a7ec746f5400fe65e3e1db88ae61e97ae710a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103009044"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Anleitung zur Behandlung von Problemen bei Livestreaming

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]  

Dieser Artikel bietet Empfehlungen zum Behandeln von Problemen bei Livestreaming.

## <a name="issues-related-to-on-premises-encoders"></a>Probleme im Zusammenhang mit lokalen Encodern
Dieser Abschnitt enthält Empfehlungen zum Beheben von Problemen im Zusammenhang mit lokalen Encodern, die zum Senden eines Single-Bitrate-Datenstroms an für Livecodierung aktivierte AMS-Kanäle konfiguriert sind.

### <a name="problem-would-like-to-see-logs"></a>Problem: Anzeigen von Protokollen
* **Mögliches Problem:** Encoderprotokolle, mit denen möglicherweise Fehler behoben werden können, können nicht gefunden werden.
  
  * **Telestream Wirecast**: Normalerweise finden Sie Protokolle unter „C:\Benutzer\{Benutzername}\AppData\Roaming\Wirecast\“. 
  * **Elemental Live**: Links zu Protokollen finden Sie im Verwaltungsportal. Klicken Sie auf **Statistiken** und dann auf **Protokolle**. Auf der Seite **Protokolldateien** sehen Sie eine Liste der Protokolle für alle LiveEvent-Elemente. Wählen Sie das Protokoll für Ihre aktuelle Sitzung aus. 
  * **Flash Media Live-Encoder**: Sie finden das **Protokollverzeichnis** auf der Registerkarte **Codierungsprotokoll**.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problem: Es ist keine Option zum Ausgeben eines progressiven Datenstroms vorhanden
* **Mögliches Problem**: Der verwendete Encoder führt kein automatisches Deinterlacing aus. 
  
    **Problembehandlungsschritte**: Suchen Sie auf der Encoder-Benutzeroberfläche nach einer Option für Deinterlacing. Sobald Deinterlacing aktiviert ist, suchen Sie erneut nach Einstellungen für die progressive Ausgabe. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Problem: Ich habe verschiedene Encoderausgabeeinstellungen ausprobiert und kann immer noch keine Verbindung herstellen.
* **Mögliches Problem**: Der Azure-Codierungskanal wurde nicht ordnungsgemäß zurückgesetzt. 
  
    **Problembehandlungsschritte**: Stellen Sie sicher, dass der Encoder keine Pushübertragungen an AMS mehr ausführt, beenden Sie den Kanal, und setzen Sie ihn zurück. Sobald er erneut ausgeführt wird, versuchen Sie mit den neuen Einstellungen, den Encoder zu verbinden. Wenn das Problem damit immer noch nicht behoben ist, erstellen Sie einen vollkommen neuen Kanal. Manchmal werden Kanäle nach mehreren fehlgeschlagenen Versuchen beschädigt.  
* **Mögliches Problem**: Die GOP-Größe oder die Keyframe-Einstellungen sind nicht optimal. 
  
    **Problembehandlungsschritte**: Die empfohlene GOP-Größe bzw. das empfohlene Keyframe-Intervall beträgt zwei Sekunden. Manche Encoder berechnen diese Einstellung als Anzahl von Bildern und andere als Anzahl von Sekunden. Beispiel: Bei der Ausgabe von 30 BpS beträgt die GOP-Größe 60 Bilder; dies entspricht 2 Sekunden.  
* **Mögliches Problem**: Der Datenstrom wird durch geschlossene Ports blockiert. 
  
    **Problembehandlungsschritte**: Überprüfen Sie beim Streamen per RTMP in den Firewall- und/oder Proxyeinstellungen, ob die ausgehenden Ports 1935 und 1936 geöffnet sind. 

> [!NOTE]
> Wenn Sie nach dem Ausführen der Problembehandlungsschritte immer noch nicht streamen können, senden Sie über das klassische Azure-Portal ein Supportticket.
> 
> 

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]


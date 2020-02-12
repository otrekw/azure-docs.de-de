---
title: 'Registrieren für Video Indexer und Hochladen Ihres ersten Videos: Azure'
titleSuffix: Azure Media Services
description: Erfahren Sie, wie Sie sich beim Video Indexer-Portal registrieren und Ihr erstes Video hochladen.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: tutorial
ms.date: 02/03/2020
ms.author: juliako
ms.openlocfilehash: 1f3d442ea7cf6d95427aaa9c072410119e2735af
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989806"
---
# <a name="quickstart-how-to-sign-up-and-upload-your-first-video"></a>Schnellstart: Registrieren und Hochladen Ihres ersten Videos

In diesem Tutorial zu den ersten Schritten wird veranschaulicht, wie Sie sich bei der Video Indexer-Website anmelden und Ihr erstes Video hochladen.

Beim Erstellen eines Video Indexer-Kontos können Sie ein kostenloses Testkonto (mit einer bestimmten Anzahl von kostenlosen Indizierungsminuten) oder eine kostenpflichtige Option wählen (ohne Einschränkung durch eine Kontingentvorgabe). Bei der kostenlosen Testversion stellt Video Indexer bis zu 600 Minuten an kostenloser Indizierungszeit für Websitebenutzer und bis zu 2.400 Minuten an kostenloser Indizierungszeit für API-Benutzer bereit. Bei der kostenpflichtigen Option erstellen Sie ein Video Indexer-Konto, [das mit Ihrem Azure-Abonnement und einem Azure Media Services-Konto verbunden ist](connect-to-azure.md). Die Indizierungszeit wird minutenweise abgerechnet. Außerdem fallen Gebühren für das Azure Media Services-Konto an. 

## <a name="sign-up-for-video-indexer"></a>Registrieren bei Video Indexer

Um mit der Entwicklung mit Video Indexer zu beginnen, müssen Sie sich bei der [Video Indexer](https://www.videoindexer.com)-Website registrieren.

## <a name="upload-a-video-using-the-video-indexer-website"></a>Hochladen eines Videos auf der Video Indexer-Website

> [!NOTE]
> Der Name des Videos darf nicht mehr als 80 Zeichen umfassen.

### <a name="supported-file-formats-for-video-indexer"></a>Unterstützte Dateiformate für Video Indexer

Im Artikel [Eingabecontainer/Dateiformate](../latest/media-encoder-standard-formats.md#input-containerfile-formats) finden Sie eine Liste der Dateiformate, die Sie mit Video Indexer verwenden können.

### <a name="upload-a-video"></a>Hochladen eines Videos

1. Melden Sie sich bei der [Video Indexer](https://www.videoindexer.ai/)-Website an.
2. Klicken Sie auf die Schaltfläche bzw. den Link **Hochladen**, um ein Video hochzuladen.

    ![Upload](./media/video-indexer-get-started/video-indexer-upload.png)

    Nachdem Ihr Video hochgeladen wurde, beginnt Video Indexer mit dem Indizieren und Analysieren des Videos.

    ![Hochgeladen](./media/video-indexer-get-started/video-indexer-uploaded.png) 

    Wenn Video Indexer die Analyse abgeschlossen hat, erhalten Sie eine Benachrichtigung mit einem Link zu Ihrem Video und einer kurzen Beschreibung dazu, was in Ihrem Video gefunden wurde. Beispiel: Personen, Themen, OCR-Daten.

## <a name="see-also"></a>Weitere Informationen

Weitere Informationen finden Sie unter [Hochladen und Indizieren Ihrer Videos](upload-index-videos.md).

Nach dem Hochladen und Indizieren eines Videos können Sie die [Video Indexer-Website](video-indexer-view-edit.md) oder das [Video Indexer-Entwicklerportal](video-indexer-use-apis.md) verwenden, um die aus dem Video gewonnenen Erkenntnisse anzuzeigen. 

[Einstieg in APIs](video-indexer-use-apis.md)

## <a name="next-steps"></a>Nächste Schritte

Eine ausführliche Einführung erhalten Sie in unserem entsprechenden [Lab](https://github.com/Azure-Samples/media-services-video-indexer/blob/master/IntroToVideoIndexer.md). 

Am Ende des Workshops sind Sie gut darüber informiert, welche Arten von Informationen aus Video- und Audioinhalten extrahiert werden können. Sie sind dann besser vorbereitet, um Chancen in Bezug auf „Content Intelligence“ zu erkennen, KI-Funktionen für Videos in Azure vorzustellen und verschiedene Szenarien in Video Indexer zu demonstrieren.


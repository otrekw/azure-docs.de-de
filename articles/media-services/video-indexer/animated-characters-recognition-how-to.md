---
title: 'Erkennung animierter Figuren mit Video Indexer: Vorgehensweise'
titleSuffix: Azure Media Services
description: Hier erfahren Sie, wie Sie die Erkennung animierter Figuren mit Video Indexer verwenden.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.custom: references_regions
ms.topic: how-to
ms.date: 12/07/2020
ms.author: juliako
ms.openlocfilehash: 29dbdb896d1a6063fec277afa33327c84cb2f0cd
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98880926"
---
# <a name="use-the-animated-character-detection-preview-with-portal-and-api"></a>Verwenden der Erkennung animierter Figuren (Vorschau) mithilfe des Portals und der API 

Der Video Indexer-Dienst von Azure Media Services unterstützt die Ermittlung, Gruppierung und Erkennung von Figuren in animierten Inhalten. Diese Funktion ist über das Azure-Portal und die API verfügbar. Lesen Sie diese [Übersicht](animated-characters-recognition.md).

Im vorliegenden Artikel wird veranschaulicht, wie Sie die Erkennung animierter Figuren mit dem Azure-Portal und der Video Indexer-API verwenden.

## <a name="use-the-animated-character-detection-with-portal"></a>Verwenden der Erkennung animierter Figuren über das Portal 

Da die Custom Vision-Integration in den Testkonten von Video Indexer verwaltet wird, können Sie mit dem Erstellen und Verwenden des animierten Figurenmodells beginnen. Wenn Sie das Testkonto verwenden, können Sie den folgenden Abschnitt überspringen („Herstellen einer Verbindung mit Ihrem Custom Vision-Konto“).

### <a name="connect-your-custom-vision-account-paid-accounts-only"></a>Herstellen einer Verbindung mit Ihrem Custom Vision-Konto (nur kostenpflichtige Konten)

Wenn Sie über ein kostenpflichtiges Video Indexer-Konto verfügen, müssen Sie zunächst eine Verbindung mit einem Custom Vision-Konto herstellen. Sollten Sie noch nicht über ein Custom Vision-Konto verfügen, erstellen Sie eins. Weitere Informationen finden Sie unter [Was ist Azure Custom Vision?](../../cognitive-services/custom-vision-service/overview.md).

> [!NOTE]
> Beide Konten müssen sich in derselben Region befinden. Die Custom Vision-Integration wird in der Region „Japan“ derzeit nicht unterstützt.

Bei kostenpflichtigen Konten mit Zugriff auf das Custom Vision-Konto können die Modelle und getaggten Bilder dort angezeigt werden. Weitere Informationen zur Verbesserung Ihrer Klassifizierung in Custom Vision finden Sie  [hier](../../cognitive-services/custom-vision-service/getting-started-improving-your-classifier.md). 

Beachten Sie, dass das Training des Modells nur über Video Indexer und nicht über die Custom Vision-Website erfolgen sollte. 

#### <a name="connect-a-custom-vision-account-with-api"></a>Verbinden eines Custom Vision-Kontos mit API 

Führen Sie die folgenden Schritte aus, um Ihr Custom Vision-Konto mit Video Indexer zu verbinden, oder um das Custom Vision-Konto zu ändern, das zurzeit mit Video Indexer verbunden ist:

1. Navigieren Sie zu [www.customvision.ai](https://www.customvision.ai), und melden Sie sich an.
1. Kopieren Sie die Schlüssel für die Trainings- und Vorhersageressourcen:

    > [!NOTE]
    > Um alle Schlüssel bereitzustellen, benötigen Sie zwei separate Ressourcen in Custom Vision, eine für das Training und eine für die Vorhersage.
1. Geben Sie weitere Informationen an:

    * Endpunkt 
    * Vorhersageressourcen-ID
1. Wechseln Sie zu [Video Indexer](https://vi.microsoft.com/), und melden Sie sich an.
1. Klicken Sie oben rechts auf der Seite auf das Fragezeichen, und wählen Sie **API-Referenz** aus.
1. Stellen Sie sicher, dass Sie API Management abonniert haben, indem Sie auf die Registerkarte **Produkte** klicken. Wenn Sie über eine verbundene API verfügen, können Sie mit dem nächsten Schritt fortfahren, andernfalls abonnieren Sie eine. 
1. Klicken Sie im Entwicklerportal auf **Vollständige API-Referenz**, und navigieren Sie zu **Vorgänge**.  
1. Wählen Sie **Custom Vision-Konto verbinden (PREVIEW)** aus, und klicken Sie auf **Testen**.
1. Füllen Sie die erforderlichen Felder sowie das Zugriffstoken aus, und klicken Sie auf **Senden**. 

    Weitere Informationen zum Abrufen des Video Indexer-Zugriffstokens finden Sie im [Entwicklerportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Account-Access-Token?) sowie in der relevanten [Dokumentation](video-indexer-use-apis.md#obtain-access-token-using-the-authorization-api).  
1. Sobald der Aufruf die Antwort „200 OK“ zurückgegeben hat, ist Ihr Konto verbunden.
1. Um Ihre Verbindung zu überprüfen, wechseln Sie zum [Video Indexer](https://vi.microsoft.com/)-Portal:
1. Klicken Sie in der oberen rechten Ecke auf die Schaltfläche **Anpassung des Inhaltsmodells**.
1. Wechseln Sie zur Registerkarte **Animierte Figuren**.
1. Nachdem Sie in Custom Vision auf „Modelle verwalten“ geklickt haben, werden Sie zum Custom Vision-Konto weitergeleitet, mit dem Sie gerade eine Verbindung hergestellt haben.

> [!NOTE]
> Derzeit werden nur Modelle unterstützt, die über Video Indexer erstellt wurden. Modelle, die mit Custom Vision erstellt wurden, sind nicht verfügbar. Ferner besteht die bewährte Methode darin, Modelle, die mit Video Indexer erstellt wurden, ausschließlich über die Video Indexer-Plattform zu bearbeiten, da mit Custom Vision vorgenommene Änderungen zu unbeabsichtigten Ergebnissen führen können.

### <a name="create-an-animated-characters-model"></a>Erstellen eines Modells für animierte Figuren

1. Navigieren Sie zur [Video Indexer](https://vi.microsoft.com/)-Website, und melden Sie sich an.
1. Um ein Modell in Ihrem Konto anzupassen, wählen Sie links auf der Seite die Schaltfläche **Anpassung des Inhaltsmodells** aus.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/content-model-customization/content-model-customization.png" alt-text="Anpassen eines Inhaltsmodells in Video Indexer":::
1. Navigieren Sie im Abschnitt für die Modellanpassung zur Registerkarte **Animated characters** (Animierte Figuren).
1. Klicken Sie auf **Modell hinzufügen**.
1. Benennen Sie das Modell, und drücken Sie die EINGABETASTE, um den Namen zu speichern.

> [!NOTE]
> Es wird empfohlen, für jede animierte Serie jeweils ein eigenes Custom Vision-Modell zu verwenden. 

### <a name="index-a-video-with-an-animated-model"></a>Indizieren eines Videos mit einem animierten Modell

Laden Sie für das erste Training mindestens zwei Videos hoch. Jedes sollte vorzugsweise länger als 15 Minuten sein, damit ein sinnvolles Erkennungsmodell erstellt werden kann. Wenn Ihre Folgen kürzer sind, sollten Sie vor dem Training insgesamt mindestens 30 Minuten an Videoinhalten hochladen. Auf diese Weise können Sie Gruppen zusammenführen, die zu denselben Figuren in unterschiedlichen Szenen und mit verschiedenen Hintergründen gehören, und dadurch die Wahrscheinlichkeit erhöhen, dass die Figur in den später von Ihnen indizierenden Folgen erkannt wird. Wenn Sie ein Modell mit mehreren Videos (Episoden) trainieren möchten, müssen Sie alle mit demselben Animationsmodell indizieren. 

1. Klicken Sie auf die Schaltfläche **Hochladen**.
1. Wählen Sie ein hochzuladendes Video (Datei oder URL) aus.
1. Klicken Sie auf **Erweiterte Optionen**.
1. Wählen Sie unter **People / Animated characters** (Personen/animierte Figuren) die Option **Animation models** (Animationsmodelle) aus.
1. Wenn Sie über ein einzelnes Modell verfügen, wird es automatisch ausgewählt. Sind mehrere Modelle vorhanden, können Sie das passende Modell über das Dropdownmenü auswählen.
1. Klicken Sie auf „Hochladen“.
1. Nachdem das Video indiziert wurde, werden die erkannten Figuren im Abschnitt **Animated characters** (Animierte Figuren) im Bereich **Erkenntnisse** angezeigt.

Vor dem Taggen und Trainieren des Modells werden alle Figuren mit „Unknown #X“ (Unbekannt #X) benannt. Nachdem Sie das Modell trainiert haben, werden sie auch erkannt.

### <a name="customize-the-animated-characters-models"></a>Anpassen der Modelle für animierte Figuren

1. Benennen Sie die Figuren in Video Indexer.

    1. Nachdem das Modell Figurengruppen erstellt hat, sollten Sie diese Gruppen in Custom Vision überprüfen. 
    1. Um eine animierte Figur in Ihrem Video zu taggen, navigieren Sie zur Registerkarte  **Erkenntnisse**, und klicken Sie rechts oben im Fenster auf die Schaltfläche **Bearbeiten**. 
    1. Klicken Sie im Bereich  **Erkenntnisse** auf eine der erkannten animierten Figuren, und ändern Sie ihren Namen von „Unknown #X“ (Unbekannt #X) bzw. dem Namen, der der Figur zuvor zugewiesen wurde, in einen temporären Namen. 
    1. Nachdem Sie den neuen Namen eingegeben haben, klicken Sie neben dem neuen Namen auf das Prüfsymbol. Dadurch wird der neue Name in dem Modell in Video Indexer gespeichert. 
1. Nur kostenpflichtige Konten: Überprüfen der Gruppen in Custom Vision 

    > [!NOTE]
    > Bei kostenpflichtigen Konten mit Zugriff auf das Custom Vision-Konto können die Modelle und getaggten Bilder dort angezeigt werden. Weitere Informationen zur Verbesserung Ihrer Klassifizierung in Custom Vision finden Sie  [hier](../../cognitive-services/custom-vision-service/getting-started-improving-your-classifier.md). Beachten Sie unbedingt, dass das Training des Modells nur (wie in diesem Thema beschrieben) über Video Indexer und nicht über die Custom Vision-Website erfolgen sollte. 

    1. Wechseln Sie in Video Indexer zur Seite **Benutzerdefinierte Modelle**, und wählen Sie die Registerkarte **Animierte Figuren** aus. 
    1. Klicken Sie auf die Schaltfläche „Bearbeiten“ für das Modell, an dem Sie arbeiten, um es in Custom Vision zu verwalten. 
    1. Überprüfen Sie jede Figurengruppe: 

        * Wenn die Gruppe nicht zugehörige Bilder enthält, sollten Sie diese auf der Custom Vision-Website löschen. 
        * Wenn Bilder vorhanden sind, die zu einer anderen Figur gehören, ändern Sie das Tag für diese Bilder, indem Sie auf das Bild klicken, das richtige Tag hinzufügen und das falsche Tag löschen. 
        * Wenn die Gruppe falsch ist, also hauptsächlich Bilder ohne Figuren oder Bilder mit mehreren Figuren enthält, können Sie sie auf der Custom Vision-Website oder in den Video Indexer-Erkenntnissen löschen. 
        * Der Gruppierungsalgorithmus teilt die Figuren manchmal in unterschiedliche Gruppen auf. Es wird daher empfohlen, allen Gruppen, die zur selben Figur gehören, denselben Namen (in den Video Indexer-Erkenntnissen) zu geben. Dadurch werden alle diese Gruppen sofort wie auf der Custom Vision-Website angezeigt. 
    1. Nachdem die Gruppe verfeinert wurde, vergewissern Sie sich, dass der ursprüngliche Name, mit dem Sie sie gekennzeichnet haben, die Figur in der Gruppe widerspiegelt. 
1. Trainieren des Modells 

    1. Nachdem Sie alle gewünschten Namen bearbeitet haben, müssen Sie das Modell trainieren. 
    1. Sobald das Modell mit einer Figur trainiert wurde, wird sie im nächsten Video erkannt, das mit diesem Modell indiziert wird. 
    1. Öffnen Sie die Anpassungsseite, und klicken Sie auf die Registerkarte für  **animierte Figuren** und anschließend auf die Schaltfläche  **Trainieren**, um Ihr Modell zu trainieren. Um die Verbindung zwischen Video 
    
Indexer und Modell aufrechtzuerhalten, trainieren Sie das Modell nicht auf der Custom Vision-Website (kostenpflichtige Konten haben Zugriff auf die Custom Vision Website), sondern nur in Video Indexer. Nach dem Trainieren werden die trainierten Figuren in allen Videos erkannt, die erstmals oder erneut mit diesem Modell indiziert werden. 

## <a name="delete-an-animated-character-and-the-model"></a>Löschen von animierter Figur und Modell

1. Löschen Sie eine animierte Figur.

    1. Um eine animierte Figur aus Ihren Videoerkenntnissen zu löschen, navigieren Sie zur Registerkarte **Erkenntnisse**, und klicken Sie rechts oben im Fenster auf die Schaltfläche **Bearbeiten**.
    1. Wählen Sie die animierte Figur aus, und klicken Sie anschließend unter ihrem Namen auf die Schaltfläche **Löschen**.

    > [!NOTE]
    > Dadurch wird die Erkenntnis aus diesem Video gelöscht. Das Modell bleibt davon unberührt.
1. Löschen Sie ein Modell.

    1. Klicken Sie im oberen Menü auf die Schaltfläche **Anpassung des Inhaltsmodells**, und navigieren Sie zur Registerkarte **Animated characters** (Animierte Figuren).
    1. Klicken Sie rechts neben dem Modell, das Sie löschen möchten, auf die Auslassungspunkte und anschließend auf die Schaltfläche „Löschen“.
    
    * Kostenpflichtiges Konto: Die Verbindung zwischen Modell und Video Indexer wird getrennt und kann nicht wiederhergestellt werden.
    * Testkonto: Das Modell wird auch aus Custom Vision gelöscht. 
    
        > [!NOTE]
        > Bei einem Testkonto steht nur ein einzelnes Modell zur Verfügung. Nachdem Sie es gelöscht haben, können Sie keine weiteren Modelle trainieren.

## <a name="use-the-animated-character-detection-with-api"></a>Verwenden der Erkennung animierter Figuren über die API 

1. Stellen Sie eine Verbindung mit einem Custom Vision-Konto her.

    Wenn Sie über ein kostenpflichtiges Video Indexer-Konto verfügen, müssen Sie zunächst eine Verbindung mit einem Custom Vision-Konto herstellen. <br/>
    Sollten Sie noch nicht über ein Custom Vision-Konto verfügen, müssen Sie eins erstellen. Weitere Informationen finden Sie unter [Was ist Azure Custom Vision?](../../cognitive-services/custom-vision-service/overview.md).

    [Stellen Sie unter Verwendung der API eine Verbindung mit Ihrem Custom Vision-Konto her.](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Connect-Custom-Vision-Account?tags=&pattern=&groupBy=tag)
1. Erstellen Sie ein Modell für animierte Figuren.

    Verwenden Sie die [API zum Erstellen von Animationsmodellen](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Animation-Model?&groupBy=tag).
1. Indizieren Sie ein Video (erstmals oder erneut).

    Verwenden Sie die [API für die erneute Indizierung](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?). 
1. Passen Sie die Modelle für animierte Figuren an.

    Verwenden Sie die [API zum Trainieren von Animationsmodellen](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Train-Animation-Model?&groupBy=tag).

### <a name="view-the-output"></a>Anzeigen der Ausgabe

Sehen Sie sich die animierten Figuren in der generierten JSON-Datei an.

```json
"animatedCharacters": [
    {
    "videoId": "e867214582",
    "confidence": 0,
    "thumbnailId": "00000000-0000-0000-0000-000000000000",
    "seenDuration": 201.5,
    "seenDurationRatio": 0.3175,
    "isKnownCharacter": true,
    "id": 4,
    "name": "Bunny",
    "appearances": [
        {
            "startTime": "0:00:52.333",
            "endTime": "0:02:02.6",
            "startSeconds": 52.3,
            "endSeconds": 122.6
        },
        {
            "startTime": "0:02:40.633",
            "endTime": "0:03:16.6",
            "startSeconds": 160.6,
            "endSeconds": 196.6
        },
    ]
    },
]
```

## <a name="limitations"></a>Einschränkungen

* Die Animationserkennung wird momentan in der Region „Asien, Osten“ nicht unterstützt.
* Kleine oder weit entfernte Figuren werden in Videos mit schlechter Qualität möglicherweise nicht ordnungsgemäß erkannt.
* Es wird empfohlen, für jede Gruppe von animierten Figuren (beispielsweise pro animierter Serie) ein eigenes Modell zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion))
---
title: 'Speech-Dienst: Modell- und Endpunktlebenszyklus von Custom Speech'
titleSuffix: Azure Cognitive Services
description: Custom Speech bietet anpassbare Basismodelle und ermöglicht es Ihnen, benutzerdefinierte Modelle anhand Ihrer Daten zu erstellen. In diesem Artikel werden die Zeitpläne für Modelle und die Endpunkte beschrieben, die diese Modelle verwenden.
services: cognitive-services
author: heikora
manager: dongli
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/2/2021
ms.author: heikora
ms.openlocfilehash: b82a732533c3d069b519b07c3209d4b96c472900
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106385024"
---
# <a name="model-and-endpoint-lifecycle"></a>Lebenszyklus von Modell und Endpunkt

Unsere Standardsprache (nicht angepasste Sprache) basiert auf KI-Modellen, die als Basismodelle bezeichnet werden. In den meisten Fällen wird für jede gesprochene Sprache, die wir unterstützen, ein anderes Basismodell trainiert.  Wir aktualisieren den Sprachdienst alle paar Monate mit neuen Basismodellen, um die Genauigkeit und Qualität zu verbessern.  
Bei Custom Speech werden benutzerdefinierte Modelle erstellt, indem ein ausgewähltes Basismodell anhand der Daten Ihres spezifischen Kundenszenarios angepasst wird. Nachdem Sie ein benutzerdefiniertes Modell erstellt haben, wird dieses Modell nicht aktualisiert oder geändert, auch wenn das zugehörige Basismodell, von dem es angepasst wurde, im Standard-Sprachdienst aktualisiert wird.  
Diese Richtlinie ermöglicht es Ihnen, ein bestimmtes benutzerdefiniertes Modell für einen längeren Zeitraum zu verwenden, nachdem Sie ein benutzerdefiniertes Modell verwendet haben, das Ihren Anforderungen entspricht.  Es wird jedoch empfohlen, dass Sie Ihr benutzerdefiniertes Modell regelmäßig neu erstellen, damit Sie sich vom neuesten Basismodell aus anpassen können, um die verbesserte Genauigkeit und Qualität zu nutzen.

Weitere Schlüsselbegriffe im Zusammenhang mit dem Modelllebenszyklus sind:

* **Anpassung:** Die Übernahme eines Basismodells und dessen Anpassung mithilfe von Text- und/oder Audiodaten an Ihre Domäne/Ihr Szenario.
* **Decodierung:** Die Verwendung eines Modells und Durchführung der Spracherkennung (Decodierung von Audio- in Textdaten).
* **Endpunkt**: Eine benutzerspezifische Bereitstellung eines Basismodells oder eines benutzerdefinierten Modells, das *nur* für einen bestimmten Benutzer zugänglich ist.

### <a name="expiration-timeline"></a>Zeitskala für den Ablauf

Wenn neue Modelle und neue Funktionalitäten verfügbar werden und ältere, weniger genaue Modelle eingestellt werden, finden Sie weitere Informationen in den folgenden Zeitskalen für den Ablauf von Modell und Endpunkt:

**Basismodelle** 

* Anpassung: Verfügbar für ein Jahr. Nachdem das Modell importiert wurde, steht es für ein Jahr zur Verfügung, um benutzerdefinierte Modelle zu erstellen. Nach einem Jahr müssen neue benutzerdefinierte Modelle aus einer neueren Basismodellversion erstellt werden.  
* Decodierung: Für zwei Jahre nach dem Import verfügbar. Das bedeutet, dass Sie einen Endpunkt erstellen und mithilfe dieses Modells die Batch-Transkription für zwei Jahre verwenden können. 
* Endpunkte: Auf derselben Zeitskala wie die Decodierung verfügbar.

**Benutzerdefinierte Modelle**

* Decodierung: Verfügbar für zwei Jahre, nachdem das Modell erstellt wurde. Dies bedeutet, dass Sie das benutzerdefinierte Modell nach seiner Erstellung zwei Jahre lang (Batch/Echtzeit/Test) verwenden können. Nach zwei Jahren sollten Sie *Ihr Modell erneut trainieren*, da das Basismodell in in der Regel für die Anpassung veraltet sein wird.  
* Endpunkte: Auf derselben Zeitskala wie die Decodierung verfügbar.

Wenn entweder ein Basismodell oder ein benutzerdefiniertes Modell abläuft, wird immer ein Fallback auf die *neueste Basismodellversion* durchgeführt. Auf diese Weise wird Ihre Implementierung niemals unterbrochen, aber sie ist möglicherweise für *Ihre bestimmten Daten* weniger genau, wenn benutzerdefinierte Modelle das Ablaufdatum erreichen. Sie können das Ablaufdatum für ein Modell an den folgenden Stellen im Custom Speech-Bereich von Speech Studio sehen:

* Zusammenfassung zum Modelltraining
* Modelltrainingsdetails
* Zusammenfassung der Bereitstellungen
* Bereitstellungsdetails

Hier sehen Sie ein Beispiel aus der Zusammenfassung des Modelltrainings:

![Zusammenfassung des Modelltrainings](media/custom-speech/custom-speech-model-training-with-expiry.png) Hier sehen Sie ein Beispiel der Detailseite des Modelltrainings:

![Modelltrainingsdetails](media/custom-speech/custom-speech-model-details-with-expiry.png)

Sie können die Ablaufdaten auch über die Custom Speech-APIs [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) und [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) unter der Eigenschaft `deprecationDates` in der JSON-Antwort überprüfen.

Im Folgenden finden Sie ein Beispiel für Ablaufdaten aus dem GetModel-API-Aufruf. Die **DEPRECATIONDATES** zeigen, wann das Modell abläuft: 
```json
{
    "SELF": "HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/{id}",
    "BASEMODEL": {
    "SELF": HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/BASE/{id}
    },
    "DATASETS": [
    {
        "SELF": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/datasets/{id}
    }
    ],
    "LINKS": {
    "MANIFEST": "HTTPS://WESTUS2.API.COGNITIVE.MICROSOFT.COM/SPEECHTOTEXT/V3.0/MODELS/{id}/MANIFEST",
    "COPYTO": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/models/{id}/copyto
    },
    "PROJECT": {
        "SELF": https://westus2.api.cognitive.microsoft.com/speechtotext/v3.0/projects/{id}
    },
    "PROPERTIES": {
    "DEPRECATIONDATES": {
        "ADAPTATIONDATETIME": "2022-01-15T00:00:00Z",     // last date the base model can be used for adaptation
        "TRANSCRIPTIONDATETIME": "2023-03-01T21:27:29Z"   // last date this model can be used for decoding
    }
    },
    "LASTACTIONDATETIME": "2021-03-01T21:27:40Z",
    "STATUS": "SUCCEEDED",
    "CREATEDDATETIME": "2021-03-01T21:27:29Z",
    "LOCALE": "EN-US",
    "DISPLAYNAME": "EXAMPLE MODEL",
    "DESCRIPTION": "",
    "CUSTOMPROPERTIES": {
    "PORTALAPIVERSION": "3"
    }
}
```
Beachten Sie, dass Sie das Modell über einen Custom Speech-Endpunkt ohne Downtime aktualisieren können, indem Sie das vom Endpunkt verwendete Modell im Abschnitt „Bereitstellung“ in Speech Studio oder über die Custom Speech-API ändern.

## <a name="what-happens-when-models-expire-and-how-to-update-them"></a>Was geschieht, wenn Modelle ablaufen und wie man sie aktualisiert
Was geschieht, wenn ein Modell abläuft und wie es aktualisiert wird, hängt davon ab, wie es verwendet wird.
### <a name="batch-transcription"></a>Batch-Transkription
Wenn ein Modell abläuft, das mit [Batch-Transkriptions](batch-transcription.md) Transkriptionsanforderungen verwendet wird, tritt ein 4xx-Fehler auf. Um dies zu verhindern, aktualisieren Sie den `model`-Parameter im JSON-Code, der im Anforderungstext **Transkription erstellen** gesendet wurde, um entweder auf ein aktuelleres Basismodell oder auf ein neueres benutzerdefiniertes Modell zu zeigen. Sie können auch den `model`-Eintrag aus dem JSON-Code entfernen, um immer das neueste Basismodell zu verwenden.
### <a name="custom-speech-endpoint"></a>Custom Speech-Endpunkt
Wenn ein Modell abläuft, das von einem [benutzerdefinierten Speech-Endpunkt](how-to-custom-speech-train-model.md)verwendet wird, wird der Dienst automatisch auf das neueste Basismodell für die verwendete Sprache zurückgreifen. Verwenden Sie die Option **Bereitstellung** im Menü **Custom Speech** oben auf der Seite, und klicken Sie dann auf den Namen des Endpunkts, um seine Details anzuzeigen. Am oberen Rand der Detailseite wird die Schaltfläche **Modell aktualisieren** angezeigt, mit der Sie das von diesem Endpunkt verwendete Modell ohne Ausfallzeiten nahtlos aktualisieren können. Sie können diese Änderung auch vom Programm gesteuert vornehmen, mithilfe der Rest-API [**Modell aktualisieren**](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/UpdateModel).

## <a name="next-steps"></a>Nächste Schritte

* [Trainieren und Bereitstellen eines Modells](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Vorbereiten und Testen Ihrer Daten](./how-to-custom-speech-test-and-train.md)
* [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
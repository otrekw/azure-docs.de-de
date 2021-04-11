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
ms.date: 03/10/2021
ms.author: heikora
ms.openlocfilehash: b8e02071eca139cde02a8bad1b0e0e443db6ab86
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103547954"
---
# <a name="model-and-endpoint-lifecycle"></a>Modell- und Endpunktlebenszyklus

Custom Speech verwendet sowohl *Basismodelle* als auch *benutzerdefinierte Modelle*. Jede Sprache verfügt über ein oder mehrere Basismodelle. Wenn ein neues Sprachmodell für den regulären Speech-Dienst freigegeben wird, wird es im Allgemeinen auch als neues Basismodell in den Custom Speech-Dienst importiert. Sie werden alle sechs bis zwölf Monate aktualisiert. Ältere Modelle werden in der Regel mit der Zeit weniger nützlich, da das neueste Modell in der Regel eine höhere Genauigkeit aufweist.

Benutzerdefinierte Modelle werden hingegen erstellt, indem ein ausgewähltes Basismodell anhand der Daten Ihres spezifischen Kundenszenarios angepasst wird. Sie können ein bestimmtes benutzerdefiniertes Modell für einen längeren Zeitraum weiterhin verwenden, sobald Sie eines für Ihre Anforderungen gefunden haben. Es wird jedoch empfohlen, regelmäßig ein Update auf das neueste Basismodell durchzuführen und es mit zusätzlichen Daten erneut zu trainieren. 

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

Im Folgenden finden Sie ein Beispiel für Ablaufdaten aus dem GetModel-API-Aufruf. Der Wert von DEPRECATIONDATES gibt das Ablaufdatum an: 
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
        "ADAPTATIONDATETIME": "2022-01-15T00:00:00Z",     // last date this model can be used for adaptation
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

## <a name="next-steps"></a>Nächste Schritte

* [Trainieren und Bereitstellen eines Modells](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Vorbereiten und Testen Ihrer Daten](./how-to-custom-speech-test-and-train.md)
* [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
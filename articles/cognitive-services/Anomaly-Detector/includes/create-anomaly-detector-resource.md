---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/10/2020
ms.author: mbullwin
ms.openlocfilehash: 037b9746571678e24e80bd76e7e2ed173ab8eb90
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97677633"
---
## <a name="create-an-anomaly-detector-resource"></a>Erstellen einer Anomalieerkennungsressource

1. Melden Sie sich beim <a href="https://portal.azure.com" target="_blank">Azure-Portal<span class="docon docon-navigate-external x-hidden-focus"></span></a> an.
1. Klicken Sie auf <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector" target="_blank">Create Anomaly Detector<span class="docon docon-navigate-external x-hidden-focus"></span></a> resource (Anomalieerkennungsressource erstellen).
1. Geben Sie alle erforderlichen Einstellungen ein:

    |Einstellung|value|
    |--|--|
    |Name|Gewünschter Name (2 bis 64 Zeichen)|
    |Subscription|Wählen Sie ein passendes Abonnement aus|
    |Location|Wählen Sie einen nahe gelegenen und verfügbaren Speicherort aus|
    |Preisstufe|`F0`: 10 Aufrufe pro Sekunde, 20.000 Transaktionen pro Monat <br> Oder:<br> `S0`: 80 Aufrufe pro Sekunde|
    |Ressourcengruppe|Wählen Sie eine verfügbare Ressourcengruppe aus|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wird. Nachdem sie erstellt wurde, navigieren Sie zur Ressourcenseite.
1. Erfassen Sie den konfigurierten `endpoint` und einen API-Schlüssel:

    |Die Registerkarte „Keys and Endpoint“ (Schlüssel und Endpunkt) im Portal|Einstellung|value|
    |--|--|--|
    |**Übersicht**|Endpunkt|Kopieren Sie den Endpunkt. Er sieht etwa so aus: ` https://<your-resource-name>.cognitiveservices.azure.com/`.|
    |**Schlüssel**|API-Schlüssel|Kopieren Sie einen der beiden Schlüssel. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|




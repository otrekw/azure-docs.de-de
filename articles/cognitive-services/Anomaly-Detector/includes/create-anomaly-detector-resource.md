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
ms.openlocfilehash: feb79d047a6c3b25176a13dcc3c3afd53a51459e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102444461"
---
## <a name="create-an-anomaly-detector-resource"></a>Erstellen einer Anomalieerkennungsressource

1. Melden Sie sich beim <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> an.
1. Klicken Sie auf <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector" target="_blank">Create Anomaly Detector</a> resource (Anomalieerkennungsressource erstellen).
1. Geben Sie alle erforderlichen Einstellungen ein:

    |Einstellung|Wert|
    |--|--|
    |Name|Gewünschter Name (2 bis 64 Zeichen)|
    |Subscription|Wählen Sie ein passendes Abonnement aus|
    |Standort|Wählen Sie einen nahe gelegenen und verfügbaren Speicherort aus|
    |Preisstufe|`F0`: 10 Aufrufe pro Sekunde, 20.000 Transaktionen pro Monat <br> Oder:<br> `S0`: 80 Aufrufe pro Sekunde|
    |Ressourcengruppe|Wählen Sie eine verfügbare Ressourcengruppe aus|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wird. Nachdem sie erstellt wurde, navigieren Sie zur Ressourcenseite.
1. Erfassen Sie den konfigurierten `endpoint` und einen API-Schlüssel:

    |Die Registerkarte „Keys and Endpoint“ (Schlüssel und Endpunkt) im Portal|Einstellung|Wert|
    |--|--|--|
    |**Übersicht**|Endpunkt|Kopieren Sie den Endpunkt. Er sieht etwa so aus: ` https://<your-resource-name>.cognitiveservices.azure.com/`.|
    |**Schlüssel**|API-Schlüssel|Kopieren Sie einen der beiden Schlüssel. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|




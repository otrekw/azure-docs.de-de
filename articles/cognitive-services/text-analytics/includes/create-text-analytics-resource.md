---
title: Erstellen einer Cognitive Services-Ressource zur Textanalyse
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Cognitive Services-Ressource zur Textanalyse erstellen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/09/2021
ms.author: aahi
ms.openlocfilehash: 5b6479d48a51ba962f2f6bfba16dac3b0886a9ff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "101750330"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Erstellen einer Cognitive Services-Ressource zur Textanalyse

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf **Ressource erstellen**, und navigieren Sie dann zu **KI + Machine Learning** > **Textanalyse**.
   Alternativ können Sie zu [Textanalyse erstellen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) navigieren.
1. Geben Sie alle erforderlichen Einstellungen ein:

    |Einstellung|Wert|
    |--|--|
    |Name|Geben Sie einen Namen ein (2–64 Zeichen).|
    |Subscription|Wählen Sie das entsprechende Abonnement aus.|
    |Standort|Wählen Sie einen nahe gelegenen Speicherort aus.|
    |Tarif| Geben Sie **S** als Standardtarif ein.|
    |Resource group|Wählen Sie eine verfügbare Ressourcengruppe aus.|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wurde. Ihr Browser leitet Sie automatisch zur neu erstellten Ressourcenseite weiter.
1. Rufen Sie den konfigurierten `endpoint` und einen API-Schlüssel ab:

    |Registerkarte „Ressource“ im Portal|Einstellung|Wert|
    |--|--|--|
    |**Übersicht**|Endpunkt|Kopieren Sie den Endpunkt. Dieser sieht etwa so aus: `https://my-resource.cognitiveservices.azure.com/text/analytics/v3.0`.|
    |**Schlüssel**|API-Schlüssel|Kopieren Sie einen der beiden Schlüssel. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|

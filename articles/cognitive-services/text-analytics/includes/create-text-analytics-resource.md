---
title: Erstellen einer Cognitive Services-Ressource zur Textanalyse
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Cognitive Services-Ressource zur Textanalyse erstellen.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: d3b65174f3d161e0b8780a890f297d9d1c812b44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "74383435"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Erstellen einer Cognitive Services-Ressource zur Textanalyse

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf **Ressource erstellen**, und navigieren Sie dann zu **KI + Machine Learning** > **Textanalyse**.
   Alternativ können Sie zu [Textanalyse erstellen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) navigieren.
1. Geben Sie alle erforderlichen Einstellungen ein:

    |Einstellung|value|
    |--|--|
    |Name|Geben Sie einen Namen ein (2 bis 64 Zeichen).|
    |Subscription|Wählen Sie das entsprechende Abonnement aus.|
    |Location|Wählen Sie einen nahe gelegenen Speicherort aus.|
    |Tarif| Geben Sie **S** für den Tarif „Standard“ ein.|
    |Resource group|Wählen Sie eine verfügbare Ressourcengruppe aus.|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wird. Ihr Browser leitet Sie automatisch zur neu erstellten Ressourcenseite weiter.
1. Rufen Sie den konfigurierten `endpoint` und einen API-Schlüssel ab:

    |Registerkarte „Ressource“ im Portal|Einstellung|value|
    |--|--|--|
    |**Übersicht**|Endpunkt|Kopieren Sie den Endpunkt. Dieser sieht etwa so aus: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`.|
    |**Schlüssel**|API-Schlüssel|Kopieren Sie einen der beiden Schlüssel. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|

---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/07/2019
ms.author: dapine
ms.openlocfilehash: 2cb2cfbdfbac5d496f109d85977b41a050766ab0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "73499102"
---
## <a name="create-an-computer-vision-resource"></a>Erstellen einer Maschinelles Sehen-Ressource

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf die Ressource [Erstellen**Maschinelles Sehen**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision).
1. Geben Sie alle erforderlichen Einstellungen ein:

    |Einstellung|value|
    |--|--|
    |Name|Gewünschter Name (2 bis 64 Zeichen)|
    |Subscription|Wählen Sie ein passendes Abonnement aus|
    |Location|Wählen Sie einen nahe gelegenen und verfügbaren Speicherort aus|
    |Preisstufe|`F0` – der Mindesttarif|
    |Ressourcengruppe|Wählen Sie eine verfügbare Ressourcengruppe aus|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wird. Nachdem sie erstellt wurde, navigieren Sie zur Ressourcenseite.
1. Erfassen Sie den konfigurierten `{ENDPOINT_URI}` und `{API_KEY}`. Weitere Informationen finden Sie unter [Ermitteln erforderlicher Parameter](../computer-vision-how-to-install-containers.md#gathering-required-parameters).

---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: c202ba1d7363af9791daa801f0c447c49a80859b
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2020
ms.locfileid: "85378498"
---
>[!NOTE]
> Nach dem 1. Juli 2019 erstellte Endpunkte nutzen das unten gezeigte benutzerdefinierte Format für Subdomänen. Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie im [Azure-Portal](../../cognitive-services-apis-create-account.md) eine Ressource für die Freihanderkennung.

Rufen Sie nach dem Erstellen einer Ressource den Endpunkt und den Schlüssel ab, indem Sie die Ressource im [Azure-Portal](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade) öffnen und auf **Schnellstart** klicken.

Erstellen Sie zwei [Umgebungsvariablen](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource):

* `INK_RECOGNITION_SUBSCRIPTION_KEY`: Der Endpunkt für die Ressource. Er sieht wie folgt aus: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

* `INK_RECOGNITION_ENDPOINT`: Der Abonnementschlüssel zum Authentifizieren Ihrer Anforderungen   

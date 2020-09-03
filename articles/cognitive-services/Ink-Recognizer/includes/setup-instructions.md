---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 9d46b304d598b4830cf325909f77eea6b68af757
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304033"
---
>[!NOTE]
> Nach dem 1. Juli 2019 erstellte Endpunkte nutzen das unten gezeigte benutzerdefinierte Format für Subdomänen. Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie im [Azure-Portal](../../cognitive-services-apis-create-account.md) eine Ressource für die Freihanderkennung.

Rufen Sie nach dem Erstellen einer Ressource den Endpunkt und den Schlüssel ab, indem Sie die Ressource im [Azure-Portal](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade) öffnen und auf **Schnellstart** klicken.

Erstellen Sie zwei [Umgebungsvariablen](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource):

* `INK_RECOGNITION_SUBSCRIPTION_KEY`: Der Abonnementschlüssel zum Authentifizieren Ihrer Anforderungen 

* `INK_RECOGNITION_ENDPOINT`: Der Endpunkt für die Ressource. Er sieht wie folgt aus: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com`   

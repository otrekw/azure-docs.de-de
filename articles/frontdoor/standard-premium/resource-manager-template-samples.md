---
title: 'Beispiele für Resource Manager-Vorlagen: Azure Front Door'
description: Hier finden Sie Informationen zu Azure Resource Manager-Beispielvorlagen für Azure Front Door.
services: frontdoor
author: johndowns
ms.author: jodowns
ms.service: frontdoor
ms.topic: sample
ms.date: 04/16/2021
ms.openlocfilehash: e939aec8f1cf5fa615904bb81bad82ea3cf22c61
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107587475"
---
# <a name="azure-resource-manager-templates-for-azure-front-door"></a>Azure Resource Manager-Vorlagen für Azure Front Door

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für Azure Front Door mit Referenzarchitekturen, die andere Azure-Dienste beinhalten.

| Beispiel | BESCHREIBUNG |
|-|-|
| [Front Door (Schnellerstellung)](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium/) | Erstellt ein einfaches Front Door-Profil, einschließlich Endpunkt, Ursprungsgruppe, Ursprung und Route.  |
| [Regelsatz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-rule-set/) | Erstellt ein Front Door-Profil und einen Regelsatz.  |
| [WAF-Richtlinie mit verwaltetem Regelsatz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-waf-managed/) | Erstellt ein Front Door-Profil und eine WAF mit verwaltetem Regelsatz.  |
| [WAF-Richtlinie mit benutzerdefinierter Regel](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-waf-custom/) | Erstellt ein Front Door-Profil und eine WAF mit benutzerdefinierter Regel.  |
|**App Service-Ursprünge**| **Beschreibung** |
| [App Service](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-app-service-public) | Erstellt eine App Service-App mit einem öffentlichen Endpunkt und ein Front Door-Profil.  |
| [App Service mit Private Link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-app-service-private-link) | Erstellt eine App Service-App mit einem privaten Endpunkt und ein Front Door-Profil.  |
| [App Service-Umgebung mit Private Link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-app-service-environment-internal-private-link) | Erstellt eine App Service-Umgebung, eine App mit einem privaten Endpunkt und ein Front Door-Profil.  |
|**Azure Functions-Ursprünge**| **Beschreibung** |
| [Azure-Funktionen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-function-public/) | Erstellt eine App Functions-App mit einem öffentlichen Endpunkt und ein Front Door-Profil.  |
| [Azure Functions mit Private Link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-function-private-link) | Erstellt eine App Functions-App mit einem privaten Endpunkt und ein Front Door-Profil.  |
|**API Management-Ursprünge**| **Beschreibung** |
| [API Management (extern)](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-api-management-external) | Erstellt eine API Management-Instanz mit externer VNet-Integration und ein Front Door-Profil.  |
|**Storage-Ursprünge**| **Beschreibung** |
| [Statische Speicherwebsite](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-storage-static-website) | Erstellt ein Azure Storage-Konto und eine statische Website mit einem öffentlichen Endpunkt sowie ein Front Door-Profil.  |
| [Storage-Blobs mit Private Link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-storage-blobs-private-link) | Erstellt ein Azure Storage-Konto und einen Blobcontainer mit einem privaten Endpunkt sowie ein Front Door-Profil.  |
|**Application Gateway-Ursprünge**| **Beschreibung** |
| [Application Gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-application-gateway-public) | Erstellt eine Application Gateway-Instanz und ein Front Door-Profil. |
|**VM-Ursprünge**| **Beschreibung** |
| [Virtueller Computer mit Private Link-Dienst](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-vm-private-link) | Erstellt einen virtuellen Computer und einen Private Link-Dienst sowie ein Front Door-Profil. |
| | |

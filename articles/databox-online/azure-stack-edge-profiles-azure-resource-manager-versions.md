---
title: Ressourcenanbieter und Versionsnummern für API-Profile in Azure Stack Edge | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Azure Resource Manager-API-Versionen, die von Profilen in Azure Stack Edge unterstützt werden.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/17/2021
ms.author: alkohli
ms.openlocfilehash: 485b1f8dccf104a9d31abb9aa79fd1ea4eb3940a
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111965355"
---
# <a name="resource-provider-api-profile-versions-for-azure-stack-edge"></a>Ressourcenanbieter und Versionsnummern für API-Profile in Azure Stack Edge


[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel können Sie den Ressourcenanbieter und die Versionsnummern für jedes API-Profil finden, das von Azure Stack Edge verwendet wird. In den Tabellen in diesem Artikel sind die Versionen aufgeführt, die für die einzelnen Ressourcenanbieter und die API-Versionen der Profile unterstützt werden. Jeder Ressourcenanbieter enthält einen Satz mit Ressourcentypen und spezifischen Versionsnummern.

Für das API-Profil gelten drei Benennungskonventionen:

- **Neueste**
- **jjjj-mm-tt-hybrid**
- **jjjj-mm-tt-profile**


## <a name="overview-of-the-2019-03-01-hybrid-profile"></a>Übersicht über das Profil „2019-03-01-hybrid“

|Ressourcenanbieter                                   |API-Version |
|----------------------------------------------------|------------|
|Microsoft.Compute                                   |2017-12-01  |
|Microsoft.Network                                   |2017-10-01<br>VPN Gateway: 2017-10-01 |
|Microsoft.Storage (Datenebene)                      |2019-07-07  |
|Microsoft.Storage (Steuerungsebene)                   |2019-06-01  |
|Microsoft.Resources (Azure Resource Manager selbst) |2020-06-01  |
|Microsoft.Authorization (Richtlinienvorgänge)         |2016-09-01  |

Eine Liste mit den Versionen jedes einzelnen Ressourcentyps für die Anbieter im API-Profil finden Sie unter [Details zum Profil „2019-03-01-hybrid“](/azure-stack/user/azure-stack-profiles-azure-resource-manager-versions?view=azs-2008&preserve-view=true#details-for-the-2019-03-01-hybrid-profile).

## <a name="details-for-the-2019-03-01-hybrid-profile"></a>Details zum Profil „2019-03-01-hybrid“

### <a name="microsoftcompute"></a>Microsoft.Compute

Die Azure Compute-APIs ermöglichen Ihnen den programmgesteuerten Zugriff auf virtuelle Computer und die zugehörigen unterstützenden Ressourcen. Weitere Informationen finden Sie unter [Azure Compute](/rest/api/compute/).

|Ressourcentyp               |API-Version |
|----------------------------|------------|
|Speicherorte                   |2017-12-01  |
|Speicherorte/VM-Größen           |2017-12-01  |
|Virtual Machines            |2017-12-01  |
|Virtuelle Computer/Erweiterungen |2017-12-01  |

### <a name="microsoftnetwork"></a>Microsoft.Network

Das Ergebnis des Vorgangsaufrufs ist eine Darstellung der Liste verfügbarer Netzwerkcloudvorgänge. Weitere Informationen finden Sie unter [REST-API für Vorgänge](/rest/api/operation/).

|Ressourcentypen     |API-Versionen|
|-------------------|------------|
|Netzwerkschnittstellen |2017-10-01  |
|Virtuelle Netzwerke   |2017-10-01  |

### <a name="microsoftresources"></a>Microsoft.Resources

Mit Azure Resource Manager können Sie die Infrastruktur für Ihre Azure-Lösungen bereitstellen und verwalten. Sie organisieren zugehörige Ressourcen in Ressourcengruppen und stellen Ihre Ressourcen mit JSON-Vorlagen bereit. Eine Einführung in die Bereitstellung und Verwaltung von Ressourcen mit Resource Manager finden Sie in der [Übersicht über Azure Resource Manager](../azure-resource-manager/management/overview.md).

|Ressourcentypen                        |API-Versionen|
|--------------------------------------|------------|
|Bereitstellungen                           |2020-06-01  |
|Bereitstellungen/Vorgänge                |2020-06-01  |
|Links                                 |2020-06-01  |
|Speicherorte                             |2020-06-01  |
|Operations                            |2020-06-01  |
|Anbieter                             |2020-06-01  |
|ResourceGroups                        |2020-06-01  |
|Ressourcen                             |2020-06-01  |
|Abonnements                         |2018-09-01  |
|Subscriptions/locations               |2018-09-01  |
|Subscriptions/operationresults        |2020-06-01  |
|Abonnements/Anbieter               |2020-06-01  |
|Subscriptions/ResourceGroups          |2020-06-01  |
|Abonnements/Ressourcengruppen/Ressourcen|2020-06-01  |
|Abonnements/Ressourcen               |2020-06-01  |
|Abonnements/Tagnamen                |2020-06-01  |
|Abonnements/Tagnamen/Tagwerte      |2020-06-01  |
|Mandanten                               |2018-09-01  |

### <a name="microsoftstorage"></a>Microsoft.Storage

Mit dem Speicherressourcenanbieter (Storage Resource Provider, SRP) können Sie Ihr Speicherkonto und Ihre Schlüssel programmgesteuert verwalten. Weitere Informationen finden Sie in der [Referenz zur Ressourcenanbieter-REST-API für Azure Storage](/rest/api/storagerp/).

|Ressourcentypen       |API-Versionen|
|---------------------|------------|
|CheckNameAvailability|2019-06-01  |
|Speicherorte            |2019-06-01  |
|Speicherorte/Kontingente     |2019-06-01  |
|Operations           |2019-06-01  |
|Speicherkonten      |2019-06-01  |
|Verwendungen               |2019-06-01  |

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten eines Azure Stack Edge Pro-GPU-Geräts mithilfe von Windows PowerShell](azure-stack-edge-gpu-connect-powershell-interface.md)
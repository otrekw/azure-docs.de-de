---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 3c9679f3d66d58c7a6c847b54c84438c979ecd39
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936860"
---
Ein [privater Azure-Endpunkt](../articles/private-link/private-endpoint-overview.md) ist eine Netzwerkschnittstelle, die Sie privat und sicher mit einem Dienst verbindet, der von Azure Private Link unterstützt wird.  Ein privater Endpunkt verwendet eine private IP-Adresse aus Ihrem virtuellen Netzwerk und macht den Dienst so in Ihrem virtuellen Netzwerk verfügbar.

Sie können einen privaten Endpunkt für ihre Funktionen verwenden, die in den Plänen [Premium](../articles/azure-functions/functions-premium-plan.md) und [App Service](../articles/azure-functions/dedicated-plan.md) gehostet werden.

Wenn Sie eine eingehende private Endpunktverbindung für Funktionen erstellen, benötigen Sie auch einen DNS-Eintrag, um die private Adresse aufzulösen.  Standardmäßig wird ein privater DNS-Eintrag für Sie erstellt, wenn Sie einen privaten Endpunkt mithilfe des Azure-Portals erstellen.

Weitere Informationen finden Sie unter [Verwenden privater Endpunkte für Web-Apps](../articles/app-service/networking/private-endpoint.md).
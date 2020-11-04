---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 10/26/2020
ms.author: larryfr
ms.openlocfilehash: 1fde2947719079e411bc233bcce426feec8fa996
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92631041"
---
> [!IMPORTANT]
> Die Cosmos DB-Instanz wird zusammen mit allen benötigten Ressourcen in einer von Microsoft verwalteten Ressourcengruppe in __Ihrem Abonnement__ erstellt. Dies bedeutet, dass Ihnen diese Cosmos DB-Instanz in Rechnung gestellt wird. Die verwaltete Ressourcengruppe wird im Format `<AML Workspace Resource Group Name><GUID>` benannt. Wenn im Azure Machine Learning-Arbeitsbereich ein privater Endpunkt verwendet wird, wird auch ein virtuelles Netzwerk für die Cosmos DB-Instanz erstellt. Dieses VNet wird zum Sichern der Kommunikation zwischen Cosmos DB und Azure Machine Learning verwendet.
> 
> * __Löschen Sie weder die Ressourcengruppe__ , die diese Cosmos DB-Instanz enthält, noch Ressourcen, die automatisch in dieser Gruppe erstellt wurden. Wenn Sie die Ressourcengruppe, die Cosmos DB-Instanz usw. löschen müssen, müssen Sie den Azure Machine Learning-Arbeitsbereich löschen, der diese verwendet. Die Ressourcengruppe, die Cosmos DB-Instanz und andere automatisch erstellte Ressourcen werden gelöscht, wenn der zugehörige Arbeitsbereich gelöscht wird.
> * Die standardmäßigen [__Anforderungseinheiten__](../articles/cosmos-db/request-units.md) für dieses Cosmos-DB-Konto sind auf __8000__ festgelegt. __Das Ändern dieses Werts wird nicht unterstützt.__
> * Sie __können kein eigenes VNet für die Verwendung mit der erstellten Cosmos DB-Instanz bereitstellen__. Außerdem __können Sie das virtuelle Netzwerk nicht ändern__. Beispielsweise können Sie den verwendeten IP-Adressbereich nicht ändern.
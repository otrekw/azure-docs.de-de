---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 04/21/2021
ms.author: larryfr
ms.openlocfilehash: 6d9b6fe18d7eb4ce532c6c2960944420eee952f0
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107903960"
---
> [!IMPORTANT]
> Diese Cosmos DB-Instanz wird in einer von Microsoft verwalteten Ressourcengruppe in __Ihrem Abonnement__ erstellt. Die folgenden Dienste werden ebenfalls in dieser Ressourcengruppe erstellt und von der Konfiguration des kundenseitig verwalteten Schlüssels verwendet:
> * Azure Storage-Konto
> * Azure Search
>
> Da diese Dienste in Ihrem Azure-Abonnement erstellt werden, bedeutet dies, dass Ihnen diese Dienstinstanzen in Rechnung gestellt werden. 
>
> Die verwaltete Ressourcengruppe wird im Format `<AML Workspace Resource Group Name><GUID>` benannt. Wenn im Azure Machine Learning-Arbeitsbereich ein privater Endpunkt verwendet wird, wird in dieser Ressourcengruppe auch ein virtuelles Netzwerk erstellt. Dieses VNet wird verwendet, um die Kommunikation zwischen den Diensten in dieser Ressourcengruppe und Ihrem Azure Machine Learning-Arbeitsbereich zu sichern.
> 
> * __Löschen Sie weder die Ressourcengruppe__, die diese Cosmos DB-Instanz enthält, noch Ressourcen, die automatisch in dieser Gruppe erstellt wurden. Wenn Sie die Ressourcengruppe, die Cosmos DB-Instanz usw. löschen müssen, müssen Sie den Azure Machine Learning-Arbeitsbereich löschen, der diese verwendet. Die Ressourcengruppe, die Cosmos DB-Instanz und andere automatisch erstellte Ressourcen werden gelöscht, wenn der zugehörige Arbeitsbereich gelöscht wird.
> * Die standardmäßigen [__Anforderungseinheiten__](../articles/cosmos-db/request-units.md) für dieses Cosmos-DB-Konto sind auf __8000__ festgelegt.
> * Sie __können kein eigenes VNet für die Verwendung mit der erstellten Cosmos DB-Instanz bereitstellen__. Außerdem __können Sie das virtuelle Netzwerk nicht ändern__. Beispielsweise können Sie den verwendeten IP-Adressbereich nicht ändern.
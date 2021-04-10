---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 12/01/2020
ms.author: larryfr
ms.openlocfilehash: e92d3ac9ed4330cc1680428a426e881538cb0e78
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102623563"
---
* Wenn Sie einen neuen Arbeitsbereich erstellen, können Sie die vom Arbeitsbereich benötigten Dienste automatisch erstellen oder vorhandene Dienste verwenden. Wenn Sie __vorhandene Dienste aus einem anderen Azure-Abonnement__ als dem des Arbeitsbereichs verwenden möchten, müssen Sie den Azure Machine Learning-Namespace in dem Abonnement mit diesen Dienst registrieren. Wenn Sie z. B. einen Arbeitsbereich in Abonnement A erstellen, der ein Speicherkonto aus Abonnement B verwendet, muss der Azure Machine Learning-Namespace in Abonnement B registriert werden, damit Sie das Speicherkonto mit dem Arbeitsbereich verwenden können.

    Der Ressourcenanbieter für Machine Learning ist __Microsoft.MachineLearningServices__. Informationen zum Überprüfen und Durchführen der Registrierung finden Sie im Artikel [Azure-Ressourcenanbieter und -typen](../articles/azure-resource-manager/management/resource-providers-and-types.md).

    > [!IMPORTANT]
    > Dies betrifft für Ressourcen, die beim Erstellen des Arbeitsbereichs bereitgestellt werden, also Azure Storage-Konten, Azure Container Registry, Azure Key Vault und Application Insights.

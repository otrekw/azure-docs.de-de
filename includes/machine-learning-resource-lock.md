---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 10/16/2020
ms.author: larryfr
ms.openlocfilehash: 25e304daf75b60a7d037640d496ed0972581f13a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92204460"
---
Azure ermöglicht Ihnen das Einrichten von _Sperren_ für Ressourcen, damit diese nicht gelöscht werden können oder schreibgeschützt sind. __Das Sperren einer Ressource kann zu unerwarteten Ergebnissen führen.__ Bei einigen Vorgängen, die die Ressource nicht zu ändern scheinen, sind tatsächlich Aktionen erforderlich, die von der Sperre blockiert werden. 

Wenn Sie beispielsweise eine Löschsperre auf die Ressourcengruppe für Ihren Arbeitsbereich anwenden, werden Skalierungsvorgänge für Azure ML-Computecluster unterbunden.

Weitere Informationen zum Sperren von Ressourcen finden Sie unter [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](../articles/azure-resource-manager/management/lock-resources.md).
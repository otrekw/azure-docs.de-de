---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 09/01/2020
ms.author: tomfitz
ms.openlocfilehash: 543aa50d72de5a06a9a1c7ac88ac5ecae993bc9d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98697902"
---
| Resource | Begrenzung |
| --- | --- |
| Ressourcen pro [Ressourcengruppe](../articles/azure-resource-manager/management/overview.md#resource-groups) | Ressourcen sind nicht durch die Ressourcengruppe beschränkt. Sie sind stattdessen durch den Ressourcentyp in einer Ressourcengruppe beschränkt. Weitere Informationen finden Sie in der nächsten Zeile. |
| Ressourcen pro Ressourcengruppe pro Ressourcentyp |800: Einige Ressourcentypen können den Grenzwert von 800 überschreiten. Weitere Informationen finden Sie unter [Ressourcen ohne Beschränkung auf 800 Instanzen pro Ressourcengruppe](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md). |
| Bereitstellungen pro Ressourcenverlauf im Bereitstellungsverlauf |800<sup>1</sup> |
| Ressourcen pro Bereitstellung |800 |
| Managementsperren pro eindeutigem [Bereich](../articles/azure-resource-manager/management/overview.md#understand-scope)  |20 |
| Anzahl Tags pro Ressource oder Ressourcengruppe |50 |
| Tagschlüssellänge |512 |
| Tagwertlänge |256 |

<sup>1</sup>Bereitstellungen werden automatisch aus dem Verlauf gelöscht, wenn Sie den Grenzwert fast erreicht haben. Das Löschen eines Eintrags aus dem Bereitstellungsverlauf wirkt sich nicht auf die bereitgestellten Ressourcen aus. Weitere Informationen finden Sie unter [Automatische Löschungen aus dem Bereitstellungsverlauf](../articles/azure-resource-manager/templates/deployment-history-deletions.md).

#### <a name="template-limits"></a>Vorlagengrenzwerte

| Wert | Begrenzung |
| --- | --- |
| Parameter |256 |
| Variables |256 |
| Ressourcen (einschließlich copy-Anzahl) |800 |
| Ausgaben |64 |
| Vorlagenausdruck |24.576 Zeichen |
| Ressourcen in exportierten Vorlagen |200 |
| Vorlagengröße |4 MB |
| Parameterdateigröße |64 KB |

Sie können einige Vorlagengrenzwerte überschreiten, indem Sie eine geschachtelte Vorlage verwenden. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](../articles/azure-resource-manager/templates/linked-templates.md). Um die Anzahl von Parametern, Variablen oder Ausgaben zu reduzieren, können Sie mehrere Werte in einem Objekt kombinieren. Weitere Informationen finden Sie unter [Objekte als Parameter](/azure/architecture/guide/azure-resource-manager/advanced-templates/objects-as-parameters).
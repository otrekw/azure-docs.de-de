---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/24/2020
ms.author: tomfitz
ms.openlocfilehash: c1a52e1bc2dde74289cb270fcae832be24de5a06
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77592420"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Ressourcen pro [Ressourcengruppe](../articles/azure-resource-manager/management/overview.md#resource-groups) | – | Ressourcen sind nicht durch die Ressourcengruppe beschränkt. Sie sind stattdessen durch den Ressourcentyp in einer Ressourcengruppe beschränkt. Weitere Informationen finden Sie in der nächsten Zeile. | 
| Ressourcen pro Ressourcengruppe pro Ressourcentyp |800 |Einige Ressourcentypen können den Grenzwert von 800 überschreiten. Weitere Informationen finden Sie unter [Ressourcen ohne Beschränkung auf 800 Instanzen pro Ressourcengruppe](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md). |
| Bereitstellungen pro Ressourcenverlauf im Bereitstellungsverlauf |800<sup>1</sup> |800 |
| Ressourcen pro Bereitstellung |800 |800 |
| Managementsperren pro eindeutigem Bereich |20 |20 |
| Anzahl Tags pro Ressource oder Ressourcengruppe |50 |50 |
| Tagschlüssellänge |512 |512 |
| Tagwertlänge |256 |256 |

<sup>1</sup>Wenn der Grenzwert von 800 Bereitstellungen pro Ressourcengruppe erreicht ist, löschen Sie nicht mehr benötigte Bereitstellungen aus dem Verlauf. Das Löschen eines Eintrags aus dem Bereitstellungsverlauf wirkt sich nicht auf die bereitgestellten Ressourcen aus. Weitere Informationen finden Sie unter [Beheben des Fehlers, dass die Anzahl der Bereitstellungen 800 überschreitet](../articles/azure-resource-manager/templates/deployment-quota-exceeded.md).

#### <a name="template-limits"></a>Vorlagengrenzwerte

| value | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Parameter |256 |256 |
| Variables |256 |256 |
| Ressourcen (einschließlich copy-Anzahl) |800 |800 |
| Ausgaben |64 |64 |
| Vorlagenausdruck |24.576 Zeichen |24.576 Zeichen |
| Ressourcen in exportierten Vorlagen |200 |200 | 
| Vorlagengröße |4 MB |4 MB |
| Parameterdateigröße |64 KB |64 KB |

Sie können einige Vorlagengrenzwerte überschreiten, indem Sie eine geschachtelte Vorlage verwenden. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](../articles/azure-resource-manager/templates/linked-templates.md). Um die Anzahl von Parametern, Variablen oder Ausgaben zu reduzieren, können Sie mehrere Werte in einem Objekt kombinieren. Weitere Informationen finden Sie unter [Objekte als Parameter](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

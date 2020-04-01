---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/24/2020
ms.author: tomfitz
ms.openlocfilehash: c883383d3c870689bb95f808f6f60c5185c165c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334660"
---
| Resource | Begrenzung |
| --- | --- |
| Ressourcen pro [Ressourcengruppe](../articles/azure-resource-manager/management/overview.md#resource-groups) | Ressourcen sind nicht durch die Ressourcengruppe beschränkt. Sie sind stattdessen durch den Ressourcentyp in einer Ressourcengruppe beschränkt. Weitere Informationen finden Sie in der nächsten Zeile. |
| Ressourcen pro Ressourcengruppe pro Ressourcentyp |800: Einige Ressourcentypen können den Grenzwert von 800 überschreiten. Weitere Informationen finden Sie unter [Ressourcen ohne Beschränkung auf 800 Instanzen pro Ressourcengruppe](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md). |
| Bereitstellungen pro Ressourcenverlauf im Bereitstellungsverlauf |800<sup>1</sup> |
| Ressourcen pro Bereitstellung |800 |
| Managementsperren pro eindeutigem Bereich |20 |
| Anzahl Tags pro Ressource oder Ressourcengruppe |50 |
| Tagschlüssellänge |512 |
| Tagwertlänge |256 |

<sup>1</sup>Wenn der Grenzwert von 800 Bereitstellungen pro Ressourcengruppe erreicht ist, löschen Sie nicht mehr benötigte Bereitstellungen aus dem Verlauf. Das Löschen eines Eintrags aus dem Bereitstellungsverlauf wirkt sich nicht auf die bereitgestellten Ressourcen aus. Weitere Informationen finden Sie unter [Beheben des Fehlers, dass die Anzahl der Bereitstellungen 800 überschreitet](../articles/azure-resource-manager/templates/deployment-quota-exceeded.md).

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

Sie können einige Vorlagengrenzwerte überschreiten, indem Sie eine geschachtelte Vorlage verwenden. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](../articles/azure-resource-manager/templates/linked-templates.md). Um die Anzahl von Parametern, Variablen oder Ausgaben zu reduzieren, können Sie mehrere Werte in einem Objekt kombinieren. Weitere Informationen finden Sie unter [Objekte als Parameter](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

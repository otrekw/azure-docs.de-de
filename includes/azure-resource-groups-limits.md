---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 09/01/2020
ms.author: tomfitz
ms.openlocfilehash: c29abc456c5825a2f22b20a4893bb14eb09058af
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107984454"
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
| Parameterdateigröße |4 MB |

Sie können einige Vorlagengrenzwerte überschreiten, indem Sie eine geschachtelte Vorlage verwenden. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](../articles/azure-resource-manager/templates/linked-templates.md). Um die Anzahl von Parametern, Variablen oder Ausgaben zu reduzieren, können Sie mehrere Werte in einem Objekt kombinieren. Weitere Informationen finden Sie unter [Objekte als Parameter](/azure/architecture/guide/azure-resource-manager/advanced-templates/objects-as-parameters).

Bei einer Vorlagen- oder Parameterdatei mit einer Größe von weniger als 4 MB wird unter Umständen ein Fehler angezeigt, wenn die Anforderung insgesamt zu groß ist. Weitere Informationen zum Vereinfachen der Vorlage, um eine große Anforderung zu vermeiden, finden Sie unter [Beheben von Fehlern des Typs „Auftragsgröße überschritten“](../articles/azure-resource-manager/templates/error-job-size-exceeded.md).
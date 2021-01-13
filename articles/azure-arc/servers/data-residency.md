---
title: Datenresidenz
description: Datenresidenz und Informationen zu Azure Arc-fähigen Servern.
ms.topic: reference
ms.date: 10/08/2020
ms.custom: references_regions
ms.openlocfilehash: c5ece96acc3ee07ba2896279888363c7d52d737e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91856448"
---
# <a name="azure-arc-enabled-servers-data-residency"></a>Server mit Azure Arc-Unterstützung: Datenresidenz

In diesem Artikel wird das Konzept der Datenresidenz und dessen Anwendung auf Azure Arc-fähigen Servern erläutert.

Azure Arc-fähige Server sind in den **USA, in Europa, dem Vereinigten Königreich, Australien und der Asien-Pazifik-Region** **[verfügbar](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc)** .

## <a name="data-residency"></a>Datenresidenz

Azure Arc-fähige Server speichern Konfigurationseinstellungen (d. h. Eigenschaftswerte) der [Azure-VM-Erweiterung](manage-vm-extensions.md), die für die Erweiterung angegeben werden müssen, bevor Sie versuchen, sie auf dem verbundenen Computer zu aktivieren. Wenn Sie z. B. die Log Analytics-VM-Erweiterung aktivieren, werden Sie nach der **Arbeitsbereichs-ID** und dem **Primärschlüssel** von Log Analytics gefragt.

Metadateninformationen zum verbundenen Computer werden ebenfalls gesammelt. Dies betrifft insbesondere:

* Betriebssystemname und -version
* Computername
* Vollqualifizierter Domänenname (FQDN) des Computers
* Version des Connected Machine-Agents

Bei Azure Arc-fähigen Servern können Sie die Region angeben, in der Ihre Daten gespeichert werden sollen. Microsoft kann die Daten in Hinblick auf die Datenresilienz in andere Regionen replizieren; Microsoft repliziert aber keine Daten außerhalb des geografischen Gebiets oder verschiebt Daten in diesen Raum. Diese Daten werden in der Region gespeichert, in der die Azure Arc-Computerressource konfiguriert wird. Wenn der Computer z. B. mit Azure Arc in der Region „USA, Osten“ registriert ist, werden diese Daten in der Region „USA“ gespeichert.

Weitere Informationen zur Unterstützung der regionalen Resilienz und Compliance finden Sie unter [Azure-Geographie](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwerfen für [Azure-Resilienz](/azure/architecture/reliability/architect).
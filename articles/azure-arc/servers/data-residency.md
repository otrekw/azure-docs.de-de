---
title: Datenresidenz
description: Datenresidenz und Informationen zu Azure Arc-fähigen Servern (Vorschau).
ms.topic: reference
ms.date: 08/25/2020
ms.custom: references_regions
ms.openlocfilehash: 8f207f5889c1764eebcc6081960ff70c0d5bca3a
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89048855"
---
# <a name="azure-arc-enabled-servers-preview-data-residency"></a>Azure Arc-fähige Server (Vorschau): Datenresidenz

In diesem Artikel wird das Konzept der Datenresidenz und dessen Anwendung auf Azure Arc-fähigen Servern (Vorschau) erläutert.

Azure Arc-fähige Server (Vorschau) sind **in den USA, in Europa oder in der Asien-Pazifik-Region** **[in der Vorschau verfügbar](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc)** .

## <a name="data-residency"></a>Datenresidenz

Azure Arc-fähige Server (Vorschau) speichern Konfigurationseinstellungen (d. h. Eigenschaftswerte) der [Azure-VM-Erweiterung](manage-vm-extensions.md), die für die Erweiterung angegeben werden müssen, bevor Sie versuchen, sie auf dem verbundenen Computer zu aktivieren. Wenn Sie z. B. die Log Analytics-VM-Erweiterung aktivieren, werden Sie nach der **Arbeitsbereichs-ID** und dem **Primärschlüssel** von Log Analytics gefragt.

Metadateninformationen zum verbundenen Computer werden ebenfalls gesammelt. Dies betrifft insbesondere:

* Betriebssystemname und -version
* Computername
* Vollqualifizierter Domänenname (FQDN) des Computers
* Version des Connected Machine-Agents

Bei Azure Arc-fähigen Servern (Vorschau) können Sie die Region angeben, in der Ihre Daten gespeichert werden sollen. Microsoft kann die Daten in Hinblick auf die Datenresilienz in andere Regionen replizieren; Microsoft repliziert aber keine Daten außerhalb des geografischen Gebiets oder verschiebt Daten in diesen Raum. Diese Daten werden in der Region gespeichert, in der die Azure Arc-Computerressource konfiguriert wird. Wenn der Computer z. B. mit Azure Arc in der Region „USA, Osten“ registriert ist, werden diese Daten in der Region „USA“ gespeichert.

Weitere Informationen zur Unterstützung der regionalen Resilienz und Compliance finden Sie unter [Azure-Geographie](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwerfen für [Azure-Resilienz](/azure/architecture/reliability/architect).
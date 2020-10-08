---
title: Auswählen eines Tools für das regionsübergreifende Verschieben von Azure-Ressourcen
description: Überprüfen der Optionen und Tools für das regionsübergreifende Verschieben von Azure-Ressourcen
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: overview
ms.date: 09/09/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 1b233028b52175842c73660ff116ac592d8296e0
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90603355"
---
# <a name="choose-a-tool-for-moving-azure-resources"></a>Auswählen eines Tools für das Verschieben von Azure-Ressourcen

Sie können Ressourcen innerhalb von Azure folgendermaßen verschieben:

- **Regionsübergreifendes Verschieben von Ressourcen:** Sie können Ressourcen innerhalb des Resource Mover-Hubs oder innerhalb einer Ressourcengruppe verschieben. 
- **Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement:** Sie können Ressourcen innerhalb einer Ressourcengruppe verschieben. 
- **Verschieben von Ressourcen zwischen Azure-Clouds:** Mithilfe des Azure Site Recovery-Diensts können Sie Ressourcen zwischen öffentlichen Clouds und denen von Behörden verschieben.
- **Verschieben von Ressourcen zwischen Verfügbarkeitszonen derselben Region:** Mithilfe des Azure Site Recovery-Diensts können Sie Ressourcen zwischen Verfügbarkeitszonen in derselben Azure-Region verschieben.


## <a name="compare-move-tools"></a>Vergleich der Tools für das Verschieben

**Tool** | **Einsatzgebiete** | **Weitere Informationen**
--- | --- | ---
**Verschieben innerhalb einer Ressourcengruppe** | Sie möchten Ressourcen in eine andere Ressourcengruppe/in ein anderes Abonnement oder regionsübergreifend verschieben.<br/><br/> Wenn Sie regionsübergreifend verschieben, wählen Sie die gewünschten Ressourcen in der Ressourcengruppe aus und verschieben diese dann in den Resource Mover-Hub, um Abhängigkeiten zu überprüfen und die Ressourcen in die Zielregion zu verschieben. | [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md)<br/><br/> [Regionsübergreifendes Verschieben von Ressourcen (aus Ressourcengruppe)](move-region-within-resource-group.md)
**Verschieben im Resource Mover-Hub** | Sie möchten Ressourcen regionsübergreifend verschieben. <br/><br/> Sie können Ressourcen in eine Zielregion oder in eine bestimmte Verfügbarkeitszone, eine bestimmte Verfügbarkeitsgruppe oder innerhalb der Zielregion verschieben. | [Auswählen eines Tools für das Verschieben von Azure-Ressourcen]()
**Verschieben virtueller Computer mit Site Recovery** | Sie möchten Azure-VMs zwischen Clouds von Behörden und öffentlichen Clouds verschieben.<br/><br/> Sie möchten VMs zwischen Verfügbarkeitszonen in derselben Region verschieben. |[Verschieben von virtuellen Azure IaaS-Computern zwischen Azure Government und öffentlichen Regionen](../site-recovery/region-move-cross-geos.md), [Aktivieren der Notfallwiederherstellung für virtuelle Azure-Computer zwischen Verfügbarkeitszonen](../site-recovery/azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery.md)

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](about-move-process.md) über Resource Mover-Komponenten und den Verschiebungsvorgang.

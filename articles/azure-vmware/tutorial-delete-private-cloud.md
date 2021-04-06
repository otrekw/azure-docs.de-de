---
title: 'Tutorial: Löschen einer privaten Azure VMware Solution-Cloud'
description: Es wird beschrieben, wie Sie eine private Azure VMware Solution-Cloud löschen, die Sie nicht mehr benötigen.
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: 453e7a3316c342cd724a951eafea0ae9fa045506
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103462098"
---
# <a name="tutorial-delete-an-azure-vmware-solution-private-cloud"></a>Tutorial: Löschen einer privaten Azure VMware Solution-Cloud

Wenn Sie über eine private Azure VMware Solution-Cloud verfügen, die Sie nicht mehr benötigen, können Sie sie löschen. Die private Cloud umfasst eine isolierte Netzwerkdomäne, mindestens einen bereitgestellten vSphere-Cluster auf dedizierten Serverhosts und mehrere virtuelle Computer (VMs). Wenn Sie eine private Cloud löschen, werden alle VMs, die darauf enthaltenen Daten und die Cluster gelöscht. Die dedizierten Azure VMware Solution-Hosts werden auf sichere Weise bereinigt und an den freien Pool zurückgegeben. Der bereitgestellte Netzwerkadressraum wird ebenfalls gelöscht.  

> [!CAUTION]
> Das Löschen der privaten Cloud kann nicht rückgängig gemacht werden. Die Daten können nach dem Löschen der privaten Cloud nicht wiederhergestellt werden, da alle aktiven Workloads und Komponenten beendet und sämtliche Daten und Konfigurationseinstellungen der privaten Cloud zerstört werden (einschließlich der öffentlichen IP-Adressen).

## <a name="prerequisites"></a>Voraussetzungen

Falls Sie die VMs und die darauf enthaltenen Daten später noch benötigen, sollten Sie die Daten sichern, bevor Sie die private Cloud löschen.  Es gibt keine Möglichkeit, die VMs und die zugehörigen Daten wiederherzustellen.


## <a name="delete-the-private-cloud"></a>Löschen der privaten Cloud

1. Greifen Sie im [Azure-Portal](https://portal.azure.com) auf die Azure VMware Solution-Konsole zu.

2. Wählen Sie die zu löschende private Cloud aus.
 
3. Geben Sie den Namen der privaten Cloud ein, und wählen Sie **Ja** aus. 

>[!NOTE]
>Der Löschvorgang dauert einige Stunden.  

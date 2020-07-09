---
title: Verwenden des Portals zum Bereitstellen von Azure Spot-VMs
description: Erfahren Sie, wie Sie Spot-VMs mit Azure PowerShell bereitstellen, um Kosten zu sparen.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 03/25/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 6e7723a437e90807063e3c3b7af2bf068dca5b9f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82100650"
---
# <a name="deploy-spot-vms-using-the-azure-portal"></a>Bereitstellen von Spot-VMs mithilfe des Azure-Portals

Mithilfe von [Spot-VMs](spot-vms.md) können Sie unsere ungenutzte Kapazität mit signifikanten Kosteneinsparungen nutzen. Wenn die Kapazität von Azure wieder benötigt wird, werden die Spot-VMs durch die Azure-Infrastruktur entfernt. Aus diesem Grund eignen sich Spot-VMs hervorragend für Workloads, die Unterbrechungen tolerieren, z. B. Batchverarbeitungsaufträge, Dev/Test-Umgebungen, umfangreiche Computeworkloads und mehr.

Die Preise für Spot-VMs variieren je nach Region und SKU. Weitere Informationen finden Sie unter den VM-Preisen für [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Weitere Informationen zum Festlegen des maximalen Preises finden Sie unter [Spot-VMs – Preise](spot-vms.md#pricing).

Sie haben die Möglichkeit, einen maximalen Preis festzulegen, den Sie pro Stunde für die VM bezahlen möchten. Der maximale Preis für eine Spot-VM kann in US-Dollar (USD) mit bis zu fünf Dezimalstellen festgelegt werden. Der Wert `0.05701` würde beispielsweise einem maximalen Preis von 0,05701 US-Dollar pro Stunde entsprechen. Wenn Sie den maximalen Preis auf `-1` festlegen, wird die VM nicht basierend auf dem Preis entfernt. Der Preis für die VM entspricht dem aktuellen Preis für Spot-VMs oder dem Preis für eine Standard-VM, je nachdem, welcher Preis niedriger ist, solange Kapazitäten und Kontingente verfügbar sind.


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Das Erstellen einer VM, die Spot-VMs nutzt, entspricht den Ausführungen im [Schnellstart](quick-create-portal.md). Wenn Sie eine VM bereitstellen, können Sie eine Azure Spot-Instanz verwenden.


Im Abschnitt **Instanzdetails** auf der Registerkarte **Grundlagen** ist **Nein** der Standardwert für die Verwendung einer Azure Spot-Instanz.

![Screenshot: Auswählen der Option „Nein“ hinsichtlich der Verwendung einer Azure Spot-Instanz](media/spot-portal/no.png)

Wenn Sie **Ja** auswählen, wird der Abschnitt erweitert, und Sie können den [Entfernungstyp und die Entfernungsrichtlinie bestimmen](spot-vms.md#eviction-policy). 

![Screenshot: Auswählen der Option „Ja“ hinsichtlich der Verwendung einer Azure Spot-Instanz](media/spot-portal/yes.png)


## <a name="next-steps"></a>Nächste Schritte

Sie können Spot-VMs auch mithilfe von [PowerShell](spot-powershell.md) erstellen.
---
title: Verfügbare Größen für Azure Cloud Services (erweiterter Support)
description: Verfügbare Größen für Bereitstellungen von Azure Cloud Services (erweiterter Support)
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: c3515b559ef647b9a043a04282b4edfc6e7fa9be
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743934"
---
# <a name="available-sizes-for-azure-cloud-services-extended-support"></a>Verfügbare Größen für Azure Cloud Services (erweiterter Support)

In diesem Artikel werden die verfügbaren VM-Größen für Cloud Services-Instanzen (erweiterter Support) beschrieben.   

| SKU-Familie |  ACU/Kern | 
|---|---|
| [A5–7](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#a-series)| 100 |
|[A8-A11](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#a-series---compute-intensive-instances) | 225* |
|[Av2](https://docs.microsoft.com/azure/virtual-machines/av2-series) | 100 | 
|[D](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#d-series) | 160 | 
|[Dv2](https://docs.microsoft.com/azure/virtual-machines/dv2-dsv2-series) | 160 - 190* |
|[Dv3](https://docs.microsoft.com/azure/virtual-machines/dv3-dsv3-series) | 160 - 190* |
|[Ev3](https://docs.microsoft.com/azure/virtual-machines/ev3-esv3-series) | 160 - 190*
|[G](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#g-series) | 180 - 240* |
|[H](https://docs.microsoft.com/azure/virtual-machines/h-series) | 290 - 300* | 

>[!NOTE]
> Mit * gekennzeichnete ACUs verwenden die Intel® Turbo-Technologie, um die CPU-Frequenz zu erhöhen eine Leistungssteigerung zu erzielen. Das Maß der Leistungssteigerung variiert basierend auf der Größe und Workload des virtuellen Computers sowie auf anderen Workloads, die auf dem gleichen Host ausgeführt werden.

## <a name="configure-sizes-for-cloud-services-extended-support"></a>Konfigurieren von Größen für Cloud Services (erweiterter Support)

Sie können die VM-Größe einer Rolleninstanz als Teil des Dienstmodells in der Dienstdefinitionsdatei angeben. Die Größe einer Rolle bestimmt die Anzahl der CPU-Kerne, die Speicherkapazität und die lokale Dateisystemgröße.

Legen Sie z. B. die Größe der Webrolleninstanz auf `Standard_D2` fest: 

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2"> 
</WorkerRole> 
```

## <a name="change-the-size-of-an-existing-role"></a>Ändern der Größe einer vorhandenen Rolle

Um die Größe einer vorhandenen Rolle zu ändern, ändern Sie die VM-Größe in der Dienstdefinitionsdatei (CSDEF-Datei), packen Sie den Clouddienst erneut, und stellen Sie ihn dann erneut bereit. 

## <a name="get-a-list-of-available-sizes"></a>Abrufen einer Liste der verfügbaren Größen 

Informationen zum Abrufen einer Liste der verfügbaren Größen finden Sie unter [Ressourcen-SKUs: Liste](https://docs.microsoft.com/rest/api/compute/resourceskus/list). Wenden Sie die folgenden Filter an:


`ResourceType = virtualMachines ` <br>
`VMDeploymentTypes = PaaS `


## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.

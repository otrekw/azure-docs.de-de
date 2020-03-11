---
title: Einrichtung von AMD-GPU-Treibern für die Azure N-Serie unter Windows
description: Einrichten von AMD-GPU-Treibern für virtuelle Computer der N-Serie unter Windows Server oder Windows in Azure
services: virtual-machines-windows
author: vikancha
manager: jkabat
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/4/2019
ms.author: vikancha
ms.openlocfilehash: 164f07f6545c1c225814958bba5722536b11a9b4
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2020
ms.locfileid: "78269437"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Installieren von AMD-GPU-Treibern für virtuelle Computer der N-Serie unter Windows

Um die GPU-Funktionen von virtuellen Computern der neuen Azure NVv4-Serie unter Windows nutzen zu können, müssen AMD-GPU-Treiber installiert werden. Die AMD-Treibererweiterung wird in den nächsten Wochen zur Verfügung stehen. In diesem Artikel finden Sie Informationen zu unterstützten Betriebssystemen und Treibern sowie Schritte zur manuellen Installation und Überprüfung.

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [GPU-optimierte Größen von virtuellen Windows-Computern](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

| OS | Treiber |
| -------- |------------- |
| Windows 10 EVD - Build 1903 <br/><br/>Windows 10 - Build 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [20.Q1.1](https://download.microsoft.com/download/3/8/9/3893407b-e8aa-4079-8592-735d7dd1c19a/Radeon-Pro-Software-for-Enterprise-GA.exe) (.exe) |


## <a name="driver-installation"></a>Treiberinstallation

1. Stellen Sie über Remotedesktop eine Verbindung mit den einzelnen virtuellen Computern der NVv4-Serie her.

2. Wenn Sie Kunde von NVv4 Vorschau sind, beenden Sie die VM, und warten Sie dann, bis sie den Staus „Stopped(Deallocated)“ (Beendet, Zuordnung aufgehoben) aufweist.

3. Starten Sie die VM, und deinstallieren Sie den Vorschautreiber, indem Sie „amdcleanuputility-x64.exe“ im Ordner „...\AMDCleanUninstallUtility“ ausführen. Der genaue Pfad hängt davon ab, wo sich die vorherigen Treiberinstallationsdateien befinden.  

4. Laden Sie den neuesten Treiber herunter, und installieren Sie ihn.

5. Starten Sie die VM neu.

## <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation

Sie können die Treiberinstallation im Geräte-Manager überprüfen. Das folgende Beispiel zeigt die erfolgreiche Konfiguration der Radeon Instinct MI25-Karte auf einem virtuellen Azure-NVv4-Computer.
<br />
![Eigenschaften des GPU-Treibers](./media/n-series-amd-driver-setup/device-manager.png)

Mit dxdiag können Sie die GPU-Anzeigeeigenschaften einschließlich Video-RAM überprüfen. Das folgende Beispiel zeigt eine 1/8-Partition der Radeon Instinct MI25-Karte auf einem virtuellen Azure-NVv4-Computer.
<br />
![Eigenschaften des GPU-Treibers](./media/n-series-amd-driver-setup/dxdiag.png)

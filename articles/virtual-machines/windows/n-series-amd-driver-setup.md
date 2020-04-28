---
title: Einrichtung von AMD-GPU-Treibern für die Azure N-Serie unter Windows
description: Einrichten von AMD-GPU-Treibern für virtuelle Computer der N-Serie unter Windows Server oder Windows in Azure
author: vikancha
manager: jkabat
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 12/4/2019
ms.author: vikancha
ms.openlocfilehash: 63114bdf60c1feb2b6cb1092ef78397efdc5b666
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "81865750"
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

3. Starten Sie die VM, und laden Sie das aktuelle [AMD-Bereinigungsprogramm](https://download.microsoft.com/download/4/f/1/4f19b714-9304-410f-9c64-826404e07857/AMDCleanupUtilityni.exe) herunter. Deinstallieren Sie den vorhandenen Treiber, indem Sie „amdcleanuputility-x64. exe“ ausführen. Verwenden Sie KEIN vorhandenes Bereinigungsprogramm, das mit dem vorherigen Treiber installiert wurde.  

4. Laden Sie den neuesten Treiber herunter, und installieren Sie ihn.

5. Starten Sie die VM neu.

## <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation

Sie können die Treiberinstallation im Geräte-Manager überprüfen. Das folgende Beispiel zeigt die erfolgreiche Konfiguration der Radeon Instinct MI25-Karte auf einem virtuellen Azure-NVv4-Computer.
<br />
![Eigenschaften des GPU-Treibers](./media/n-series-amd-driver-setup/device-manager.png)

Mit dxdiag können Sie die GPU-Anzeigeeigenschaften einschließlich Video-RAM überprüfen. Das folgende Beispiel zeigt eine 1/2-Partition der Radeon Instinct MI25-Karte auf einem virtuellen Azure-NVv4-Computer.
<br />
![Eigenschaften des GPU-Treibers](./media/n-series-amd-driver-setup/dxdiag-output.png)

Wenn Sie Windows 10 Build 1903 oder höher ausführen, zeigt „dxdiag“ keine Informationen auf der Registerkarte „Anzeige“. Verwenden Sie die Option „Alle Informationen speichern“ im unteren Bereich, dann zeigt die Ausgabedatei die Informationen zur AMD-MI25-GPU.

![Eigenschaften des GPU-Treibers](./media/n-series-amd-driver-setup/dxdiag-details.png)



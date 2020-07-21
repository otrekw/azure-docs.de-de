---
title: Einrichtung von AMD-GPU-Treibern für die Azure N-Serie unter Windows
description: Einrichten von AMD-GPU-Treibern für virtuelle Computer der N-Serie unter Windows Server oder Windows in Azure
author: vikancha-MSFT
manager: jkabat
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 12/4/2019
ms.author: vikancha
ms.openlocfilehash: 4693ad8b168ce1ddd7c07afe650a89fc1888ccd7
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86183323"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Installieren von AMD-GPU-Treibern für virtuelle Computer der N-Serie unter Windows

Um die GPU-Funktionen von virtuellen Computern der neuen Azure NVv4-Serie unter Windows nutzen zu können, müssen AMD-GPU-Treiber installiert werden. Die [AMD-GPU-Treibererweiterung](../extensions/hpccompute-amd-gpu-windows.md) installiert AMD-GPU-Treiber auf einer VM der NVv4-Reihe. Installieren oder verwalten Sie die Erweiterung mithilfe des Azure-Portals oder mit Tools wie Azure PowerShell oder Azure Resource Manager-Vorlagen. Informationen zu unterstützten Betriebssystemen und Bereitstellungsschritten finden Sie in der [Dokumentation zur AMD-GPU-Treibererweiterung](../extensions/hpccompute-amd-gpu-windows.md).

Wenn Sie AMD-GPU-Treiber manuell installieren möchten, finden Sie in diesem Artikel Informationen zu unterstützten Betriebssystemen und Treibern sowie Schritte zur Installation und Überprüfung.

Auf NVv4-VMs werden nur von Microsoft veröffentlichte GPU-Treiber unterstützt. Installieren Sie KEINE GPU-Treiber aus einer anderen Quelle.

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [GPU-optimierte Größen von virtuellen Windows-Computern](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

| OS | Treiber |
| -------- |------------- |
| Windows 10 Enterprise mit mehreren Sitzungen: Build 1903 <br/><br/>Windows 10 - Build 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [20.Q1.1](https://download.microsoft.com/download/3/8/9/3893407b-e8aa-4079-8592-735d7dd1c19a/Radeon-Pro-Software-for-Enterprise-GA.exe) (.exe) |


## <a name="driver-installation"></a>Treiberinstallation

1. Stellen Sie über Remotedesktop eine Verbindung mit den einzelnen virtuellen Computern der NVv4-Serie her.

2. Wenn Sie die vorherige Treiberversion deinstallieren müssen, laden Sie [hier](https://download.microsoft.com/download/4/f/1/4f19b714-9304-410f-9c64-826404e07857/AMDCleanupUtilityni.exe) das AMD-Bereinigungsprogramm herunter. Verwenden Sie nicht das Programm aus der vorherigen Treiberversion.

3. Laden Sie den neuesten Treiber herunter, und installieren Sie ihn.

4. Starten Sie die VM neu.

## <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation

Sie können die Treiberinstallation im Geräte-Manager überprüfen. Das folgende Beispiel zeigt die erfolgreiche Konfiguration der Radeon Instinct MI25-Karte auf einem virtuellen Azure-NVv4-Computer.
<br />
![Eigenschaften des GPU-Treibers](./media/n-series-amd-driver-setup/device-manager.png)

Mit dxdiag können Sie die GPU-Anzeigeeigenschaften einschließlich Video-RAM überprüfen. Das folgende Beispiel zeigt eine 1/2-Partition der Radeon Instinct MI25-Karte auf einem virtuellen Azure-NVv4-Computer.
<br />
![Eigenschaften des GPU-Treibers](./media/n-series-amd-driver-setup/dxdiag-output-new.png)

Wenn Sie Windows 10 Build 1903 oder höher ausführen, zeigt „dxdiag“ keine Informationen auf der Registerkarte „Anzeige“. Verwenden Sie die Option „Alle Informationen speichern“ im unteren Bereich, dann zeigt die Ausgabedatei die Informationen zur AMD-MI25-GPU.

![Eigenschaften des GPU-Treibers](./media/n-series-amd-driver-setup/dxdiag-details.png)



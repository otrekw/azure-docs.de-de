---
title: include file
description: include file
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 5a48dd8158ed08aee3da459b4373fb0e4b36fb52
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113364940"
---
## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

### <a name="nvidia-tesla-cuda-drivers"></a>NVIDIA Tesla-Treiber (CUDA)

NVIDIA Tesla-Treiber (CUDA) für VMs der Serien NC, NCv2, NCv3, NCasT4_v3, ND und NDv2 (optional für NV-Serie) werden nur unter den in der folgenden Tabelle aufgeführten Betriebssystemen unterstützt. Die Links zum Herunterladen der Treiber sind zum Zeitpunkt der Veröffentlichung aktuell. Die neuesten Treiber finden Sie auf der [NVIDIA](https://www.nvidia.com/)-Website.

> [!TIP]
> Alternativ zur manuellen CUDA-Treiberinstallation auf einem virtuellen Windows Server-Computer können Sie ein [Azure Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md)-Image bereitstellen. Bei den DSVM-Editionen für Windows Server 2016 sind NVIDIA CUDA-Treiber, die CUDA Deep Neural Network-Bibliothek und weitere Tools vorinstalliert.


| OS | Treiber |
| -------- |------------- |
| Windows Server 2019 | [451.82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) (EXE-Datei) |
| Windows Server 2016 | [451.82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) (EXE-Datei) |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-Treiber

Microsoft vertreibt Installationsprogramme für NVIDIA GRID-Treiber für virtuelle Computer der NV- und NVv3-Serie, die als virtuelle Arbeitsstationen oder für virtuelle Anwendungen verwendet werden. Installieren Sie nur diese GRID-Treiber auf virtuellen Azure-Computern der NV-Serie, und zwar nur unter den in der folgenden Tabelle aufgeführten Betriebssystemen. In diesen Treibern ist die Lizenzierung für virtuelle GRID-GPU-Software in Azure enthalten. Sie müssen keinen NVIDIA vGPU-Softwarelizenzserver einrichten.

Die von Azure neu verteilten GRID-Treiber funktionieren nicht auf VMs anderer Serien als der NV-Serie, wie NCv2, NCv3, ND und NDv2. Die einzige Ausnahme ist die VM-Serie „NCas_T4_V3“, bei der die GRID-Treiber die Grafikfunktionen ähnlich wie bei der NV-Serie aktivieren.

Die NC-Serie mit Nvidia K80-GPUs unterstützt keine GRID-/Grafikanwendungen.  

Beachten Sie, dass durch die NVIDIA-Erweiterung immer der neueste Treiber installiert wird. Für Kunden, die eine ältere Version benötigen, stellen wir hier Links zur Vorgängerversion bereit.

Für Windows Server 2019, Windows Server 2016 1607, 1709 und Windows 10 (bis Build 20H2):
- [GRID 12.2 (462.31)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe)
- [GRID 12.1 (461.33)](https://download.microsoft.com/download/9/7/e/97e1be73-d24b-410b-9c08-cc98c7becfa3/461.33_grid_win10_server2016_server2019_64bit_azure_swl.exe) (EXE-Datei) 

Für Windows Server 2012 R2: 
- [GRID 12.2 (462.31)](https://download.microsoft.com/download/1/2/0/120551f5-cc05-4911-bd29-88fb2747213c/462.31_grid_server2012R2_64bit_azure_swl.exe) (.exe)
- [GRID 12.1 (461.33)](https://download.microsoft.com/download/9/9/c/99caf5c6-af9f-48b2-bcb0-af5ec64b8592/461.33_grid_server2012R2_64bit_azure_swl.exe) (EXE-Datei) 


Eine vollständige Liste aller vorherigen NVIDIA GRID-Treiber finden Sie auf [GitHub](https://github.com/Azure/azhpc-extensions/blob/master/NvidiaGPU/resources.json).

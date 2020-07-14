---
title: VHD wird beim Erstellen eines virtuellen Computers in Azure nicht unterstützt | Microsoft-Dokumentation
description: Dieser Artikel hilft beim Korrigieren von VHD-Fehlern beim Ausführen eines virtuellen Computers in Microsoft Azure.
services: virtual-machines
documentationCenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
ms.service: virtual-machines
ms.assetid: 5488aba9-c3da-435d-b4a5-63470f455b07
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 06/29/2020
ms.author: genli
ms.openlocfilehash: ff4822b513ed2aea6a18ba45bffc1d060ee2410e
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937467"
---
# <a name="vhd-is-not-supported-when-you-create-a-virtual-machine-in-azure"></a>VHD wird beim Erstellen eines virtuellen Computers in Azure nicht unterstützt

Dieser Artikel hilft beim Korrigieren von VHD-Fehlern beim Ausführen eines virtuellen Computers unter Windows oder Linux.

## <a name="symptoms"></a>Symptome

Wenn Sie einen virtuellen Computer in Microsoft Azure mit einer hochgeladenen VHD erstellen, tritt bei der Bereitstellung ein Fehler auf und die folgende Fehlermeldung wird zurückgegeben: 

```
New-AzureRmVM : Long running operation failed with status 'Failed'.
ErrorCode: InvalidVhd
ErrorMessage: The specified cookie value in VHD footer indicates that disk 'diskname' with blob https://xxxxxx.blob.core.windows.net/vhds/samplename.vhd is not a supported VHD. Disk is expected to have cookie value 'conectix'.
```

## <a name="cause"></a>Ursache

Dieses Problem tritt aus einem der folgenden Gründe auf:

- Die VHD-Datei entspricht nicht der 1-MB-Ausrichtung (Offset). Die Größe des unterstützten Datenträgers muss 1 MB * N betragen, z.B. 102.401 MB.
- Die VHD ist beschädigt oder wird nicht unterstützt. 

## <a name="resolution"></a>Lösung

> [!NOTE]
> Um die folgende Fehlerbehebung durchzuführen, muss der Kunde diese Schritte vor dem Hochladen der VHD in Azure durchführen.

Ändern Sie zur Behebung dieses Problems die Größe des Datenträgers so, dass die 1-MB-Ausrichtung eingehalten wird:

- Verwenden Sie das [PowerShell-Cmdlet zum Ändern der Größe der VHD](https://docs.microsoft.com/powershell/module/hyper-v/resize-vhd) (Resize-VHD), um das Problem in Windows zu beheben. Beachten Sie, dass **Resize-VHD** kein Azure PowerShell-Cmdlet ist.

  1. [Installieren der Hyper-V-Rolle auf Windows Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server)
  1. [Konvertieren des virtuellen Datenträgers in eine VHD mit fester Größe](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image#convert-the-virtual-disk-to-a-fixed-size-vhd)

- Verwenden Sie zur Lösung des Problems unter Linux den Befehl [qemu-img](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic).

Weitere Informationen zum Erstellen und Hochladen einer VHD zur Erstellung von virtuellen Azure-Computern finden Sie in den folgenden Artikeln:

- [Hochladen und Erstellen eines virtuellen Linux-Computers aus einem benutzerdefinierten Datenträgerimage mithilfe der Azure CLI 1.0](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd)
- [Erstellen und Hochladen einer Windows Server-VHD in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed)

Anhaltende Probleme können auf einen beschädigte VHD hinweisen. In dieser Situation empfiehlt es sich, die VHD von Grund auf neu zu erstellen.

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Informationen zur Windows-VHD](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#about-vhds)
- [Informationen zur Linux-VHD](https://docs.microsoft.com/azure/virtual-machines/linux/about-disks-and-vhds#about-vhds)
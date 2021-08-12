---
title: Behandeln von Problemen beim Hochladen von VM-Images in Azure Stack Edge Pro GPU | Microsoft-Dokumentation
description: Beschreibt die Behandlung von Problemen, die beim Hochladen, Herunterladen oder Löschen von VM-Images in Azure Stack Edge Pro GPU auftreten.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 05/13/2021
ms.author: alkohli
ms.openlocfilehash: 3ec5cfb952e666832fe887f65672e228004423bc
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110482549"
---
# <a name="troubleshoot-virtual-machine-image-uploads-in-azure-stack-edge-pro-gpu"></a>Behandeln von Problemen beim Hochladen von VM-Images in Azure Stack Edge Pro GPU

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird die Behandlung von Problemen beschrieben, die beim Herunterladen und Verwalten von VM-Images auf einem Azure Stack Edge Pro GPU-Gerät auftreten.


## <a name="unable-to-add-vm-image-to-blob-container"></a>VM-Image kann Blobcontainer nicht hinzugefügt werden

**Fehlerbeschreibung:** Wenn Sie im Azure-Portal versuchen, ein VM-Image in einen Blobcontainer hochzuladen, ist die Schaltfläche **Hinzufügen** nicht verfügbar, und das Image kann nicht hochgeladen werden. Die Schaltfläche **Hinzufügen** ist nicht verfügbar, wenn Sie nicht über die erforderlichen Berechtigungen der Rolle „Mitwirkender“ für die Ressourcengruppe oder das Abonnement für das Gerät verfügen.

**Lösungsvorschlag:** Stellen Sie sicher, dass Sie über die erforderlichen Berechtigungen als „Mitwirkender“ verfügen, um Dateien zur Ressourcengruppe oder zum Speicherkonto hinzuzufügen. Weitere Informationen finden Sie unter [Voraussetzungen für die Azure Stack Edge-Ressource](azure-stack-edge-deploy-prep.md#prerequisites).


## <a name="invalid-blob-type-for-the-source-blob-uri"></a>Ungültiger Blobtyp für den Quellblob-URI

**Fehlerbeschreibung:** Eine als Blockblob gespeicherte VHD kann nicht heruntergeladen werden. Zum Herunterladen muss eine VHD als Seitenblob gespeichert sein.

**Lösungsvorschlag:** Laden Sie die VHD als Seitenblob in das Azure Storage-Konto hoch. Dann laden Sie das Blob herunter. Anweisungen zum Hochladen finden Sie unter [Verwenden des Storage-Explorer für den Upload](azure-stack-edge-gpu-deploy-virtual-machine-templates.md#use-storage-explorer-for-upload).


## <a name="only-blobs-formatted-as-vhds-can-be-imported"></a>Nur als VHDs formatierte Blobs können importiert werden

**Fehlerbeschreibung:** Die VHD kann nicht importiert werden, da sie nicht den Formatierungsanforderungen entspricht. Zum Importieren muss eine virtuelle Festplatte eine VHD der Generation 1 mit fester Größe sein.

**Lösungsvorschläge:** 

- Führen Sie die Schritte unter [Vorbereiten eines generalisierten Images der Windows-VHD zum Bereitstellen von VMs in Azure Stack Edge Pro GPU](azure-stack-edge-gpu-prepare-windows-vhd-generalized-image.md), um eine VHD mit fester Größe für einen virtuellen Computer der Generation 1 aus Ihrer Quell-VHD oder -VHDX zu erstellen.

- Bei Verwendung von PowerShell:

   - Sie können [Convert-VHD](/powershell/module/hyper-v/convert-vhd?view=windowsserver2019-ps&preserve-view=true) im Windows PowerShell-Modul für Hyper-V verwenden. Mit Convert-VHD können Sie jedoch ein VM-Image von einem virtuellen Computer der Generation 2 nicht in eines der Generation 1 konvertieren. Verwenden Sie stattdessen die Portalverfahren, die unter [Vorbereiten eines generalisierten Images der Windows-VHD zum Bereitstellen von VMs in Azure Stack Edge Pro GPU](azure-stack-edge-gpu-prepare-windows-vhd-generalized-image.md) beschrieben sind.
  
   - Wenn Sie den aktuellen VHD-Typ ermitteln müssen, verwenden Sie [Get-VHD](/powershell/module/hyper-v/get-vhd?view=windowsserver2019-ps&preserve-view=true).


## <a name="the-condition-specified-using-http-conditional-headers-is-not-met"></a>Die mit bedingtem HTTP-Header angegebene Bedingung ist nicht erfüllt.

**Fehlerbeschreibung:** Wenn Sie versuchen, eine VHD aus Azure herunterzuladen, und daran Änderungen vorgenommen werden, tritt beim Herunterladen ein Fehler auf, da die VHD in Azure nicht mit der heruntergeladenen VHD übereinstimmt. Dieser Fehler tritt auch auf, wenn ein Download versucht wird, bevor der Upload der VHD in Azure abgeschlossen ist.

**Lösungsvorschlag:** Warten Sie, bis der Upload der VHD abgeschlossen ist und keine Änderungen an der VHD vorgenommen werden. Versuchen Sie dann erneut, die VHD herunterzuladen.


## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Bereitstellen von VMs über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md).
* Informieren Sie sich über das [Bereitstellen von VMs über Azure PowerShell](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md).

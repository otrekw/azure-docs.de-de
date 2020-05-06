---
title: Erstellen einer VM auf der Grundlage eines verwalteten Images in Azure
description: Erstellen Sie einen virtuellen Windows-Computer auf der Grundlage eines generalisierten verwalteten Images mit Azure PowerShell oder dem Azure-Portal.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: b0c6689b66037067a4c5174738945b7c6fabd5b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82086316"
---
# <a name="create-a-vm-from-a-managed-image"></a>Erstellen eines virtuellen Computers aus einem verwalteten Image

Sie können über das Azure-Portal oder mit PowerShell mehrere virtuelle Computer aus einem verwalteten Azure-VM-Image erstellen. Ein verwaltetes VM-Image enthält die erforderlichen Informationen zum Erstellen eines virtuellen Computers, einschließlich der Datenträger für Betriebssystem und Daten. Die virtuellen Festplatten, aus denen das Image besteht, einschließlich der Betriebssystem-Datenträger und sonstiger Datenträger, werden als verwaltete Datenträger gespeichert. 

Vor dem Erstellen eines neuen virtuellen Computers müssen Sie [ein verwaltetes VM-Image erstellen](capture-image-resource.md), das als Quellimage verwendet werden soll, und auf dem Image jedem Benutzer, der darauf zugreifen soll, Lesezugriff gewähren. 


## <a name="use-the-portal"></a>Verwenden des Portals

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach einem verwalteten Image. Suchen Sie nach **Images**, und wählen Sie diese Option aus.
3. Wählen Sie das gewünschte Image aus der Liste aus. Die Seite **Übersicht** für Images wird geöffnet.
4. Klicken Sie im Menü auf **VM erstellen**.
5. Geben Sie die Informationen zum virtuellen Computer ein. Der hier eingegebene Benutzername und das Kennwort werden zum Anmelden beim virtuellen Computer verwendet. Wählen Sie **OK** aus, wenn Sie fertig sind. Sie können die neue VM in einer bestehenden Ressourcengruppe erstellen oder **Neu erstellen** auswählen, um eine neue Ressourcengruppe zum Speichern der VM zu erstellen.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. 
7. Nehmen Sie unter **Einstellungen** Änderungen nach Bedarf vor, und klicken Sie auf **OK**. 
8. Auf der Seite „Zusammenfassung“ sollte Ihr Imagename unter **Privates Image** aufgelistet werden. Klicken Sie auf **OK**, um die Bereitstellung des virtuellen Computers zu starten.


## <a name="use-powershell"></a>Verwenden von PowerShell

Sie können PowerShell verwenden, um eine VM aus einem Image zu erstellen, indem Sie den vereinfachten Parametersatz für das Cmdlet [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) verwenden. Das Image muss sich in derselben Ressourcengruppe befinden, in der Sie die VM erstellen möchten.

 

Der vereinfachte Parametersatz für [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) erfordert nur die Angabe eines Namens, einer Ressourcengruppe und eines Imagenamens, um eine VM aus einem Image zu erstellen. Mit „New-AzVm“ wird der Wert des Parameters **-Name** als Name für alle Ressourcen verwendet, die das Cmdlet automatisch erstellt. In diesem Beispiel geben wir ausführlichere Namen für die einzelnen Ressourcen an, lassen sie aber automatisch vom Cmdlet erstellen. Sie können Ressourcen, z.B. das virtuelle Netzwerk, auch im Voraus erstellen und den Ressourcennamen an das Cmdlet übergeben. Mit „New-AzVm“ werden die vorhandenen Ressourcen verwendet, wenn sie anhand des Namens gefunden werden können.

Im folgenden Beispiel wird eine VM mit dem Namen *myVMFromImage* in der Ressourcengruppe *myResourceGroup* aus dem Image *myImage* erstellt. 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Nächste Schritte
[Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


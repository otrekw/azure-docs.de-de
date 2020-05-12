---
title: Erstellen von VM-Images (Vorschauversion)
description: Informationen zur Verwendung von Azure PowerShell zum Erstellen eines Images einer Azure-VM in Shared Image Gallery
author: cynthn
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: 454ed810f950924d3dd790a2442fe29816bf940d
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838466"
---
# <a name="preview-create-an-image-from-a-vm"></a>Vorschau: Erstellen von VM-Images

Wenn Sie über eine VM verfügen, die Sie verwenden möchten, um mehrere identische VMs zu erstellen, können Sie mithilfe von Azure PowerShell ein Image dieser VM in Shared Image Gallery erstellen. Ebenso können Sie auch die [Azure CLI](image-version-vm-cli.md) verwenden, um ein VM-Image zu erstellen.

Mithilfe von Azure PowerShell können Sie sowohl für [spezialisierte als auch für generalisierte](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#generalized-and-specialized-images) VMs ein Image erstellen. 

Images in Imagekatalogen verfügen über zwei Komponenten, die in diesem Beispiel erstellt werden:
- Eine **Imagedefinition** enthält Informationen zum Image und zu den Anforderungen für dessen Verwendung. Dies umfasst Angaben dazu, ob das Image unter Windows oder Linux erstellt wurde, ob es sich um eine spezialisierte oder eine generalisierte VM handelt und welche Mindest- und Höchstanforderungen für den Arbeitsspeicher gelten. Außerdem sind darin Hinweise zur jeweiligen Version enthalten. Es ist eine Definition eines Imagetyps. 
- Bei der Verwendung von Shared Image Gallery werden **Imageversionen** verwendet, um VMs zu erstellen. Sie können nach Bedarf mehrere Versionen eines Images für Ihre Umgebung haben. Bei der Erstellung von VMs werden die Imageversionen verwendet, um neue Datenträger für die VMs zu erstellen. Imageversionen können mehrmals verwendet werden.


## <a name="before-you-begin"></a>Voraussetzungen

Sie benötigen eine Shared Image Gallery-Instanz sowie eine VM in Azure, die als Quelle verwendet werden kann. 

Wenn ein Datenträger an die VM angefügt ist, darf dieser nicht größer als 1 TB sein.

Ersetzen Sie, wenn nötig, im Laufe der Bearbeitung dieses Artikels die Ressourcennamen.


## <a name="get-the-gallery"></a>Abrufen des Katalogs

Sie können alle Kataloge und Imagedefinitionen nach Name auflisten. Die Ergebnisse liegen im Format `gallery\image definition\image version` vor.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Compute/galleries | Format-Table
```

Wenn Sie die richtigen Katalog- und Imagedefinitionen gefunden haben, erstellen Sie Variablen für diese, die Sie später verwenden können. In diesem Beispiel heißt der Katalog in der Ressourcengruppe *myResourceGroup* *myGallery*.

```azurepowershell-interactive
$gallery = Get-AzGallery `
   -Name myGallery `
   -ResourceGroupName myResourceGroup
```

## <a name="get-the-vm"></a>Abrufen der VM

Mit [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) können Sie eine Liste der VMs abrufen, die in einer Ressourcengruppe verfügbar sind. Wenn Sie den Namen und die Ressourcengruppe der VM kennen, können Sie `Get-AzVM` verwenden, um das VM-Objekt abzurufen und für die spätere Verwendung in einer Variable zu speichern. Dieses Beispiel ruft die VM *sourceVM* aus der Ressourcengruppe „myResourceGroup“ ab und weist es der Variablen *$sourceVm* zu. 

```azurepowershell-interactive
$sourceVm = Get-AzVM `
   -Name sourceVM `
   -ResourceGroupName myResourceGroup
```

Es empfiehlt sich, den virtuellen Computer vor dem Erstellen eines Images mit [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) zu beenden oder die Zuordnung aufzuheben.

```azurepowershell-interactive
Stop-AzVM `
   -ResourceGroupName $sourceVm.ResourceGroupName `
   -Name $sourceVm.Name `
   -Force
```

## <a name="create-an-image-definition"></a>Erstellen einer Imagedefinition 

Imagedefinitionen erstellen eine logische Gruppierung von Images. Sie werden zum Verwalten von Informationen über das Image verwendet. Namen für Imagedefinition können aus Groß- und Kleinbuchstaben, Zahlen, Punkten und (Binde)Strichen bestehen. 

Stellen Sie beim Erstellen der Imagedefinition sicher, dass diese alle zutreffenden Informationen beinhaltet. Wenn Sie die VM generalisiert haben (über Sysprep für Windows oder waagent-deprovision für Linux) sollten Sie unter Verwendung von `-OsState generalized` eine Imagedefinition erstellen. Wenn Sie die VM nicht generalisiert haben, erstellen Sie unter Verwendung von `-OsState specialized` eine Imagedefinition.

Weitere Informationen zu den Werten, die Sie für eine Imagedefinition angeben können, finden Sie unter [Imagedefinitionen](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Erstellen Sie die Imagedefinition mit [New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). 

In diesem Beispiel heißt die Imagedefinition *myImageDefinition* und ist für eine spezialisierte VM gedacht, die unter Windows ausgeführt wird. Verwenden Sie `-OsType Linux`, um eine Definition für Images unter Linux zu erstellen. 

```azurepowershell-interactive
$imageDefinition = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $gallery.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState specialized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```


## <a name="create-an-image-version"></a>Erstellen einer Imageversion

Erstellen Sie mithilfe von [New-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion) eine Imageversion. 

Zulässige Zeichen für die Imageversion sind Zahlen und Punkte. Zahlen müssen im Bereich einer ganzen 32-Bit-Zahl liegen. Format: *Hauptversion*.*Nebenversion*.*Patch*.

In diesem Beispiel lautet die Imageversion *1.0.0*. Sie wird in den Rechenzentren *USA, Westen-Mitte* und *USA, Süden-Mitte* repliziert. Bei der Auswahl der Zielregionen für die Replikation ist zu beachten, dass Sie auch die *Quell*region als Ziel für die Replikation angeben müssen.

Verwenden Sie `$vm.Id.ToString()` für die `-Source`, um eine Imageversion der VM zu erstellen.

```azurepowershell-interactive
   $region1 = @{Name='South Central US';ReplicaCount=1}
   $region2 = @{Name='East US';ReplicaCount=2}
   $targetRegions = @($region1,$region2)

$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $imageDefinition.Name`
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $gallery.ResourceGroupName `
   -Location $gallery.Location `
   -TargetRegion $targetRegions  `
   -Source $sourceVm.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-12-01' `  
   -asJob 
```

Es dauert eine Weile, das Image für alle Zielregionen zu replizieren. Daher haben wir zum Verfolgen des Fortschritts einen Auftrag erstellt. Um den Fortschritt des Auftrags anzuzeigen, geben Sie `$job.State` ein.

```azurepowershell-interactive
$job.State
```

> [!NOTE]
> Sie müssen warten, bis die Imageversion vollständig erstellt und repliziert wurde, bevor Sie dieses verwaltete Image verwenden können, um eine weitere Imageversion zu erstellen.
>
> Sie können Ihr Image auch in einem Premium-Speicher durch Hinzufügen von `-StorageAccountType Premium_LRS` oder in einem [zonenredundanten Speicher](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) speichern, indem Sie `-StorageAccountType Standard_ZRS` hinzufügen, wenn Sie die Imageversion erstellen.
>

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie überprüft haben, ob die neue Imageversion ordnungsgemäß funktioniert, können Sie eine VM erstellen. Erstellen Sie eine VM aus einer [spezialisierten](vm-specialized-image-version-powershell.md) oder einer [generalisierten](vm-generalized-image-version-powershell.md) Imageversion.

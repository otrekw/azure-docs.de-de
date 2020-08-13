---
title: Migrieren eines verwalteten Images zu einem Katalog mit freigegebenen Images
description: Erfahren Sie, wie Sie ein verwaltetes Image mithilfe von Azure PowerShell zu einer Imageversion in einem Katalog mit freigegebenen Images migrieren.
author: cynthn
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: c119ebc8d87c00608fc515099711463f47b8ed1a
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87901809"
---
# <a name="migrate-from-a-managed-image-to-a-shared-image-gallery-image"></a>Migrieren von einem verwalteten Image zu einem Image in einem Katalog mit freigegebenen Images

Wenn Sie über ein vorhandenes verwaltetes Image verfügen, das Sie zu einem Katalog mit freigegebenen Images migrieren möchten, können Sie direkt aus dem verwalteten Image ein Image im Katalog erstellen. Nachdem Sie das neue Image getestet haben, können Sie das verwaltete Quellimage löschen. Sie können auch mithilfe der [Azure-Befehlszeilenschnittstelle](image-version-managed-image-cli.md) von einem verwalteten Image zu einem Katalog mit freigegebenen Images migrieren.

Images in Imagekatalogen weisen zwei Komponenten auf, die in diesem Beispiel erstellt werden:
- Eine **Imagedefinition** enthält Informationen zum Image und zu den Anforderungen für dessen Verwendung. Dies umfasst Angaben dazu, ob es sich um ein Windows- oder Linux-Image handelt, ob es spezialisiert oder generalisiert ist sowie welche Mindest- und Höchstanforderungen für den Arbeitsspeicher gelten. Außerdem sind Versionshinweise enthalten. Es ist eine Definition eines Imagetyps. 
- Bei der Verwendung eines Katalogs mit freigegebenen Images werden **Imageversionen** verwendet, um VMs zu erstellen. Sie können nach Bedarf mehrere Versionen eines Images für Ihre Umgebung haben. Bei der Erstellung einer VM werden anhand der Imageversionen neue Datenträger für die VM erstellt. Imageversionen können mehrmals verwendet werden.


## <a name="before-you-begin"></a>Voraussetzungen

Zum Abschließen dieses Artikels muss ein verwaltetes Image vorhanden sein. Wenn das verwaltete Image einen Datenträger für Daten enthält, darf dieser nicht größer als 1 TB sein.

Ersetzen Sie beim Durcharbeiten dieses Artikels bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.

## <a name="get-the-gallery"></a>Abrufen des Katalogs

Sie können alle Kataloge und Imagedefinitionen nach Namen auflisten. Die Ergebnisse liegen im Format `gallery\image definition\image version` vor.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Compute/galleries | Format-Table
```

Nachdem Sie den richtigen Katalog gefunden haben, erstellen Sie eine Variable zur späteren Verwendung. In diesem Beispiel heißt der Katalog *myGallery* in der Ressourcengruppe *myResourceGroup*.

```azurepowershell-interactive
$gallery = Get-AzGallery `
   -Name myGallery `
   -ResourceGroupName myResourceGroup
```


## <a name="create-an-image-definition"></a>Erstellen einer Imagedefinition 

Imagedefinitionen erstellen eine logische Gruppierung von Images. Sie werden zum Verwalten von Informationen zum Image verwendet. Namen für Imagedefinition können aus Groß- und Kleinbuchstaben, Zahlen, Punkten und (Binde)Strichen bestehen. 

Stellen Sie beim Erstellen der Imagedefinition sicher, dass diese alle richtigen Informationen beinhaltet. Da verwaltete Images immer generalisiert sind, sollten Sie `-OsState generalized` festlegen. 

Weitere Informationen zu den Werten, die Sie für eine Imagedefinition angeben können, finden Sie unter [Imagedefinitionen](./windows/shared-image-galleries.md#image-definitions).

Erstellen Sie die Imagedefinition mit [New-AzGalleryImageDefinition](/powershell/module/az.compute/new-azgalleryimageversion). In diesem Beispiel heißt die Imagedefinition *myImageDefinition* und ist für ein generalisiertes Windows-Betriebssystem vorgesehen. Um eine Definition für Images zu erstellen, die ein Linux-Betriebssystem verwenden, verwenden Sie `-OsType Linux`. 

```azurepowershell-interactive
$imageDefinition = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $gallery.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```

## <a name="get-the-managed-image"></a>Abrufen des verwalteten Images

Mit [Get-AzImage](/powershell/module/az.compute/get-azimage) können Sie eine Liste der Images anzeigen, die in einer Ressourcengruppe verfügbar sind. Wenn Sie den Namen und die Ressourcengruppe des Images kennen, können Sie `Get-AzImage` erneut ausführen, um das Imageobjekt abzurufen und für die spätere Verwendung in einer Variable zu speichern. Dieses Beispiel ruft das Image *myImage* aus der Ressourcengruppe „myResourceGroup“ ab und weist es der Variable *$managedImage* zu. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```


## <a name="create-an-image-version"></a>Erstellen einer Imageversion

Erstellen Sie mit [New-AzGalleryImageVersion](/powershell/module/az.compute/new-azgalleryimageversion) eine Imageversion aus dem verwalteten Image. 

Zulässige Zeichen für die Imageversion sind Zahlen und Punkte. Zahlen müssen im Bereich einer ganzen 32-Bit-Zahl liegen. Format: *Hauptversion*.*Nebenversion*.*Patch*.

In diesem Beispiel lautet die Imageversion *1.0.0*. Sie wird in den Rechenzentren *USA, Westen-Mitte* und *USA, Süden-Mitte* repliziert. Bei der Auswahl der Zielregionen für die Replikation ist zu beachten, dass Sie auch die *Quell*region als Ziel für die Replikation angeben müssen. 


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $imageDefinition.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $imageDefinition.ResourceGroupName `
   -Location $imageDefinition.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-12-31' `
   -asJob 
```

Es dauert eine Weile, das Image für alle Zielregionen zu replizieren. Daher haben wir zum Verfolgen des Fortschritts einen Auftrag erstellt. Geben Sie `$job.State` ein, um den Fortschritt anzuzeigen.

```azurepowershell-interactive
$job.State
```


> [!NOTE]
> Sie müssen warten, bis die Imageversion vollständig erstellt und repliziert wurde, bevor Sie dieses verwaltete Image verwenden können, um eine weitere Imageversion zu erstellen. 
>
> Sie können Ihr Image auch in einem Premium-Speicher durch Hinzufügen von `-StorageAccountType Premium_LRS` oder in einem [zonenredundanten Speicher](../storage/common/storage-redundancy.md) speichern, indem Sie `-StorageAccountType Standard_ZRS` hinzufügen, wenn Sie die Imageversion erstellen.
>

## <a name="delete-the-managed-image"></a>Löschen des verwalteten Images

Nachdem Sie sich vergewissert haben, dass die neue Imageversion ordnungsgemäß funktioniert, können Sie das verwaltete Image löschen.

```azurepowershell-interactive
Remove-AzImage `
   -ImageName $managedImage.Name `
   -ResourceGroupName $managedImage.ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich vergewissert haben, dass die Replikation abgeschlossen ist, können Sie eine VM aus dem [generalisierten Image](vm-generalized-image-version-powershell.md) erstellen.

Informationen zur Bereitstellung von Erwerbsplaninformationen finden Sie unter [Bereitstellen von Azure Marketplace-Erwerbsplaninformationen beim Erstellen von Images](marketplace-images.md).

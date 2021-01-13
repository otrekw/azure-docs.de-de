---
title: 'Windows-Start-Manager-Fehler – Status 0xC0000428: ungültiger Imagehash'
titlesuffix: Azure Virtual Machines
description: In diesem Artikel sind die erforderlichen Schritte beschrieben, um Probleme zu behandeln, bei denen eine Azure-VM nicht gestartet werden kann, weil ein Vorschauimage verwendet wurde und der Testzeitraum abgelaufen ist.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 26c411f9-6c22-4f45-a445-c5781e547828
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/03/2020
ms.author: v-miegge
ms.openlocfilehash: e58e349d7b7385ec913986c39462c17deadcb61d
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91969601"
---
# <a name="windows-boot-manager-error---0xc0000428-status-invalid-image-hash"></a>Windows-Start-Manager-Fehler – Status 0xC0000428: ungültiger Imagehash

In diesem Artikel sind die erforderlichen Schritte beschrieben, um Probleme zu behandeln, bei denen eine Azure-VM nicht gestartet werden kann, weil ein Vorschauimage verwendet wurde und der Testzeitraum abgelaufen ist.

## <a name="symptom"></a>Symptom

Wenn Sie den Screenshot der VM mit der [Startdiagnose](./boot-diagnostics.md) anzeigen, sehen Sie, dass der Windows-Start-Manager mit folgender Meldung angezeigt wird:

  `File: \windows\system32\boot\winload.exe`

  `Status: 0xc0000428`

  `Info: Windows cannot verify the digital signature for this file.`

  ![Abbildung 1 zeigt den Windows-Start-Manager mit dem Status Ox0000428 und der Meldung „Windows kann die digitale Signatur für diese Datei nicht überprüfen“.](./media/windows-boot-error-invalid-image-hash/1-cannot-verify-signature.png)

Möglicherweise wird auch diese Meldung angezeigt:

  `File: \Windows\system32\winload.exe`

  `Status: 0xc0000428`

  `Info: The digital signature for this file couldn't be verified.`
    
  ![Abbildung 2 zeigt den Windows-Start-Manager mit dem Status Ox0000428 und der Meldung „Die digitale Signatur dieser Datei konnte nicht überprüft werden“.](./media/windows-boot-error-invalid-image-hash/2-digital-signature-not-verified.png)

## <a name="cause"></a>Ursache

Die VM wurde nicht mit einem RTM-Image (Release to Manufacturing), sondern mit einem Vorschauimage erstellt, für das ein Ablaufdatum gilt. 

Vorschauimages haben einen festgelegten Lebenszyklus. Der gezeigte Screenshot wird angezeigt, wenn Sie das Ablaufdatum überschritten haben, der Testzeitraum des Images also beendet ist.

### <a name="example-of-preview-images"></a>Beispiel für Vorschauimages

`a699494373c04fc0bc8f2bb1389d6106__Windows-Server-Technical-Preview-201505.01-en.us-127GB.vhd`

Sie können das Ablaufdatum eines Vorschauversionsimages nicht verlängern. Nach Ablauf der Vorschauphase ist die VM nicht mehr startfähig.

- Der Testzeitraum variiert abhängig vom Produkt. Für Windows-Vorschauimages gilt z. B. ein Testzeitraum von 180 Tagen.

- In Azure enthält die Beschreibung aller Images für Windows, bei denen es sich um Vorschauversionen handelt, einen Hinweis, aus dem hervorgeht, dass sich die Images nicht für Produktionsumgebungen eignen und nur für einen festgelegten Testzeitraum bzw. als Vorschaurelease verwendet werden können.

## <a name="solution"></a>Lösung

Wenn es sich bei Ihrem Image um ein Vorschauimage handelt, kann der Testzeitraum für dieses Image nicht verlängert werden. Sie müssen [eine neue VM mit einem Image bereitstellen](../windows/quick-create-portal.md), bei dem es sich nicht um ein Vorschauimage handelt. Über die folgenden Schritte können Sie ermitteln, ob Sie ein Vorschauimage verwendet haben. Außerdem erhalten Sie Unterstützung beim Verschieben von Daten von dieser VM zu einer neuen VM. Wenn Sie herausgefunden haben, dass es sich bei Ihrem Image um ein Vorschauimage handelt, ist das Image abgelaufen und kann nicht wiederhergestellt werden.

Um herauszufinden, ob es sich bei Ihrem Image um ein Vorschauimage handelt, können Sie eine Abfrage über Azure PowerShell oder die Azure CLI ausführen. Zum Überprüfen, ob das Image ein Vorschauimage ist, können die folgenden Befehle verwendet werden.

### <a name="query-using-azure-powershell"></a>Abfrage über Azure PowerShell

1. Öffnen Sie die Windows PowerShell-Anwendung.
1. Führen Sie die folgenden Befehle aus:

   ```powershell
   $locName = "<LOCATION>" 
   $pubName = "<PUBLISHER NAME>" 
   $offerName = "<OFFER NAME>" 
   $skuName= "<YEAR WHEN THIS IMAGE WAS RELEASED>"
   Get-AzVMImagePublisher -Location $locName | Select $pubName
   Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
   Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
   Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Skus $skuName | Select Version
   ```

- Ersetzen Sie in den vorherigen Befehlen `<LOCATION>`, `<PUBLISHER NAME>`, `<OFFER NAME>` und `<YEAR WHEN THIS IMAGE WAS RELEASED>` durch die angegebenen Informationen. Entfernen Sie außerdem die Symbole < und >.

  Sehen Sie sich folgendes Beispiel an:

  ```powershell
  $locName = "West US" 
  $pubName = "MicrosoftWindowsServer" 
  $offerName = "WindowsServer" 
  $skuName= "2016-Datacenter"
  Get-AzVMImagePublisher -Location $locName | Select $pubName
  Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
  Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
  Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Skus $skuName | Select Version
  ```

### <a name="query-using-the-azure-cli"></a>Abfrage über die Azure CLI

1. [Installieren Sie die Azure CLI](/cli/azure/install-azure-cli) (sofern noch nicht geschehen).
1. Geben Sie nach dem Herunterladen entweder an einer Eingabeaufforderung oder über PowerShell den Befehl `az login` ein, und melden Sie sich mit Ihren Kontoanmeldeinformationen an.
1. Geben Sie nach der Anmeldung die folgenden Befehle ein:

   ```powershell
   az vm image list-publishers --location "<LOCATION>"
   az vm image list-offers --location "West US" --publisher "<PUBLISHER NAME>"
   az vm image list-skus --location "West US" --publisher "<PUBLISHER NAME>" --offer "<OFFER NAME>"
   az vm image list  --location "West US" --publisher "<PUBLISHER NAME>" --offer "<OFFER NAME>" --sku "<YEAR WHEN THIS IMAGE WAS RELEASED>"
   ```

- Ersetzen Sie in den vorherigen Befehlen `<LOCATION>`, `<PUBLISHER NAME>`, `<OFFER NAME>` und `<YEAR WHEN THIS IMAGE WAS RELEASED>` durch die angegebenen Informationen. Entfernen Sie außerdem die Symbole < und >.

  Sehen Sie sich folgendes Beispiel an:

  ```powershell
  az vm image list-publishers --location "West US"
  az vm image list-offers --location "West US" --publisher "MicrosoftWindowsServer"
  az vm image list-skus --location "West US" --publisher "MicrosoftWindowsServer" --offer "WindowsServer"
  az vm image list  --location "West US" --publisher "MicrosoftWindowsServer" --offer "WindowsServer" --sku "2016-Datacenter"
  ```
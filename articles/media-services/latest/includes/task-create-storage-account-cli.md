---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: a6349188a2c6b4da68009df93fbea5fa6eabacf1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102244548"
---
<!-- ### Create a storage account -->

Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. Weitere Informationen zur Verwendung von Speicherkonten in Media Services finden Sie unter [Storage accounts](../storage-account-concept.md) (Storage-Konten).

Sie müssen über ein **primäres** Storage-Konto verfügen. Darüber hinaus können Sie beliebig viele **sekundäre** Storage-Konten an Ihr Media Services-Konto anfügen. Media Services unterstützt Konten des Typs **Allgemein v2** (GPv2) oder **Allgemein v1** (GPv1). Reine Blobkonten sind als **primäre** Konten nicht zulässig. Weitere Informationen zu Storage-Konten finden Sie unter [Optionen für Azure Storage-Konten](../../../storage/common/storage-account-overview.md). 

In diesem Beispiel erstellen Sie ein universelles LRS-Standardkonto der Version 2. Falls Sie mit Speicherkonten experimentieren möchten, verwenden Sie `--sku Standard_LRS`. Wenn Sie eine SKU für die Produktion auswählen, sollten Sie jedoch die Verwendung von `--sku Standard_RAGRS` erwägen, da diese Option geografische Replikation zum Gewährleisten der Geschäftskontinuität bietet. Weitere Informationen finden Sie unter [Storage accounts](/cli/azure/storage/account) (Speicherkonten).

Der folgende Befehl erstellt ein Storage-Konto, das dem Media Services-Konto zugeordnet werden soll. Im folgenden Skript können Sie `storageaccountforams` durch Ihren Wert ersetzen. `amsResourceGroup` muss dem Wert entsprechen, den Sie für die Ressourcengruppe im vorherigen Schritt angegeben haben. Der Speicherkontoname muss kürzer als 24 Zeichen sein.

```azurecli
az storage account create --name storageaccountforams --kind StorageV2 --sku Standard_LRS -l westus2 -g amsResourceGroup
```

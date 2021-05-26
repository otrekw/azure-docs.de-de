---
title: Erstellen eines Azure Media Services-Kontos
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie ein Azure Media Services-Konto erstellen.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/4/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7c6486c156cfc9cec9065854c55b2d8e2788fdc0
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110062547"
---
# <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Um mit dem Verschlüsseln, Codieren, Analysieren, Verwalten und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Das Media Services-Konto muss einem oder mehreren Speicherkonten zugeordnet werden. In diesem Artikel werden die Schritte zum Erstellen eines neuen Azure Media Services-Kontos beschrieben.

[!INCLUDE [note 2020-05-01 API](./includes/note-2020-05-01-account-creation.md)]

 Sie können entweder das Azure-Portal oder die CLI verwenden, um ein Media Services-Konto zu erstellen. Wählen Sie die Registerkarte für die Methode aus, die Sie verwenden möchten.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<!-- NOTE: The following are in the includes folder and are reused in other How To articles. All task based content should be in the includes folder with the task- prefix prepended to the file name. -->

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [the media services account and storage account must be in the same subscription](./includes/note-account-storage-same-subscription.md)]

[!INCLUDE [create a media services account in the portal](./includes/task-create-media-services-account-portal.md)]

## <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli/)

[!INCLUDE [the media services account and storage account must be in the same subscription](./includes/note-account-storage-same-subscription.md)]

## <a name="set-the-azure-subscription"></a>Einrichten des Azure-Abonnements

[!INCLUDE [Set the Azure subscription with CLI](./includes/task-set-azure-subscription-cli.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create a resource group with CLI](./includes/task-create-resource-group-cli.md)]

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. Weitere Informationen zur Verwendung von Speicherkonten in Media Services finden Sie unter [Storage accounts](storage-account-concept.md) (Storage-Konten).

Sie müssen über ein **primäres** Storage-Konto verfügen. Darüber hinaus können Sie beliebig viele **sekundäre** Storage-Konten an Ihr Media Services-Konto anfügen. Media Services unterstützt Konten des Typs **Allgemein v2** (GPv2) oder **Allgemein v1** (GPv1). Reine Blobkonten sind als **primäre** Konten nicht zulässig. Weitere Informationen zu Storage-Konten finden Sie unter [Optionen für Azure Storage-Konten](../../storage/common/storage-account-overview.md).

In diesem Beispiel erstellen Sie ein universelles LRS-Standardkonto der Version 2. Falls Sie mit Speicherkonten experimentieren möchten, verwenden Sie `--sku Standard_LRS`. Wenn Sie eine SKU für die Produktion auswählen, sollten Sie jedoch die Verwendung von `--sku Standard_RAGRS` erwägen, da diese Option geografische Replikation zum Gewährleisten der Geschäftskontinuität bietet. Weitere Informationen finden Sie unter [Storage accounts](/cli/azure/storage/account) (Speicherkonten).

[!INCLUDE [Create a storage account with CLI](./includes/task-create-storage-account-cli.md)]

## <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

[!INCLUDE [Create a Media Services account with CLI](./includes/task-create-media-services-account-cli.md)]

---

## <a name="next-steps"></a>Nächste Schritte

[Streamen einer Datei](stream-files-dotnet-quickstart.md)
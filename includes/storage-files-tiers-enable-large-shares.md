---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8a2e5defd0672516d52d4f3477641f39eca63368
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597804"
---
Standardmäßig können Standarddateifreigaben nur bis zu 5 TiB umfassen. Das Freigabelimit kann jedoch auf 100 TiB erhöht werden. Zu diesem Zweck muss das Feature *große Dateifreigabe* auf Speicherkontoebene aktiviert werden. Storage Premium-Konten (*FileStorage*-Speicherkonten) verfügen nicht über das Featureflag „große Dateifreigabe“, da alle Premium-Dateifreigaben bereits für die Bereitstellung bis zur gesamten 100-TiB-Kapazität aktiviert sind.

Sie können nur große Dateifreigaben für lokal redundante oder zonenredundante Standardspeicherkonten aktivieren. Sobald Sie das Featureflag für große Dateifreigaben aktiviert haben, können Sie die Redundanzebene nicht mehr in georedundanten oder geozonenredundanten Speicher ändern.

Um große Dateifreigaben für ein vorhandenes Speicherkonto zu aktivieren, navigieren Sie zur Ansicht **Konfiguration** im Inhaltsverzeichnis des Speicherkontos, und schalten Sie den Kippschalter für die große Dateifreigabe auf „aktiviert“:

![Screenshot des Aktivierens des Kippschalters für große Dateifreigabe im Azure-Portal](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

Sie können 100-TiB-Dateifreigaben auch über das [`Set-AzStorageAccount`](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount)-PowerShell-Cmdlet und den Azure CLI-Befehl [`az storage account update`](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) aktivieren.

Weitere Informationen zum Aktivieren von großen Dateifreigaben in neuen Speicherkonten finden Sie unter [Erstellen einer Dateifreigabe](../articles/storage/files/storage-how-to-create-file-share.md).
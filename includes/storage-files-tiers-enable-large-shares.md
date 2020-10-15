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
ms.openlocfilehash: cd7b889560acbe484581f065b641375c222f7ca8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81536483"
---
Standardmäßig können Standarddateifreigaben nur bis zu 5 TiB umfassen. Das Freigabelimit kann jedoch auf 100 TiB erhöht werden. Zu diesem Zweck muss das Feature *große Dateifreigabe* auf Speicherkontoebene aktiviert werden. Storage Premium-Konten (*FileStorage*-Speicherkonten) verfügen nicht über das Featureflag „große Dateifreigabe“, da alle Premium-Dateifreigaben bereits für die Bereitstellung bis zur gesamten 100-TiB-Kapazität aktiviert sind.

Sie können nur große Dateifreigaben für lokal redundante oder zonenredundante Standardspeicherkonten aktivieren. Sobald Sie das Featureflag für große Dateifreigaben aktiviert haben, können Sie die Redundanzebene nicht mehr in georedundanten oder geozonenredundanten Speicher ändern.

Um große Dateifreigaben für ein vorhandenes Speicherkonto zu aktivieren, navigieren Sie zur Ansicht **Konfiguration** im Inhaltsverzeichnis des Speicherkontos, und schalten Sie den Kippschalter für die große Dateifreigabe auf „aktiviert“:

![Screenshot des Aktivierens des Kippschalters für große Dateifreigabe im Azure-Portal](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

Sie können 100-TiB-Dateifreigaben auch über das [`Set-AzStorageAccount`](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount)-PowerShell-Cmdlet und den Azure CLI-Befehl [`az storage account update`](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) aktivieren. Ausführliche Anweisungen zum Aktivieren großer Dateifreigaben finden Sie unter[Aktivieren und Erstellen großer Dateifreigaben](../articles/storage/files/storage-files-how-to-create-large-file-share.md).

Weitere Informationen zum Erstellen großer Dateifreigaben in neuen Speicherkonten finden Sie unter [Erstellen einer Azure-Dateifreigabe](../articles/storage/files/storage-how-to-create-file-share.md).

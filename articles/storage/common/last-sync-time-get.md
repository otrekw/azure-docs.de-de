---
title: Überprüfen der Eigenschaft „Letzte Synchronisierung“ für ein Speicherkonto
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie die Eigenschaft „Letzte Synchronisierung“ für ein Speicherkonto mit Georeplikation überprüfen. Die Eigenschaft „Letzte Synchronisierung“ gibt den Zeitpunkt der letzten erfolgreichen Schreibvorgänge aus der primären Region in die sekundäre Region an.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/28/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 242700c05053aa9d07e3a561a21986c8451a46c7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91612443"
---
# <a name="check-the-last-sync-time-property-for-a-storage-account"></a>Überprüfen der Eigenschaft „Letzte Synchronisierung“ für ein Speicherkonto

Wenn Sie ein Speicherkonto konfigurieren, können Sie angeben, dass Ihre Daten in eine sekundäre Region kopiert werden, die Hunderte Kilometer von der primären Region entfernt ist. Die Georeplikation bietet Dauerhaftigkeit für Ihre Daten im Fall eines signifikanten Ausfalls in der primären Region, z. B. bei einer Naturkatastrophe. Wenn Sie darüber hinaus Lesezugriff auf die sekundäre Region aktivieren, bleiben Ihre Daten auch dann noch für Lesevorgänge verfügbar, wenn die primäre Region nicht mehr verfügbar ist. Sie können Ihre Anwendung so entwerfen, dass sie nahtlos zum Lesen aus der sekundären Region wechselt, wenn die primäre Region nicht reagiert.

Georedundanter Speicher (GRS) und geozonenredundanter Speicher (GZRS) replizieren Ihre Daten asynchron in eine sekundäre Region. Für den Lesezugriff in der sekundären Region aktivieren Sie georedundanten Speicher mit Lesezugriff (RA-GRS) oder geozonenredundanten Speicher mit Lesezugriff (RA-GZRS). Weitere Informationen zu den verschiedenen Redundanzoptionen, die von Azure Storage angeboten werden, finden Sie unter [Azure Storage-Redundanz](storage-redundancy.md).

In diesem Artikel wird beschrieben, wie Sie die Eigenschaft **Letzte Synchronisierung** für Ihr Speicherkonto überprüfen, um mögliche Abweichungen zwischen der primären und der sekundären Region auswerten zu können.

## <a name="about-the-last-sync-time-property"></a>Informationen zur Eigenschaft „Letzte Synchronisierung“

Da die Georeplikation asynchron erfolgt, ist es möglich, dass Daten, die in die primäre Region geschrieben wurden, zum Zeitpunkt eines Ausfalls noch nicht in die sekundäre Region geschrieben wurden. Die Eigenschaft **Letzte Synchronisierung** gibt den Zeitpunkt an, an dem zuletzt Daten aus der primären Region erfolgreich in die sekundäre Region geschrieben wurden. Alle Schreibvorgänge, die vor dem Zeitpunkt der letzten Synchronisierung in der primären Region durchgeführt wurden, sind zum Lesen am sekundären Standort verfügbar. Schreibvorgänge in der primären Region, die nach dem Zeitpunkt in der Eigenschaft „Letzte Synchronisierung“ stattgefunden haben, stehen unter Umständen noch nicht zum Lesen zur Verfügung.

Die Eigenschaft **Last Sync Time** ist ein GMT-Datums-/Uhrzeitwert.

## <a name="get-the-last-sync-time-property"></a>Abrufen der Eigenschaft „Letzte Synchronisierung“

Sie können PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, um den Wert der Eigenschaft **Letzte Synchronisierung** abzurufen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Wenn Sie den Zeitpunkt der letzten Synchronisierung für das Speicherkonto mit PowerShell abrufen möchten, installieren Sie Version 1.11.0 oder höher des [Az.Storage](https://www.powershellgallery.com/packages/Az.Storage)-Moduls. Überprüfen Sie dann die Eigenschaft **GeoReplicationStats.LastSyncTime** des Speicherkontos. Denken Sie daran, die Platzhalterwerte durch Ihre eigenen Werte zu ersetzen:

```powershell
$lastSyncTime = $(Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -Name <storage-account> `
    -IncludeGeoReplicationStats).GeoReplicationStats.LastSyncTime
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Rufen Sie den Zeitpunkt der letzten Synchronisierung für das Speicherkonto mithilfe der Azure-Befehlszeilenschnittstelle ab, indem Sie die **geoReplicationStats.lastSyncTime**-Eigenschaft des Speicherkontos überprüfen. Verwenden Sie den Parameter `--expand`, um Werte für die unter **geoReplicationStats** geschachtelten Eigenschaften zurückzugeben. Denken Sie daran, die Platzhalterwerte durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
$lastSyncTime=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --expand geoReplicationStats \
    --query geoReplicationStats.lastSyncTime \
    --output tsv)
```

---

## <a name="see-also"></a>Weitere Informationen

- [Azure-Speicherredundanz](storage-redundancy.md)
- [Ändern der Redundanzoption für ein Speicherkonto](redundancy-migration.md)
- [Verwenden von Georedundanz zum Entwerfen von hochverfügbaren Anwendungen](geo-redundant-design.md)

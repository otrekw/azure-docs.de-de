---
title: Aktualisieren von Media Services v3 nach dem Austausch der Speicherzugriffsschlüssel | Microsoft-Dokumentation
description: In diesem Artikel lernen erfahren Sie, wie Sie Media Services v3 nach dem Austausch der Speicherzugriffsschlüssel aktualisieren.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2021
ms.author: inhenkel
ms.openlocfilehash: 385e6109a80c8e1e455f98b5d02ca5762f8051c7
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106281276"
---
# <a name="update-media-services-v3-after-rolling-storage-access-keys"></a>Aktualisieren von Media Services v3 nach dem Austausch der Speicherzugriffsschlüssel

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Wenn Sie ein neues Azure Media Services Konto (AMS) erstellen, werden Sie aufgefordert, ein Azure Storage-Konto auszuwählen.  Sie können Ihrem Media Services-Konto mehrere Speicherkonten hinzufügen. In diesem Artikel wird das Rotieren von Speicherschlüsseln erläutert. Außerdem wird erläutert, wie einem Medienkonto Speicherkonten hinzugefügt werden.

Für die in diesem Artikel beschriebenen Aktionen werden [Azure Resource Manager-APIs](/rest/api/media/operations/azure-media-services-rest-api-reference) und [PowerShell](/powershell/module/az.media) verwendet.  Weitere Informationen finden Sie unter [Verwalten von Azure-Ressourcen mit PowerShell und Resource Manager](../../azure-resource-manager/management/manage-resource-groups-powershell.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="storage-access-key-generation"></a>Generieren eines Speicherzugriffsschlüssel

Wenn ein neues Speicherkonto erstellt wird, generiert Azure zwei 512-Bit-Speicherzugriffsschlüssel, die zum Authentifizieren des Zugriffs auf Ihr Speicherkonto verwendet werden. Zum besseren Schutz Ihrer Speicherverbindungen sollten Sie Ihren Speicherzugriffsschlüssel in regelmäßigen Abständen neu generieren und rotieren. Zwei Zugriffsschlüssel (primär und sekundär) werden bereitgestellt, damit Sie mit einem der Zugriffsschlüssel die Verbindungen mit dem Speicherkonto aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren. Dieser Vorgang wird als Austausch der Zugriffsschlüssel bezeichnet.

Für Media Services muss ein Speicherschlüssel bereitgestellt werden. Insbesondere hängen die Locators, mit denen Ihre Assets gestreamt oder heruntergeladen werden, vom angegebenen Speicherzugriffsschlüssel ab. Beim Erstellen eines AMS-Kontos wird standardmäßig die Abhängigkeit vom primären Speicherzugriffsschlüssel übernommen. Sie können als Benutzer aber den Speicherschlüssel von AMS aktualisieren. Sie müssen Media Services mitteilen, welcher Schlüssel verwendet werden soll. Führen Sie dazu die folgenden Schritte aus:

>[!NOTE]
> Wenn Sie mehrere Speicherkonten verwenden, müssen Sie dieses Verfahren für jedes Speicherkonto einzeln ausführen. Die Reihenfolge beim Rotieren von Speicherschlüsseln ist nicht festgelegt. Sie können zuerst den sekundären Schlüssel und dann den primären Schlüssel rotieren oder auch umgekehrt.
>
> Testen Sie die Schritte zunächst mit einem Vorproduktionskonto, bevor Sie sie für ein Produktionskonto ausführen.
>

## <a name="steps-to-rotate-storage-keys"></a>Schritte zum Rotieren von Speicherschlüsseln
 
 1. Ändern Sie den Primärschlüssel des Speicherkontos über das PowerShell-Cmdlet oder über das [Azure-Portal](https://portal.azure.com/).
 2. Rufen Sie das Cmdlet `Sync-AzMediaServiceStorageKeys` mit entsprechenden Parametern auf, um für das Medienkonto das Abrufen von Speicherkontoschlüsseln zu erzwingen.
 
    Das folgende Beispiel zeigt, wie Schlüssel mit Speicherkonten synchronisiert werden.
  
    `Sync-AzMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId`
  
 3. Warten Sie etwa eine Stunde ab. Stellen Sie sicher, dass die Streamingszenarien funktionieren.
 4. Ändern Sie den Sekundärschlüssel des Speicherkontos über das PowerShell-Cmdlet oder über das Azure-Portal.
 5. Rufen Sie das PowerShell-Cmdlet `Sync-AzMediaServiceStorageKeys` mit entsprechenden Parametern auf, um für das Medienkonto das Abrufen neuer Speicherkontoschlüssel zu erzwingen.
 6. Warten Sie etwa eine Stunde ab. Stellen Sie sicher, dass die Streamingszenarien funktionieren.
 
### <a name="a-powershell-cmdlet-example"></a>Beispiel für ein PowerShell-Cmdlet

Das folgende Beispiel veranschaulicht, wie das Speicherkonto abgerufen und mit dem AMS-Konto synchronisiert wird.

```console
$regionName = "West US"
$resourceGroupName = "SkyMedia-USWest-App"
$mediaAccountName = "sky"
$storageAccountName = "skystorage"
$storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

Sync-AzMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
```

## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>Schritte zum Hinzufügen von Speicherkonten zu Ihrem AMS-Konto

Im folgenden Artikel wird gezeigt, wie Sie Ihrem AMS-Konto Speicherkonten hinzufügen: [Anfügen von mehreren Speicherkonten an ein einzelnes Media Services-Konto](storage-managing-multiple-storage-accounts-how-to.md).

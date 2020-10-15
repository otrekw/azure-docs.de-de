---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b2ff542d2782293e89b66e5d25cb67a9bcde6da8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "75772909"
---
Dieser Fehler kann auftreten, wenn der Zugriff auf den Azure-Dateisynchronisierungsdienst vom Server aus nicht verfügbar ist. Sie können diesen Fehler beheben, indem Sie die folgenden Schritte durchführen:

1. Stellen Sie sicher, dass der Windows-Dienst `FileSyncSvc.exe` nicht von der Firewall blockiert wird.
2. Stellen Sie sicher, dass Port 443 für ausgehende Verbindungen mit dem Azure-Dateisynchronisierungsdienst geöffnet ist. Hierfür können Sie das `Test-NetConnection`-Cmdlet verwenden. Die URL für den `<azure-file-sync-endpoint>`-Platzhalter unten finden Sie im Dokument [Proxy- und Firewall-Einstellungen der Azure-Dateisynchronisierung](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall). 

    ```powershell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Stellen Sie sicher, dass die Proxykonfiguration wie erwartet festgelegt ist. Verwenden Sie dazu das `Get-StorageSyncProxyConfiguration`-Cmdlet. Weitere Informationen zur Proxykonfiguration für die Azure-Dateisynchronisierung finden Sie unter [Proxy- und Firewall-Einstellungen der Azure-Dateisynchronisierung](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```powershell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
4. Verwenden Sie das Test-Cmdlet „StorageSyncNetworkConnectivity“ zum Überprüfen der Netzwerkkonnektivität mit den Dienstendpunkten. Wenn Sie weitere Informationen benötigen, lesen Sie [Testen der Netzwerkkonnektivität mit Dienstendpunkten](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#test-network-connectivity-to-service-endpoints).    

5. Wenden Sie sich an Ihren Netzwerkadministrator, um weitere Unterstützung zur Problembehandlung bei Netzwerkverbindungen zu erhalten.

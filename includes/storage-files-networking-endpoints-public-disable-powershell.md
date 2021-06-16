---
title: Datei einfügen
description: Datei einfügen
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file, devx-track-azurepowershell
ms.openlocfilehash: d53bfaab5e00b95be52f652b72e9d55a76c396a6
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110721810"
---
Mit dem folgenden PowerShell-Befehl wird der gesamte Datenverkehr an den öffentlichen Endpunkt des Speicherkontos abgelehnt. Beachten Sie, dass für diesen Befehl der Parameter `-Bypass` auf `AzureServices` festgelegt ist. Hierdurch können vertrauenswürdige Erstanbieterdienste, z. B. die Azure-Dateisynchronisierung, über den öffentlichen Endpunkt auf das Speicherkonto zugreifen.

```PowerShell
# This assumes $storageAccount is still defined from the beginning of this of this guide.
$storageAccount | Update-AzStorageAccountNetworkRuleSet `
        -DefaultAction Deny `
        -Bypass AzureServices `
        -WarningAction SilentlyContinue `
        -ErrorAction Stop | `
    Out-Null
```

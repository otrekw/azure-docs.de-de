---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a0a9bc29c3e20a025fb2c46a71c2f134c37bee04
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "84465121"
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
---
title: Rotieren von Speicherkonto-Zugriffsschlüsseln mit PowerShell
titleSuffix: Azure Storage
description: Erstellen Sie ein Azure Storage-Konto, und rufen Sie dann einen der Zugriffsschlüssel für das Konto ab, und rotieren Sie ihn.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.devlang: powershell
ms.topic: sample
ms.date: 12/04/2019
ms.author: tamram
ms.openlocfilehash: 52ebed3de093f15d8188ee5fec49d75d5a4a206d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80060806"
---
# <a name="rotate-storage-account-access-keys-with-powershell"></a>Rotieren von Speicherkonto-Zugriffsschlüsseln mit PowerShell

Mit diesem Skript wird ein Azure Storage-Konto erstellt, der primäre Zugriffsschlüssel des neuen Speicherkontos wird angezeigt, und dann wird der Schlüssel erneuert (rotiert).

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.ps1 "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, das Speicherkonto und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name rotatekeystestrg
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um das Speicherkonto zu erstellen und einen der Zugriffsschlüssel abzurufen und zu rotieren. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzLocation](/powershell/module/az.resources/get-azlocation) | Ruft alle Standorte und die unterstützten Ressourcenanbieter für jeden Standort ab. |
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Azure-Ressourcengruppe. |
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Erstellt ein Speicherkonto. |
| [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) | Ruft die Zugriffsschlüssel für ein Azure-Speicherkonto ab. |
| [New-AzStorageAccountKey](/powershell/module/az.storage/new-azstorageaccountkey) | Generiert einen Zugriffsschlüssel für ein Azure Storage-Konto neu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für Speicher finden Sie in den [PowerShell-Beispielen für Azure Blob Storage](../blobs/storage-samples-blobs-powershell.md).

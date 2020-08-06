---
title: Azure CLI-Beispielskript – Einbinden eines Betriebssystem-Datenträgers
description: Azure CLI-Beispielskript – Einbinden eines Betriebssystem-Datenträgers
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 735b16ad054edfea7cf0ad2e1e5b9c76b664e6c3
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87479626"
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Problembehandlung bei einem Betriebssystem-Datenträger eines virtuellen Computers

Wenn bei einem virtuellen Computer ein Fehler oder Probleme aufgetreten sind, bindet dieses Skript dessen Betriebssystem-Datenträger als Datenträger für Daten in einen zweiten virtuellen Computer ein. Dies kann bei der Behandlung von Datenträgerproblemen oder bei der Wiederherstellung von Daten nützlich sein.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az vm show](/cli/azure/vm) | Gibt eine Liste virtueller Computer zurück. In diesem Fall wird die Abfrageoption verwendet, um den Betriebssystem-Datenträger des virtuellen Computers zurückzugeben. Dieser Wert wird dann einer Variablen namens „uri“ hinzugefügt. |
| [az vm delete](/cli/azure/vm) | Löscht einen virtuellen Computer. |
| [az vm create](/cli/azure/vm) | Erstellt einen virtuellen Computer.  |
| [az vm disk attach](/cli/azure/vm/disk) | Fügt einem virtuellen Computer einen Datenträger an. |
| [az vm list-ip-addresses](/cli/azure/vm) | Gibt die IP-Adressen eines virtuellen Computers zurück. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

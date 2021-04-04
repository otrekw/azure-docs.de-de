---
title: Azure CLI-Voraussetzungen für Azure HPC Cache
description: Einrichtungsschritte, bevor Sie die Azure CLI zum Erstellen oder Ändern einer Azure HPC Cache-Instanz verwenden können
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/08/2020
ms.author: v-erkel
ms.openlocfilehash: 13f45c96a830110bd0f4a2d4a2b422921d7a2e31
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "94654455"
---
# <a name="set-up-azure-cli-for-azure-hpc-cache"></a>Einrichten der Azure-CLI für Azure HPC Cache

Führen Sie die unten beschriebenen Schritte aus, um die Umgebung vorzubereiten, bevor Sie die Azure CLI zum Erstellen oder Verwalten einer Azure HPC Cache-Instanz verwenden.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

 - Für Azure HPC Cache ist mindestens Version 2.7 der Azure-Befehlszeilenschnittstelle (Azure CLI) erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="set-default-resource-group-optional"></a>Festlegen der Standardressourcengruppe (optional)

Die meisten hpc-cache-Befehle erfordern, dass Sie die Ressourcengruppe des Cache übergeben. Mit [az configure](/cli/azure/reference-index#az-configure) können Sie die Standardressourcengruppe festlegen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Azure CLI-Erweiterung installiert und sich angemeldet haben, können Sie die Azure CLI verwenden, um Azure HPC Cache-Systeme zu erstellen und zu verwalten.

* [Erstellen einer Azure HPC Cache-Instanz](hpc-cache-create.md)
* [Dokumentation zu den hpc-cache-Befehlen der Azure CLI](/cli/azure/ext/hpc-cache/hpc-cache)

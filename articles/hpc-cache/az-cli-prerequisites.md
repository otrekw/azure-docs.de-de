---
title: Azure CLI-Voraussetzungen für Azure HPC Cache
description: Einrichtungsschritte, bevor Sie die Azure CLI zum Erstellen oder Ändern einer Azure HPC Cache-Instanz verwenden können
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/08/2020
ms.author: v-erkel
ms.openlocfilehash: 0b8e1158bc60c4cceea508db988000fe952a90a4
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107864287"
---
# <a name="set-up-azure-cli-for-azure-hpc-cache"></a>Einrichten der Azure-CLI für Azure HPC Cache

Führen Sie die unten beschriebenen Schritte aus, um die Umgebung vorzubereiten, bevor Sie die Azure CLI zum Erstellen oder Verwalten einer Azure HPC Cache-Instanz verwenden.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

 - Für Azure HPC Cache ist mindestens Version 2.7 der Azure-Befehlszeilenschnittstelle (Azure CLI) erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="set-default-resource-group-optional"></a>Festlegen der Standardressourcengruppe (optional)

Die meisten hpc-cache-Befehle erfordern, dass Sie die Ressourcengruppe des Cache übergeben. Mit [az configure](/cli/azure/reference-index#az_configure) können Sie die Standardressourcengruppe festlegen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Azure CLI-Erweiterung installiert und sich angemeldet haben, können Sie die Azure CLI verwenden, um Azure HPC Cache-Systeme zu erstellen und zu verwalten.

* [Erstellen einer Azure HPC Cache-Instanz](hpc-cache-create.md)
* [Dokumentation zu den hpc-cache-Befehlen der Azure CLI](/cli/azure/hpc-cache)

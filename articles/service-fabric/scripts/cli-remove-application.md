---
title: Azure Service Fabric-CLI-Skript (sfctl) – Entfernungsbeispiel
description: Entfernen einer Anwendung aus einem Azure Service Fabric-Cluster mithilfe der Azure Service Fabric-CLI
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 12/06/2017
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: fd5d20ed3f2cc41f4d7d51f06d4bc90a6afd22eb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "75526531"
---
# <a name="remove-an-application-from-a-service-fabric-cluster-using-the-service-fabric-cli"></a>Entfernen einer Anwendung aus einem Service Fabric-Cluster mithilfe der Service Fabric-CLI

Dieses Beispielskript löscht eine ausgeführte Service Fabric-Anwendungsinstanz und hebt dann die Registrierung für einen Anwendungstyp und eine Version im Cluster auf.  Durch das Löschen der Anwendungsinstanz werden auch alle ausgeführten Dienstinstanzen gelöscht, die dieser Anwendung zugeordnet sind. Als Nächstes werden die Anwendungsdateien aus dem Imagespeicher gelöscht. 

Installieren Sie ggf. die [Service Fabric-CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Service Fabric-Befehlszeilenschnittstelle](../service-fabric-cli.md).

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).

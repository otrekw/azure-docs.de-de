---
title: Voraussetzungen | Direkter Verbindungsmodus
description: Hier werden die Voraussetzungen zum Bereitstellen des Datencontrollers im direkten Verbindungsmodus erläutert.
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 03/31/2021
ms.topic: overview
ms.openlocfilehash: 940c08dc56e8b0f2217e556da1af31b44ceeda0a
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107716293"
---
# <a name="deploy-data-controller---direct-connect-mode-prerequisites"></a>Bereitstellen des Datencontrollers: Direkter Verbindungsmodus (Voraussetzungen)

In diesem Artikel wird beschrieben, wie Sie die Bereitstellung eines Datencontrollers für Azure Arc-fähige Datendienste im direkten Verbindungsmodus vorbereiten.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

Ganz allgemein müssen u. a. folgende Voraussetzungen erfüllt werden:

1. Installieren von Tools
1. Hinzufügen von Erweiterungen
1. Erstellen des Dienstprinzipals und Konfigurieren von Rollen für Metriken
1. Verbinden eines Kubernetes-Clusters mit Azure mithilfe von Kubernetes mit Azure Arc-Unterstützung

Nachdem Sie diese Voraussetzungen erfüllt haben, können Sie die Schritte unter [Bereitstellen eines Azure Arc-Datencontrollers | Direkter Verbindungsmodus](deploy-data-controller-direct-mode.md) ausführen.

In den übrigen Abschnitten dieses Artikels werden die Voraussetzungen beschrieben.

## <a name="install-tools"></a>Installieren von Tools

- Helm-Version 3.3 oder höher ([Installieren](https://helm.sh/docs/intro/install/))
- Azure CLI ([Installieren](/sql/azdata/install/deploy-install-azdata))

## <a name="add-extensions-for-azure-cli"></a>Hinzufügen von Erweiterungen für die Azure CLI

Darüber hinaus sind die folgenden az-Erweiterungen erforderlich:
- `k8s-extension` der Azure CLI (0.2.0)
- `customlocation` der Azure CLI (0.1.0)

Das `az`-Beispiel und die zugehörigen CLI-Erweiterungen lauten wie folgt:

```console
$ az version
{
  "azure-cli": "2.19.1",
  "azure-cli-core": "2.19.1",
  "azure-cli-telemetry": "1.0.6",
  "extensions": {
    "connectedk8s": "1.1.0",
    "customlocation": "0.1.0",
    "k8s-configuration": "1.0.0",
    "k8s-extension": "0.2.0"
  }
}
```

## <a name="create-service-principal-and-configure-roles-for-metrics"></a>Erstellen des Dienstprinzipals und Konfigurieren von Rollen für Metriken

Führen Sie die im Artikel [Hochladen von Nutzungsdaten, Metriken und Protokollen in Azure Monitor](upload-metrics-and-logs-to-azure-monitor.md) beschriebenen Schritte aus, erstellen Sie einen Dienstprinzipal, und gewähren Sie die Rollen wie in diesem Artikel beschrieben. 

Beim [Bereitstellen des Azure Arc-Datencontrollers](deploy-data-controller-direct-mode.md) sind die Informationen für die SPN-Client-ID, die Mandanten-ID und den geheimen Clientschlüssel erforderlich. 

## <a name="connect-kubernetes-cluster-to-azure-using-azure-arc-enabled-kubernetes"></a>Verbinden eines Kubernetes-Clusters mit Azure mithilfe von Kubernetes mit Azure Arc-Unterstützung

Führen Sie zum Abschließen dieser Aufgabe die Schritte unter [Schnellstart: Herstellen einer Verbindung eines vorhandenen Kubernetes-Clusters mit Azure Arc](../kubernetes/quickstart-connect-cluster.md) aus.

## <a name="next-steps"></a>Nächste Schritte

Wenn diese Voraussetzungen erfüllt sind, können Sie die Schritte unter [Bereitstellen eines Azure Arc-Datencontrollers | Direkter Verbindungsmodus](deploy-data-controller-direct-mode.md) ausführen.

---
title: Bereitstellen von Azure IoT Edge-Workloads (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 02/19/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Bereitstellen von Azure IoT Edge-Workloads
keywords: Kubernetes, Arc, Azure, K8s, Container
ms.openlocfilehash: c352455b69360df0b26f5aac38fc40ccb30fb9de
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101650412"
---
# <a name="deploy-azure-iot-edge-workloads-preview"></a>Bereitstellen von Azure IoT Edge-Workloads (Vorschauversion)

## <a name="overview"></a>Übersicht

Azure Arc und Azure IoT Edge ergänzen sich in ihren Funktionen sehr gut. 

Azure Arc bietet Mechanismen für Clusteroperatoren zum Konfigurieren der zugrunde liegenden Komponenten eines Clusters sowie zum Anwenden und Erzwingen von Clusterrichtlinien. 

Azure IoT Edge ermöglicht dann Anwendungsoperatoren die bedarfsgerechte Remotebereitstellung und -verwaltung der Workloads mit sinnvollen Grundtypen für Clouderfassung und bidirektionale Kommunikation. 

Das folgende Diagramm veranschaulicht die Beziehungen zwischen Azure Arc und Azure IoT Edge:

![IoT Arc-Konfiguration](./media/edge-arc.png)

## <a name="pre-requisites"></a>Voraussetzungen

* [Registrieren eines IoT Edge-Geräts](../../iot-edge/quickstart-linux.md#register-an-iot-edge-device) und [Bereitstellen des Moduls für den simulierten Temperatursensor](../../iot-edge/quickstart-linux.md#deploy-a-module). Beachten Sie die Verbindungszeichenfolge des Geräts für die unten genannte Datei *values.yaml*.

* Verwenden Sie die [IoT Edge-Unterstützung für Kubernetes](https://aka.ms/edgek8sdoc), um die Bereitstellung über den Flux-Operator von Azure Arc auszuführen.

* Laden Sie die Datei [*values.yaml*](https://github.com/Azure/iotedge/blob/preview/iiot/kubernetes/charts/edge-kubernetes/values.yaml) für das IoT Edge-Helm-Chart herunter, und ersetzen Sie den Platzhalter `deviceConnectionString` am Ende der Datei durch die notierte Verbindungszeichenfolge. Legen Sie die anderen unterstützte Optionen für die Chartinstallation entsprechend Ihren Anforderungen fest. Erstellen Sie einen Namespace für die IoT Edge-Workload, und generieren Sie darin ein Geheimnis:

  ```
  $ kubectl create ns iotedge

  $ kubectl create secret generic dcs --from-file=fully-qualified-path-to-values.yaml --namespace iotedge
  ```

  Sie können dies auch remote mithilfe des [Clusterkonfigurationsbeispiels](./tutorial-use-gitops-connected-cluster.md) einrichten.

## <a name="connect-a-cluster"></a>Verbinden eines Clusters

Verwenden Sie die `az`-Azure CLI-Erweiterung `connectedk8s`, um einen Kubernetes-Cluster mit Azure Arc zu verbinden:

  ```
  az connectedk8s connect --name AzureArcIotEdge --resource-group AzureArcTest
  ```

## <a name="create-a-configuration-for-iot-edge"></a>Erstellen einer Konfiguration für IoT Edge

Das [Git-Beispielrepository](https://github.com/veyalla/edgearc) verweist auf das IoT Edge-Helm-Chart und auf das Geheimnis, das im Abschnitt „Voraussetzungen“ erstellt wurde.

Verwenden Sie die `az`-Azure CLI-Erweiterung `k8s-configuration`, um eine Konfiguration zum Verknüpfen des verbundenen Clusters mit dem Git-Repository zu erstellen:

  ```
  az k8s-configuration create --name iotedge --cluster-name AzureArcIotEdge --resource-group AzureArcTest --operator-instance-name iotedge --operator-namespace azure-arc-iot-edge --enable-helm-operator --helm-operator-chart-version 0.6.0 --helm-operator-chart-values "--set helm.versions=v3" --repository-url "git://github.com/veyalla/edgearc.git" --cluster-scoped
  ```

Nach wenigen Minuten sollten die IoT Edge-Workloadmodule im Namespace `iotedge` in Ihrem Cluster bereitgestellt werden. 

Sie können die Protokolle des Pods `SimulatedTemperatureSensor` in diesem Namespace anzeigen, um die generierten Beispielwerte einzusehen. Sie können die bei Ihrem IoT-Hub eingehenden Nachrichten auch mit der [Azure IoT Hub-Toolkit-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) anzeigen.

## <a name="cleanup"></a>Cleanup

Sie können die Konfiguration folgendermaßen entfernen:

```
az k8s-configuration delete -g AzureArcTest --cluster-name AzureArcIotEdge --name iotedge
```

## <a name="next-steps"></a>Nächste Schritte

[Verwenden von Azure Policy zum Steuern der Clusterkonfiguration](./use-azure-policy.md)

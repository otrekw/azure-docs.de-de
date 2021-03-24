---
title: Bereinigen eines eigenständigen Clusters
description: In diesem Tutorial erfahren Sie, wie Sie für Ihren eigenständigen Service Fabric-Cluster AWS- oder Azure-Ressourcen löschen.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: 0d46e9068a311594f779411c3ccee2b408febb3f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91842885"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Tutorial: Bereinigen Ihres eigenständigen Clusters

Eigenständige Service Fabric-Cluster bieten die Möglichkeit, Ihre eigene Umgebung zum Hosten von Service Fabric auszuwählen. In dieser Tutorialreihe erstellen Sie einen in AWS oder Azure gehosteten eigenständigen Cluster und stellen darin eine Anwendung bereit.

Dieses Tutorial ist der vierte Teil einer Serie. In diesem Teil des Tutorials erfahren Sie, wie Sie die AWS- oder Azure-Ressourcen löschen, die Sie zum Hosten Ihres Service Fabric-Clusters erstellt haben.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Entfernen eines Service Fabric-Clusters
> * Löschen Ihrer AWS- oder Azure-Ressourcen

## <a name="remove-a-service-fabric-cluster"></a>Entfernen eines Service Fabric-Clusters

1. Stellen Sie eine RDP-Verbindung mit der VM her, die Sie zum Installieren von Service Fabric verwendet haben.
2. Öffnen Sie PowerShell.
3. Wechseln Sie zum Verzeichnis des extrahierten Ordners aus dem zweiten Tutorial.
4. Führen Sie den folgenden Befehl aus, um den Service Fabric-Cluster zu entfernen:

  ```powershell
  .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
  ```

5. Geben Sie `Y` ein, wenn Sie dazu aufgefordert werden. Wenn der Vorgang erfolgreich war, erhalten Sie eine Ausgabe wie die folgende (mit Ihren eigenen IP-Adressen):

  ```powershell
  Best Practices Analyzer completed successfully.
  Removing configuration from machine 172.31.21.141
  Removing configuration from machine 172.31.27.1
  Removing configuration from machine 172.31.20.163
  Configuration removed from machine 172.31.21.141
  Configuration removed from machine 172.31.27.1
  Configuration removed from machine 172.31.20.163
  The cluster is successfully removed.
  ```

## <a name="delete-aws-resources"></a>Löschen der AWS-Ressourcen

1. Melden Sie sich bei Ihrem AWS-Konto an.
2. Navigieren Sie zur EC2-Konsole.
3. Wählen Sie die drei Knoten aus, die Sie im ersten Teil des Tutorials erstellt haben.
4. Wählen Sie **Aktionen** > **Instanzstatus** > **Beenden** aus.

## <a name="delete-azure-resources"></a>Löschen von Azure-Ressourcen

1. Melden Sie sich beim Azure-Portal an.
2. Wechseln Sie zum Abschnitt **Virtuelle Computer**.
3. Aktivieren Sie die Kontrollkästchen der drei Knoten, die Sie im ersten Teil des Tutorials erstellt haben.
4. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie die in vorherigen Schritten erstellten Ressourcen löschen.

> [!div class="checklist"]
> * Bereinigen von Ressourcen

> [!div class="nextstepaction"]
> [Zum Anfang](service-fabric-tutorial-standalone-create-infrastructure.md)

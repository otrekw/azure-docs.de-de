---
title: Entfernen eines Knotentyps in Azure Service Fabric | Microsoft-Dokumentation
description: Erfahren Sie, wie ein Knotentyp aus einem in Azure ausgeführten Service Fabric-Cluster entfernt wird.
author: inputoutputcode
manager: sridmad
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: chrpap
ms.openlocfilehash: 6cc7cbcc8344c5015d60d9721c682b6a856cbb6e
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86247233"
---
# <a name="how-to-remove-a-service-fabric-node-type"></a>Entfernen eines Knotentyps in Service Fabric
In diesem Artikel wird beschrieben, wie Sie einen Azure Service Fabric-Cluster skalieren, indem Sie einen vorhandenen Knotentyp aus einem Cluster entfernen. Ein Service Fabric-Cluster enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Ein physischer oder virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern als Gruppe bereitstellen und verwalten können. Jeder Knotentyp, der in einem Azure-Cluster definiert ist, wird [als separate Skalierungsgruppe eingerichtet](service-fabric-cluster-nodetypes.md). Jeder Knotentyp kann dann separat verwaltet werden. Nachdem Sie einen Service Fabric-Cluster erstellt haben, können Sie einen Cluster horizontal skalieren, indem Sie einen Knotentyp (VM-Skalierungsgruppe) und alle seine Knoten entfernen.  Sie können die Skalierung für den Cluster jederzeit durchführen – auch bei Ausführung von Workloads im Cluster.  Wenn der Cluster skaliert wird, werden Ihre Anwendungen ebenfalls automatisch skaliert.

> [!WARNING]
> Es wird nicht empfohlen, diesen Ansatz regelmäßig zu befolgen, um einen Knotentyp aus einem Produktionscluster zu entfernen. Es handelt sich um einen sehr gefährlichen Befehl, da er die VM-Skalierungsgruppenressource hinter dem Knotentyp löscht. 

## <a name="durability-characteristics"></a>Dauerhaftigkeitsmerkmale
Bei der Verwendung von „Remove-AzServiceFabricNodeType“ wird die Sicherheit über die Geschwindigkeit gestellt. Der Knotentyp muss aus den folgenden Gründen die [Dauerhaftigkeitsstufe](./service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster) „Silber“ oder „Gold“ aufweisen:
- „Bronze“ gibt Ihnen keine Garantie für die Speicherung von Zustandsinformationen.
- Die Dauerhaftigkeit von Typ „Silber“ und „Gold“ fängt alle Änderungen an der Skalierungsgruppe ab.
- Mit „Gold“ erhalten Sie außerdem Kontrolle über die Azure-Updates, die im Hintergrund der Skalierungsgruppe erfolgen.

Service Fabric „orchestriert“ die zugrunde liegenden Änderungen und Updates, damit keine Daten verloren gehen. Wenn Sie einen Knotentyp mit der Dauerhaftigkeit „Bronze“ entfernen, können jedoch Zustandsinformationen verloren gehen. Wenn Sie einen primären Knotentyp entfernen und Ihre Anwendung zustandslos ist, ist „Bronze“ akzeptabel. Wenn Sie zustandsbehaftete Workloads in der Produktion ausführen, sollte die Mindestkonfiguration „Silber“ sein. Analog dazu sollte für Produktionsszenarien der primären Knotentyp immer „Silber“ oder „Gold“ sein.

### <a name="more-about-bronze-durability"></a>Weitere Informationen zur Dauerhaftigkeitsstufe „Bronze“

Wenn Sie einen Knotentyp „Bronze“ entfernen, fallen alle Knoten im Knotentyp sofort aus. Service Fabric fängt keine Knotenupdates von VM-Skalierungsgruppen vom Typ „Bronze“ ab, daher fallen alle virtuellen Computer sofort aus. Wenn zustandsbehaftete Elemente auf diesen Knoten vorhanden waren, gehen die Daten verloren. Selbst als zustandslose Elemente nehmen alle Knoten in Service Fabric am Ring teil, sodass eine ganze Umgebung verloren gehen kann, was den Cluster selbst destabilisieren könnte.

## <a name="remove-a-node-type"></a>Entfernen eines Knotentyps

1. Berücksichtigen Sie diese Voraussetzungen, bevor Sie den Prozess starten.

    - Der Cluster ist fehlerfrei.
    - Nach dem Entfernen des Knotentyps ist weiterhin ausreichend Kapazität vorhanden, z. B. für die Anzahl der Knoten zum Platzieren der erforderlichen Anzahl von Replikaten.

2. Verschieben Sie alle Dienste, die bei Verwenden des Knotentyps Platzierungseinschränkungen aufweisen, aus dem Knotentyp heraus.

    - Ändern Sie das Anwendungs-/Dienstmanifest so, dass es nicht mehr auf den Knotentyp verweist.
    - Stellen Sie die Änderung bereit.

    Überprüfen Sie dann, ob Folgendes zutrifft:
    - Alle oben geänderten Dienste werden nicht mehr auf dem Knoten ausgeführt, der zum Knotentyp gehört.
    - Alle Dienste sind fehlerfrei.

3. Heben Sie die Markierung des Knotentyps als nicht primär auf (für nicht primäre Knotentypen überspringen).

    - Bestimmen Sie die für die Bereitstellung verwendete Azure Resource Manager-Vorlage.
    - Ermitteln Sie den Abschnitt, der sich auf den Knotentyp im Abschnitt „Service Fabric“ bezieht.
    - Ändern Sie die IsPrimary-Eigenschaft in FALSE. ** Entfernen Sie in dieser Aufgabe nicht den Abschnitt, der sich auf den Knotentyp bezieht.
    - Stellen Sie die geänderte Azure Resource Manager-Vorlage bereit. ** Je nach Clusterkonfiguration kann dieser Schritt einige Zeit in Anspruch nehmen.
    
    Überprüfen Sie dann, ob Folgendes zutrifft:
    - Der Abschnitt „Service Fabric“ im Portal besagt, dass der Cluster bereit ist.
    - Der Cluster ist fehlerfrei.
    - Keine der Knoten, die zum Knotentyp gehören, sind als Seedknoten markiert.

4. Deaktivieren Sie Daten für den Knotentyp.

    Stellen Sie mithilfe von PowerShell eine Verbindung mit dem Cluster her, und führen Sie den folgenden Schritt aus.
    
    ```powershell
    $nodeType = "" # specify the name of node type
    $nodes = Get-ServiceFabricNode
    
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Disable-ServiceFabricNode -Intent RemoveNode -NodeName $node.NodeName -Force
      }
    }
    ```

    - Warten Sie für die Dauerhaftigkeit „Bronze“, bis alle Knoten im Zustand „Deaktiviert“ sind.
    - Für die Dauerhaftigkeit „Silber“ und „Gold“ wechseln einige Knoten in den Zustand „Deaktiviert“. Der Rest wechselt in den Zustand „Wird deaktiviert“. Überprüfen Sie die Registerkarte „Details“ der Knoten im deaktivierten Zustand. Wenn alle Knoten bei der Sicherstellung des Quorums für die Infrastrukturdienstpartitionen hängen bleiben, können Sie sicher fortfahren.

5. Beenden Sie Daten für den Knotentyp.

    Stellen Sie mithilfe von PowerShell eine Verbindung mit dem Cluster her, und führen Sie den folgenden Schritt aus.
    
    ```powershell
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Start-ServiceFabricNodeTransition -Stop -OperationId (New-Guid) -NodeInstanceId $node.NodeInstanceId -NodeName $node.NodeName -StopDurationInSeconds 10000
      }
    }
    ```
    
    Warten Sie, bis alle Knoten für den Knotentyp als „Ausgefallen“ markiert sind.
    
6. Entfernen Sie Daten für den Knotentyp.

    Stellen Sie mithilfe von PowerShell eine Verbindung mit dem Cluster her, und führen Sie den folgenden Schritt aus.
    
    ```powershell
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Remove-ServiceFabricNodeState -NodeName $node.NodeName -Force
      }
    }
    ```

    Warten Sie, bis alle Knoten aus dem Cluster entfernt wurden. Die Knoten sollten nicht in SFX angezeigt werden.

7. Entfernen Sie den Knotentyp aus dem Abschnitt „Service Fabric“.

    - Bestimmen Sie die für die Bereitstellung verwendete Azure Resource Manager-Vorlage.
    - Ermitteln Sie den Abschnitt, der sich auf den Knotentyp im Abschnitt „Service Fabric“ bezieht.
    - Entfernen Sie den Abschnitt, der dem Knotentyp entspricht.
    - Nur für Cluster mit der Dauerhaftigkeit „Silber“ und höher aktualisieren Sie die Clusterressource in der Vorlage und konfigurieren die Integritätsrichtlinien so, dass der Zustand von fabric:/Systemanwendungen ignoriert wird, indem Sie `applicationDeltaHealthPolicies` unter Clusterressource `properties` wie unten angegeben hinzufügen. Die folgende Richtlinie sollte bestehende Fehler ignorieren, aber keine neuen Integritätsfehler zulassen. 
 
 
     ```json
    "upgradeDescription":  
    { 
      "forceRestart": false, 
      "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807", 
      "healthCheckWaitDuration": "00:05:00", 
      "healthCheckStableDuration": "00:05:00", 
      "healthCheckRetryTimeout": "00:45:00", 
      "upgradeTimeout": "12:00:00", 
      "upgradeDomainTimeout": "02:00:00", 
      "healthPolicy": { 
        "maxPercentUnhealthyNodes": 100, 
        "maxPercentUnhealthyApplications": 100 
      }, 
      "deltaHealthPolicy":  
      { 
        "maxPercentDeltaUnhealthyNodes": 0, 
        "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0, 
        "maxPercentDeltaUnhealthyApplications": 0, 
        "applicationDeltaHealthPolicies":  
        { 
            "fabric:/System":  
            { 
                "defaultServiceTypeDeltaHealthPolicy":  
                { 
                        "maxPercentDeltaUnhealthyServices": 0 
                } 
            } 
        } 
      } 
    },
    ```

    - Stellen Sie die geänderte Azure Resource Manager-Vorlage bereit. ** Dieser Schritt dauert eine Weile, in der Regel bis zu zwei Stunden. Durch dieses Upgrade ändern sich die Einstellungen für InfrastructureService. Daher ist ein Neustart des Knotens erforderlich. In diesem Fall wird `forceRestart` ignoriert. 
    Der Parameter `upgradeReplicaSetCheckTimeout` gibt die maximale Zeit an, die Service Fabric wartet, bis sich eine Partition in einem sicheren Zustand befindet, sofern dies noch nicht der Fall ist. Sobald die Sicherheitsprüfungen für alle Partitionen eines Knotens absolviert sind, fährt Service Fabric mit dem Upgrade auf diesem Knoten fort.
    Der Wert für den Parameter `upgradeTimeout` kann auf 6 Stunden verkürzt werden, aber für maximale Sicherheit sollten 12 Stunden gewählt werden.

    Überprüfen Sie dann, ob Folgendes zutrifft:
    - Service Fabric-Ressource wird im Portal als „bereit“ angezeigt.

8. Entfernen Sie alle Verweise auf die Ressourcen im Zusammenhang mit dem Knotentyp.

    - Bestimmen Sie die für die Bereitstellung verwendete Azure Resource Manager-Vorlage.
    - Entfernen Sie die VM-Skalierungsgruppe und andere Ressourcen im Zusammenhang mit dem Knotentyp aus der Vorlage.
    - Stellen Sie die Änderungen bereit.

    Führen Sie dann folgende Schritte aus:
    - Warten Sie, bis die Bereitstellung abgeschlossen ist.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die [Dauerhaftigkeitsmerkmale](./service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster) eines Clusters.
- Erfahren Sie mehr über [Knotentypen und VM-Skalierungsgruppen](service-fabric-cluster-nodetypes.md).
- Erfahren Sie mehr über [Service Fabric-Clusterskalierung](service-fabric-cluster-scaling.md).

---
title: 'Konzepte: Speicher'
description: Erfahren Sie mehr über die wichtigsten Speicherfunktionen in privaten Clouds in der Vorschau von Azure VMware Solution (AVS).
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 7e58a829a40d590b7936a58ccdc866211a4f5cb4
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739782"
---
# <a name="azure-vmware-solution-avs-preview-storage-concepts"></a>Speicherkonzepte von Azure VMware Solution (AVS) Vorschau

Private AVS-Clouds bieten nativen, clusterweiten Speicher mit VMware vSAN. Der gesamte lokale Speicher jedes Hosts in einem Cluster wird in einem vSAN-Datenspeicher verwendet, und Verschlüsselung ruhender Daten ist verfügbar und standardmäßig aktiviert. Sie können Azure Storage-Ressourcen zum Erweitern der Speicherfunktionen Ihrer privaten Clouds verwenden.

## <a name="vsan-clusters"></a>vSAN-Cluster

Der lokale Speicher in jedem Clusterhost wird als Teil eines vSAN-Datenspeichers verwendet. Alle Datenträgergruppen verwenden eine NVMe-Cacheebene von 1,6 TB mit SSD-basierter Rohkapazität von 15,4 TB pro Host. Die Größe der Rohkapazitätsebene eines Clusters entspricht der Kapazität pro Host multipliziert mit der Anzahl der Hosts. Beispielsweise wird in einem Cluster mit vier Hosts eine Rohkapazität von 61,6 TB in der vSAN-Kapazitätsebene bereitgestellt.

Der lokale Speicher in Clusterhosts wird im clusterweiten vSAN-Datenspeicher verwendet. Alle Datenspeicher werden im Rahmen einer privaten Cloudbereitstellung erstellt und können sofort verwendet werden. Der Cloudadministratorbenutzer und alle Benutzer in der CloudAdmin-Gruppe können Datenspeicher mit diesen vSAN-Berechtigungen verwalten:
- Datastore.AllocateSpace
- Datastore.Browse
- Datastore.Config
- Datastore.DeleteFile
- Datastore.FileManagement
- Datastore.UpdateVirtualMachineMetadata

## <a name="data-at-rest-encryption"></a>Verschlüsselung ruhender Daten

vSAN-Datenspeicher verwenden standardmäßig Verschlüsselung ruhender Daten. Die Verschlüsselungslösung ist KMS-basiert und unterstützt vCenter-Vorgänge für die Schlüsselverwaltung. Schlüssel werden verschlüsselt gespeichert und von einem HSM-basierten Azure Key Vault-Hauptschlüssel umschlossen. Wenn ein Host aus irgendeinem Grund aus einem Cluster entfernt wird, werden die Daten auf SSDs sofort ungültig.

## <a name="scaling"></a>Skalierung

Die native Clusterspeicherkapazität wird durch Hinzufügen von Hosts zu einem Cluster skaliert. Für Cluster, die HE-Hosts verwenden, wird die clusterweite Rohkapazität um 15,4 TB mit jedem zusätzlichen Host erhöht. Cluster, die GP-Hosts erstellt werden, wird die Rohkapazität um 7,7 TB mit jedem zusätzlichen Host erhöht. Bei beiden Clustertypen dauert es ungefähr 10 Minuten, bis Hosts einem Cluster hinzugefügt werden. Anweisungen zum Skalieren von Clustern finden Sie im [Tutorial zum Skalieren von privaten Clouds] [tutorial-scale-private-cloud].

## <a name="azure-storage-integration"></a>Azure-Speicherintegration

Sie können Azure-Speicherdienste für Workloads verwenden, die in Ihrer privaten Cloud ausgeführt werden. Die Azure-Speicherdienste umfassen Speicherkonten, Table Storage und Blob Storage. Die Verbindung von Workloads mit Azure-Speicherdiensten durchläuft nicht das Internet. Diese Konnektivität bietet zusätzliche Sicherheit und ermöglicht Ihnen die Verwendung von SLA-basierten Azure Storage-Diensten in den Workloads Ihrer privaten Clouds.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Schritt erfahren Sie mehr über [Identitätskonzepte privater Clouds][concepts-identity].

<!-- LINKS - external-->

<!-- LINKS - internal -->
[tutorials-scale-private-cloud]: ./tutorials-scale-private-cloud.md
[concepts-identity]: ./concepts-identity.md
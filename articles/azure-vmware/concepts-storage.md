---
title: 'Konzepte: Speicher'
description: Informieren Sie sich über die wichtigsten Speicherfunktionen bei privaten Azure VMware Solution-Clouds.
ms.topic: conceptual
ms.date: 03/13/2021
ms.openlocfilehash: 83a4a52f8414e656b5bc688796db6e93a53d4d76
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104801620"
---
#  <a name="azure-vmware-solution-storage-concepts"></a>Speicherkonzepte von Azure VMware Solution

Private Azure VMware Solution-Clouds bieten nativen, clusterweiten Speicher mit VMware vSAN. Der gesamte lokale Speicher jedes Hosts in einem Cluster wird in einem vSAN-Datenspeicher verwendet, und Verschlüsselung ruhender Daten ist verfügbar und standardmäßig aktiviert. Sie können Azure Storage-Ressourcen zum Erweitern der Speicherfunktionen Ihrer privaten Clouds verwenden.

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

vSAN-Datenspeicher verwenden standardmäßig Verschlüsselung ruhender Daten. Die Verschlüsselungslösung ist KMS-basiert und unterstützt vCenter-Vorgänge für die Schlüsselverwaltung. Schlüssel werden verschlüsselt gespeichert und von einem Azure Key Vault-Hauptschlüssel umschlossen. Wenn ein Host aus einem Cluster entfernt wird, werden die Daten auf SSDs sofort ungültig.

## <a name="scaling"></a>Skalierung

Die native Clusterspeicherkapazität wird durch Hinzufügen von Hosts zu einem Cluster skaliert. Bei Clustern, die AVS36-Hosts verwenden, wird die clusterweite Rohkapazität durch jeden hinzugefügten Host um 15,4 TB erhöht. Es dauert ungefähr 10 Minuten, bis Hosts einem Cluster hinzugefügt werden.  Eine Anleitung zum Skalieren von Clustern finden Sie im [Tutorial zum Skalieren privater Clouds][tutorial-scale-private-cloud].

## <a name="azure-storage-integration"></a>Azure-Speicherintegration

Sie können Azure-Speicherdienste in Workloads verwenden, die in Ihrer privaten Cloud ausgeführt werden. Die Azure-Speicherdienste umfassen Speicherkonten, Table Storage und Blob Storage. Die Verbindung von Workloads mit Azure-Speicherdiensten durchläuft nicht das Internet. Diese Konnektivität bietet mehr Sicherheit und ermöglicht Ihnen die Verwendung von SLA-basierten Azure Storage-Diensten in den Workloads Ihrer privaten Clouds.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit den Speicherkonzepten von Azure VMware Solution vertraut gemacht haben, informieren Sie sich über die folgenden Themen:

- [Identitätskonzepte privater Clouds](concepts-identity.md)
- [Rollenbasierte Zugriffssteuerung in vSphere für Azure VMware Solution](concepts-identity.md)
- [Aktivieren einer Azure VMware Solution-Ressource](enable-azure-vmware-solution.md)
- [Azure NetApp Files mit Azure VMware Solution](netapp-files-with-azure-vmware-solution.md)

<!-- LINKS - external-->

<!-- LINKS - internal -->
[tutorial-scale-private-cloud]: ./tutorial-scale-private-cloud.md
[concepts-identity]: ./concepts-identity.md

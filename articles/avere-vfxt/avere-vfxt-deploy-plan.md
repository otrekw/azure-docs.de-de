---
title: Planen des Avere vFXT-Systems – Azure
description: Erläutert die Planung vor der Bereitstellung von Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: rohogue
ms.openlocfilehash: cd0c74c8aa40b3e96716ef37aa27b08b5f6aece1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76547539"
---
# <a name="plan-your-avere-vfxt-system"></a>Planen des Avere vFXT-Systems

In diesem Artikel wird erläutert, wie Sie einen neuen Avere vFXT für Azure-Cluster planen, der entsprechend Ihren Anforderungen angemessen positioniert und dimensioniert ist.

Bevor Sie zum Azure Marketplace wechseln oder VMs erstellen, beachten Sie diese Details:

* Wie interagiert der Cluster mit anderen Azure-Ressourcen?
* Wo sollten sich Clusterelemente in privaten Netzwerken und Subnetzen befinden?
* Welche Art von Back-End-Speicher wird verwendet, und wie greift der Cluster darauf zu?
* Wie leistungsfähig müssen Ihre Clusterknoten sein, um Ihren Workflow zu unterstützen?

Weitere Informationen finden Sie im Folgenden.

## <a name="learn-the-components-of-the-system"></a>Kennenlernen der Komponenten des Systems

Wenn Sie mit der Planung beginnen, kann es hilfreich sein, die Komponenten des Avere vFXT for Azure-Systems zu verstehen.

* Clusterknoten: Der Cluster besteht aus mindestens drei virtuellen Computern, die als Clusterknoten konfiguriert sind. Mehr Knoten verschaffen dem System einen höheren Durchsatz und einen größeren Cache.

* Cache: Die Cachekapazität wird gleichmäßig zwischen den Clusterknoten aufgeteilt. Legen Sie die Cachegröße pro Knoten fest, wenn Sie den Cluster erstellen. Die Knotengrößen werden addiert und ergeben so die Gesamtgröße des Caches.

* Clustercontroller: Der Clustercontroller ist eine zusätzliche VM, die sich innerhalb desselben Subnetzes wie die Clusterknoten befindet. Der Controller wird benötigt, um den Cluster zu erstellen, sowie für laufende Verwaltungsaufgaben.

* Back-End-Speicher: Die Daten, die zwischengespeichert werden sollen, werden langfristig in einem Hardwarespeichersystem oder einem Azure-BLOB-Container gespeichert. Sie können Speicher hinzufügen, nachdem Sie den Avere vFXT for Azure-Cluster erstellt haben, oder wenn Sie BLOB-Speicher verwenden, können Sie den Container beim Erstellen des Clusters hinzufügen und konfigurieren.

* Clients: Clientcomputer, die die zwischengespeicherten Dateien verwenden, stellen eine Verbindung mit dem Cluster mithilfe eines virtuellen Dateipfads her, anstatt direkt auf die Speichersysteme zuzugreifen. (Weitere Informationen finden Sie unter [Einbinden des Avere vFXT-Clusters](avere-vfxt-mount-clients.md).)

## <a name="subscription-resource-group-and-network-infrastructure"></a>Abonnement, Ressourcengruppe und Netzwerkinfrastruktur

Überlegen Sie, wo sich die Elemente Ihrer Avere vFXT für Azure-Bereitstellung befinden werden. Das folgende Diagramm zeigt eine mögliche Anordnung der Avere vFXT für Azure-Komponenten:

![Diagramm, das den Clustercontroller und die virtuellen Clustercomputer innerhalb eines Subnetzes darstellt. Die Subnetzgrenze ist von einer VNet-Begrenzung umgeben. Innerhalb des VNets befindet sich ein Sechseck, das den Endpunkt des Speicherdiensts darstellt. Es ist mit einem gestrichelten Pfeil mit einem Blob-Speicher außerhalb des VNets verbunden.](media/avere-vfxt-components-option.png)

Gehen Sie bei der Planung der Netzwerkinfrastruktur Ihres Avere vFXT-Clusters nach den folgenden Richtlinien vor:

* Erstellen Sie ein neues Abonnement für jede Avere vFXT for Azure-Bereitstellung. Verwalten Sie alle Komponenten in diesem Abonnement.

  Zu den Vorteilen der Verwendung eines neuen Abonnements für jede Bereitstellung gehören:
  * Einfachere Nachverfolgung der Kosten: Zeigen Sie alle Kosten aus Ressourcen, Infrastruktur und Computezyklen in einem Abonnement an, und überprüfen Sie sie.
  * Einfachere Bereinigung: Sie können das gesamte Abonnement entfernen, wenn Sie das Projekt abgeschlossen haben.
  * Bequeme Partitionierung von Ressourcenkontingenten: Isolieren Sie die Avere vFXT-Clients und -Cluster in einem einzigen Abonnement, um andere kritische Workloads vor möglicher Ressourcendrosselung zu schützen. Durch diese Trennung wird ein Konflikt vermieden, wenn Sie eine große Anzahl von Clients für einen Hochleistungs-Computingworkflow nutzen.

* Platzieren Sie Ihre Clientcomputersysteme in der Nähe des vFXT-Clusters. Der Back-End-Speicher kann weiter entfernt sein.  

* Siedeln Sie die vFXT-Cluster-VM und die Clustercontroller-VM gemeinsam an, insbesondere sollten sie sich:

  * im selben virtuellen Netzwerk,
  * in derselben Ressourcengruppe
  * und im selben Speicherkonto befinden.
  
  Die Clustererstellungsvorlage übernimmt diese Konfiguration für die meisten Situationen.

* Der Cluster muss sich in einem eigenen Subnetz befinden, um IP-Adressenkonflikte mit Clients oder anderen Computeressourcen zu vermeiden.

* Verwenden Sie die Vorlage für die Clustererstellung, um den Großteil der erforderlichen Infrastruktur für den Cluster zu erstellen, einschließlich Ressourcengruppen, virtueller Netzwerke, Subnetze und Speicherkonten.

  Wenn Sie Ressourcen verwenden möchten, die bereits vorhanden sind, stellen Sie sicher, dass sie die Anforderungen in dieser Tabelle erfüllen.

  | Resource | Vorhandene verwenden? | Requirements (Anforderungen) |
  |----------|-----------|----------|
  | Resource group | Ja, wenn leer. | Muss leer sein.|
  | Speicherkonto | **Ja**, wenn ein Blob-Container nach der Erstellung des Clusters verbunden wird. <br/>  **Nein**, wenn ein Blob-Container während der Erstellung des Clusters erstellt. | Vorhandener Blob-Container muss leer sein. <br/> &nbsp; |
  | Virtuelles Netzwerk | Ja | Muss einen Speicherdienst-Endpunkt umfassen, wenn ein neuer Azure Blob-Container erstellt wird. |
  | Subnet | Ja | Kann keine anderen Ressourcen enthalten |

## <a name="ip-address-requirements"></a>Anforderungen an die IP-Adresse

Stellen Sie sicher, dass das Subnetz Ihres Clusters einen ausreichend großen IP-Adressbereich aufweist, um den Cluster zu unterstützen.

Der Avere vFXT-Cluster verwendet die folgenden IP-Adressen:

* Eine IP-Adresse zur Verwaltung des Clusters. Diese Adresse kann nach Bedarf im Cluster von Knoten zu Knoten verschoben werden, damit sie immer verfügbar ist. Verwenden Sie diese Adresse, um eine Verbindung mit dem Konfigurationstool für die Avere-Systemsteuerung herzustellen.
* Für jeden Clusterknoten:
  * Mindestens eine clientseitige IP-Adresse. (Alle clientseitigen Adressen werden vom *vserver* des Clusters verwaltet, der die IP-Adressen bei Bedarf zwischen den Knoten verschieben kann.)
  * Eine IP-Adresse für die Clusterkommunikation
  * Eine Instanz-IP-Adresse (dem virtuellen Computer zugeordnet)

Wenn Sie Azure-BLOB-Speicher verwenden, erfordert dieser möglicherweise auch IP-Adressen aus dem virtuellen Netzwerk Ihres Clusters:  

* Ein Azure-BLOB-Speicherkonto erfordert mindestens fünf IP-Adressen. Beachten Sie diese Anforderung, wenn Sie Blob-Speicher im selben virtuellen Netzwerk wie Ihren Cluster positionieren.
* Wenn Sie Azure-BLOB-Speicher verwenden, der sich außerhalb des virtuellen Netzwerks des Clusters befindet, erstellen Sie einen Speicherdienstendpunkt innerhalb des virtuellen Netzwerks. Der Endpunkt verwendet keine IP-Adresse.

Sie haben die Möglichkeit, Netzwerkressourcen und Blob-Speicher (falls verwendet) in verschiedenen Ressourcengruppen des Clusters zu positionieren.

## <a name="vfxt-node-size"></a>vFXT-Knotengröße

Die virtuellen Computer, die als Clusterknoten dienen, bestimmen den Anforderungsdurchsatz und die Speicherkapazität Ihres Cache. <!-- The instance type offered has been chosen for its memory, processor, and local storage characteristics. You can choose from two instance types, with different memory, processor, and local storage characteristics. -->

Die einzelnen vFXT-Knoten sind identisch. Das bedeutet, dass Sie beim Erstellen eines Clusters mit drei Knoten über drei virtuelle Computer des gleichen Typs und mit identischer Größe verfügen.

| Instanztyp | vCPUs | Arbeitsspeicher  | Lokaler SSD-Speicher  | Max. Anzahl Datenträger | Durchsatz des Datenträgers ohne Cache | NIC (Anzahl) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GB  | 32 | 51.200 IOPS <br/> 768 MBit/s | 16.000 MBit/s (8)  |

Der Datenträgercache pro Knoten ist konfigurierbar und kann zwischen 1000 GB und 8000 GB liegen. 4 TB pro Knoten ist die empfohlene Cachegröße für Standard_E32s_v3-Knoten.

Weitere Informationen zu diesen virtuellen Computern finden Sie in der Microsoft Azure-Dokumentation: [Arbeitsspeicheroptimierte Größen virtueller Computer](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Kontokontingent

Stellen Sie sicher, dass Ihr Abonnement über die Kapazität verfügt, den Avere vFXT-Cluster sowie alle verwendeten Computer- oder Clientsysteme auszuführen. Weitere Informationen finden Sie unter [Kontingent für den vFXT-Cluster](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster).

## <a name="back-end-data-storage"></a>Back-End-Datenspeicher

Back-End-Speichersysteme stellen sowohl Dateien im Cache des Clusters bereit, erhalten aber auch geänderte Daten aus dem Cache. Entscheiden Sie, ob Ihr Arbeitssatz langfristig in einem neuen Blobcontainer oder in einem vorhandenen Speichersystem (Cloud oder Hardware) gespeichert wird. Diese Back-End-Speichersysteme werden als *Kernspeichereinheiten* bezeichnet.

### <a name="hardware-core-filers"></a>Hardware-Kernspeichereinheiten

Fügen Sie dem vFXT-Cluster Hardwarespeichersysteme hinzu, nachdem Sie den Cluster erstellt haben. Sie können viele verbreitete Hardwaresysteme verwenden, einschließlich lokaler Systeme, sofern das Speichersystem vom Subnetz des Clusters aus erreichbar ist.

Ausführliche Anweisungen zum Hinzufügen eines vorhandenen Speichersystems zum Avere vFXT-Cluster finden Sie unter [Konfigurieren von Speicher](avere-vfxt-add-storage.md).

### <a name="cloud-core-filers"></a>Cloud-Kernspeichereinheiten

Das Avere vFXT for Azure-System kann leere BLOB-Container als Back-End-Speicher verwenden. Container müssen leer sein, wenn sie dem Cluster hinzugefügt werden. Das vFXT-System muss in der Lage sein, seinen Objektspeicher zu verwalten, ohne dass bestehende Daten erhalten bleiben müssen.

> [!TIP]
> Wenn Sie den Azure-BLOB-Speicher für das Back-End verwenden möchten, erstellen Sie im Rahmen der Erstellung des vFXT-Clusters einen neuen Container. Die Vorlage für die Clustererstellung kann einen neuen BLOB-Container erstellen und konfigurieren, sodass er sofort nach der Verfügbarkeit des Clusters einsatzbereit ist. Das spätere Hinzufügen eines Containers ist komplizierter.
>
> Details finden Sie unter [Erstellen des Avere vFXT-Systems für Azure](avere-vfxt-deploy.md#create-the-avere-vfxt-for-azure).

Nachdem Sie den leeren BLOB-Speichercontainer als Kernspeichereinheit hinzugefügt haben, können Sie Daten über den Cluster in den Container kopieren. Verwenden Sie einen parallelen Multithread-Kopiermechanismus. Lesen Sie [Verschieben von Daten in den vFXT-Cluster](avere-vfxt-data-ingest.md), um zu erfahren, wie Sie Daten effizient in den neuen Container des Clusters kopieren können, indem Sie Clientcomputer und den Avere vFXT-Cache verwenden.

## <a name="cluster-access"></a>Clusterzugriff

Der Avere vFXT-Cluster für Azure befindet sich in einem privaten Subnetz und besitzt keine öffentliche IP-Adresse. Sie müssen über eine Möglichkeit verfügen, um auf das private Subnetz für die Clusterverwaltung und Clientverbindungen zuzugreifen.

Folgende Zugriffsoptionen sind verfügbar:

* Jump Host: Weisen Sie einer separaten VM im privaten Netzwerk eine öffentliche IP-Adresse zu, und erstellen Sie damit einen SSL-Tunnel zu den Clusterknoten.

  > [!TIP]
  > Wenn Sie eine öffentliche IP-Adresse für den Clustercontroller festlegen, können Sie ihn als Jump Host verwenden. Weitere Informationen finden Sie unter [Clustercontroller als Jump Host](#cluster-controller-as-jump-host).

* Virtuelles privates Netzwerk (VPN): Konfigurieren Sie ein Point-to-Site- oder Site-to-Site-VPN zwischen Ihrem privaten Netzwerk in Azure und Unternehmensnetzwerken.

* Azure ExpressRoute: Konfigurieren Sie eine private Verbindung über einen ExpressRoute-Partner.

Ausführliche Informationen zu diesen Optionen finden Sie in der [Dokumentation zu Azure Virtual Network unter „Internetkommunikation“](../virtual-network/virtual-networks-overview.md#communicate-with-the-internet).

### <a name="cluster-controller-as-jump-host"></a>Clustercontroller als Jump Host

Wenn Sie eine öffentliche IP-Adresse für den Clustercontroller festlegen, können Sie diesen als Jump Host verwenden, um Kontakt mit dem Avere vFXT-Cluster von außerhalb des privaten Subnetzes herzustellen. Da der Controller jedoch Zugriffsrechte zum Ändern von Clusterknoten besitzt, entsteht ein kleines Sicherheitsrisiko.

Zur Erhöhung der Sicherheit für einen Controller mit einer öffentlichen IP-Adresse erstellt das Bereitstellungsskript automatisch eine Netzwerksicherheitsgruppe, die den eingehenden Zugriff ausschließlich auf Port 22 beschränkt. Sie können das System weiter schützen, indem Sie den Zugriff auf Ihren Bereich von IP-Quelladressen sperren, d.h. nur Verbindungen von Computern erlauben, die Sie für den Clusterzugriff verwenden möchten.

Beim Erstellen des Clusters können Sie auswählen, ob Sie eine öffentliche IP-Adresse für den Clustercontroller erstellen möchten.

* Wenn Sie ein **neues virtuelles Netzwerk** oder ein **neues Subnetz** erstellen, wird dem Clustercontroller eine **öffentliche** IP-Adresse zugewiesen.
* Wenn Sie ein vorhandenes virtuelles Netzwerk und Subnetz auswählen, verfügt der Clustercontroller nur über **private** IP-Adressen.

## <a name="vm-access-roles"></a>Rollen für den Zugriff auf virtuelle Computer

Azure verwendet die [rollenbasierte Zugriffssteuerung](../role-based-access-control/index.yml) (RBAC), um die virtuellen Clustercomputer für die Ausführung bestimmter Aufgaben zu autorisieren. Für den Clustercontroller ist beispielsweise die Autorisierung zum Erstellen und Konfigurieren der Clusterknoten-VMs erforderlich. Die Clusterknoten müssen anderen Clusterknoten IP-Adressen zuweisen oder neu zuweisen können.

Für die virtuellen Avere vFXT-Computer werden zwei integrierte Azure-Rollen verwendet:

* Der Clustercontroller verwendet die integrierte Rolle [Avere-Mitwirkender](../role-based-access-control/built-in-roles.md#avere-contributor).
* Clusterknoten verwenden die integrierte Rolle [Avere-Bediener](../role-based-access-control/built-in-roles.md#avere-operator).

Wenn Sie die Zugriffsrollen für Avere vFXT-Komponenten anpassen möchten, müssen Sie eine eigene Rolle definieren und diese dann den virtuellen Computern bei der Erstellung zuweisen. Sie können nicht die Bereitstellungsvorlage im Azure Marketplace verwenden. Sie können sich an den Microsoft-Kundendienst und -Support wenden, indem Sie wie unter [Abrufen von Hilfe zu Ihrem System](avere-vfxt-open-ticket.md) beschrieben ein Supportticket im Azure-Portal erstellen.

## <a name="next-steps"></a>Nächste Schritte

Die [Übersicht über die Bereitstellung](avere-vfxt-deploy-overview.md) bietet eine umfassende Übersicht über alle erforderlichen Schritte, um ein Avere vFXT for Azure-System zu erstellen und es für die Bereitstellung von Daten vorzubereiten.

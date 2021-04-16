---
title: Bewerten einer großen Anzahl von Servern in einer VMware-Umgebung für die Migration zu Azure mithilfe von Azure Migrate
description: Hier erfahren Sie, wie Sie eine große Anzahl von Servern in einer VMware-Umgebung mithilfe des Azure Migrate-Diensts für die Migration zu Azure bewerten.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 03/23/2020
ms.openlocfilehash: 10b8aaeaa25e49140dbf6f31c064c7f823d23e31
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104778252"
---
# <a name="assess-large-numbers-of-servers-in-vmware-environment-for-migration-to-azure"></a>Bewerten einer großen Anzahl von Servern in einer VMware-Umgebung für die Migration zu Azure


In diesem Artikel wird beschrieben, wie eine große Anzahl von lokalen Servern (1.000-35.000) in einer VMware-Umgebung mithilfe des Ermittlungs- und Bewertungstools von Azure Migrate für die Migration zu Azure bewertet wird.

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs). 

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
> * Planen Sie die Bewertung in gewünschtem Umfang.
> * Konfigurieren Sie Azure-Berechtigungen, und bereiten Sie VMware für die Bewertung vor.
> * Erstellen Sie ein Azure Migrate-Projekt, und erstellen Sie eine Bewertung.
> * Überprüfen Sie die Bewertung beim Planen der Migration.


> [!NOTE]
> Wenn Sie einen Proof of Concept ausprobieren möchten, um ein paar Server zu bewerten, bevor Sie eine Bewertung im gewünschtem Umfang durchführen, lesen Sie unsere [Tutorialreihe](./tutorial-discover-vmware.md).

## <a name="plan-for-assessment"></a>Planen für die Bewertung

Bei der Planung der Bewertung einer großen Anzahl von Servern in einer VMware-Umgebung sind einige Dinge zu berücksichtigen:

- **Planen von Azure Migrate-Projekten**: Finden Sie heraus, wie Azure Migrate-Projekte bereitgestellt werden. Wenn sich Ihre Rechenzentren beispielsweise in verschiedenen geografischen Regionen befinden oder Sie ermittlungs-, bewertungs- oder migrationsspezifische Metadaten in einer anderen geografischen Region speichern müssen, benötigen Sie möglicherweise mehrere Projekte. 
- **Planen von Appliances**: Azure Migrate verwendet für die kontinuierliche Erkennung von Servern eine lokale Azure Migrate-Appliance, die als VMware-VM bereitgestellt wird. Die Appliance überwacht Umgebungsänderungen wie z. B. das Hinzufügen von Servern, Datenträgern oder Netzwerkadaptern. Sie sendet außerdem deren Meta- und Leistungsdaten an Azure. Sie müssen herausfinden, wie viele Appliances Sie bereitstellen sollten.
- **Planen von Konten für die Ermittlung:** Die Azure Migrate-Appliance verwendet ein Konto mit Zugriff auf vCenter Server, um Server für die Bewertung und Migration zu ermitteln. Wenn Sie mehr als 10.000 Server entdecken, richten Sie mehrere Konten ein, da es keine Überlappungen zwischen Servern gibt, die von zwei beliebigen Appliances in einem Projekt entdeckt wurden. 

> [!NOTE]
> Wenn Sie mehrere Appliances einrichten, stellen Sie sicher, dass es keine Überlappungen zwischen den Servern der bereitgestellten vCenter-Konten gibt. Eine Entdeckung mit einer solchen Überlappung ist ein nicht unterstütztes Szenario. Wenn ein Server von mehr als einer Appliance erkannt wird, führt dies zu Duplikaten bei der Erkennung und zu Problemen, während gleichzeitig die Replikation für den Server über das Azure-Portal bei der Servermigration ermöglicht wird.

## <a name="planning-limits"></a>Planen von Einschränkungen
 
Orientieren Sie sich bei der Planung an den in dieser Tabelle zusammengefassten Einschränkungen.

**Planung** | **Einschränkungen**
--- | --- 
**Azure Migrate-Projekte** | Bewerten Sie bis zu 35.000 Server in einem Projekt.
**Azure Migrate-Appliance** | Eine Appliance kann bis zu 10.000 Server auf einer vCenter Server-Instanz ermitteln.<br/> Eine Appliance kann nur eine Verbindung mit einer einzelnen vCenter Server-Instanz herstellen.<br/> Eine einzelne Appliance kann nur einer einzelnen Azure Migrate-Ressource zugeordnet werden.<br/>  Einer einzelnen Azure Migrate-Ressource können beliebig viele Appliances zugeordnet werden. <br/><br/> 
**Gruppe** | Sie können einer einzelnen Gruppe bis zu 35.000 Server hinzufügen.
**Azure Migrate-Bewertung** | Sie können bis zu 35.000 Server in einem einzelnen Vorgang bewerten.

In Anbetracht dieser Einschränkungen sind hier einige Beispiele für Bereitstellungen aufgeführt:


**vCenter-Server** | **Server auf dem Server** | **Empfehlung** | **Aktion**
---|---|---|---
Eine | < 10.000 | Ein Azure Migrate-Projekt<br/> Eine Appliance<br/> Ein vCenter-Konto für die Ermittlung | Richten Sie die Appliance ein, und stellen Sie eine Verbindung mit vCenter Server mit einem Konto her.
Eine | > 10.000 | Ein Azure Migrate-Projekt<br/> Mehrere Appliances<br/> Mehrere vCenter-Konten | Richten Sie eine Appliance für je 10.000 Server ein.<br/><br/> Richten Sie vCenter-Konten ein, und teilen Sie das Inventar auf, um den Zugriff auf ein Konto auf weniger als 10.000 Server zu beschränken.<br/> Stellen Sie für jede Appliance eine Verbindung mit der vCenter Server-Instanz mit einem Konto her.<br/> Sie können Abhängigkeiten zwischen Servern analysieren, die mit unterschiedlichen Appliances ermittelt wurden. <br/> <br/> Stellen Sie sicher, dass es keine Überlappungen zwischen den Servern auf den bereitgestellten vCenter-Konten gibt. Eine Entdeckung mit einer solchen Überlappung ist ein nicht unterstütztes Szenario. Wenn ein Server von mehr als einer Appliance erkannt wird, führt dies zu Duplikaten bei der Erkennung und zu Problemen, während gleichzeitig die Replikation für den Server über das Azure-Portal bei der Servermigration ermöglicht wird.
Mehrere | < 10.000 |  Ein Azure Migrate-Projekt<br/> Mehrere Appliances<br/> Ein vCenter-Konto für die Ermittlung | Richten Sie Appliances ein, und stellen Sie eine Verbindung mit vCenter Server mit einem Konto her.<br/> Sie können Abhängigkeiten zwischen Servern analysieren, die mit unterschiedlichen Appliances ermittelt wurden.
Mehrere | > 10.000 | Ein Azure Migrate-Projekt<br/> Mehrere Appliances<br/> Mehrere vCenter-Konten | Wenn die vCenter Server-Ermittlung weniger als 10.000 Server umfasst, richten Sie für jede vCenter Server-Instanz eine Appliance ein.<br/><br/> Wenn die vCenter Server-Ermittlung mehr als 10.000 Server umfasst, richten Sie für je 10.000 Server eine Appliance ein.<br/> Richten Sie vCenter-Konten ein, und teilen Sie das Inventar auf, um den Zugriff auf ein Konto auf weniger als 10.000 Server zu beschränken.<br/> Stellen Sie für jede Appliance eine Verbindung mit der vCenter Server-Instanz mit einem Konto her.<br/> Sie können Abhängigkeiten zwischen Servern analysieren, die mit unterschiedlichen Appliances ermittelt wurden. <br/><br/> Stellen Sie sicher, dass es keine Überlappungen zwischen den Servern auf den bereitgestellten vCenter-Konten gibt. Eine Entdeckung mit einer solchen Überlappung ist ein nicht unterstütztes Szenario. Wenn ein Server von mehr als einer Appliance erkannt wird, führt dies zu Duplikaten bei der Erkennung und zu Problemen, während gleichzeitig die Replikation für den Server über das Azure-Portal bei der Servermigration ermöglicht wird.



## <a name="plan-discovery-in-a-multi-tenant-environment"></a>Planen der Ermittlung in einer Umgebung mit mehreren Mandanten

Wenn Sie eine Umgebung mit mehreren Mandanten planen, können Sie die Ermittlung auf die vCenter Server-Instanz beschränken.

- Sie können den Umfang der Appliance-Ermittlung auf vCenter Server-Rechenzentren, Cluster, Clusterordner, Hosts, Hostordner oder einzelne Server festlegen.
- Wenn Ihre Umgebung für mehrere Mandanten gemeinsam genutzt wird und Sie jeden Mandanten separat ermitteln möchten, können Sie den Zugriff auf das vCenter-Konto einschränken, das die Appliance für die Ermittlung verwendet. 
    - Sie können den Umfang nach VM-Ordnern festlegen, wenn die Mandanten Hosts gemeinsam nutzen. Azure Migrate kann keine Server erkennen, wenn dem vCenter-Konto auf Ebene des vCenter-VM-Ordners Zugriff gewährt wird. Wenn Ihre Ermittlung auf VM-Ordnern basieren soll, muss das vCenter-Konto reinen Lesezugriff auf der Serverebene haben. [Weitere Informationen](set-discovery-scope.md)

## <a name="prepare-for-assessment"></a>Vorbereiten für die Bewertung

Vorbereiten von Azure und VMware für das Ermittlungs- und Bewertungstool:

1. Überprüfen Sie die [VMware-Unterstützungsanforderungen und -einschränkungen](migrate-support-matrix-vmware.md).
2. Richten Sie Berechtigungen für Ihr Azure-Konto zur Interaktion mit Azure Migrate ein.
3. Bereiten Sie VMware für die Bewertung vor.

Befolgen Sie die Anweisungen in [diesem Tutorial](./tutorial-discover-vmware.md), um diese Einstellungen zu konfigurieren.


## <a name="create-a-project"></a>Erstellen eines Projekts

Führen Sie in Übereinstimmung mit Ihren Planungsanforderungen folgende Schritte aus:

1. Erstellen Sie Azure Migrate-Projekte.
2. Fügen Sie das Ermittlungs- und Bewertungstool von Azure Migrate den Projekten hinzu.

[Weitere Informationen](./create-manage-projects.md)

## <a name="create-and-review-an-assessment"></a>Erstellen und Überprüfen einer Bewertung

1. Erstellen Sie Bewertungen für Server in einer VMware-Umgebung.
1. Überprüfen Sie die Bewertungen in Vorbereitung auf die Migrationsplanung.


Befolgen Sie die Anweisungen in [diesem Tutorial](./tutorial-assess-vmware-azure-vm.md), um diese Einstellungen zu konfigurieren.
    

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:
 
> [!div class="checklist"] 
> * Planen der Skalierung von Azure Migrate-Bewertungen für Server in einer VMware-Umgebung
> * Vorbereiten von Azure und VMware auf die Bewertung
> * Erstellen eines Azure Migrate-Projekts und Ausführen von Bewertungen
> * Überprüfen der Bewertungen in Vorbereitung auf die Migrationsplanung.

[Erfahren Sie nun](concepts-assessment-calculation.md), wie Bewertungen berechnet und [geändert](how-to-modify-assessment.md) werden.
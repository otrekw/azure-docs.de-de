---
title: Häufig gestellte Fragen zur Azure Migrate-Appliance
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Azure Migrate-Appliance.
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: c8bcebeee58401cb3d2b65ae82e51d31ab4dad0b
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029131"
---
# <a name="azure-migrate-appliance-common-questions"></a>Azure Migrate-Appliance: Häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zur Azure Migrate-Appliance beantwortet. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum). Wenn Sie weitere Fragen haben, lesen Sie die folgenden Artikel:

- [Allgemeine Fragen](resources-faq.md) zu Azure Migrate.
- [Fragen](common-questions-discovery-assessment.md) zur Ermittlung, Bewertung und Abhängigkeitsvisualisierung.


## <a name="what-is-the-azure-migrate-appliance"></a>Was ist die Azure Migrate-Appliance?

Die Azure Migrate-Appliance ist eine einfache Appliance, die von Azure Migrate verwendet wird: Serverbewertungstool zum Ermitteln und Bewerten von lokalen Servern, das von Azure Migrate verwendet wird: Servermigrationstool für die Migration ohne Agent für lokale VMware-VMs. 

Die Appliance wird lokal als VM oder physischer Computer bereitgestellt. Die Appliance ermittelt lokale Computer und sendet kontinuierlich Computermetadaten und Leistungsdaten an Azure Migrate. Die Applianceermittlung erfolgt ohne Agent. Auf ermittelten Computern wird nichts installiert. [Weitere Informationen](migrate-appliance.md) zur Appliance.

## <a name="how-does-the-appliance-connect-to-azure"></a>Wie stellt die Appliance eine Verbindung mit Azure her?

Die Verbindung kann über das Internet erfolgen. Sie können auch Azure ExpressRoute mit öffentlichem/Microsoft-Peering verwenden.

## <a name="does-appliance-analysis-impact-performance"></a>Wirkt sich die Applianceanalyse auf die Leistung aus?

Die Azure Migrate-Appliance erstellt fortlaufend Profile von lokalen Computern zur Messung von VM-Leistungsdaten. Diese Profilerstellung besitzt fast keine Auswirkungen auf die Leistung der Hyper-V-/ESXi-Hosts oder VMware vCenter Server.

### <a name="can-i-harden-the-appliance-vm"></a>Kann ich die Appliance-VM härten?

Wenn Sie die Appliance-VM mithilfe der heruntergeladenen Vorlage erstellen, können Sie der Vorlage zusätzliche Komponenten (z. B. Antivirensoftware) hinzufügen, solange die für die Azure Migrate-Appliance erforderlichen Kommunikations- und Firewallregeln unverändert bleiben.


## <a name="what-network-connectivity-is-needed"></a>Welche Netzwerkkonnektivität ist erforderlich?

Lesen Sie Folgendes:
- VMware-Bewertung mithilfe der Azure Migrate-Appliance: Zugriffsanforderungen für [URL](migrate-appliance.md#url-access) und [Port](migrate-support-matrix-vmware.md#port-access).
- VMware-Bewertung ohne Agent mithilfe der Azure Migrate-Appliance: Zugriffsanforderungen für [URL](migrate-appliance.md#url-access) und [Port](migrate-support-matrix-vmware-migration.md#agentless-ports).
- Hyper-V-Bewertung mithilfe der Azure Migrate-Appliance: Zugriffsanforderungen für [URL](migrate-appliance.md#url-access) und [Port](migrate-support-matrix-hyper-v.md#port-access).


## <a name="what-data-does-the-appliance-collect"></a>Welche Daten werden von der Appliance erfasst?

Informieren Sie sich über die erfassten Daten:

- [Leistungsdaten](migrate-appliance.md#collected-performance-data-vmware) und [Metadaten](migrate-appliance.md#collected-metadata-vmware) der VMware-VM.
- [Leistungsdaten](migrate-appliance.md#collected-performance-data-hyper-v) und [Metadaten](migrate-appliance.md#collected-metadata-hyper-v) der Hyper-V-VM.


## <a name="how-is-data-stored"></a>Wie werden die Daten gespeichert?

Die von der Azure Migrate-Appliance erfassten Daten werden an dem Azure-Speicherort gespeichert, den Sie beim Erstellen des Migrationsprojekts auswählen. 

- Die Daten werden sicher in einem Microsoft-Abonnement gespeichert und gelöscht, wenn Sie das Azure Migrate-Projekt löschen.
- Wenn Sie Für die [Visualisierung von Abhängigkeiten](concepts-dependency-visualization.md) verwenden, werden die erfassten Daten in den USA in einem Log Analytics-Arbeitsbereich gespeichert, der im Azure-Abonnement erstellt wurde. Diese Daten werden gelöscht, wenn Sie den Log Analytics-Arbeitsbereich in Ihrem Abonnement löschen.

## <a name="how-much-data-is-uploaded-in-continuous-profiling"></a>Wie viele Daten werden bei der kontinuierlichen Profilerstellung hochgeladen?

Das an Azure Migrate gesendete Datenvolumen hängt von verschiedenen Parametern ab. Sie können dabei davon ausgehen, dass ein Azure Migrate-Projekt mit 10 Computern (jeweils mit einem Datenträger und einem NIC) pro Tag rund 50 MB übermittelt. Dieser Wert ist ungefähr und hängt von der Anzahl der Datenpunkte für die NICs und Datenträger ab. Die Menge der gesendeten Daten steigt nicht linear, wenn sich die Anzahl der Computer, NICs oder Datenträger erhöht.

## <a name="is-data-encrypted-at-restin-transit"></a>Werden Daten im Ruhezustand und bei der Übertragung verschlüsselt?

Ja, für beides.

- Metadaten werden sicher über das Internet per HTTPS an den Azure Migrate-Dienst gesendet.
- Metadaten werden in einer [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md)-Datenbank und in [Azure Blob Storage](../storage/common/storage-service-encryption.md) in einem Microsoft-Abonnement gespeichert. Die Metadaten werden im Ruhezustand verschlüsselt.
- Die für die Abhängigkeitsanalyse gesammelten Daten werden auch während der Übertragung verschlüsselt (sicheres HTTPS). Sie werden in einem Log Analytics-Arbeitsbereich in Ihrem Abonnement gespeichert. Außerdem erfolgt eine Verschlüsselung im Ruhezustand.

## <a name="how-does-the-appliance-connect-to-vcenter-server"></a>Wie stellt die Appliance eine Verbindung mit vCenter Server her?

1. Die Appliance stellt mit den bei der Einrichtung der Appliance angegebenen Anmeldeinformationen eine Verbindung zu vCenter Server (Port 443) her.
2. Die Appliance nutzt VMware PowerCLI, um vCenter Server abzufragen und Metadaten über die von vCenter Server verwalteten VMs zu sammeln.
3. Die Appliance erfasst Konfigurationsdaten zu virtuellen Computern (Kerne, Arbeitsspeicher, Datenträger, NICs) sowie den Leistungsverlauf für jeden virtuellen Computer für den letzten Monat.
4. Die erfassten Metadaten werden an Azure Migrate gesendet: Serverbewertung (über das Internet mit HTTPS) zur Bewertung gesendet.

## <a name="can-i-connect-the-appliance-to-multiple-vcenter-servers"></a>Kann ich die Appliance mit mehreren vCenter-Servern verbinden?

Nein. Zwischen einer Appliance und vCenter Server besteht eine 1: 1-Zuordnung. Wenn Sie VMs in mehreren vCenter Server-Instanzen ermitteln möchten, müssen Sie mehrere Appliances bereitstellen.

### <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Wie viele VMs können mit einer Appliance ermittelt werden?

Sie können bis zu 10.000 VMware-VMs und bis zu 5.000 Hyper-V-VMs mit einer einzigen Appliance ermitteln. Wenn Sie mehr Computer in Ihrer lokalen Umgebung haben, informieren Sie sich über die Skalierung der [Hyper-V](scale-hyper-v-assessment.md)- und [VMware](scale-vmware-assessment.md)-Bewertung.

## <a name="can-i-delete-an-appliance"></a>Kann ich eine Appliance löschen?

Das Löschen der Appliance aus dem Projekt wird derzeit nicht unterstützt.

- Die einzige Möglichkeit zum Löschen der Appliance besteht darin, die Ressourcengruppe zu löschen, die das Azure Migrate-Projekt enthält, das mit der Appliance verknüpft ist.
- Beim Löschen der Ressourcengruppe werden jedoch auch andere registrierte Appliances, der ermittelte Bestand, Bewertungen und alle anderen Azure-Komponenten, die dem Projekt in der Ressourcengruppe zugeordnet sind, gelöscht.


## <a name="can-i-use-the-appliance-with-a-different-subscriptionproject"></a>Kann ich die Appliance mit einem anderen Abonnement oder Projekt verwenden?

Nach der Verwendung der Appliance zum Initiieren der Ermittlung können Sie sie nicht mit einem anderen Azure-Abonnement oder in einem anderen Azure Migrate-Projekt neu konfigurieren. Es ist auch nicht möglich, virtuelle Computer in einer anderen vCenter Server-Instanz zu ermitteln. Richten Sie für diese Aufgaben eine neue Appliance ein.

## <a name="can-i-set-up-the-appliance-on-an-azure-vm"></a>Kann ich die Appliance auf einem virtuellen Azure-Computer einrichten?
Derzeit nicht unterstützt. 

## <a name="can-i-discover-on-an-esxi-host"></a>Kann die Ermittlung auf einem ESXi-Host durchgeführt werden?
Nein, zum Ermitteln von virtuellen VMware-Computern benötigen Sie eine vCenter Server-Instanz.

## <a name="how-do-i-update-the-appliance"></a>Wie aktualisiere ich die Appliance?

Standardmäßig werden die Appliance und die zugehörigen installierten Agents automatisch aktualisiert. Die Appliance prüft alle 24 Stunden, ob Updates verfügbar sind. Wenn beim Updateprozess Fehler auftreten, wird die Aktualisierung wiederholt. Automatische Updates werden nur für die Appliance und die Appliance-Agents durchgeführt. Das Betriebssystem wird nicht aktualisiert. Verwenden Sie Microsoft Updates, um das Betriebssystem auf dem neuesten Stand zu halten.

## <a name="can-i-check-agent-health"></a>Kann ich die Agent-Integrität überprüfen?

Navigieren Sie im Portal im Tool „Serverbewertung“ oder „Servermigration“ zur Seite **Agent-Integrität**. Dort können Sie den Verbindungsstatus zwischen den Ermittlungs- und Bewertungs-Agents auf der Appliance und in Azure überprüfen.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Übersicht zu Azure Migrate](migrate-services-overview.md) an.

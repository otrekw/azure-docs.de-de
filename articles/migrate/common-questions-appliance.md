---
title: Häufig gestellte Fragen zur Azure Migrate-Appliance
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Azure Migrate-Appliance.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 529ead1baa769ee3e71f6fcf77ef7e020ed196a6
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81529687"
---
# <a name="azure-migrate-appliance-common-questions"></a>Azure Migrate-Appliance: Häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zur Azure Migrate-Appliance beantwortet. Wenn Sie weitere Fragen haben, sehen Sie sich die folgenden Ressourcen an:

- [Allgemeine Fragen](resources-faq.md) zu Azure Migrate
- Fragen zur [Ermittlung, Bewertung und Abhängigkeitsvisualisierung](common-questions-discovery-assessment.md)
- Fragen zur [Servermigration](common-questions-server-migration.md)
- Antworten auf Fragen im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum)

## <a name="what-is-the-azure-migrate-appliance"></a>Was ist die Azure Migrate-Appliance?

Die Azure Migrate-Appliance ist eine einfache Appliance, die vom Azure Migrate: Serverbewertungstool verwendet wird, um lokale Server zu entdecken und zu bewerten. Azure Migrate: verwendet die Appliance auch für die Migration ohne Agent für lokale VMware-VMs.

Weitere Informationen zur Azure Migrate-Appliance:

- Die Appliance wird lokal als VM oder physischer Computer bereitgestellt.
- Die Appliance ermittelt lokale Computer und sendet kontinuierlich Computermetadaten und Leistungsdaten an Azure Migrate.
- Die Applianceermittlung erfolgt ohne Agent. Auf ermittelten Computern wird nichts installiert.

[Weitere Informationen](migrate-appliance.md) zur Appliance.

## <a name="how-can-i-deploy-the-appliance"></a>Wie kann ich die Appliance bereitstellen?

Die Appliance kann wie folgt bereitgestellt werden:

- Mithilfe einer Vorlage für VMware-VMs und Hyper-V-VMs (OVA-Vorlage für VMware oder VHD für Hyper-V).
- Wenn Sie keine Vorlage verwenden möchten oder in Azure Government arbeiten, können Sie die Appliance für VMware oder Hyper-V mithilfe eines PowerShell-Skripts bereitstellen.
- Bei physischen Servern stellen Sie die Appliance immer mithilfe eines Skripts bereit.


## <a name="how-does-the-appliance-connect-to-azure"></a>Wie stellt die Appliance eine Verbindung mit Azure her?

Die Verbindung der Appliance kann über das Internet oder über ExpressRoute mit öffentlichem/Microsoft-Peering erfolgen.

## <a name="does-appliance-analysis-affect-performance"></a>Wirkt sich die Applianceanalyse auf die Leistung aus?

Die Azure Migrate-Appliance erstellt fortlaufend Profile von lokalen Computern zur Messung von Leistungsdaten. Diese Profilerstellung hat fast keinen Einfluss auf die Leistung von Profilerstellungscomputern.

## <a name="can-i-harden-the-appliance-vm"></a>Kann ich die Appliance-VM härten?

Wenn Sie die Appliance-VM mithilfe der heruntergeladenen Vorlage erstellen, können Sie der Vorlage zusätzliche Komponenten (z. B. Antivirensoftware) hinzufügen, solange die für die Azure Migrate-Appliance erforderlichen Kommunikations- und Firewallregeln bestehen bleiben.

## <a name="what-network-connectivity-is-required"></a>Welche Netzwerkkonnektivität ist erforderlich?


Die Appliance benötigt Zugriff auf Azure-URLs. [Überprüfen](migrate-appliance.md#url-access) Sie die URL-Liste.

## <a name="what-data-does-the-appliance-collect"></a>Welche Daten werden von der Appliance erfasst?

In den folgenden Artikeln finden Sie Informationen zu den Daten, die von der Azure Migrate-Appliance auf VMs erfasst werden:

- **Virtueller VMware-Computer:** [Überprüfen](migrate-appliance.md#collected-data---vmware) gesammelter Daten. [
- **Virtueller Hyper-V-Computer**: [Überprüfen](migrate-appliance.md#collected-data---hyper-v) gesammelter Daten.

## <a name="how-is-data-stored"></a>Wie werden die Daten gespeichert?

Die von der Azure Migrate-Appliance erfassten Daten werden an dem Azure-Speicherort gespeichert, an dem Sie das Azure Migrate-Projekt erstellt haben.

Weitere Informationen zur Speicherung von Daten:

- Die erfassten Daten werden sicher in CosmosDB in einem Microsoft-Abonnement gespeichert. Die Daten werden gelöscht, wenn Sie das Azure Migrate-Projekt löschen. Die Speicherung wird von Azure Migrate durchgeführt. Sie können nicht speziell ein Speicherkonto für erfasste Daten auswählen.
- Wenn Sie die [Visualisierung von Abhängigkeiten](concepts-dependency-visualization.md) verwenden, werden die erfassten Daten in den USA in einem Azure Log Analytics-Arbeitsbereich gespeichert, der in Ihrem Azure-Abonnement erstellt wurde. Die Daten werden gelöscht, wenn Sie den Log Analytics-Arbeitsbereich in Ihrem Abonnement löschen.

## <a name="how-much-data-is-uploaded-during-continuous-profiling"></a>Wie viele Daten werden bei der kontinuierlichen Profilerstellung hochgeladen?

Das an Azure Migrate gesendete Datenvolumen hängt von mehreren Parametern ab. Beispielsweise übermittelt ein Azure Migrate-Projekt mit 10 Computern (jeweils mit einem Datenträger und einer NIC) pro Tag rund 50 MB Daten. Dieser ist ein ungefährer Wert. Der tatsächliche Wert ist je nach Anzahl der Datenpunkte für die Datenträger und NICs unterschiedlich. Wenn sich die Anzahl der Computer, Datenträger oder NICs erhöht, steigt die Menge der gesendeten Daten nicht linear.

## <a name="is-data-encrypted-at-rest-and-in-transit"></a>Werden die Daten im Ruhezustand und bei der Übertragung verschlüsselt?

Ja, in beiden Fällen:

- Metadaten werden sicher über das Internet per HTTPS an den Azure Migrate-Dienst gesendet.
- Metadaten werden in einer [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md)-Datenbank und in [Azure Blob Storage](../storage/common/storage-service-encryption.md) in einem Microsoft-Abonnement gespeichert. Die Metadaten werden im Ruhezustand für die Speicherung verschlüsselt.
- Die für die Abhängigkeitsanalyse gesammelten Daten werden ebenfalls während der Übertragung verschlüsselt (durch sicheres HTTPS). Sie werden in einem Log Analytics-Arbeitsbereich in Ihrem Abonnement gespeichert. Die Daten werden im Ruhezustand für die Abhängigkeitsanalyse verschlüsselt.

## <a name="how-does-the-appliance-connect-to-vcenter-server"></a>Wie stellt die Appliance eine Verbindung mit vCenter Server her?

In den folgenden Schritten wird beschrieben, wie die Appliance eine Verbindung mit VMware vCenter Server herstellt:

1. Die Appliance stellt mithilfe der von Ihnen bei der Einrichtung der Appliance angegebenen Anmeldeinformationen eine Verbindung mit vCenter Server (Port 443) her.
2. Die Appliance nutzt VMware PowerCLI, um vCenter Server abzufragen und Metadaten über die von vCenter Server verwalteten VMs zu sammeln.
3. Die Appliance erfasst Konfigurationsdaten zu virtuellen Computern (Kerne, Arbeitsspeicher, Datenträger, NICs) sowie den Leistungsverlauf für jeden virtuellen Computer für den letzten Monat.
4. Die erfassten Metadaten werden an das Azure Migrate: Serverbewertungstool (über das Internet mit HTTPS) zur Bewertung gesendet.

## <a name="can-the-azure-migrate-appliance-connect-to-multiple-vcenter-servers"></a>Kann die Azure Migrate-Appliance eine Verbindung mit mehreren vCenter Server-Instanzen herstellen?

Nein. Zwischen einer [Azure Migrate-Appliance](migrate-appliance.md) und vCenter Server besteht eine 1: 1-Zuordnung. Wenn Sie VMs in mehreren vCenter Server-Instanzen ermitteln möchten, müssen Sie mehrere Appliances bereitstellen. 

## <a name="can-an-azure-migrate-project-have-multiple-appliances"></a>Kann ein Azure Migrate-Projekt über mehrere Appliances verfügen?
An ein Projekt können mehrere Appliances angefügt sein. Eine Appliance kann jedoch nur einem einzelnen Projekt zugeordnet werden. 

## <a name="can-the-azure-migrate-appliancereplication-appliance-connect-to-the-same-vcenter"></a>Kann die Azure Migrate-Appliance/Replikations-Appliance eine Verbindung mit demselben vCenter herstellen?
Ja. Sie können sowohl die Azure Migrate-Appliance (wird für die Bewertung und die VMware-Migration ohne Agent verwendet) als auch die Replikations-Appliance (wird für die Agent-basierte Migration von VMware-VMS verwendet) demselben vCenter-Server hinzufügen.


## <a name="how-many-vms-or-servers-can-i-discover-with-an-appliance"></a>Wie viele VMs oder Server können mit einer Appliance ermittelt werden?

Sie können bis zu 10.000 VMware-VMs und bis zu 5.000 Hyper-V-VMs sowie bis zu 250 physische Server mit einer einzigen Appliance ermitteln. Wenn Sie mehr Computer in Ihrer lokalen Umgebung haben, informieren Sie sich über die Skalierung der [Hyper-V-](scale-hyper-v-assessment.md), [VMware-](scale-vmware-assessment.md) und [physischen](scale-physical-assessment.md) Bewertung.

## <a name="can-i-delete-an-appliance"></a>Kann ich eine Appliance löschen?

Das Löschen einer Appliance aus dem Projekt wird derzeit nicht unterstützt.

Die einzige Möglichkeit zum Löschen der Appliance besteht darin, die Ressourcengruppe zu löschen, die das Azure Migrate-Projekt enthält, das mit der Appliance verknüpft ist.

Beim Löschen der Ressourcengruppe werden jedoch auch andere registrierte Appliances, der ermittelte Bestand, Bewertungen und alle anderen Azure-Komponenten in der Ressourcengruppe, die dem Projekt zugeordnet sind, gelöscht.

## <a name="can-i-use-the-appliance-with-a-different-subscription-or-project"></a>Kann ich die Appliance mit einem anderen Abonnement oder Projekt verwenden?

Nachdem Sie die Appliance zum Initiieren der Ermittlung verwendet haben, können Sie sie nicht für die Verwendung mit einem anderen Azure-Abonnement neu konfigurieren und sie auch nicht in einem anderen Azure Migrate-Projekt verwenden. Es ist auch nicht möglich, VMs in einer anderen vCenter Server-Instanz zu ermitteln. Richten Sie für diese Aufgaben eine neue Appliance ein.

## <a name="can-i-set-up-the-appliance-on-an-azure-vm"></a>Kann ich die Appliance auf einem virtuellen Azure-Computer einrichten?

Nein. Diese Option wird derzeit nicht unterstützt. 

## <a name="can-i-discover-on-an-esxi-host"></a>Kann die Ermittlung auf einem ESXi-Host durchgeführt werden?

Nein. Zum Ermitteln von VMware-VMs benötigen Sie vCenter Server.

## <a name="how-do-i-update-the-appliance"></a>Wie aktualisiere ich die Appliance?

Standardmäßig werden die Appliance und die zugehörigen installierten Agents automatisch aktualisiert. Die Appliance prüft alle 24 Stunden, ob Updates verfügbar sind. Fehlerhafte Updates werden wiederholt. 

Nur die Appliance und die Appliance-Agents werden durch diese automatischen Updates aktualisiert. Das Betriebssystem wird durch automatische Azure Migrate-Updates nicht aktualisiert. Verwenden Sie Windows-Updates, um das Betriebssystem auf dem neuesten Stand zu halten.

## <a name="can-i-check-agent-health"></a>Kann ich die Agent-Integrität überprüfen?

Ja. Navigieren Sie im Portal zur Seite **Agent-Integrität** für das Azure Migrate: Serverbewertung oder Azure Migrate: Servermigration“ kommunizieren kann. Dort können Sie den Verbindungsstatus zwischen Azure und den Ermittlungs- und Bewertungs-Agents auf der Appliance überprüfen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Übersicht zu Azure Migrate](migrate-services-overview.md) an.

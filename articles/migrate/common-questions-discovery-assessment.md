---
title: 'Häufig gestellte Fragen: Ermittlung, Bewertung und Abhängigkeitsanalyse in Azure Migrate'
description: Erhalten Sie Antworten auf häufig gestellte Fragen zur Ermittlung, Bewertung und Abhängigkeitsanalyse in Azure Migrate.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 179be4f5b39806c717e94789c9fe52842d2a07d8
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589231"
---
# <a name="common-questions-about-discovery-assessment-and-dependency-analysis"></a>Häufig gestellte Fragen zur Ermittlung, Bewertung und Abhängigkeitsanalyse

In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen zur Ermittlung, Bewertung und Abhängigkeitsanalyse in Azure Migrate. Wenn Sie weitere Fragen haben, lesen Sie die folgenden Artikel:

- [Allgemeine Fragen](resources-faq.md) zu Azure Migrate.
- [Fragen](common-questions-appliance.md) zur Azure Migrate-Appliance.
- [Fragen](common-questions-server-migration.md) zur Servermigration.
- Veröffentlichen von Fragen im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum)



## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Wie viele VMs können mit einer Appliance ermittelt werden?

Sie können bis zu 10.000 VMware-VMs und bis zu 5.000 Hyper-V-VMs sowie bis zu 250 Server mit einer einzigen Appliance ermitteln. Wenn Sie mehr Computer in Ihrer lokalen Umgebung haben, informieren Sie sich über die Skalierung der [Hyper-V](scale-hyper-v-assessment.md)-, [VMware](scale-vmware-assessment.md)- und [physischen](scale-physical-assessment.md) Bewertung.



## <a name="vm-size-changed-can-i-run-an-assessment-again"></a>Die VM-Größe hat sich geändert. Kann ich die Bewertung erneut ausführen?

Die Azure Migrate-Appliance sammelt kontinuierlich Informationen zur lokalen Umgebung. Eine Bewertung ist jedoch eine Point-in-Time-Momentaufnahme von lokalen VMs. Wenn Sie die Einstellungen für eine VM, die Sie bewerten möchten, ändern, verwenden Sie die Option „Neu berechnen“, um die Bewertung mit den neuesten Änderungen zu aktualisieren.

### <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>Wie ermittle ich VMs in einer mehrinstanzenfähigen Umgebung?

- Wenn Sie für VMware eine Umgebung haben, die von Mandanten gemeinsam genutzt wird, und Sie die VMs eines Mandanten nicht im Abonnement eines anderen Mandanten ermitteln möchten, erstellen Sie vCenter Server-Anmeldeinformationen mit Zugriff nur auf die VMs, die Sie ermitteln möchten. Verwenden Sie diese Anmeldeinformationen dann, wenn Sie die Ermittlung in der Azure Migrate-Appliance starten.
- Bei Hyper-V werden bei der Ermittlung Anmeldeinformationen für den Hyper-V-Host verwendet. Wenn virtuelle Computer denselben Hyper-V-Host nutzen, gibt es derzeit keine Möglichkeit, die Ermittlung zu trennen.  


### <a name="do-i-need-vcenter-server-for-vmware-vm-discovery"></a>Benötig ich vCenter Server zum Ermitteln von VMware-VMs?

Ja, Azure Migrate benötigt für die Ermittlung in einer VMware-Umgebung vCenter Server. Die Ermittlung von ESXi-Hosts, die nicht von vCenter Server verwaltet werden, wird nicht unterstützt.


## <a name="whats-the-difference-sizing-options"></a>Worin besteht der Unterschied bei den Größenoptionen?

Bei der lokal orientierten Größenanpassung berücksichtigt Azure Migrate keine VM-Leistungsdaten für die Bewertung. Die VM-Größen werden basierend auf der lokalen Konfiguration dimensioniert. Bei der leistungsbasierten Größenanpassung basiert diese auf Auslastungsdaten.

- Wenn eine lokale VM beispielsweise über 4 Kerne und 8 GB Arbeitsspeicher verfügt, tritt bei einer CPU- und Arbeitsspeicherauslastung von jeweils 50 % der folgende Fehler auf:
    - Bei der lokal orientierten Größenanpassung wird eine Azure-VM-SKU mit 4 Kernen und 8 GB Arbeitsspeicher empfohlen.
    - Bei der leistungsbasierten Größenanpassung wird jedoch eine VM-SKU mit 2 Kernen und 4 GB Arbeitsspeicher empfohlen, da auch der Auslastungsprozentsatz berücksichtigt wird.

- Entsprechend hängt die Größenanpassung der Datenträger von zwei Bewertungseigenschaften ab: dem Größenkriterium und dem Speichertyp.
    - Wenn das leistungsbasierte Größenkriterium und der Speichertyp „Automatisch“ verwendet werden, berücksichtigt Azure Migrate die IOPS- und Durchsatzwerte des Datenträgers, um den Typ des Zieldatenträgers zu ermitteln (Standard oder Premium).
    - Wenn das leistungsbasierte Größenkriterium und der Speichertyp „Premium“ verwendet werden, empfiehlt Azure Migrate eine Premium-Datenträger-SKU basierend auf dem lokalen Datenträger. Die gleiche Logik wird verwendet, um die Größenanpassung für Datenträger durchzuführen, wenn die lokal orientierte Größenanpassung verwendet wird und der Speichertyp „Standard“ oder „Premium“ lautet.

## <a name="does-performance-historyutilization-impact-sizing"></a>Wirkt sich der Leistungsverlauf bzw. die Auslastung auf die Größenanpassung aus?

Diese Eigenschaften gelten nur für die leistungsbasierte Größenanpassung.

- Azure Migrate sammelt den Leistungsverlauf von lokalen Computern und nutzt diese Daten, um die VM-Größe und den Datenträgertyp in Azure zu empfehlen.
- Die Appliance erstellt für die lokale Umgebung kontinuierlich Profildaten, um alle 20 Sekunden in Echtzeit Auslastungsdaten erfassen zu können.
- Die Appliance führt für die 20-Sekunden-Stichproben ein Rollup aus und erstellt alle 15 Minuten einen einzelnen Datenpunkt.
- Zum Erstellen des Datenpunkts wählt die Appliance den Spitzenwert aus den 20-Sekunden-Stichproben aus.
- Diesen Datenpunkt sendet die Appliance an Azure.

Beim Erstellen einer Bewertung in Azure berechnet Azure Migrate basierend auf der Leistungsdauer und dem Perzentilwert des Leistungsverlaufs den geltenden Auslastungswert und verwendet ihn für die Größenanpassung.

- Wenn Sie die Leistungsdauer beispielsweise auf einen Tag und den Perzentilwert auf 95 festgelegt haben, sortiert Azure Migrate die vom Collector gesendeten 15-Minuten-Stichprobenpunkte für den gesamten letzten Tag in aufsteigender Reihenfolge und wählt den Wert des 95. Perzentils als gültige Auslastung aus.
- Die Verwendung des Werts des 95. Perzentils stellt sicher, dass Ausreißer ignoriert werden. Ausreißer können eingeschlossen werden, indem Sie das 99. Perzentil verwenden. Falls Sie die Spitzenauslastung für den Zeitraum erfassen möchten, ohne Ausreißer zu verpassen, sollten Sie das 99. Perzentil verwenden.

## <a name="what-is-dependency-visualization"></a>Was ist die Visualisierung von Abhängigkeiten?

Verwenden Sie die Visualisierung von Abhängigkeiten, um Gruppen von VMs für die Migration mit größerer Sicherheit zu bewerten. Die Visualisierung von Abhängigkeiten überprüft die Abhängigkeiten zwischen Computern, bevor Sie eine Bewertung ausführen. Sie stellt sicher, dass nichts übersehen wird, und vermeidet so unerwartete Ausfälle bei der Migration zu Azure. Azure Migrate verwendet die Dienstzuordnungslösung in Azure Monitor, um Abhängigkeitsvisualisierung zu ermöglichen. [Weitere Informationen](concepts-dependency-visualization.md).

> [!NOTE]
> Abhängigkeitsvisualisierung ist in Azure Government nicht verfügbar.

## <a name="do-i-pay-for-dependency-visualization"></a>Ist die Visualisierung der Abhängigkeiten kostenpflichtig?
Nein. Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="what-do-i-install-for-dependency-visualization"></a>Was muss ich für die Visualisierung der Abhängigkeiten installieren?

Sie müssen für die Verwendung der Visualisierung von Abhängigkeiten Agents auf alle lokalen Computer, die Sie bewerten möchten, herunterladen und dort installieren.

Sie müssen die folgenden Agents auf jedem Computer installieren:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)
- [Dependency-Agent](../azure-monitor/platform/agents-overview.md#dependency-agent)
- Falls Sie über Computer ohne Internetverbindung verfügen, ist es erforderlich, auf diesen das Log Analytics-Gateway herunterzuladen und zu installieren.

Sie benötigen diese Agents nur, wenn Sie die Visualisierung von Abhängigkeiten verwenden.

## <a name="can-i-use-an-existing-workspace"></a>Kann ich einen bestehenden Arbeitsbereich verwenden?

Ja, Sie können einen vorhandenen Arbeitsbereich an das Migrationsprojekt anfügen und für die Visualisierung von Abhängigkeiten nutzen. 

## <a name="can-i-export-the-dependency-visualization-report"></a>Kann ich den Bericht über die Abhängigkeitsvisualisierung exportieren?

Nein, Visualisierungen von Abhängigkeiten können nicht exportiert werden. Azure Migrate verwendet jedoch die Dienstzuordnung, sodass Sie die Abhängigkeiten mithilfe der [Service Map-REST-API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) im JSON-Format abrufen können.

## <a name="can-i-automate--mmadependency-agent-installation"></a>Kann ich die MMA-/Abhängigkeits-Agent-Installation automatisieren?

Verwenden Sie [dieses Skript für die Installation des Dependency-Agents](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples). [Befolgen Sie diese Anweisungen](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent), um MMA über die Befehlszeile oder automatisiert zu installieren. Für MMA verwenden Sie [dieses Skript](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Zusätzlich zu Skripts können Sie auch Bereitstellungstools wie Microsoft Endpoint Configuration Manager und [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) zum Bereitstellen der Agents verwenden.


## <a name="what-operating-systems-does-mma-support"></a>Welche Betriebssysteme unterstützt MMA?

- Überprüfen Sie die Liste der [von MMA unterstützten Windows-Betriebssysteme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
- Überprüfen Sie die Liste der [von MMA unterstützten Linux-Betriebssysteme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

## <a name="can-i-visualize-dependencies-for-more-than-an-hour"></a>Kann ich Abhängigkeiten für mehr als eine Stunde visualisieren?
Nein. Sie können Abhängigkeiten bis zu einer Stunde visualisieren. Sie können zu einem bestimmten Datum im Verlauf zurückkehren (höchstens einen Monat), aber die maximale Dauer der Visualisierung beträgt eine Stunde. Sie können beispielsweise den Zeitraum im Abhängigkeitsdiagramm verwenden, um Abhängigkeiten für den gestrigen Tag anzuzeigen. Sie können die Abhängigkeiten allerdings nur für ein Zeitfenster von einer Stunde anzeigen. Sie können allerdings zum [Abfragen der Abhängigkeitsdaten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) über einen längeren Zeitraum Azure Monitor-Protokolle verwenden.

## <a name="can-visualize-dependencies-for-groups-of-more-than-10-vms"></a>Kann ich für Gruppen mit mehr als 10 VMs Abhängigkeiten visualisieren?
Sie können [Abhängigkeiten für Gruppen visualisieren](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies), die bis zu 10 VMs umfassen. Falls eine Gruppe aus mehr als 10 VMs besteht, sollten Sie die Gruppe in kleinere Gruppen aufteilen und die Abhängigkeiten anschließend visualisieren.




## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Übersicht zu Azure Migrate](migrate-services-overview.md) an.

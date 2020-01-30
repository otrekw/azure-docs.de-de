---
title: Bewertungen in Azure Migrate
description: Erfahren Sie etwas über Bewertungen in Azure Migrate.
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 5fc61d9987c9e728a5d83cb3ab3f91b8e8f5f740
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76833327"
---
# <a name="about-assessments-in-azure-migrate"></a>Informationen zu Bewertungen in Azure Migrate

Dieser Artikel führt die Berechnung von Bewertungen in [Azure Migrate: Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool) aus. Sie führen Bewertungen für Gruppen von lokalen Computern aus, um festzustellen, ob sie für die Migration zu Azure Migrate bereit sind.

## <a name="how-do-i-run-an-assessment"></a>Ausführen einer Bewertung
Sie können eine Bewertung mithilfe von „Azure Migrate: Serverbewertung“ oder mit einem anderen Azure-Tool oder einem Drittanbietertool ausführen. Nach dem Erstellen eines Azure Migrate-Projekts fügen Sie das entsprechende Tool hinzu. [Weitere Informationen](how-to-add-tool-first-time.md)

### <a name="collect-compute-data"></a>Erfassen von Computedaten

Leistungsdaten für Computeeinstellungen werden wie folgt erfasst:

1. Die [Azure Migrate-Appliance](migrate-appliance.md) erfasst Echtzeit-Abtastpunkte:

    - **VMware-VMs:** Bei virtuellen VMware-Computern sammelt die Azure Migrate-Appliance Echtzeit-Abtastpunkte in einem 20-Sekunden-Intervall.
    - **Virtuelle Hyper-V-Computer:** Bei Hyper-V-VMs werden die Echtzeit-Abtastpunkte in einem Intervall von 30 Sekunden erfasst.
    - **Physische Server**: Bei physischen Servern werden die Echtzeit-Abtastpunkte in einem Intervall von 5 Minuten erfasst. 
    
2. Die Appliance fragt die Abtastpunkte ab (20 Sekunden, 30 Sekunden, fünf Minuten), um alle 10 Minuten einen einzelnen Datenpunkt zu erstellen. Zum Erstellen des Datenpunkts wählt die Appliance den Spitzenwert aus allen Stichproben aus und sendet diesen dann an Azure.
3. Die Serverbewertung speichert alle 10-Minuten-Abtastpunkte des letzten Monats.
4. Wenn Sie eine Bewertung erstellen, wird bei der Serverbewertung basierend auf den Perzentilwerten für den *Leistungsverlauf* und den *Perzentilwert der Nutzung* der entsprechende Datenpunkt identifiziert, der für die richtige Größenanpassung verwendet werden soll.

    - Wenn der Leistungsverlauf beispielsweise auf eine Woche festgelegt ist und der Perzentilwert der Nutzung das 95. Perzentil ist, sortiert die Serverbewertung die 10-minütigen Abtastpunkte für die letzte Woche in aufsteigender Reihenfolge und wählt das 95. Perzentil für die richtige Größe aus. 
    - Mit dem Wert des 95. Quantils wird sichergestellt, dass Ausreißer ignoriert werden. Diese können enthalten sein, wenn Sie das 99. Quantil wählen.
    - Falls Sie die Spitzenauslastung für den Zeitraum wählen möchten und keine Ausreißer verpassen möchten, sollten Sie das 99. Quantil als Quantilauslastung wählen.

5. Dieser Wert wird dann mit dem Komfortfaktor multipliziert, um die effektiven Leistungsauslastungsdaten für jede Metrik (CPU-Auslastung, Speicherauslastung, Festplatten-IOPS (Lesen und Schreiben), Festplattendurchsatz (Lesen und Schreiben), Netzwerkdurchsatz (Ein- und Ausgabe)) zu erhalten, die die Appliance sammelt.

Zum Ausführen von Bewertungen in der Serverbewertung bereiten Sie die Bewertung lokal und in Azure vor und richten die Azure Migrate-Appliance so ein, dass lokale Computer kontinuierlich ermittelt werden. Nachdem Computer ermittelt wurden, werden sie zur Bewertung in Gruppen zusammengefasst. Für ausführlichere und zuverlässige Bewertungen können Sie Abhängigkeiten zwischen Computern visualisieren und zuordnen, um zu ermitteln, wie sie migriert werden.

- Erfahren Sie etwas über das Ausführen von Bewertungen für [virtuelle VMware-Computer](tutorial-prepare-vmware.md), [virtuelle Hyper-V-Computer](tutorial-prepare-hyper-v.md) und [physische Server](tutorial-prepare-physical.md).
- Erfahren Sie etwas über die Bewertung von [mithilfe einer CSV-Datei importierten](tutorial-assess-import.md) Servern.
- Erfahren Sie etwas über das Einrichten der [Visualisierung von Abhängigkeiten](concepts-dependency-visualization.md).

## <a name="assessments-in-server-assessment"></a>Bewertungen in der Serverbewertung 

Bewertungen, die Sie mit Azure Migrate-Serverbewertungen erstellen, sind Momentaufnahmen von Daten zu einem gewissen Zeitpunkt. Das Serverbewertungstool ermöglicht zwei Arten von Bewertungen.

**Bewertungstyp** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen, die Empfehlungen auf der Grundlage der erfassten Leistungsdaten aussprechen | Die Empfehlung zur VM-Größe basiert auf CPU- und Speicherauslastungsdaten.<br/><br/> Die Empfehlung zum Datenträgertyp (HDD/SSD Standard oder Premium) basiert auf dem IOPS und dem Durchsatz der lokalen Datenträger.
**Aktuelle lokale Umgebung** | Bewertungen, die keine Leistungsdaten verwenden, um Empfehlungen auszusprechen. | Die Empfehlung zur VM-Größe basiert auf der lokalen VM-Größe<br/><br> Der empfohlene Datenträgertyp basiert auf dem ausgewählten Speichertyp für die Bewertung.

## <a name="collecting-performance-data"></a>Erfassen von Leistungsdaten

Leistungsdaten werden wie folgt erfasst:

1. Die [Azure Migrate-Appliance](migrate-appliance.md) erfasst Echtzeit-Abtastpunkte:

    - **VMware-VMs:** Bei virtuellen VMware-Computern sammelt die Azure Migrate-Appliance Echtzeit-Abtastpunkte in einem 20-Sekunden-Intervall.
    - **Virtuelle Hyper-V-Computer:** Bei Hyper-V-VMs werden die Echtzeit-Abtastpunkte in einem Intervall von 30 Sekunden erfasst.
    - **Physische Server**: Bei physischen Servern werden die Echtzeit-Abtastpunkte in einem Intervall von 5 Minuten erfasst. 
    
2. Die Appliance fragt die Abtastpunkte ab (20 Sekunden, 30 Sekunden, fünf Minuten), um alle 10 Minuten einen einzelnen Datenpunkt zu erstellen. Zum Erstellen des Datenpunkts wählt die Appliance den Spitzenwert aus allen Stichproben aus und sendet diesen dann an Azure.
3. Die Serverbewertung speichert alle 10-Minuten-Abtastpunkte des letzten Monats.
4. Wenn Sie eine Bewertung erstellen, wird bei der Serverbewertung basierend auf den Perzentilwerten für den *Leistungsverlauf* und den *Perzentilwert der Nutzung* der entsprechende Datenpunkt identifiziert, der für die richtige Größenanpassung verwendet werden soll.

    - Wenn der Leistungsverlauf beispielsweise auf eine Woche festgelegt ist und der Perzentilwert der Nutzung das 95. Perzentil ist, sortiert die Serverbewertung die 10-minütigen Abtastpunkte für die letzte Woche in aufsteigender Reihenfolge und wählt das 95. Perzentil für die richtige Größe aus. 
    - Mit dem Wert des 95. Quantils wird sichergestellt, dass Ausreißer ignoriert werden. Diese können enthalten sein, wenn Sie das 99. Quantil wählen.
    - Falls Sie die Spitzenauslastung für den Zeitraum wählen möchten und keine Ausreißer verpassen möchten, sollten Sie das 99. Quantil als Quantilauslastung wählen.

5. Dieser Wert wird dann mit dem Komfortfaktor multipliziert, um die effektiven Leistungsauslastungsdaten für jede Metrik (CPU-Auslastung, Speicherauslastung, Festplatten-IOPS (Lesen und Schreiben), Festplattendurchsatz (Lesen und Schreiben), Netzwerkdurchsatz (Ein- und Ausgabe)) zu erhalten, die die Appliance sammelt.
## <a name="whats-in-an-assessment"></a>Was umfasst eine Bewertung?

Eigenschaften in einer Bewertung in „Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen.

**Eigenschaft** | **Details**
--- | ---
**Zielstandort** | Der Standort, zu dem die Migration durchgeführt werden soll. Bei der Serverbewertung werden derzeit folgende Azure-Zielregionen unterstützt:<br/><br/> „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „Indien, Mitte“, „USA, Mitte“, „China, Osten“, „China, Norden“, „Asien, Osten“, „USA, Osten“, „Deutschland, Mitte“, „Deutschland, Nordosten“, „USA, Osten 2“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „US Gov Arizona“, „US Gov Texas“, „US Gov Virginia“, „USA, Westen-Mitte“, „Europa, Westen“, „Indien, Westen“, „USA, Westen“ und „USA, Westen 2“.
*Zielspeicherdatenträger (übernommene Größe)* * | Der Typ der Datenträger, die für die Speicherung in Azure verwendet werden sollen. <br/><br/> Geben Sie den Zielspeicherdatenträger als verwalteten Premium-, verwalteten SSD Standard- oder verwalteten HDD Standard-Datenträger an.
**Zielspeicherdatenträger (leistungsbasierte Größenanpassung)** | Geben Sie den Zielspeicherdatenträger als automatischen, verwalteten Premium-, verwalteten HDD Standard- oder verwalteten SSD Standard-Datenträger an.<br/><br/> **Automatisch**: Die Datenträgerempfehlung erfolgt basierend auf den Leistungsdaten der Datenträger (Eingabe-/Ausgabevorgänge pro Sekunde (IOPS) und Durchsatz).<br/><br/>**Premium/Standard:**  Für die Bewertung wird eine Datenträger-SKU innerhalb des ausgewählten Speichertyps empfohlen.<br/><br/> Wenn Sie eine Einzelinstanz-VM-SLA von 99,9 % erreichen möchten, können Sie verwaltete Premium-Datenträger verwenden. Dadurch wird sichergestellt, dass für alle Datenträger in der Bewertung Managed Disks Premium empfohlen wird.<br/><br/> Azure Migrate unterstützt nur verwaltete Datenträger für die Migrationsbewertung.
**Reservierte Instanzen (RI)** | Geben Sie [reservierte Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure an, damit bei Kostenschätzungen in der Bewertung RI-Rabatte berücksichtigt werden.<br/><br/> Reservierte Instanzen werden derzeit nur für das Angebot mit nutzungsbasierter Bezahlung in Azure Migrate unterstützt.
**Größenkriterien** | Wird zur Größenanpassung des virtuellen Computers in Azure verwendet.<br/><br/> Sie können die Größe übernehmen oder die leistungsbasierte Größenanpassung verwenden.
**Leistungsverlauf** | Wird bei der leistungsbasierten Größenanpassung verwendet. Gibt die verwendete Dauer bei der Bewertung der Leistungsdaten an.
**Perzentilwert der Nutzung** | Wird bei der leistungsbasierten Größenanpassung verwendet. Gibt den für die Größenanpassung zu verwendenden Perzentilwert des Leistungspunkts an. 
**VM-Serie** | Geben Sie die Azure-VM-Serie an, die für die Größenanpassung berücksichtigt werden soll. Wenn Sie beispielsweise in der Produktionsumgebung keine virtuellen Computer der A-Serie in Azure benötigen, können Sie die A-Serie aus der Liste oder Serie ausschließen.
**Komfortfaktor** | Ein bei der Bewertung verwendeter Puffer. Wird zusätzlich zu den Daten zur Computernutzung für virtuelle Computer (CPU, Arbeitsspeicher, Datenträger und Netzwerk) angewandt. Berücksichtigt Bereiche wie saisonale Nutzung, kurzer Leistungsverlauf und voraussichtliche Zunahme der zukünftigen Nutzung.<br/><br/> Beispiel: Für einen virtuellen Computer mit zehn Kernen und einer Auslastung von 20 % ergibt sich normalerweise ein virtueller Computer mit zwei Kernen. Bei einem Komfortfaktor von 2,0 ist das Ergebnis dagegen ein virtueller Computer mit vier Kernen.
**Angebot** | Zeigt das [Azure-Angebot](https://azure.microsoft.com/support/legal/offer-details/) an, bei dem Sie registriert sind. Dies wird bei der Serverbewertung bei der Kostenschätzung entsprechend berücksichtigt.
**Währung** | Abrechnungswährung für Ihr Konto.
**Rabatt (%)** | Listet alle abonnementspezifischen Rabatte auf, die Sie zusätzlich zum Azure-Angebot erhalten. Die Standardeinstellung ist 0 %.
**VM-Betriebszeit** | Wenn virtuelle Azure-Computer nicht rund um die Uhr aktiv sind, können Sie den Zeitraum angeben (Anzahl der Tage pro Monat und Anzahl der Stunden pro Tag), in dem sie ausgeführt werden. Die Kostenschätzungen werden entsprechend angepasst.<br/><br/> Der Standardwert sind 31 Tage pro Monat und 24 Stunden pro Tag.
**Azure-Hybridvorteil** | Gibt an, ob Sie über Software Assurance verfügen und den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/) nutzen können. Wenn diese Eigenschaft auf „Ja“ (Standardeinstellung) festgelegt ist, werden für virtuelle Windows-Computer Nicht-Windows-Azure-Preise veranschlagt.

Sehen Sie sich die [bewährten Methoden](best-practices-assessment.md) für die Erstellung einer Bewertung mit der Serverbewertung an.

## <a name="how-are-assessments-calculated"></a>Wie werden Bewertungen berechnet? 

Bewertungen in „Azure Migrate: Serverbewertung“ werden anhand der Metadaten berechnet, die zu den lokalen Computern erfasst wurden. Wenn Sie eine Bewertung für mithilfe einer CSV-Datei importierte Computer ausführen, geben Sie die Metadaten für die Berechnung an. Berechnungen erfolgen in drei Phasen:

1. **Berechnen der Azure-Bereitschaft:** Bewerten, ob die Computer für die Migration zu Azure geeignet sind
2. **Berechnen der Größenempfehlungen:** Schätzen der Compute-, Speicher- und Netzwerkgröße 
2. **Berechnen der monatlichen Kosten:** Berechnen der geschätzten monatlichen Compute- und Speicherkosten für die Ausführung der Computer in Azure nach der Migration

Die Berechnungen werden in dieser Reihenfolge durchgeführt. Ein Server gelangt nur in die nächste Phase, wenn er die vorherige besteht. Beispiel: Besteht ein Server die Azure-Bereitschaftsprüfung nicht, wird er als ungeeignet für Azure markiert, und die Größen- und Kostenschätzungen werden für diesen Server nicht durchgeführt.



## <a name="calculate-readiness"></a>Berechnen der Bereitschaft

Nicht alle Computer sind für die Ausführung in Azure geeignet. Bei der Serverbewertung wird jeder lokale Computer bewertet und einer Bereitschaftskategorie zugewiesen. 
- **Bereit für Azure**: Der Computer kann ohne Änderungen zu Azure migriert werden. Er wird in Azure mit vollständiger Azure-Unterstützung gestartet.
- **Bedingt bereit für Azure:** Der Computer kann gegebenenfalls in Azure gestartet werden, verfügt jedoch möglicherweise nicht über vollständigen Azure-Support. Beispielsweise wird ein Computer, auf dem eine ältere Version von Windows Server ausgeführt wird, in Azure nicht unterstützt. Gehen Sie vor der Migration dieser Computer zu Azure vorsichtig vor. Befolgen Sie die in der Bewertung empfohlene Korrekturanleitung, um die Bereitschaftsprobleme zu beheben.
- **Nicht bereit für Azure**: Der Computer wird in Azure nicht gestartet. Wenn der Datenträger eines lokalen Computers beispielsweise größer als 64 TB ist, kann er nicht in Azure gehostet werden. Befolgen Sie vor der Migration die Korrekturanleitung, um das Problem zu beheben. 
- **Bereitschaft unbekannt**: Azure Migrate konnte die Bereitschaft eines Computers aufgrund unzureichender Metadaten nicht ermitteln.

Zur Berechnung der Bereitschaft werden bei der Serverbewertung die in den folgenden Tabellen aufgeführten Computereigenschaften und Betriebssystemeinstellungen überprüft. 

### <a name="machine-properties"></a>Computereigenschaften

Die Serverbewertung überprüft die folgenden Eigenschaften des lokalen virtuellen Computers, um zu ermitteln, ob dieser in Azure ausgeführt werden kann.

**Eigenschaft** | **Details** | **Azure-Bereitschaftsstatus**
--- | --- | ---
**Starttyp** | Azure unterstützt virtuelle Computer mit dem Starttyp BIOS, jedoch nicht UEFI. | Bedingt bereit, wenn der Starttyp UEFI ist.
**Kerne** | Die Anzahl der Kerne in den Computern darf maximal der Anzahl der für einen virtuellen Azure-Computer unterstützten Kerne (128) entsprechen.<br/><br/> Wenn der Leistungsverlauf verfügbar ist, berücksichtigt Azure Migrate die genutzten Kerne beim Vergleich. Wenn in den Bewertungseinstellungen ein Komfortfaktor festgelegt ist, wird die Anzahl der genutzten Kerne mit dem Komfortfaktor multipliziert.<br/><br/> Wenn kein Leistungsverlauf vorhanden ist, verwendet Azure Migrate die zugeordneten Kerne ohne Anwendung des Komfortfaktors. | Bereit, wenn kleiner als die Grenzwerte oder gleich diesen.
**Memory** | Die Größe des Computerarbeitsspeichers darf maximal dem zulässigen Arbeitsspeicher (3.892 GB bei Azure M-Serie Standard_M128m&nbsp;<sup>2</sup>) für einen virtuellen Azure-Computer entsprechen. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)<br/><br/> Wenn der Leistungsverlauf verfügbar ist, berücksichtigt Azure Migrate den genutzten Arbeitsspeicher beim Vergleich. Wenn ein Komfortfaktor festgelegt ist, wird der genutzte Arbeitsspeicher mit dem Komfortfaktor multipliziert.<br/><br/> Wenn kein Verlauf vorhanden ist, wird der zugeordnete Arbeitsspeicher ohne Anwendung des Komfortfaktors verwendet.<br/><br/> | Bereit, wenn innerhalb der Grenzen.
**Speicherdatenträger** | Die zugeteilte Größe eines Datenträgers darf höchstens 32 TB betragen. Obwohl in Azure 64-TB-Datenträger mit SSD Ultra-Datenträgern unterstützt werden, überprüft die Serverbewertung von Azure Migrate derzeit die Datenträgergröße auf Überschreitung von 32 TB, da sie SSD Ultra noch nicht unterstützt. <br/><br/> An den Computer dürfen einschließlich des Betriebssystemdatenträgers höchstens 65 Datenträger angefügt sein. | Bereit, wenn innerhalb der Grenzen.
**Netzwerk** | An einen Computer dürfen maximal 32 Netzwerkschnittstellen (NICs) angefügt sein. | Bereit, wenn innerhalb der Grenzen.

### <a name="guest-operating-system"></a>Gastbetriebssystem
Neben den VM-Eigenschaften untersucht die Serverbewertung auch das Gastbetriebssystem der Computer, um zu ermitteln, ob sie auf Azure ausgeführt werden können.

> [!NOTE]
> Für VMware-VMs verwendet die Serverbewertung das für die VM in vCenter Server angegebene Betriebssystem, um die Gastbetriebssystemanalyse durchzuführen. Für Linux-VMs, die unter VMware ausgeführt werden, wird die Kernelversion des Gastbetriebssystems derzeit nicht genau identifiziert.

Zur Ermittlung der Azure-Bereitschaft basierend auf dem Betriebssystem wird von der Serverbewertung folgende Logik verwendet.

**Betriebssystem** | **Details** | **Azure-Bereitschaftsstatus**
--- | --- | ---
Windows Server 2016 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2012 R2 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2012 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2008 R2 mit allen SPs | Azure bietet vollständige Unterstützung.| Bereit für Azure
Windows Server 2008 (32-Bit und 64-Bit) | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2003, 2003 R2 | Der Unterstützungszeitraum für diese Betriebssysteme ist abgelaufen. Für die Unterstützung in Azure wird eine benutzerdefinierte Supportvereinbarung ([Custom Support Agreement, CSA](https://aka.ms/WSosstatement)) benötigt. | Bedingt bereit für Azure. Ziehen Sie vor der Migration zu Azure ein Upgrade des Betriebssystems in Erwägung.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Diese Betriebssysteme haben das Ende des Supports überschritten. Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. | Bedingt bereit für Azure. Es wird empfohlen, vor der Migration zu Azure ein Upgrade des Betriebssystems auszuführen.
Windows Client 7, 8 und 10 | Azure bietet [nur mit Visual Studio-Abonnement](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) Unterstützung. | Bedingt bereit für Azure
Windows 10 Pro Desktop | Azure bietet Unterstützung mit [mehrinstanzenfähigen Hostingrechten](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment). | Bedingt bereit für Azure
Windows Vista, XP Professional | Diese Betriebssysteme haben das Ende des Supports überschritten. Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. | Bedingt bereit für Azure. Es wird empfohlen, vor der Migration zu Azure ein Upgrade des Betriebssystems auszuführen.
Linux | Azure empfiehlt diese [Linux-Betriebssysteme](../virtual-machines/linux/endorsed-distros.md). Andere Linux-Betriebssysteme können in Azure gestartet werden. Es empfiehlt sich jedoch ein Upgrade des jeweiligen Betriebssystems auf eine unterstützte Version vor der Migration zu Azure. | Bereit für Azure, wenn die Version unterstützt wird.<br/><br/>Bedingt bereit, wenn die Version nicht unterstützt wird.
Andere Betriebssysteme<br/><br/> Beispielsweise Oracle Solaris, Apple Mac OS, FreeBSD usw. | Azure unterstützt diese Betriebssysteme nicht. Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. | Bedingt bereit für Azure. Es wird empfohlen, vor der Migration zu Azure ein unterstütztes Betriebssystem zu installieren.  
In vCenter Server als **Sonstige** angegebenes Betriebssystem | In diesem Fall kann Azure Migrate das Betriebssystem nicht identifizieren. | Bereitschaft unbekannt. Stellen Sie sicher, dass das auf dem virtuellen Computer ausgeführte Betriebssystem in Azure unterstützt wird.
32-Bit-Betriebssysteme | Der Computer kann in Azure gestartet werden, Azure bietet jedoch möglicherweise keine vollständige Unterstützung. | Bedingt bereit für Azure. Ziehen Sie vor der Migration zu Azure ein Upgrade des Computerbetriebssystems von 32 Bit auf 64 Bit in Betracht.

## <a name="calculate-sizing-as-is-on-premises"></a>Berechnen der Größe (wie in der lokalen Umgebung)

Nachdem ein Computer als bereit für Azure markiert wurde, werden bei der Serverbewertung Größenempfehlungen zur Identifizierung des virtuellen Azure-Computers und der Festplatten-SKU angegeben. Wenn Sie die lokale Größe übernehmen, wird der Leistungsverlauf der virtuellen Computer und Datenträger bei der Serverbewertung nicht berücksichtigt.

**Computegröße**: Eine Azure-VM-SKU wird basierend auf der lokal zugeordneten Größe zugewiesen.
**Speicher-/Datenträgergröße:** Bei der Serverbewertung wird der Speichertyp berücksichtigt, der in den Bewertungseigenschaften angegeben wurde (HDD Standard, SSD Standard oder Premium), und ein entsprechender Datenträgertyp empfohlen. Als Standardspeichertyp werden Premium-Datenträger verwendet.
**Netzwerkgröße**: Bei der Serverbewertung wird der Netzwerkadapter auf dem lokalen Computer berücksichtigt.


## <a name="calculate-sizing-performance-based"></a>Berechnen der Größe (leistungsbasierte Anpassung)

Nachdem ein Computer als bereit für Azure markiert wurde, werden bei Verwendung der leistungsbasierten Größenanpassung bei der Serverbewertung wie folgt Größenempfehlungen angegeben:

- Bei der Serverbewertung wird der Leistungsverlauf des Computers berücksichtigt, um die VM-Größe und den Datenträgertyp in Azure zu ermitteln.
- Wenn Server mithilfe einer CSV-Datei importiert wurden, werden die von Ihnen angegebenen Werte verwendet. Diese Methode ist besonders hilfreich, wenn Sie den lokalen Computer überbelegt haben, die Auslastung jedoch gering ist, und Sie die Größe des virtuellen Computers in Azure optimal anpassen möchten, um Kosten zu sparen. 
- Wenn Sie die Leistungsdaten nicht verwenden möchten, setzen Sie die Größenkriterien auf die lokale Größe zurück (siehe vorheriger Abschnitt).

### <a name="calculate-storage-sizing"></a>Berechnen der Speichergröße

In Bezug auf die Speichergröße versucht Azure Migrate wie folgt, jeden an den Computer angefügten Datenträger einem Datenträger in Azure zuzuordnen:

1. Die Serverbewertung addiert die Lese- und Schreib-IOPS eines Datenträgers, um die gesamten benötigten IOPS zu ermitteln. Ebenso werden die Werte für Lese- und Schreibdurchsatz addiert, um den gesamten Durchsatz jedes Datenträgers zu erhalten.
2. Wenn Sie als Speichertyp basierend auf den effektiven IOPS- und Durchsatzwerten „Automatisch“ angegeben haben, ermittelt die Serverbewertung dann, ob der Datenträger auf einem HDD Standard-Datenträger, einem SSD Standard-Datenträger oder einem Premium-Datenträger in Azure abgebildet werden soll. Wenn als Speichertyp HDD Standard-/SSD Standard-/Premium-Datenträger festgelegt wurde, wird versucht, eine Datenträger-SKU innerhalb des ausgewählten Speichertyps (HDD Standard-/SSD Standard-/Premium-Datenträger) zu finden.
3. Die Datenträger werden wie folgt ausgewählt:
    - Wenn die Serverbewertung keinen Datenträger mit den erforderlichen IOPS- und Durchsatzwerten finden kann, wird der Computer als ungeeignet für Azure gekennzeichnet.
    - Wenn die Serverbewertung mehrere geeignete Datenträger findet, wählt sie diejenigen aus, die den in den Bewertungseinstellungen angegebenen Speicherort unterstützen.
    - Falls mehrere geeignete Datenträger vorhanden sind, wählt die Serverbewertung den mit den geringsten Kosten aus.
    - Wenn für eine Festplatte keine Leistungsdaten verfügbar sind, werden die Konfigurationsdaten des Datenträgers (Datenträgergröße) verwendet, um einen SSD Standard-Datenträger in Azure zu finden.

### <a name="calculate-network-sizing"></a>Berechnen der Netzwerkgröße

Die Serverbewertung versucht, einen virtuellen Azure-Computer zu finden, der die Anzahl der an den lokalen Computer angefügten Netzwerkadapter und die für diese Netzwerkadapter erforderliche Leistung unterstützen kann.
- Um die effektive Netzwerkleistung des lokalen virtuellen Computers zu ermitteln, aggregiert die Azure Migrate-Serverbewertung die vom Computer (Netzwerk ausgehend) pro Sekunde übertragenen Daten (Mbit/s) für alle Netzwerkadapter und wendet den Komfortfaktor an. Mithilfe dieser Zahl wird ein virtueller Azure-Computer gesucht, der die erforderliche Netzwerkleistung unterstützen kann.
- Neben der Netzwerkleistung prüft die Serverbewertung zudem, ob der virtuelle Azure-Computer die erforderliche Anzahl der Netzwerkadapter unterstützen kann.
- Wenn keine Netzwerkleistungsdaten verfügbar sind, berücksichtigt die Serverbewertung bei der Größenanpassung für virtuelle Computer nur die Anzahl der Netzwerkadapter.


### <a name="calculate-compute-sizing"></a>Berechnen der Computegröße

Nach der Berechnung der Speicher- und Netzwerkanforderungen prüft die Serverbewertung die CPU- und Arbeitsspeicheranforderungen, um eine geeignete VM-Größe in Azure zu suchen.
- Azure Migrate betrachtet die effektiv genutzten Kerne und den Speicher, um eine geeignete VM-Größe in Azure zu finden.
- Wenn keine geeignete Größe gefunden wird, wird der Computer als ungeeignet für Azure gekennzeichnet.
- Wenn eine geeignete Größe gefunden wird, wendet Azure Migrate die Speicher -und Netzwerkberechnungen an. Für die endgültige VM-Größenempfehlung werden dann noch Standort- und Tarifeinstellungen angewandt.
- Wenn mehrere geeignete Azure-VM-Größen vorhanden sind, wird die mit den geringsten Kosten empfohlen.


### <a name="calculate-confidence-ratings"></a>Berechnen der Zuverlässigkeitsstufen

Jeder leistungsbasierten Bewertung in Azure Migrate wird eine Zuverlässigkeitsstufe zugeordnet, die zwischen ein (niedrigster Wert) bis fünf Sterne (höchster Wert) betragen kann.
- Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind.
- Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe gilt nicht für Bewertungen vom Typ *Wie lokal*.
- Die Serverbewertung benötigt für die leistungsbasierte Dimensionierung die folgenden Informationen:
    - Die Nutzungsdaten für CPU und VM-Arbeitsspeicher
    - IOPS- und Durchsatzdaten für jeden an den virtuellen Computer angefügten Datenträger
    - Informationen zur Netzwerk-E/A für die leistungsbasierte Größenanpassung für jeden Netzwerkadapter, der an einen virtuellen Computer angefügt ist

   Steht eine dieser Nutzungsangaben in vCenter Server nicht zur Verfügung, ist die Größenempfehlung unter Umständen nicht zuverlässig.

Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent).

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0–20 % | 1 Stern
   21–40 % | 2 Sterne
   41–60 % | 3 Sterne
   61–80 % | 4 Sterne
   81–100 % | 5 Sterne

> [!NOTE]
> Bewertungen von Servern, die über eine CSV-Datei in Azure Migrate importiert wurden, werden keine Zuverlässigkeitsstufen zugewiesen. 

#### <a name="low-confidence-ratings"></a>Niedrige Zuverlässigkeitsstufen

Einige mögliche Gründe für eine niedrige Zuverlässigkeitsstufe einer Bewertung:

- Sie haben für den Zeitraum, für den Sie die Bewertung erstellen, kein Profil der Umgebung erstellt. Wenn Sie z. B. die Bewertung mit einer auf 1 Tag festgelegten Leistungsdauer erstellen, müssen Sie bis mindestens einen Tag nach dem Start der Ermittlung warten, bis alle Datenpunkte gesammelt sind.
- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, heruntergefahren. Wenn einige VMs für eine gewisse Zeit heruntergefahren wurden, kann die Serverbewertung für diesen Zeitraum keine Leistungsdaten sammeln.
- Einige virtuelle Computer wurden während des Zeitraums erstellt, für den die Bewertung berechnet wird. Wenn Sie beispielsweise eine Bewertung für den Leistungsverlauf des letzten Monats erstellen, aber einige VMs erst vor einer Woche in der Umgebung erstellt wurden, dann ist der Leistungsverlauf der neuen VMs nicht für die gesamte Dauer abrufbar.

> [!NOTE]
> Bei einer Zuverlässigkeitsstufe von weniger als fünf Sternen wird empfohlen, mindestens einen Tag zu warten, damit die Appliance ein Profil der Umgebung erstellen kann. Führen Sie dann eine Neuberechnung der Bewertung durch. Wenn Sie dies nicht tun, ist die leistungsbasierte Dimensionierung möglicherweise nicht zuverlässig. In diesem Fall wird empfohlen, die Bewertung auf die Größenanpassung „Wie lokal“ zu ändern.

## <a name="calculate-monthly-costs"></a>Berechnen der monatlichen Kosten

Nachdem die Größenempfehlungen ermittelt wurden, berechnet Azure Migrate die Compute- und Speicherkosten nach der Migration.

- **Computekosten**: Azure Migrate berechnet anhand der empfohlenen Azure-VM-Größe mithilfe der Abrechnungs-API die monatlichen Kosten für den virtuellen Computer.
    - Bei der Berechnung werden Betriebssystem, Software Assurance, reservierte Instanzen, VM-Betriebszeit, Standort und Währungseinstellungen berücksichtigt.
    - Die Kosten aller Computer werden zusammengefasst, um die monatlichen Gesamtcomputekosten zu berechnen.
- **Speicherkosten**: Die monatlichen Speicherkosten für einen Computer werden berechnet, indem die monatlichen Kosten aller an den Computer angefügten Datenträger wie folgt zusammengefasst werden:
    - Die Serverbewertung berechnet die gesamten monatlichen Speicherkosten durch Aggregieren der Speicherkosten aller Computer.
    - Derzeit werden bei der Berechnung keine Angebote berücksichtigt, die in den Bewertungseinstellungen angegeben sind.

Kosten werden in der Währung angezeigt, die in den Bewertungseinstellungen festgelegt ist.


## <a name="next-steps"></a>Nächste Schritte

[Bewährte Methoden für die Erstellung von Bewertungen](best-practices-assessment.md) 

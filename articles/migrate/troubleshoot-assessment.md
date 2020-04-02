---
title: Behandeln von Problemen bei der Bewertungs- und Abhängigkeitsvisualisierung in Azure Migrate
description: Erhalten Sie Hilfe beim Behandeln von Problemen bei der Bewertungs- und Abhängigkeitsvisualisierung in Azure Migrate.
ms.service: azure-migrate
ms.topic: troubleshooting
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.date: 01/02/2020
ms.openlocfilehash: 205b52201edb849abab02809b58ff9dc77a32a29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127680"
---
# <a name="troubleshoot-assessmentdependency-visualization"></a>Behandeln von Problemen bei der Bewertung- und Abhängigkeitsvisualisierung

Dieser Artikel hilft Ihnen bei der Problembehandlung bei der Bewertungs- und Abhängigkeitsvisualisierung mit [Azure Migrate: Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool) aus.


## <a name="assessment-readiness-issues"></a>Probleme mit der Bewertungsbereitschaft

Beheben Sie Probleme mit der Bewertungsbereitschaft wie folgt:

**Problem** | **Behebung**
--- | ---
Nicht unterstützter Starttyp | Azure unterstützt keine virtuellen Computer mit dem Starttyp „EFI“. Wir empfehlen, dass Sie vor einer Migration den Starttyp in „BIOS“ ändern. <br/><br/>Sie können die Azure Migrate-Servermigration verwenden, um die Migration solcher virtuellen Computer zu verarbeiten. Sie ändert den Starttyp des virtuellen Computers im Rahmen der Migration in „BIOS“.
Bedingt unterstütztes Windows-Betriebssystem | Der Unterstützungszeitraum für das Betriebssystem ist abgelaufen. Für die [Unterstützung in Azure](https://aka.ms/WSosstatement) wird eine benutzerdefinierte Supportvereinbarung (Custom Support Agreement, CSA) benötigt. Ziehen Sie vor der Migration zu Azure ein Upgrade in Erwägung.
Nicht unterstütztes Windows-Betriebssystem | Azure unterstützt nur [ausgewählte Windows-Betriebssystemversionen](https://aka.ms/WSosstatement). Ziehen Sie vor der Migration zu Azure ein Upgrade des Computers in Erwägung.
Bedingt unterstütztes Linux-Betriebssystem | Azure unterstützt nur [ausgewählte Linux-Betriebssystemversionen](../virtual-machines/linux/endorsed-distros.md). Ziehen Sie vor der Migration zu Azure ein Upgrade des Computers in Erwägung.
Nicht unterstütztes Linux-Betriebssystem | Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. Ziehen Sie vor der Migration zu Azure ein Upgrade auf eine [unterstützte Linux-Version](../virtual-machines/linux/endorsed-distros.md) in Erwägung.
Unbekanntes Betriebssystem | Das Betriebssystem der VM wurde in vCenter Server als „Sonstige“ angegeben. Dieses Verhalten hindert Azure Migrate daran, die Azure-Bereitschaft der VM zu überprüfen. Stellen Sie sicher, dass das Betriebssystem von Azure [unterstützt](https://aka.ms/azureoslist) wird, bevor Sie den Computer migrieren.
Nicht unterstützte Bitversion | Virtuelle Computer mit einem 32-Bit-Betriebssystem starten zwar unter Umständen in Azure, es empfiehlt sich jedoch, vor der Migration zu Azure ein Upgrade auf 64-Bit durchzuführen.
Erfordert ein Microsoft Visual Studio-Abonnement. | Auf dem Computer wird ein Windows-Clientbetriebssystem ausgeführt, das nur mit einem Visual Studio-Abonnement unterstützt wird.
Kein virtueller Computer für erforderliche Speicherleistung gefunden. | Die erforderliche Speicherleistung (Eingabe/Ausgabe-Vorgänge pro Sekunde (IOPS) und Durchsatz) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie vor der Migration die Speicheranforderungen für den Computer.
Kein virtueller Computer für erforderliche Netzwerkleistung gefunden. | Die erforderliche Netzwerkleistung (ein-/ausgehend) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie die Netzwerkanforderungen für den Computer.
Kein virtueller Computer am angegebenen Speicherort gefunden. | Geben Sie vor der Migration einen anderen Zielort an.
Mindestens ein Datenträger ist ungeeignet. | Mindestens ein an den virtuellen Computer angeschlossener Datenträger erfüllt die Azure-Anforderungen nicht.<br/><br/> Von der Azure Die Serverbewertung unterstützt derzeit keine SSD Ultra-Datenträger und bewertet die Datenträger anhand der Datenträgergrenzwerte für verwaltete Premium-Datenträger (32 TB).<br/><br/> Stellen Sie für jeden an den virtuellen Computer angeschlossenen Datenträger sicher, dass die Größe des Datenträgers kleiner als 64 TB (von SSD Ultra-Datenträgern unterstützt) ist.<br/><br/> Wenn dies nicht der Fall ist, reduzieren Sie die Datenträgergröße vor der Migration zu Azure, oder verwenden Sie in Azure mehrere Datenträger, und [kombinieren Sie sie in Stripesets](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping), um höhere Speichergrenzwerte zu erzielen. Stellen Sie sicher, dass die von jedem Datenträger benötigte Leistung (IOPS und Durchsatz) von [verwalteten Azure-Datenträgern virtueller Computer](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#storage-limits) unterstützt wird.
Mindestens ein Netzwerkadapter ist ungeeignet. | Entfernen Sie nicht verwendete Netzwerkadapter vor der Migration vom Computer.
Anzahl der Datenträger überschreitet den Grenzwert | Entfernen Sie nicht verwendete Datenträger vor der Migration vom Computer.
Datenträgergröße überschreitet den Grenzwert | Von der Azure Die Serverbewertung unterstützt derzeit keine SSD Ultra-Datenträger und bewertet die Datenträger anhand der Grenzwerte für Premium-Datenträger (32 TB).<br/><br/> Azure unterstützt jedoch Datenträger mit einer Größe von bis zu 64 TB (von SSD Ultra-Datenträgern unterstützt). Reduzieren Sie die Datenträgergröße vor der Migration auf weniger als 64 TB, oder verwenden Sie mehrere Datenträger in Azure, und [kombinieren Sie sie in Stripesets](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping), um höhere Speichergrenzwerte zu erzielen.
Datenträger am angegebenen Speicherort nicht verfügbar | Stellen Sie vor der Migration sicher, dass sich der Datenträger am Zielspeicherort befindet.
Datenträger für die angegebene Redundanz nicht verfügbar | Der Datenträger muss den in den Bewertungseinstellungen definierten Redundanzspeichertyp verwenden (standardmäßig LRS).
Datenträgereignung konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Kein virtueller Computer gefunden, der die Kern- und Arbeitsspeicheranforderungen erfüllt | Azure konnte keinen geeigneten VM-Typ finden. Reduzieren Sie vor der Migration den Arbeitsspeicher und die Anzahl der Kerne auf dem lokalen Computer.
Eignung des virtuellen Computers konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Datenträger aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Netzwerkadapter aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.

## <a name="linux-vms-are-conditionally-ready"></a>Virtuelle Linux-Computer sind „bedingt bereit“

Bei der Serverbewertung werden virtuelle Linux-Computer aufgrund einer bekannten Lücke in der Serverbewertung als „bedingt bereit“ gekennzeichnet.

- Die Lücke verhindert die Erkennung der Nebenversion des Linux-Betriebssystems, das auf den lokalen virtuellen Computern installiert ist.
- Beispielsweise erkennt die Serverbewertung bei RHEL 6.10 derzeit nur RHEL 6 als Betriebssystemversion.
-  Da Azure nur bestimmte Versionen von Linux unterstützt, werden die virtuellen Linux-Computer in der Serverbewertung derzeit als „bedingt bereit“ markiert.
- Sie können ermitteln, ob das Linux-Betriebssystem des lokalen virtuellen Computers für Azure geeignet ist. Informationen hierzu finden Sie unter [Linux-Unterstützung von Azure](https://aka.ms/migrate/selfhost/azureendorseddistros).
-  Nachdem Sie die unterstützte Distribution überprüft haben, können Sie diese Warnung ignorieren.

## <a name="azure-skus-bigger-than-on-premises"></a>Azure-SKUs größer als lokal

Je nach Art der Bewertung empfiehlt die Azure Migrate-Serverbewertung möglicherweise Azure-VM-SKUs mit mehr Kernen und Arbeitsspeicher als bei der aktuellen lokalen Zuordnung:


- Die Empfehlung für VM-SKUs richtet sich nach den Bewertungseigenschaften.
- Dies hängt von der Art der Bewertung ab, die Sie in der Serverbewertung ausführen: *Leistungsbasiert* oder *Wie lokal*.
- Bei leistungsbasierten Bewertungen berücksichtigt die Serverbewertung die Nutzungsdaten der lokalen virtuellen Computer (CPU-, Speicher-, Datenträger- und Netzwerkauslastung), um die richtige Ziel-VM-SKU für Ihre lokalen virtuellen Computer zu bestimmen. Außerdem wird bei der Bestimmung der effektiven Auslastung ein Komfortfaktor hinzugefügt.
- Bei der lokalen Dimensionierung werden Leistungsdaten nicht berücksichtigt, und die Ziel-SKU wird auf der Grundlage der lokalen Zuordnung empfohlen.

Um zu veranschaulichen, wie sich dies auf Empfehlungen auswirken kann, sehen wir uns ein Beispiel an:

Wir haben einen lokalen virtuellen Computer mit vier Kernen und 8 GB Arbeitsspeicher, mit einer CPU- und Speicherauslastung von jeweils 50 % und einem angegebenen Komfortfaktor von 1,3.

-  Beim Bewertungstyp **Wie lokal** wird eine Azure-VM-SKU mit vier Kernen und 8 GB Arbeitsspeicher empfohlen.
- Bei der leistungsbasierten Bewertung würde auf Basis der effektiven CPU- und Speicherauslastung (50 % von 4 Kernen * 1,3 = 2,6 Kerne und 50 % von 8 GB Arbeitsspeicher * 1,3 = 5,3 GB Arbeitsspeicher) die günstigste VM-SKU mit vier Kernen (nächste unterstützte Kernanzahl) und 8 GB Arbeitsspeichergröße (nächste unterstützte Arbeitsspeichergröße) empfohlen.
- Weitere Informationen zur Dimensionierung bei Bewertungen finden Sie [hier](concepts-assessment-calculation.md#types-of-assessments).

## <a name="azure-disk-skus-bigger-than-on-premises"></a>Azure-Datenträger-SKUs größer als lokal

Die Azure Migrate-Serverbewertung empfiehlt möglicherweise je nach Art der Bewertung einen größeren Datenträger.
- Die Datenträgerdimensionierung in der Serverbewertung hängt von zwei Bewertungseigenschaften ab: dem Dimensionierungskriterium und dem Speichertyp.
- Wenn das Dimensionierungskriterium **Leistungsbasiert** ist und der Speichertyp **Automatisch** lautet, werden bei der Ermittlung des Zieldatenträgertyps (HDD Standard, SSD Standard oder Premium) die IOPS- und Durchsatzwerte des Datenträgers berücksichtigt. Eine Datenträger-SKU dieses Datenträgertyps wird dann unter Berücksichtigung der Größenanforderungen des lokalen Datenträgers empfohlen.
- Wenn das Dimensionierungskriterium **Leistungsbasiert** ist und der Speichertyp **Premium** lautet, wird eine Premium-Datenträger-SKU in Azure basierend auf den IOPS, dem Durchsatz und den Größenanforderungen des lokalen Datenträgers empfohlen. Die gleiche Logik wird verwendet, um die Datenträgerdimensionierung durchzuführen, wenn das Dimensionierungskriterium **Wie lokal** verwendet wird und der Speichertyp **HDD Standard**, **SSD Standard** oder **Premium** lautet.

Wenn Sie beispielsweise einen lokalen Datenträger mit 32 GB Speicher haben, die aggregierten Lese- und Schreib-IOPS für den Datenträger aber 800 IOPS sind, empfiehlt die Serverbewertung einen Premium-Datenträger (aufgrund der höheren IOPS-Anforderungen) und dann eine Datenträger-SKU, welche die erforderlichen IOPS und die Größe unterstützen kann. Die nächstliegende Übereinstimmung in diesem Beispiel wäre P15 (256 GB, 1100 IOPS). Für den lokalen Datenträger ist also nur eine Größe von 32 GB erforderlich, von der Serverbewertung wurde jedoch aufgrund der hohen IOPS-Anforderungen des lokalen Datenträgers ein größerer Datenträger empfohlen.

## <a name="utilized-corememory-percentage-missing"></a>Prozentsatz der Kern-/Arbeitsspeicherauslastung fehlt

Die Serverbewertung gibt die Meldung „PercentageOfCoresUtilizedMissing“ oder „PercentageOfMemoryUtilizedMissing“ aus, wenn die Azure Migrate-Appliance keine Leistungsdaten für den relevanten lokalen virtuellen Computer sammeln kann.

- Dies kann vorkommen, wenn die virtuellen Computer während der Bewertung ausgeschaltet sind. Die Appliance kann keine Leistungsdaten für eine VM erfassen, wenn sie ausgeschaltet ist.
- Falls nur Arbeitsspeicher-Leistungsindikatoren fehlen und Sie versuchen, virtuelle Hyper-V-Computer zu bewerten, überprüfen Sie, ob auf diesen virtuellen Computern dynamischer Arbeitsspeicher aktiviert ist. Aufgrund eines bekannten Problems, das nur bei virtuellen Hyper-V-Computern auftritt, kann die Azure Migrate-Appliance keine Arbeitsspeichernutzungsdaten für virtuelle Computer sammeln, für die kein dynamischer Arbeitsspeicher aktiviert ist.
- Wenn einer der Leistungsindikatoren fehlt, greift die Azure Migrate-Serverbewertung auf die zugewiesenen Kerne und den Arbeitsspeicher zurück und empfiehlt eine entsprechende VM-Größe.
- Wenn alle Leistungsindikatoren fehlen, stellen Sie sicher, dass die Portzugriffsanforderungen für die Bewertung erfüllt sind. Erfahren Sie mehr über die Portzugriffsanforderungen für [VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#port-access)-, [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#port-access)- und [physische](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-physical#port-access) Serverbewertung.

## <a name="is-the-operating-system-license-included"></a>Ist die Betriebssystemlizenz inbegriffen?

Die Azure Migrate-Serverbewertung berücksichtigt zurzeit die Kosten einer Betriebssystemlizenz nur für Windows-Computer. Die Lizenzkosten für Linux-Computer werden derzeit nicht berücksichtigt.

## <a name="how-does-performance-based-sizing-work"></a>Wie funktioniert die leistungsbezogene Größenanpassung?

Die Serverbewertung sammelt fortlaufend Leistungsdaten von lokalen Computern und nutzt diese Daten, um die VM-SKU und die Datenträger-SKU in Azure zu empfehlen. [Erfahren Sie](concepts-assessment-calculation.md#calculate-sizing-performance-based), wie leistungsbasierten Daten gesammelt werden.


## <a name="dependency-visualization-in-azure-government"></a>Abhängigkeitsvisualisierung in Azure Government

Azure Migrate ist bei der Funktion zur Visualisierung von Abhängigkeiten von Service Map abhängig. Da Service Map in Azure Government zurzeit nicht verfügbar ist, ist diese Funktionalität auch nicht in Azure Government verfügbar.

## <a name="dependencies-dont-show-after-agent-install"></a>Abhängigkeiten werden nicht nach der Installation von Agents angezeigt

Nachdem Sie die Agents für die Abhängigkeitsvisualisierung auf lokalen virtuellen Computern installiert haben, benötigt Azure Migrate üblicherweise 15 bis 30 Minuten, um die Abhängigkeiten im Portal anzuzeigen. Stellen Sie nach einer Wartezeit von mehr als 30 Minuten sicher, dass Microsoft Monitoring Agent (MMA) eine Verbindung zum Log Analytics-Arbeitsbereich herstellen kann.

Für virtuelle Windows-Computer:
1. Starten Sie MMA in der Systemsteuerung.
2. Stellen Sie unter **Microsoft Monitoring Agent-Eigenschaften** > **Azure Log Analytics (OMS)** sicher, dass der **Status** für den Arbeitsbereich grün angezeigt wird.
3. Wenn der Status nicht grün angezeigt wird, entfernen Sie den Arbeitsbereich, und fügen Sie ihn erneut zu MMA hinzu.

    ![MMA-Status](./media/troubleshoot-assessment/mma-properties.png)

Stellen Sie bei virtuellen Linux-Computern sicher, dass die Installationsbefehle für MMA und den Abhängigkeits-Agent erfolgreich ausgeführt wurden.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- **MMS-Agent**: Überprüfen Sie die unterstützten [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)- und [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)-Betriebssysteme.
- **Dependency-Agent**: die unterstützten [Windows- und Linux](../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems)-Betriebssysteme.

## <a name="visualize-dependencies-for--hour"></a>Visualisieren von Abhängigkeiten für > Stunde

Mit Azure Migrate können Sie zwar um bis zu einen Monat zu einem bestimmten Datum zurückgehen. Die Abhängigkeiten können Sie jedoch nur für einen Zeitraum von maximal einer Stunde visualisieren.

So können Sie beispielsweise mithilfe der Zeitraumfunktionalität im Abhängigkeitsdiagramm Abhängigkeiten für den gestrigen Tag, jedoch nur für einen Zeitraum von einer Stunde, anzeigen.

Sie können jedoch zum [Abfragen der Abhängigkeitsdaten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) über einen längeren Zeitraum Azure Monitor-Protokolle verwenden.

## <a name="visualized-dependencies-for--10-machines"></a>Visualisieren von Abhängigkeiten für > 10 Computer

Bei der Azure Migrate-Serverbewertung können Sie [Abhängigkeiten für Gruppen](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) mit bis zu 10 virtuellen Computern visualisieren. Bei größeren Gruppen empfiehlt es sich für die visuelle Darstellung von Abhängigkeiten, die virtuellen Computer in kleinere Gruppen aufzuteilen.

## <a name="machines-show-install-agent"></a>Computer zeigen „Agent installieren“ an

Nach dem Migrieren von Computern mit aktivierter Abhängigkeitsvisualisierung zu Azure zeigen Computer möglicherweise aufgrund des folgenden Verhaltens die Aktion „Agent installieren“ anstelle von „Abhängigkeiten anzeigen“ an:


- Nach der Migration zu Azure werden die lokalen Computer ausgeschaltet, und die entsprechenden virtuellen Computer werden in Azure hochgefahren. Diese Computer beziehen eine andere MAC-Adresse.
- Je nachdem, ob Sie die lokale IP-Adresse beibehalten haben oder nicht, haben Computer möglicherweise auch eine andere IP-Adresse.
- Wenn sich sowohl Mac- als auch IP-Adressen von den lokalen Adressen unterscheiden, verknüpft Azure Migrate die lokalen Computer nicht mit Service Map-Abhängigkeitsdaten. In diesem Fall wird die Option zum Installieren des Agents anstatt die Option zum Anzeigen von Abhängigkeiten angezeigt.
- Nach einer Testmigration zu Azure bleiben die lokalen Computer erwartungsgemäß eingeschaltet. Entsprechende Computer, die in Azure hochgefahren wurden, erhalten andere MAC-Adressen und u. U. auch andere IP-Adressen. Sofern Sie ausgehenden Datenverkehr von Azure Monitor-Protokollen von diesen Computern nicht sperren, ordnet Azure Migrate den lokalen Computern keine Service Map-Abhängigkeitsdaten zu und zeigt daher die Option zum Installieren von Agents statt der Option zum Anzeigen von Abhängigkeiten an.


## <a name="capture-network-traffic"></a>Erfassen des Netzwerkdatenverkehrs

Sammeln Sie Protokolle zum Netzwerkdatenverkehr wie folgt:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Drücken Sie F12, um die Entwicklertools zu starten. Deaktivieren Sie ggf. die Einstellung **Einträge beim Navigieren löschen**.
3. Wählen Sie die Registerkarte **Netzwerk** aus, und starten Sie die Erfassung von Netzwerkdatenverkehr:
   - Wählen Sie in Chrome die Option **Protokoll speichern**. Die Aufzeichnung sollte automatisch gestartet werden. Ein roter Kreis gibt an, dass der Datenverkehr erfasst wird. Wenn der rote Kreis nicht angezeigt wird, wählen Sie zum Starten den schwarzen Kreis aus.
   - In Microsoft Edge und Internet Explorer sollte die Aufzeichnung automatisch gestartet werden. Ist dies nicht der Fall, wählen Sie die grüne Wiedergabeschaltfläche aus.
4. Versuchen Sie, den Fehler zu reproduzieren.
5. Nachdem der Fehler während der Aufzeichnung aufgetreten ist, beenden Sie die Aufzeichnung, und speichern Sie eine Kopie der aufgezeichneten Aktivität:
   - Klicken Sie in Chrome mit der rechten Maustaste, und wählen Sie **Als HAR mit Inhalt speichern** aus. Durch diese Aktion werden die Protokolle komprimiert und als HAR-Datei exportiert.
   - Wählen Sie in Microsoft Edge oder Internet Explorer das Symbol **Aufgezeichneten Datenverkehr exportieren** aus. Durch diese Aktion werden die Protokolle komprimiert und exportiert.
6. Wählen Sie die Registerkarte **Konsole** aus, um sie auf Warnungen oder Fehler zu überprüfen. So speichern Sie das Konsolenprotokoll:
   - Klicken Sie in Chrome mit der rechten Maustaste auf eine beliebige Stelle im Konsolenprotokoll. Wählen Sie **Speichern unter** aus, um das Protokoll zu exportieren und zu komprimieren.
   - Klicken Sie in Microsoft Edge oder Internet Explorer mit der rechten Maustaste auf die Fehler, und wählen Sie **Alle kopieren** aus.
7. Schließen Sie die Entwicklertools.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen](how-to-create-assessment.md) Sie eine Bewertung oder [passen](how-to-modify-assessment.md) Sie sie an.

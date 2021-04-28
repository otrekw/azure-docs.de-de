---
title: Monitor Windows Virtual Desktop Kostenvoranschläge - Azure
description: Wie Sie die Kosten und Preise für die Verwendung von Azure Monitor für Windows Virtual Desktop abschätzen können.
author: Heidilohr
ms.topic: conceptual
ms.date: 03/29/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 5bd89a734a20c913bacca1f5531aa76d76418c80
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106448218"
---
# <a name="estimate-azure-monitor-costs"></a>Azure-Monitor-Kosten abschätzen

Azure Monitor Logs ist ein Dienst, der von Ihrer Umgebung erstellte Daten sammelt, indiziert und speichert. Aufgrunddessen basiert das Azure Monitor-Preismodell auf der Datenmenge, die von Ihrem Log Analytics-Arbeitsbereich in Gigabyte pro Tag eingebracht und verarbeitet (oder „aufgenommen") wird. Die Kosten für einen Log Analytics-Arbeitsbereich basieren nicht nur auf dem Volumen der gesammelten Daten, sondern auch darauf, welchen Azure-Zahlungsplan Sie ausgewählt haben und wie lange Sie die Daten, die Ihre Umgebung generiert, speichern möchten.

In diesem Artikel werden die folgenden Punkte erläutert, die Ihnen dabei helfen zu verstehen, wie die Preisgestaltung für Azure Monitor funktioniert:

- Wie Sie die Kosten für die Datenerfassung und -Speicherung im Voraus abschätzen, bevor Sie diese Funktion aktivieren
- Wie Sie Ihre Aufnahme und Speicherung messen und steuern, um die Kosten zu reduzieren, wenn Sie diese Funktion nutzen

>[!NOTE]
> Bei allen in diesem Artikel aufgeführten Größen und Preise sind handelt es sich nur um Beispiele, um zu zeigen, wie die Schätzung funktioniert. Für eine genauere Einschätzung, basierend auf Ihrem Azure Monitor Log Analytics Preismodell und Ihrer Azure-Region, beziehen Sie sich bitte auf die [Azure Monitor Preisgestaltung](https://azure.microsoft.com/pricing/details/monitor/).

## <a name="estimate-data-ingestion-and-storage-costs"></a>Geschätzte Datenaufnahme und Speicherkosten

Wir empfehlen Ihnen, einen vordefinierten Datensatz zu verwenden, die als Protokolle in Ihren Log Analytics-Arbeitsbereich geschrieben wurden. In den folgenden Beispielschätzungen betrachten wir die abrechenbaren Daten in der Standardkonfiguration

Die vordefinierten Datensätze für Azure Monitor für Windows Virtual Desktop umfassen:

- Leistungszähler von den Sitzungshosts
- Windows-Ereignisprotokolle von den Sitzungshosts
- Windows Virtual Desktop Diagnose aus der Service-Infrastruktur

Die Kosten für die Aufnahme und Speicherung von Daten hängen von der Größe, dem Zustand und der Nutzung Ihrer Umgebung ab. Die Beispielschätzungen, die wir in diesem Artikel verwenden, um die zu erwartenden Kostenspannen zu berechnen, basieren auf gesunden virtuellen Maschinen mit geringem Stromverbrauch, basierend auf unseren [Richtlinien für die Dimensionierung virtueller Maschinen](/remote/remote-desktop-services/virtual-machine-recs), um eine Spanne der zu erwartenden Kosten für Datenaufnahme und Speicherung zu berechnen.

Die VM für die leichte Nutzung, die wir in unserem Beispiel verwenden, umfasst die folgenden Komponenten:

- 4 vCPUs, 1 Festplatte
- 16 Sitzungen pro Tag
- Eine durchschnittliche Sitzungsdauer von 2 Stunden (120 Minuten)
- 100 Vorgänge pro Sitzung

Die Hochleistungs-VM, die wir in unserem Beispiel verwenden, umfasst die folgenden Komponenten:

- 6 vCPUs, 1 Festplatte
- 6 Sitzungen pro Tag
- Eine durchschnittliche Sitzungsdauer von 4 Stunden (240 Minuten)
- 200 Vorgänge pro Sitzung

## <a name="estimating-performance-counter-ingestion"></a>Geschätzte Leistungszähleraufnahme

Leistungszähler zeigen an, wie sich die Systemressourcen verhalten. Der Leistungszähler für die Datenaufnahme hängt von der Größe und Nutzung Ihrer Umgebung ab. In den meisten Fällen sollten die Leistungszähler 80 bis 99 % Ihrer Datenaufnahme für Azure Monitor für Windows Virtual Desktop ausmachen.

Bevor Sie mit dem Schätzen beginnen, ist es wichtig, dass Sie verstehen, dass jeder Leistungsindikator Daten mit einer bestimmten Frequenz sendet. Wir stellen eine Standard-Sample-Rate pro Minute ein (Sie können diese Rate auch in den Einstellungen bearbeiten), aber diese Rate wird je nach Zähler mit unterschiedlichen Multiplikationsfaktoren angewandt. Die folgenden Faktoren beeinflussen die Rate:

- Für den Faktor pro virtuelle Maschine (VM) sendet jeder Zähler Daten pro VM in Ihrer Umgebung mit der Standardabtastrate pro Minute, während die VM läuft. Sie können die Anzahl der Datensätze, die diese Zähler pro Tag senden, schätzen, indem Sie die standardmäßige Abtastrate pro Minute mit der Anzahl der VMs in Ihrer Umgebung multiplizieren und diese Zahl dann mit der durchschnittlichen VM-Laufzeit pro Tag multiplizieren.

   Zusammenfassung:

   Standardabtastrate pro Minute × Anzahl der CPU-Kerne in der VM SKU × Anzahl der VMs × durchschnittliche VM-Laufzeit pro Tag = Anzahl der gesendeten Datensätze pro Tag

- Für den Faktor „pro CPU" sendet jeder Zähler mit der standardmäßigen Abtastrate pro Minute pro vCPU in jeder VM in Ihrer Umgebung, während die VM läuft. Sie können die Anzahl der Datensätze, welche die Zähler pro Tag senden, schätzen, indem Sie die Standardabtastrate pro Minute mit der Anzahl der CPU-Kerne in der VM SKU multiplizieren und diese Zahl dann mit der Anzahl der Minuten, während der die VM läuft, und der Anzahl der VMs in Ihrer Umgebung, multiplizieren.

   Zusammenfassung:
   
   Standardabtastrate pro Minute × Anzahl der CPU-Kerne in der VM SKU × Anzahl der Minuten, in denen die VM läuft × Anzahl der VMs = Anzahl der gesendeten Datensätze pro Tag

- Für den Faktor pro Festplatte, sendet jeder Zähler Daten mit der Standard-Abtastrate für jede Festplatte in jeder VM in Ihrer Umgebung. Die Anzahl der Datensätze, die diese Zähler pro Tag senden, entspricht der Standardabtastrate pro Minute multipliziert mit der Anzahl der Festplatten in der VM SKU, multipliziert mit 60 Minuten pro Stunde und schließlich multipliziert mit den durchschnittlichen aktiven Stunden für eine VM.

   Zusammenfassung:

   Standardabtastrate pro Minute × Anzahl der Festplatten in VM SKU × 60 Minuten pro Stunde × Anzahl der VMs × durchschnittliche VM-Laufzeit pro Tag = Anzahl der gesendeten Datensätze pro Tag

- Für den Faktor pro Sitzung sendet jeder Zähler Daten mit der Standardabtastrate für jede Sitzung in Ihrer Umgebung, während die Sitzung verbunden ist. Sie können die Anzahl der Datensätze, die diese Zähler pro Tag senden, schätzen, indem Sie die Standardabtastrate pro Minute mit der durchschnittlichen Anzahl von Sitzungen pro Tag und der durchschnittlichen Sitzungsdauer multiplizieren.

   Zusammenfassung:

   Standardmäßige Stichprobenrate pro Minute × Sitzungen pro Tag × durchschnittliche Sitzungsdauer = Anzahl der pro Tag gesendeten Datensätze

- Für den Faktor „pro Prozess" sendet jeder Zähler Daten mit der Standardrate für jeden Prozess in jeder Sitzung in Ihrer Umgebung. Sie können die Anzahl der Datensätze, die diese Zähler pro Tag senden, schätzen, indem Sie die Standardabtastrate pro Minute mit der durchschnittlichen Anzahl von Sitzungen pro Tag und der durchschnittlichen Sitzungsdauer multiplizieren.
  
   Zusammenfassung:

   Standardmäßige Stichprobenrate pro Minute × Sitzungen pro Tag × durchschnittliche Sitzungsdauer × durchschnittliche Anzahl von Prozessen pro Sitzung = Anzahl der pro Tag gesendeten Datensätze

Die folgende Tabelle listet die 20 Leistungszähler auf, die Azure Monitor for Windows Virtual Desktop sammelt, sowie deren Standardwerte:

| Name des Leistungsindikators | Standard-Abtastrate | Frequenz Faktor |
|--------------|---------------------|------------------|
| Logische Platte(C:) \\% freier Speicherplatz | 60 Sekunden  | Pro Festplatte             |
| Logischer Datenträger (C:)\\Mittlere Warteschlangenlänge des Datenträgers   | 30 Sekunden    | Pro Festplatte             |
| Logischer Datenträger (C:)\\Mittlere Datenträger s/übertragen  | 60 Sekunden       | Pro Festplatte             |
| Logischer Datenträger (C:)\\Aktuelle Warteschlangenlänge  | 30 Sekunden      | Pro Festplatte             |
| Arbeitsspeicher(\*)\\Verfügbare MB | 30 Sekunden    | Pro VM  |
| Arbeitsspeicher(\*)\\Seitenfehler/Sek. | 30 Sekunden   | Pro VM  |
| Arbeitsspeicher (\*)\\Seiten/Sek. | 30 Sekunden   | Pro VM  |
| Arbeitsspeicher(\*)\\Zugesicherte verwendete Bytes (%) | 30 Sekunden    | Pro VM  |
| Physischer Datenträger(\*)\\Durchschnittl. Warteschlangenlänge des Datenträgers | 30 Sekunden      | Pro Festplatte             |
| Physischer Datenträger(\*)\\Durchschnittl. Datenträger s/gelesen | 30 Sekunden  | Pro Festplatte             |
| Physischer Datenträger(\*)\\Durchschnittl. Datenträger s/übertragen | 30 Sekunden  | Pro Festplatte             |
| Physischer Datenträger(\*)\\Durchschnittl. Datenträger s/geschrieben | 30 Sekunden | Pro Festplatte             |
| Prozessorinformationen(_Total)\\%Prozessorzeit | 30 Sekunden | Pro Kern/CPU         |
| Terminaldienste(\*)\\Aktive Sitzungen          | 60 Sekunden | Pro VM  |
| Terminaldienste(\*)\\Inaktive Sitzungen        | 60 Sekunden | Pro VM  |
| Terminaldienste(\*)\\Sitzungen insgesamt | 60 Sekunden | Pro VM  |
| Benutzer-Eingangsverzögerung pro Prozess(\*)\\Maximale Eingangsverzögerung         | 30 Sekunden | Pro Prozess          |
| Benutzereingabeverzögerung pro Sitzung(\*)\\Maximale Eingabeverzögerung        | 30 Sekunden | Pro Sitzung          |
| RemoteFX-Netzwerk (\*)\\Aktuelle TCP-RTT | 30 Sekunden | Pro VM  |
| RemoteFX-Netzwerk (\*)\\Aktuelle UDP-Bandbreite     | 30 Sekunden | Pro VM  |

Wenn wir die Größe jedes Datensatzes auf 200 Byte schätzen, würde eine Beispiel-VM, die eine leichte Arbeitslast mit der Standardabtastrate ausführt, etwa 90 Megabyte an Leistungsindikator-Daten pro Tag pro VM senden. In der Zwischenzeit würde eine Beispiel-VM, die eine Leistungs-Workload ausführt, etwa 130 Megabyte an Leistungszählerdaten pro Tag pro VM senden. Die Größe des Datensatzes und die Nutzung der Umgebung können jedoch variieren, so dass die von Ihrem Einsatz verwendeten Megabytes pro Tag unterschiedlich sein können.

Informationen zu Leistungsindikatoren zur Eingabeverzögerung finden Sie unter [Leistungsindikatoren für Eingabeverzögerung](/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters/).

## <a name="estimating-windows-event-log-ingestion"></a>Abschätzen der Aufnahme von Windows-Ereignisprotokollen

Windows-Ereignisprotokolle sind Datenquellen, die von Log Analytics-Agents auf Windows-VMs gesammelt werden. Sie können Ereignisse sowohl aus Standardprotokollen wie „System“ und „Anwendung“ als auch aus benutzerdefinierten Protokollen sammeln, die von zu überwachenden Anwendungen erstellt wurden.

Dies sind die standardmäßigen Windows-Ereignisse für Azure Monitor für virtuellen Windows-Desktop:

- Application
- Microsoft-Windows-TerminalServices-RemoteConnectionManager/Admin
- Microsoft-Windows-TerminalServices-LocalSessionManager/Operational
- System
- Microsoft-FSLogix-Apps/Operational
- Microsoft-FSLogix-Apps/Admin

Windows-Ereignisse werden immer dann gesendet, wenn die Bedingungen des Ereignisses in der Umgebung erfüllt sind. Maschinen im gesunden Zustand senden weniger Ereignisse als Maschinen im ungesunden Zustand. Da die Anzahl der Ereignisse nicht vorhersehbar ist, verwenden wir für diese Schätzung einen Bereich von 1.000 bis 10.000 Ereignissen pro VM und Tag, basierend auf Beispielen aus gesunden Umgebungen. Wenn wir beispielsweise die Größe jedes Ereignisdatensatzes in diesem Beispiel auf 1.500 Byte schätzen, ergibt dies für die angegebene Umgebung etwa 2 bis 15 Megabyte an Ereignisdaten pro Tag.

Weitere Informationen zu Windows-Ereignissen finden Sie unter [Eigenschaften von Windows-Ereignisdatensätzen](../azure-monitor/agents/data-sources-windows-events.md).

## <a name="estimating-diagnostics-ingestion"></a>Abschätzung der Diagnostik-Aufnahme

Der Diagnosedienst erstellt Aktivitätsprotokolle sowohl für Benutzer- als auch für administrative Aktionen.

Dies sind die Namen der Aktivitätsprotokolle, die der Diagnosezähler verfolgt:

- WVDCheckpoints
- WVDConnections
- WVDErrors
- WVDFeeds
- WVDManagement
- WVDAgentHealthStatus

Der Dienst sendet Diagnoseinformationen immer dann, wenn die Umgebung den zum Erstellen eines Datensatzes erforderlichen Bedingungen entspricht. Da die Anzahl der Ereignisse nicht vorhersehbar ist, verwenden wir für diese Schätzung einen Bereich von 500 bis 1000 Ereignissen pro VM und Tag, basierend auf Beispielen aus gesunden Umgebungen.

Wenn wir beispielsweise schätzen, dass die Größe der einzelnen Diagnosedaten Sätze in diesem Beispiel 200 Bytes beträgt, beträgt die Gesamtzahl der erfassten Daten weniger als 1 MB pro VM pro Tag.

Weitere Informationen zu den Kategorien von Aktivitäts-Protokollen finden Sie unter [Windows Virtual Desktop Diagnostics](diagnostics-log-analytics.md).

## <a name="estimating-total-costs"></a>Schätzung der Gesamtkosten

Abschließend werden die Gesamtkosten geschätzt. In diesem Beispiel nehmen wir an, dass wir auf der Grundlage der Beispielwerte in den vorherigen Abschnitten zu den folgenden Ergebnissen kommen:

| Datenquelle        | Größenschätzung pro Tag (in Megabyte)   |
|-------------------------------------|------------------------------------------|
| Leistungsindikatoren   | 90-130 |
| Events    | 2 - 15 |
| Windows Virtual Desktop Diagnostik | \< 1 |

In diesem Beispiel liegt die gesamte Datenaufnahme für Azure Monitor for Windows Virtual Desktop zwischen 92 und 145 Megabyte pro VM pro Tag. Mit anderen Worten: Alle 31 Tage nimmt jede VM etwa 3 bis 5 Gigabyte an Daten auf.

Unter Verwendung des standardmäßigen Pay-as-you-go-Modells für die [Preisgestaltung von Log Analytics](https://azure.microsoft.com/pricing/details/monitor/) können Sie die Kosten für die Datensammlung und Speicherung von Azure Monitor pro Monat schätzen. Abhängig von der Datenerfassung können Sie auch das Kapazitäts-Reservierungs-Modell für Log Analytics Preise in Erwägung gezogen.

## <a name="manage-your-data-ingestion-to-reduce-costs"></a>Verwalten Sie Ihre Datenaufnahme, um Kosten zu senken

In diesem Abschnitt wird erläutert, wie Sie die Datenaufnahme messen und verwalten können, um die Kosten zu senken.

Informationen zur Verwaltung von Rechten und Berechtigungen für die Arbeitsmappe finden Sie unter [Zugriffssteuerung](../azure-monitor/visualize/workbooks-access-control.md).

>[!NOTE]
>Das Entfernen von Datenpunkten hat Auswirkungen auf die entsprechenden Darstellungen in Azure Monitor für Windows Virtual Desktop.

### <a name="log-analytics-settings"></a>Log Analytics-Einstellungen

Hier finden Sie einige Vorschläge zur Optimierung Ihrer Log Analytics-Einstellungen für die Verwaltung der Datenaufnahme:

- Verwenden Sie einen bestimmten Log Analytics-Arbeitsbereich für Ihre Windows Virtual Desktop-Ressourcen, um sicherzustellen, dass Log Analytics nur Leistungszähler und Ereignisse für die virtuellen Maschinen in Ihrer Windows Virtual Desktop-Bereitstellung sammelt.
- Passen Sie Ihre Log Analytics-Speichereinstellungen an, um die Kosten zu verwalten. Sie können den Aufbewahrungszeitraum reduzieren, evaluieren, ob eine feste Speicherpreisstufe kosteneffektiver wäre, oder Grenzen für die Datenmenge festlegen, die Sie aufnehmen können, um die Auswirkungen einer ungesunden Bereitstellung zu begrenzen. Weitere Informationen finden Sie unter [Verwalten von Nutzung und Kosten für Azure Monitor Logs](../azure-monitor/platform/manage-cost-storage.md).

### <a name="remove-excess-data"></a>Überschüssige Daten entfernen

Unsere Standardkonfiguration ist der einzige Datensatz, den wir für Azure Monitor für Windows Virtual Desktop empfehlen. Sie haben jederzeit die Möglichkeit, zusätzliche Datenpunkte hinzuzufügen und sie im Host-Diagnose anzuzeigen: Host-Browser oder benutzerdefinierte Diagramme für sie zu erstellen, allerdings erhöhen sich durch zusätzliche Daten die Kosten für Log Analytics. Diese können zur Kosteneinsparung entfernt werden.

### <a name="measure-and-manage-your-performance-counter-data"></a>Messen und verwalten Sie Ihre Leistungszählerdaten

Ihre tatsächlichen Überwachungskosten hängen von der Größe, der Nutzung und dem Zustand Ihrer Umgebung ab. Wie Sie den Dateneingang in Ihrem Log Analytics-Arbeitsbereich messen können, erfahren Sie unter [Verstehen des Volumens der aufgenommenen Protokolldaten](../azure-monitor/logs/manage-cost-storage.md#understanding-ingested-data-volume).

Die Leistungszähler, die von den Sitzungshosts verwendet werden, sind wahrscheinlich die größte Quelle der aufgenommenen Daten für Azure Monitor für Windows Virtual Desktop. Die folgende benutzerdefinierte Abfragevorlage für einen Log Analytics-Arbeitsbereich kann die Häufigkeit und die pro Leistungszähler aufgenommenen Megabytes während des vergangenen Tages verfolgen:

```azure
let WVDHosts = dynamic(['Host1.MyCompany.com', 'Host2.MyCompany.com']);
Perf
| where TimeGenerated > ago(1d)
| where Computer in (WVDHosts)
| extend PerfCounter = strcat(ObjectName, ":", CounterName)
| summarize Records = count(TimeGenerated), InstanceNames = dcount(InstanceName), Bytes=sum(_BilledSize) by PerfCounter
| extend Billed_MBytes = Bytes / (1024 * 1024), BytesPerRecord = Bytes / Records
| sort by Records desc
```

>[!NOTE]
>Stellen Sie sicher, dass Sie die Platzhalterwerte der Vorlage durch die in Ihrer Umgebung verwendeten Werte ersetzen, sonst funktioniert die Abfrage nicht.

Diese Abfrage zeigt alle Leistungszähler an, die Sie in der Umgebung aktiviert haben, nicht nur die Standardzähler für Azure Monitor for Windows Virtual Desktop. Diese Informationen können Ihnen helfen zu verstehen, welche Bereiche Sie ins Visier nehmen sollten, um die Kosten zu senken, z. B. die Häufigkeit eines Zählers zu reduzieren oder ihn ganz zu entfernen.

Sie können auch Kosten reduzieren, indem Sie Leistungszähler entfernen. Wie Sie Leistungszähler entfernen oder vorhandene Zähler bearbeiten können, um ihre Häufigkeit zu verringern, erfahren Sie unter [Konfigurieren von Leistungszählern](../azure-monitor/platform/data-sources-performance-counters.md#configuring-performance-counters).

### <a name="manage-windows-event-logs"></a>Verwalten von Windows-Ereignisprotokollen

Es ist unwahrscheinlich, dass Windows-Ereignisse eine Spitze im Datenaufkommen verursachen, wenn alle Hosts gesund sind. Ein ungesunder Host kann die Anzahl der an das Protokoll gesendeten Ereignisse erhöhen, die Informationen können jedoch für die Behebung der Probleme des Hosts entscheidend sein. Wir empfehlen, sie beizubehalten. Weitere Informationen zum Verwalten von Windows-Ereignisprotokollen finden Sie unter [Konfigurieren von Windows](../azure-monitor/agents/data-sources-windows-events.md#configuring-windows-event-logs)-Ereignisprotokollen.

### <a name="manage-diagnostics"></a>Verwalten Sie die Diagnosen

Die Diagnose von Windows Virtual Desktop sollte weniger als 1% Ihrer Datenspeicherkosten ausmachen, daher empfehlen wir nicht, sie zu entfernen. Um die Diagnose von Windows Virtual Desktop zu verwalten, [verwenden Sie Log Analytics für die Diagnosefunktion](diagnostics-log-analytics.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Monitor for Windows Virtual Desktop finden Sie in den folgenden Artikeln:

- [Verwenden von Azure Monitor for Windows Virtual Desktop zum Überwachen Ihrer Bereitstellung](azure-monitor.md).
- Verwenden Sie das [Glossar](azure-monitor-glossary.md), um mehr über Begriffe und Konzepte zu erfahren.
- Wenn ein Problem auftritt, finden Sie weitere Informationen im [Leitfaden zur Problembehandlung](troubleshoot-azure-monitor.md).
- Weitere Informationen zur Verwaltung Ihrer Überwachungskosten finden Sie unter[ Überwachen der Nutzung und geschätzte Kosten in Azure Monitor](../azure-monitor/usage-estimated-costs.md).

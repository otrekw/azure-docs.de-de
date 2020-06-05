---
title: 'Azure Automation: Übersicht über Änderungsnachverfolgung und Bestand'
description: In diesem Artikel wird das Feature Änderungsnachverfolgung und Bestand beschrieben, mit dem Sie Änderungen an Software und Microsoft-Diensten in Ihrer Umgebung erkennen können.
services: automation
ms.subservice: change-inventory-management
ms.date: 01/28/2019
ms.topic: conceptual
ms.openlocfilehash: 4f6ae1ad5b0f3904b84d47316c11aa1a67531a28
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835103"
---
# <a name="change-tracking-and-inventory-overview"></a>Übersicht über Änderungsnachverfolgung und Bestand

Dieser Artikel bietet eine Einführung in Änderungsnachverfolgung und Bestand in Azure Automation. Mit diesem Feature werden Änderungen an virtuellen Computern und der Serverinfrastruktur nachverfolgt, damit Sie Betriebs- und Umgebungsprobleme mit Software ermitteln können, die vom Paket-Manager der Distribution verwaltet wird. Folgende Elemente werden u. a. mit Änderungsnachverfolgung und Bestand nachverfolgt: 

- Windows-Software
- Linux-Software (Pakete)
- Windows- und Linux-Dateien
- Windows-Registrierungsschlüssel
- Microsoft-Dienste
- Linux-Daemons

> [!NOTE]
> Informationen zum Nachverfolgen von Eigenschaftenänderungen in Azure Resource Manager finden Sie im [Änderungsverlauf](../governance/resource-graph/how-to/get-resource-changes.md) von Azure Resource Graph.

Änderungsnachverfolgung und Bestand rufen ihre Daten aus Azure Monitor ab. Virtuelle Computer, die mit Log Analytics-Arbeitsbereichen verbunden sind, verwenden Log Analytics-Agents, um Daten zu Änderungen an installierter Software, Microsoft-Diensten, Windows-Registrierung und -Dateien sowie Linux-Daemons auf überwachten Servern zu sammeln. Wenn Daten verfügbar sind, sendet der Agent diese zur Verarbeitung an Azure Monitor. Azure Monitor wendet Logik auf die empfangenen Daten an, zeichnet sie auf und macht sie verfügbar. 

Änderungsnachverfolgung und Bestand ermöglichen die Funktionsbereiche Änderungsnachverfolgung und Bestand in Azure Automation. Da beide Bereiche denselben Log Analytics-Agent verwenden, ist der Prozess zum Hinzufügen eines virtuellen Computers in beiden Funktionsbereichen identisch. 

> [!NOTE]
> Wenn Sie Änderungsnachverfolgung und Bestand verwenden möchten, müssen sich alle virtuellen Computer im gleichen Abonnement und in der gleichen Region wie das Automation-Konto befinden.

Änderungsnachverfolgung und Bestand unterstützt die folgenden Elemente derzeit nicht:

* Rekursion für die Nachverfolgung der Windows-Registrierung
* Netzwerkdateisysteme
* Verschiedene Installationsmethoden
* ***EXE**-Dateien für Windows

Weitere Einschränkungen:

* Die Spalte **Maximale Dateigröße** und ihre Werte werden in der aktuellen Implementierung nicht genutzt.
* Wenn Sie im 30-minütigen Sammlungszyklus mehr als 2500 Dateien erfassen, werden Änderungsnachverfolgung und Bestand möglicherweise beeinträchtigt.
* Wenn der Netzwerkverkehr hoch ist, kann es bis zu sechs Stunden dauern, bis Änderungsdatensätze angezeigt werden.
* Wenn Sie eine Konfiguration ändern, während ein Computer heruntergefahren ist, stellt der Computer möglicherweise Änderungen bereit, die zur vorherigen Konfiguration gehören.

Folgende Probleme treten derzeit mit Änderungsnachverfolgung und Bestand auf:

* Hotfix-Updates werden für Computer mit Windows Server 2016 Core RS3 nicht erfasst.
* Für Linux-Daemons wird möglicherweise ein geänderter Zustand angezeigt, obwohl keine Änderung aufgetreten ist. Dieses Problem wird durch die Art der Erfassung von `SvcRunLevels`-Daten im Azure Monitor-Protokoll [ConfigurationChange](https://docs.microsoft.com/azure/azure-monitor/reference/tables/configurationchange) verursacht.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Änderungsnachverfolgung und Bestand werden unter allen Betriebssystemen unterstützt, die die Anforderungen des Log Analytics-Agent erfüllen. Die offiziell unterstützten Versionen des Windows-Betriebssystems sind Windows Server 2008 SP1 oder höher und Windows 7 SP1 oder höher. Außerdem wird eine Reihe von Linux-Betriebssystemen unterstützt. Weitere Informationen finden Sie unter [Übersicht über den Log Analytics-Agent](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent). 

## <a name="network-requirements"></a>Netzwerkanforderungen

Änderungsnachverfolgung und Bestand benötigt speziell die in der folgenden Tabelle aufgeführten Netzwerkadressen. Die Kommunikation mit diesen Adressen verwendet Port 443.

|Azure – Öffentlich  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com    | *.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net | *.blob.core.usgovcloudapi.net|
|*.azure-automation.net | *.azure-automation.us|

## <a name="change-tracking-and-inventory-user-interface"></a>Benutzeroberfläche von Änderungsnachverfolgung und Bestand

Verwenden Sie Änderungsnachverfolgung und Bestand im Azure-Portal, um eine Zusammenfassung der Änderungen an den überwachten Computern anzuzeigen. Um die Funktion verwenden zu können, wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** oder **Bestand** aus.  

![Dashboard der Änderungsnachverfolgung](./media/change-tracking/change-tracking-dash01.png)

Dropdownmenüs stehen am oberen Rand des Dashboards zur Verfügung. Damit können Sie das Diagramm der Änderungsnachverfolgung und die detaillierten Informationen anhand des Änderungstyps und Zeitbereichs einschränken. Sie können auch auf dem Diagramm klicken und ziehen, um einen benutzerdefinierten Zeitbereich auszuwählen. 

Sie können auf eine Änderung oder ein Ereignis klicken, um die zugehörigen Details anzuzeigen. Die verfügbaren Änderungstypen sind:

* Events
* Daemons
* Dateien
* Registrierung
* Software
* Microsoft-Dienste

Sie können jede Änderung hinzufügen, ändern oder entfernen. Wie aus dem Beispiel unten hervorgeht, wurde der Starttyp des Diensts von „Manuell“ in „Automatisch“ geändert.

![Details der Änderungsnachverfolgung](./media/change-tracking/change-tracking-details.png)

## <a name="tracking-of-file-changes"></a>Nachverfolgung von Dateiänderungen

Zum Nachverfolgen von Änderungen unter Windows und Linux verwendet Änderungsnachverfolgung und Bestand MD5-Hashes der Dateien. Das Feature verwendet die Hashes, um zu ermitteln, ob seit der letzten Inventur Änderungen vorgenommen wurden.

## <a name="tracking-of-file-content-changes"></a>Nachverfolgen von Änderungen am Dateiinhalt

Änderungsnachverfolgung und Bestand ermöglicht Ihnen, den Inhalt einer Windows- oder Linux-Datei vor und nach einer Dateiänderung anzuzeigen. Für jede Dateiänderung speichert Änderungsnachverfolgung und Bestand den Inhalt der Datei in einem [Azure Storage-Konto](../storage/common/storage-create-storage-account.md). Wenn Sie die Datei nachverfolgen, können Sie ihren Inhalt vor oder nach einer Änderung anzeigen. Sie können den Inhalt inline oder nebeneinander anzeigen. 

![Anzeigen von Änderungen in einer Datei](./media/change-tracking/view-file-changes.png)

## <a name="tracking-of-registry-keys"></a>Nachverfolgen von Registrierungsschlüsseln

Änderungsnachverfolgung und Bestand ermöglicht die Überwachung von Änderungen an Registrierungsschlüsseln. Die Überwachung dient dem Ermitteln von Erweiterungspunkten, an denen Code von Drittanbietern und Schadsoftware aktiv werden können. In der folgenden Tabelle sind vorkonfigurierte (aber nicht aktivierte) Registrierungsschlüssel aufgeführt. Um diese Schlüssel nachzuverfolgen, müssen Sie jeden einzeln aktivieren.

> [!div class="mx-tdBreakAll"]
> |Registrierungsschlüssel | Zweck |
> | --- | --- |
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit **explorer.exe** ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup` | Überwacht Skripts, die beim Start ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown` | Überwacht Skripts, die beim Herunterfahren ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run` | Überwacht Schlüssel, die geladen werden, bevor sich der Benutzer bei seinem Windows-Konto anmeldet. Der Schlüssel wird für 32-Bit-Anwendungen verwendet, die auf 64-Bit-Computern ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components` | Überwacht Änderungen an Anwendungseinstellungen.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit **explorer.exe** ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers` | Überwacht gängige Autostart-Einträge, die sich direkt bei Windows-Explorer einklinken und in der Regel In-Process mit **explorer.exe** ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | Überwacht die Symboloverlayhandler-Registrierung.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | Überwacht die Symboloverlayhandler-Registrierung für 32-Bit-Anwendungen, die auf 64-Bit-Computern ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Überwacht neue Browserhilfsobjekt-Plug-Ins für Internet Explorer. Wird für den Zugriff auf das Dokumentobjektmodell (DOM) der aktuellen Seite und zum Steuern der Navigation verwendet.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Überwacht neue Browserhilfsobjekt-Plug-Ins für Internet Explorer. Wird für den Zugriff auf das Dokumentobjektmodell (DOM) der aktuellen Seite und zum Steuern der Navigation für 32-Bit-Anwendungen verwendet, die auf 64-Bit-Computern ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Internet Explorer\Extensions` | Überwacht, ob neue Internet Explorer-Erweiterungen vorliegen, z.B. benutzerdefinierte Toolmenüs und benutzerdefinierte Symbolleistenschaltflächen.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions` | Überwacht, ob neue Internet Explorer-Erweiterungen vorliegen, z. B. benutzerdefinierte Toolmenüs und benutzerdefinierte Symbolleisten-Schaltflächen für 32-Bit-Anwendungen, die auf 64-Bit-Computern ausgeführt werden.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32` | Überwacht die mit wavemapper zugeordneten 32-Bit-Treiber, wave1 und wave2, msacm.imaadpcm, .msadpcm, .msgsm610 und vidc. Ähnlich dem Abschnitt „[drivers]“ in der Datei **system.ini**.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32` | Überwacht die mit wavemapper zugeordneten 32-Bit-Treiber, wave1 und wave2, msacm.imaadpcm, .msadpcm, .msgsm610 und vidc für 32-Bit-Anwendungen, die auf 64-Bit-Computern ausgeführt werden. Ähnlich dem Abschnitt „[drivers]“ in der Datei **system.ini**.
> |`HKEY\LOCAL\MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls` | Überwacht die Liste der bekannten oder häufig verwendeten System-DLL-Dateien. Dieses System verhindert, dass schwache Anwendungsverzeichnisberechtigungen durch Infiltration mit Trojanerversionen von System-DLLs ausgenutzt werden.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify` | Überwacht die Liste der Pakete, die Ereignisbenachrichtigungen von **winlogon.exe**, dem interaktiven Anmeldungsunterstützungsmodell für Windows, empfangen können.

## <a name="support-for-file-integrity-monitoring-in-azure-security-center"></a>Unterstützung der Überwachung der Dateiintegrität in Azure Security Center

Änderungsnachverfolgung und Bestand nutzt die [Überwachung der Dateiintegrität (File Integrity Monitoring, FIM) von Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring). FIM überwacht nur Dateien und Registrierungen. Die vollständige Lösung Änderungsnachverfolgung und Bestand überwacht darüber hinaus Folgendes:

- Softwareänderungen
- Microsoft-Dienste
- Linux-Daemons

## <a name="recursion-support"></a>Rekursionsunterstützung

Änderungsnachverfolgung und Bestand unterstützt Rekursion, sodass Sie Platzhalter angeben können, um die Nachverfolgung über Verzeichnisse hinweg zu vereinfachen. Die Rekursion bietet auch Umgebungsvariablen, mit denen Sie Dateien in mehreren Umgebungen mit unterschiedlichen oder dynamischen Laufwerknamen nachverfolgen können. Die folgende Liste enthält allgemeine Informationen, mit denen Sie beim Konfigurieren der Rekursion vertraut sein sollten:

* Platzhalter werden zum Nachverfolgen mehrerer Dateien benötigt.
* Platzhalter können nur im letzten Segment eines Pfads verwendet werden, z. B. **C:\Ordner\\Datei*** oder **/etc/*.conf**.
* Wenn eine Umgebungsvariable einen ungültigen Pfad besitzt, verläuft die Überprüfung zwar erfolgreich, doch bei der Ausführung tritt ein Fehler für den Pfad auf.
* Vermeiden Sie allgemeine Pfadnamen beim Festlegen des Pfads, da diese Art von Einstellung dazu führen kann, dass zu viele Ordner durchlaufen werden müssen.

## <a name="change-tracking-and-inventory-data-collection"></a>Datensammlung durch Änderungsnachverfolgung und Bestand

Die nächste Tabelle zeigt die Häufigkeit der Datensammlung für die Typen von Änderungen, die von Änderungsnachverfolgung und Bestand unterstützt werden. Für jeden Typ wird die Datenmomentaufnahme des aktuellen Zustands außerdem mindestens alle 24 Stunden aktualisiert.

| **Änderungstyp** | **Frequency** |
| --- | --- |
| Windows-Registrierung | 50 Minuten |
| Windows-Datei | 30 Minuten |
| Linux-Datei | 15 Minuten |
| Microsoft-Dienste | 10 Sekunden bis 30 Minuten</br> Standardwert: 30 Minuten |
| Linux-Daemons | 5 Minuten |
| Windows-Software | 30 Minuten |
| Linux-Software | 5 Minuten |

Die folgende Tabelle zeigt die Grenzwerte der nachverfolgten Elemente pro Computer für Änderungsnachverfolgung und Bestand.

| **Ressource** | **Begrenzung** |
|---|---|---|
|Datei|500|
|Registrierung|250|
|Windows-Software (ohne Hotfixes) |250|
|Linux-Pakete|1250|
|Dienste|250|
|Daemons|250|

Die durchschnittliche Nutzung von Log Analytics-Daten für einen Computer mit Änderungsnachverfolgung und Bestand beträgt je nach Umgebung ungefähr 40 MB pro Monat. Mithilfe der Funktion „Nutzung und geschätzte Kosten“ im Log Analytics-Arbeitsbereich können Sie die von Änderungsnachverfolgung und Bestand erfassten Daten in einem Nutzungsdiagramm sehen. Sie können diese Datenansicht verwenden, um die Datennutzung und die damit verbundenen Kosten zu analysieren. Weitere Informationen finden Sie unter [Verstehen Ihrer Nutzung und Schätzen von Kosten](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understand-your-usage-and-estimate-costs).  

### <a name="microsoft-service-data"></a>Daten zu Microsoft-Diensten

Die Standardsammelhäufigkeit für Microsoft-Dienste beträgt 30 Minuten. Sie können die Häufigkeit mithilfe eines Schiebereglers auf der Registerkarte **Microsoft-Dienste** unter **Einstellungen bearbeiten** konfigurieren. 

![Schieberegler für Microsoft-Dienste](./media/change-tracking/windowservices.png)

Zum Optimieren der Leistung verfolgt der Log Analytics-Agent ausschließlich Änderungen nach. Durch das Einstellen eines hohen Schwellenwerts werden Änderungen möglicherweise übergangen, wenn der Dienst in seinen ursprünglichen Zustand zurückversetzt wurde. Das Festlegen der Häufigkeit auf einen kleineren Wert ermöglicht Ihnen, Änderungen zu erfassen, die sonst verpasst würden.

> [!NOTE]
> Zwar kann der Agent Änderungen bis hinab zu einem 10-Sekunden-Intervall nachverfolgen, bis zur Anzeige der Daten im Azure-Portal vergehen jedoch einige Minuten. Änderungen, die zwischen der Erfassung und der Anzeige im Portal erfolgen, werden trotzdem nachverfolgt und protokolliert.

## <a name="support-for-alerts-on-configuration-state"></a>Unterstützung für Warnungen zum Konfigurationszustand

Eine Schlüsselfunktion von Änderungsnachverfolgung und Bestand sind Warnungen bei Änderungen am Konfigurationszustand Ihrer Hybridumgebung. Es sind viele hilfreiche Aktionen verfügbar, die als Reaktion auf Warnungen ausgelöst werden können, z. B. Aktionen in Azure Functions, Automation-Runbooks, Webhooks. Warnungen zu Änderungen an der Datei **C:\windows\system32\drivers\etc\hosts** für einen Computer sind eine gute Anwendung von Warnungen für Daten von Änderungsnachverfolgung und Bestand. Es gibt noch viele weitere Szenarien für Warnungen, einschließlich der in der nächsten Tabelle definierten Abfrageszenarien. 

|Abfrage  |BESCHREIBUNG  |
|---------|---------|
|ConfigurationChange <br>&#124; where ConfigChangeType == "Files" and FileSystemPath contains " c:\\windows\\system32\\drivers\\"|Nützlich für das Nachverfolgen von Änderungen an kritischen Systemdateien|
|ConfigurationChange <br>&#124; where FieldsChanged contains "FileContentChecksum" and FileSystemPath == "c:\\windows\\system32\\drivers\\etc\\hosts"|Nützlich für das Nachverfolgen von Änderungen an wichtige Konfigurationsdateien|
|ConfigurationChange <br>&#124; where ConfigChangeType == "Microsoft services" and SvcName contains "w3svc" and SvcState == "Stopped"|Nützlich für das Nachverfolgen von Änderungen an für das System wichtigen Diensten.|
|ConfigurationChange <br>&#124; where ConfigChangeType == "Daemons" and SvcName contains "ssh" and SvcState!= "Running"|Nützlich für das Nachverfolgen von Änderungen an für das System wichtigen Diensten.|
|ConfigurationChange <br>&#124; where ConfigChangeType == "Software" and ChangeCategory == "Added"|Nützlich für Umgebungen, in denen Softwarekonfigurationen gesperrt werden müssen|
|ConfigurationData <br>&#124; where SoftwareName contains "Monitoring Agent" and CurrentVersion!= "8.0.11081.0"|Nützlich, um zu ermitteln, auf welchen Computern eine veraltete oder nicht konforme Softwareversion installiert ist. Mit dieser Abfrage wird der letzte gemeldete Konfigurationszustand ausgegeben, aber es werden keine Änderungen gemeldet.|
|ConfigurationChange <br>&#124; mit RegistryKey == @"HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\QualityCompat"| Nützlich für das Nachverfolgen von Änderungen an wichtigen Virenschutzschlüsseln|
|ConfigurationChange <br>&#124; mit RegistryKey contains @"HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\SharedAccess\\Parameters\\FirewallPolicy"| Nützlich für das Nachverfolgen von Änderungen an Firewalleinstellungen|

## <a name="next-steps"></a>Nächste Schritte

* Ausführliche Informationen zum Arbeiten mit Änderungsnachverfolgung und Bestand finden Sie unter [Verwalten von Änderungsnachverfolgung und Bestand](change-tracking-file-contents.md).
* Informationen zum Aktivieren des Features aus einem Runbook finden Sie unter [Aktivieren von Änderungsnachverfolgung und Bestand über ein Runbook](automation-enable-changes-from-runbook.md).
* Informationen zum Aktivieren des Features über ein Automation-Konto finden Sie unter [Aktivieren von Änderungsnachverfolgung und Bestand über ein Automation-Konto](automation-enable-changes-from-auto-acct.md).
* Informationen zum Aktivieren des Features durch das Durchsuchen des Azure-Portals finden Sie unter [Aktivieren von Änderungsnachverfolgung und Bestand über das Azure-Portal](automation-onboard-solutions-from-browse.md).
* Informationen zum Aktivieren des Features über eine Azure-VM finden Sie unter [Aktivieren von Änderungsnachverfolgung und Bestand über einen virtuellen Azure-Computer](automation-enable-changes-from-vm.md).
* Wenn Sie Protokolle durchsuchen müssen, die in Ihrem Log Analytics Arbeitsbereich gespeichert sind, finden Sie unter [Protokollsuchvorgänge in Azure Monitor-Protokollen](../log-analytics/log-analytics-log-searches.md) weitere Informationen.
* Informationen zum Behandeln von Problemen bei diesem Feature finden Sie unter [Problembehandlung bei Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).
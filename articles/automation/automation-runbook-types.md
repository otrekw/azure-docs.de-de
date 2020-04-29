---
title: Azure Automation-Runbooktypen
description: Beschreibt die verschiedenen Runbooktypen, die Sie in Azure Automation verwenden können, sowie Aspekte, die Sie bei der Auswahl des geeigneten Typs berücksichtigen sollten.
services: automation
ms.subservice: process-automation
ms.date: 03/05/2019
ms.topic: conceptual
ms.openlocfilehash: 1ac6347bd8e723f356da4803da54a6ea45a4a71a
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535518"
---
# <a name="azure-automation-runbook-types"></a>Azure Automation-Runbooktypen

Der Prozessautomatisierungsdienst von Azure Automation unterstützt verschiedene Runbooktypen, die in der folgenden Tabelle definiert sind. Weitere Informationen zur Prozessautomatisierungsumgebung finden Sie unter [Ausführen von Runbooks in Azure Automation](automation-runbook-execution.md).

| type | BESCHREIBUNG |
|:--- |:--- |
| [Grafisch](#graphical-runbooks)|Grafisches Runbook, das auf Windows PowerShell basiert und vollständig im grafischen Editor im Azure-Portal erstellt und bearbeitet wird. |
| [PowerShell-Workflow, grafisch](#graphical-runbooks)|Grafisches Runbook, das auf dem Windows PowerShell-Workflow basiert und vollständig im grafischen Editor im Azure-Portal erstellt und bearbeitet wird. |
| [PowerShell](#powershell-runbooks) |Textrunbook, das auf einem Windows PowerShell-Skript basiert. |
| [PowerShell-Workflow](#powershell-workflow-runbooks)|Textrunbook, das auf einem Windows PowerShell-Workflowskript basiert. |
| [Python](#python-runbooks) |Textrunbook, das auf einem Python-Skript basiert. |

Wenn Sie festlegen, welchen Typ Sie für ein bestimmtes Runbook verwenden möchten, müssen Sie Folgendes berücksichtigen.

* Runbooks können nicht aus einem grafischen in einen textbasierten Typ oder umgekehrt konvertiert werden.
* Es gibt Einschränkungen bei der Verwendung von Runbooks verschiedener Typen als untergeordnete Runbooks. Weitere Informationen finden Sie unter [Untergeordnete Runbooks in Azure Automation](automation-child-runbooks.md).

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="graphical-runbooks"></a>Grafische Runbooks

Sie können grafische Runbooks und grafische PowerShell-Workflow-Runbooks im grafischen Editor im Azure-Portal erstellen und bearbeiten. Es ist jedoch nicht möglich, diesen Runbooktyp mit einem anderen Tool zu erstellen oder zu bearbeiten. Hauptfunktionen von grafischen Runbooks:

* Können in Dateien in Ihrem Automation-Konto exportiert und dann in ein anderes Automation-Konto importiert werden 
* Generieren PowerShell-Code 
* Können während des Imports in grafische PowerShell-Workflow-Runbooks konvertiert werden und umgekehrt 

### <a name="advantages"></a>Vorteile

* Verwenden ein visuelles Erstellungsmodell zum Einfügen/Verknüpfen/Konfigurieren
* Schwerpunkt auf Datenfluss im Prozess
* Visuelle Darstellung von Verwaltungsprozessen.
* Schließen weitere Runbooks als untergeordnete Runbooks ein, um übergeordnete Workflows zu erstellen
* Fördern modulare Programmierung

### <a name="limitations"></a>Einschränkungen

* Können nicht außerhalb des Azure-Portals erstellt oder bearbeitet werden
* Erfordern möglicherweise Aktivität mit PowerShell-Code, um komplexe Logik auszuführen
* Können nicht in eines der [Textformate](automation-runbook-types.md) konvertiert werden (Textrunbooks können darüber hinaus nicht in das grafische Format überführt werden) 
* Bieten keine Möglichkeit zum Anzeigen oder direkten Bearbeiten des vom grafischen Workflow erstellten PowerShell-Codes. Sie können den erstellten Code in allen Codeaktivitäten anzeigen.
* Keine Möglichkeit zum Ausführen von Runbooks auf einem Hybrid Runbook Worker unter Linux. Weitere Informationen finden Sie unter [Automatisieren von Ressourcen im Rechenzentrum oder in der Cloud mit Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).

## <a name="powershell-runbooks"></a>PowerShell-Runbooks

PowerShell-Runbooks basieren auf Windows PowerShell. Sie bearbeiten den Code des Runbooks direkt mit dem Text-Editor im Azure-Portal.  Sie können auch einen beliebigen Offline-Texteditor verwenden und das [Runbook in Azure Automation importieren](manage-runbooks.md) .

### <a name="advantages"></a>Vorteile

* Implementierung der gesamten komplexen Logik mit PowerShell-Code ohne die zusätzliche Komplexität des PowerShell-Workflows.
* Schnellerer Start als PowerShell-Workflow-Runbooks, da vor der Ausführung keine Kompilierung erforderlich ist
* Werden in Azure und in Hybrid Runbook Workern sowohl für Windows als auch für Linux ausgeführt

### <a name="limitations"></a>Einschränkungen

* Erfordern Kenntnisse zu PowerShell-Skripts
* Können keine [parallele Verarbeitung](automation-powershell-workflow.md#parallel-processing) zum gleichzeitigen Ausführen mehrerer Aktionen nutzen
* Können keine [Prüfpunkte](automation-powershell-workflow.md#checkpoints) zum Fortsetzen des Runbooks bei einem Fehler nutzen
* Sie können nur PowerShell-Workflow- und grafische Runbooks mithilfe des Cmdlets [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0) als untergeordnete Runbooks einfügen, wobei ein neuer Auftrag erstellt wird.

### <a name="known-issues"></a>Bekannte Probleme

Im Folgenden sind aktuell bekannte Probleme mit PowerShell-Runbooks aufgeführt:

* PowerShell-Runbooks können keine unverschlüsselten [Variablenobjekte](automation-variables.md) mit einem NULL-Wert abrufen.
* PowerShell-Runbooks können kein Variablenobjekt abrufen, dessen Name `*~*` enthält.
* Der Vorgang [Get-Process](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-process?view=powershell-7) in einer Schleife in einem PowerShell-Runbook kann nach etwa 80 Iterationen zum Absturz führen.
* Ein PowerShell-Runbook kann einen Fehler verursachen, wenn es versucht, eine große Datenmenge auf einmal in den Ausgabestream zu schreiben. Sie können dieses Problem in der Regel vermeiden, indem Sie nur die für die Arbeit mit großen Objekten benötigten Informationen in die Runbookausgabe einfügen. Anstatt `Get-Process` ohne Einschränkungen zu verwenden, können Sie z. B. nur die erforderlichen Parameter in die Cmdlet-Ausgabe einfügen wie bei `Get-Process | Select ProcessName, CPU`.

## <a name="powershell-workflow-runbooks"></a>PowerShell-Workflow-Runbooks

PowerShell-Workflow-Runbooks sind Textrunbooks, die auf einem [Windows PowerShell-Workflow](automation-powershell-workflow.md)basieren. Sie bearbeiten den Code des Runbooks direkt mit dem Text-Editor im Azure-Portal. Sie können auch einen beliebigen Offline-Texteditor verwenden und das [Runbook in Azure Automation importieren](manage-runbooks.md) .

### <a name="advantages"></a>Vorteile

* Implementierung der gesamten komplexen Logik mit PowerShell-Workflowcode.
* Verwendung von [Prüfpunkten](automation-powershell-workflow.md#checkpoints) zum Fortsetzen des Vorgangs bei einem Fehler
* Verwendung der [Parallelverarbeitung](automation-powershell-workflow.md#parallel-processing), um mehrere Aktionen gleichzeitig auszuführen
* Können andere grafische und PowerShell-Workflow-Runbooks als untergeordnete Runbooks enthalten, um übergeordnete Workflows zu erstellen

### <a name="limitations"></a>Einschränkungen

* Erfordern Kenntnisse mit dem PowerShell-Workflow
* Müssen die zusätzliche Komplexität des PowerShell-Workflows (z. B. [deserialisierte Objekte](automation-powershell-workflow.md#code-changes)) verarbeiten
* Benötigen beim Starten länger als PowerShell-Runbooks, da sie vor der Ausführung kompiliert werden müssen
* Ausschließlich PowerShell-Runbooks können mit dem Cmdlet `Start-AzAutomationRunbook` als untergeordnete Runbooks eingeschlossen werden.
* Keine Möglichkeit zum Ausführen von Runbooks auf einem Hybrid Runbook Worker und Linux

## <a name="python-runbooks"></a>Python-Runbooks

Python-Runbooks werden unter Python-2 kompiliert. Sie können den Code des Runbooks direkt mit dem Text-Editor im Azure-Portal bearbeiten. Sie können auch einen beliebigen Offline-Text-Editor verwenden und das [Runbook in Azure Automation importieren](manage-runbooks.md).

### <a name="advantages"></a>Vorteile

* Verwendung der stabilen Python-Bibliotheken
* Können in Azure oder auf Hybrid Runbook Workern unter Linux ausgeführt werden. Windows Hybrid Runbook Workers werden unterstützt, wenn [python2.7](https://www.python.org/downloads/release/latest/python2) installiert ist.

### <a name="limitations"></a>Einschränkungen

* Erfordern Kenntnisse mit Python-Skripts
* Unterstützen derzeit nur Python 2. Alle Python 3-spezifischen Funktionen führen zu Fehlern.
* Erfordern für die Verwendung von Bibliotheken von Drittanbietern das [Importieren der Pakete](python-packages.md) in das Automation-Konto

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Erstellen grafischer Runbooks finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).
* Informationen zu den Unterschieden zwischen PowerShell- und PowerShell-Workflow-Runbooks finden Sie unter [Grundlagen des Windows PowerShell-Workflows](automation-powershell-workflow.md).
* Weitere Informationen zum Erstellen oder Importieren eines Runbooks finden Sie unter [Verwalten von Runbooks in Azure Automation](manage-runbooks.md).
* Weitere Informationen zu PowerShell, einschließlich Sprachreferenz und Lernmodulen, finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/scripting/overview).

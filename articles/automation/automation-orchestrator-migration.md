---
title: Migrieren von Orchestrator zu Azure Automation (Betaversion)
description: In diesem Artikel wird beschrieben, wie Runbooks und Integrationspakete von Orchestrator nach Azure Automation migriert werden.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 6ee4a09df0f95cb809db0e5c0e63d195ee5cfdff
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98896934"
---
# <a name="migrate-from-orchestrator-to-azure-automation-beta"></a>Migrieren von Orchestrator zu Azure Automation (Betaversion)

Runbooks in [System Center 2012 – Orchestrator](/previous-versions/system-center/system-center-2012-R2/hh237242(v=sc.12)) basieren auf Aktivitäten der Integrationspakete, die speziell für Orchestrator geschrieben wurden, während Runbooks in Azure Automation auf Windows PowerShell basieren. [Grafische Runbooks](automation-runbook-types.md#graphical-runbooks) in Azure Automation haben ein ähnliches Aussehen wie Orchestrator-Runbooks, die enthaltenen Aktivitäten repräsentieren PowerShell-Cmdlets, untergeordnete Runbooks und Objekte. Zusätzlich zur Konvertierung der Runbooks selbst müssen Sie die Integrationspakete mit den von Runbooks verwendeten Aktivitäten mithilfe von Windows PowerShell-Cmdlets in Integrationsmodule konvertieren. 

[Service Management Automation](/previous-versions/system-center/system-center-2012-R2/dn469260(v=sc.12)) (SMA) dient wie Orchestrator zum Speichern und Ausführen von Runbooks in Ihrem lokalen Rechenzentrum und führt dieselben Integrationsmodule aus wie Azure Automation. Der Runbook Converter konvertiert Orchestrator-Runbooks in grafische Runbooks, die nicht in SMA unterstützt werden. Sie können in SMA dennoch das Modul mit Standardaktivitäten und die System Center Orchestrator-Integrationsmodule installieren, müssen jedoch Ihre [Runbooks manuell umschreiben](/system-center/sma/authoring-automation-runbooks).

## <a name="download-the-orchestrator-migration-toolkit"></a>Herunterladen des Orchestrator-Migrationstoolkits

Der erste Schritt bei der Migration ist das Herunterladen des [System Center Orchestrator Migration Toolkit](https://www.microsoft.com/download/details.aspx?id=47323). Dieses Toolkit umfasst Tools, die Sie beim Konvertieren von Runbooks von Orchestrator nach Azure Automation unterstützen.  

## <a name="import-the-standard-activities-module"></a>Importieren des Moduls mit Standardaktivitäten

Importieren Sie das [Modul mit Standardaktivitäten](/system-center/orchestrator/standard-activities) in Azure Automation. Dieses umfasst konvertierte Versionen der standardmäßigen Orchestrator-Aktivitäten, die von konvertierten grafischen Runbooks verwendet werden können.

## <a name="import-orchestrator-integration-modules"></a>Importieren von Orchestrator-Integrationsmodulen

Microsoft stellt [Integrationspakete](/previous-versions/system-center/packs/hh295851(v=technet.10)) zum Erstellen von Runbooks zur Automatisierung von System Center-Komponenten und anderen Produkten bereit. Einige diese Integrationspakete basieren derzeit auf dem OIT, können derzeit jedoch aufgrund von bekannten Problemen nicht in Integrationsmodule konvertiert werden. Importieren Sie für die von Ihren Runbooks verwendeten Integrationspakete, die auf System Center zugreifen, die [System Center Orchestrator-Integrationsmodule](https://www.microsoft.com/download/details.aspx?id=49555) in Azure Automation. Dieses Paket enthält konvertierte Versionen dieser Integrationspakete, die in Azure Automation und Service Management Automation importiert werden können.  

## <a name="convert-integration-packs"></a>Konvertieren von Integrationspaketen

Konvertieren Sie mit dem [Integration Pack Converter](/system-center/orchestrator/orch-integration-toolkit/integration-pack-wizard) beliebige, mit dem [OIT (Orchestrator Integration Toolkit)](/previous-versions/system-center/developer/hh855853(v=msdn.10)) erstellte Integrationspakete in Integrationsmodule, die auf PowerShell basieren und in Azure Automation oder Service Management Automation importiert werden können. Wenn Sie den Integration Pack Converter ausführen, wird ein Assistent gestartet, der Ihnen die Auswahl einer Integrationspaketdatei (.oip) ermöglicht. Der Assistent listet anschließend die im Integrationspaket enthaltenen Aktivitäten auf, und Sie können die zu migrierenden Aktivitäten auswählen. Wenn Sie den Assistenten abschließen, wird ein Integrationsmodul erstellt, dass ein entsprechendes Cmdlet für jede Aktivität im ursprünglichen Integrationspaket enthält.

> [!NOTE]
> Sie können mit dem Integration Pack Converter keine Integrationspakete konvertieren, die nicht mit dem OIT erstellt wurden. Einige von Microsoft bereitgestellte Integrationspakete können derzeit nicht mit diesem Tool konvertiert werden. Konvertierte Versionen dieser Integrationspakete stehen zum Download bereit, sodass sie in Azure Automation oder in Service Management Automation installiert werden können.

### <a name="parameters"></a>Parameter

Alle Eigenschaften einer Aktivität im Integrationspaket werden in Parameter des zugehörigen Cmdlets im Integrationsmodul konvertiert.  Windows PowerShell-Cmdlets enthalten einen Satz [allgemeiner Parameter](/powershell/module/microsoft.powershell.core/about/about_commonparameters) , die für alle Cmdlets verwendet werden können. Beispielsweise können über den Parameter "-Verbose" detaillierte Informationen zur Verwendung eines Cmdlets ausgegeben werden.  Die Cmdlets dürfen keinen Parameter aufweisen, die denselben Namen verwenden wie ein allgemeiner Parameter. Wenn eine Aktivität über eine Eigenschaft mit demselben Namen wie ein allgemeiner Parameter verfügt, fordert der Assistent Sie auf, einen anderen Namen für den Parameter anzugeben.

### <a name="monitor-activities"></a>Überwachen von Aktivitäten

Runbooks für die Überwachung beginnen in Orchestrator mit einer [Überwachungsaktivität](/previous-versions/system-center/system-center-2012-R2/hh403827(v=sc.12)) und werden fortlaufend ausgeführt, bis ein bestimmtes Ereignis ausgelöst wird. Azure Automation bietet keine Unterstützung für Runbooks zur Überwachung, deshalb werden im Integrationspaket enthaltene Überwachungsaktivitäten nicht konvertiert. Stattdessen wird im Integrationsmodul ein Platzhalter-Cmdlet für die Überwachungsaktivität erstellt.  Dieses Cmdlet hat keine Funktionalität, ermöglicht aber die Installation von konvertierten Runbooks, die die Überwachungsaktivität verwenden. Dieses Runbook kann nicht in Azure Automation ausgeführt, jedoch zur Änderung installiert werden.

Orchestrator umfasst einen Satz an [Standardaktivitäten](/previous-versions/system-center/system-center-2012-R2/hh403832(v=sc.12)) , die nicht in einem Integrationspaket enthalten sind, aber von vielen Runbooks verwendet werden.  Das Modul mit Standardaktivitäten ist ein Integrationsmodul das eine Cmdlet-Entsprechung für jede dieser Aktivitäten umfasst. Sie müssen dieses Integrationsmodul in Azure Automation installieren, bevor Sie konvertierte Runbooks importieren, die eine Standardaktivität verwenden.

Zusätzlich zur Unterstützung von konvertierten Runbooks können die Cmdlets im Modul mit Standardaktivitäten von einem Benutzer mit Orchestrator-Erfahrung dazu verwendet werden, neue Runbooks in Azure Automation zu erstellen. Wenngleich die Funktionalität aller Standardaktivitäten mithilfe von Cmdlets ausgeführt werden kann, weicht die Funktionsweise möglicherweise ab. Die Cmdlets im konvertierten Modul mit Standardaktivitäten arbeiten in gleicher Weise wie die entsprechenden Aktivitäten und verwenden dieselben Parameter. Dies kann Ihnen bei der Umstellung auf Azure Automation-Runbooks helfen.

## <a name="convert-orchestrator-runbooks"></a>Konvertieren von Orchestrator-Runbooks

Der Orchestrator Runbook Converter konvertiert Orchestrator-Runbooks in [grafische Runbooks](automation-runbook-types.md#graphical-runbooks), die in Azure Automation importiert werden können. Der Runbook Converter wird als PowerShell-Modul mit dem Cmdlet `ConvertFrom-SCORunbook` implementiert, das die Konvertierung durchführt. Wenn Sie den Converter installieren, wird eine Verknüpfung zu einer PowerShell-Sitzung erstellt, mit der das Cmdlet geladen wird.   

Im Folgenden finden Sie die grundlegenden Schritte zum Konvertieren eines Runbooks und dessen Import in Azure Automation. Details zur Verwendung des Cmdlets finden Sie weiter unten in diesem Abschnitt.

1. Exportieren Sie ein oder mehrere Runbooks aus Orchestrator.
2. Rufen Sie Integrationsmodule für alle Aktivitäten im Runbook ab.
3. Konvertieren Sie die Orchestrator-Runbooks in die exportierte Datei.
4. Überprüfen Sie die Informationen in Protokollen, um die Konvertierung zu überprüfen und um alle erforderlichen manuellen Aufgaben zu ermitteln.
5. Importieren Sie konvertierte Runbooks in Azure Automation.
6. Erstellen Sie alle erforderlichen Ressourcen in Azure Automation.
7. Bearbeiten Sie das Runbook in Azure Automation, um alle erforderlichen Aktivitäten zu ändern.

Die Syntax für `ConvertFrom-SCORunbook` lautet:

```powershell
ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>
```

* RunbookPath – Pfad zur Exportdatei mit den zu konvertierenden Runbooks.
* Modul – Durch Trennzeichen getrennte Liste der Integrationsmodule mit Aktivitäten in den Runbooks.
* OutputFolder – Pfad zum Ordner, in dem konvertierte grafische Runbooks erstellt werden.

Der folgende Beispielbefehl konvertiert die Runbooks in eine Exportdatei namens **MyRunbooks.ois_export**.  Diese Runbooks verwenden Active Directory und Data Protection Manager-Integrationspakete.

```powershell
ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"
```

### <a name="use-runbook-converter-log-files"></a>Verwenden von Runbook Converter-Protokolldateien

Der Runbook Converter erstellt die folgenden Protokolldateien am gleichen Speicherort wie das konvertierte Runbook.  Wenn die Dateien bereits vorhanden sind, werden sie mit den Informationen aus der letzten Konvertierung überschrieben.

| Datei | Contents |
|:--- |:--- |
| Runbook Converter - Progress.log |Ausführliche Konvertierungsschritte einschließlich Informationen für jede erfolgreich konvertierte Aktivität und Warnung für jede nicht konvertierte Aktivität. |
| Runbook Converter - Summary.log |Zusammenfassung der letzten Konvertierung einschließlich Warnungen und Folgeaufgaben, die Sie durchführen müssen, z. B. Erstellen einer Variablen für das konvertierte Runbook. |

### <a name="export-runbooks-from-orchestrator"></a>Exportieren von Runbooks aus Orchestrator

Der Runbook Converter arbeitet mit einer Exportdatei aus Orchestrator, die ein oder mehrere Runbooks enthält.  Es wird ein entsprechendes Azure Automation-Runbook für jedes Orchestrator-Runbook in der Exportdatei erstellt.  

Klicken Sie zum Exportieren aus Orchestrator mit der rechten Maustaste auf den Namen des Runbooks in Runbook Designer, und wählen Sie **Exportieren** aus.  Um alle Runbooks in einem Ordner zu exportieren, klicken Sie mit der rechten Maustaste auf den Namen des Ordners, und wählen Sie **Exportieren** aus.

### <a name="convert-runbook-activities"></a>Runbook-Aktivitäten zum Konvertieren

Der Runbook Converter konvertiert jede Aktivität im Orchestrator-Runbook in eine entsprechende Aktivität in Azure Automation.  Für die Aktivitäten, die nicht konvertiert werden können, wird eine Platzhalteraktivität im Runbook mit Warntext erstellt.  Nachdem Sie das konvertierte Runbook in Azure Automation importiert haben, müssen Sie eine dieser Aktivitäten mit gültigen Aktivitäten ersetzen, die die erforderliche Funktionalität ausführen.

Alle Orchestrator-Aktivitäten im Modul mit Standardaktivitäten werden konvertiert. Es gibt einige Orchestrator-Standardaktivitäten, die sich jedoch nicht in diesem Modul befinden und die nicht konvertiert werden. `Send Platform Event` verfügt beispielsweise über kein Azure Automation-Äquivalent, da das Ereignis Orchestrator-spezifisch ist.

[Überwachungsaktivitäten](/previous-versions/system-center/system-center-2012-R2/hh403827(v=sc.12)) werden nicht konvertiert, da für sie kein Äquivalent in Azure Automation vorhanden ist. Die Ausnahmen sind Überwachungsaktivitäten in konvertierten Integrationspaketen, die in die Platzhalteraktivität konvertiert werden.

Jede Aktivität aus einem konvertierten Integrationspaket wird konvertiert, wenn Sie den Pfad zum Integrationsmodul mit dem `modules`-Parameter angeben. Für System Center-Integrationspakete können Sie die System Center Orchestrator-Integrationsmodule verwenden.

### <a name="manage-orchestrator-resources"></a>Verwalten von Orchestrator-Ressourcen

Der Runbook Converter konvertiert nur Runbooks, keine anderen Orchestrator-Ressourcen wie z. B. Leistungsindikatoren, Variablen oder Verbindungen.  Leistungsindikatoren werden in Azure Automation nicht unterstützt.  Variablen und Verbindungen werden unterstützt; Sie müssen sie jedoch manuell erstellen. Die Protokolldateien weisen Sie darauf hin, dass das Runbook solche Ressourcen erfordert, und geben entsprechende Ressourcen an, die Sie für eine ordnungsgemäße Funktionsweise in Azure Automation für das konvertierte Runbook erstellen müssen.

Ein Runbook kann beispielsweise eine Variable verwenden, um einen bestimmten Wert in einer Aktivität auszufüllen.  Das konvertierte Runbook konvertiert die Aktivität und gibt eine Variablenressource in Azure Automation mit dem gleichen Namen wie die Orchestrator-Variable an. Diese Aktion wird in der **Runbook Converter – Summary.log**-Datei vermerkt, die nach der Konvertierung erstellt wird. Sie müssen diese Variablenressource manuell in Azure Automation erstellen, bevor Sie das Runbook verwenden.

### <a name="work-with-orchestrator-input-parameters"></a>Arbeiten mit Orchestrator-Eingabeparametern

Runbooks in Orchestrator akzeptieren Eingabeparameter mit der Aktivität `Initialize Data`.  Wenn das konvertierte Runbook diese Aktivität enthält, wird im Azure Automation-Runbook ein [Eingabeparameter](automation-graphical-authoring-intro.md#handle-runbook-input) für jeden Parameter in der Aktivität erstellt.  Eine [Workflowskriptsteuerelement](automation-graphical-authoring-intro.md#use-activities) -Aktivität wird im konvertierten Runbook erstellt, das jeden Parameter abruft und zurückgibt. Alle Aktivitäten im Runbook, die einen Eingabeparameter verwenden, verweisen auf die Ausgabe dieser Aktivität.

Der Grund für die Nutzung dieser Strategie ist eine optimale Spiegelung der Funktionalität im Orchestrator-Runbook.  Aktivitäten in neuen grafischen Runbooks sollten mithilfe einer Runbook-Eingabedatenquelle direkt auf Eingabeparameter verweisen.

### <a name="invoke-runbook-activity"></a>Aufrufen einer Runbookaktivität

Runbooks in Orchestrator starten andere Runbooks mit der Aktivität `Invoke Runbook`. Wenn das konvertierte Runbook diese Aktivität enthält und die Option `Wait for completion` festgelegt ist, wird dafür eine Runbookaktivität im konvertierten Runbook erstellt.  Wenn die Option `Wait for completion` nicht festgelegt ist, wird eine Workflowskriptaktivität erstellt, die [Start-AzureAutomationRunbook](/powershell/module/az.automation/start-azautomationrunbook) verwendet, um das Runbook zu starten. Nachdem Sie das konvertierte Runbook in Azure Automation importiert haben, müssen Sie diese Aktivität mit den in der Aktivität angegebenen Angaben ändern.

## <a name="create-orchestrator-assets"></a>Erstellen von Orchestrator-Objekten

Der Runbook Converter konvertiert keine Orchestrator-Objekte. Sie müssen erforderliche Orchestrator-Objekte in Azure Automation manuell erstellen.

## <a name="configure-hybrid-runbook-worker"></a>Konfigurieren von Hybrid Runbook Worker

Orchestrator speichert Runbooks auf einem Datenbankserver und führt sie auf Runbookservern aus, beides in Ihrem lokalen Rechenzentrum. Runbooks in Azure Automation werden in der Azure-Cloud gespeichert und können mit einem [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) in Ihrem lokalen Rechenzentrum ausgeführt werden. Konfigurieren Sie einen Worker zur Ausführung Ihrer aus Orchestrator konvertierten Runbooks, da sie zur Ausführung auf lokalen Servern und den Zugriff auf lokale Ressourcen konzipiert sind.

## <a name="related-articles"></a>Verwandte Artikel

* Informationen zu Orchestrator finden Sie unter [System Center 2012 – Orchestrator](/previous-versions/system-center/system-center-2012-R2/hh237242(v=sc.12)).
* Erfahren Sie mehr über das Automatisieren der Verwaltung von Diensten in [Service Management Automation](/previous-versions/system-center/system-center-2012-R2/dn469260(v=sc.12)).
* Details zu Orchestrator-Aktivitäten finden Sie unter [Orchestrator – Standardaktivitäten](/previous-versions/system-center/system-center-2012-R2/hh403832(v=sc.12)).
* Informationen zum Abrufen des Orchestrator Migration Toolkit finden Sie unter [Herunterladen des System Center Orchestrator Migration Toolkit](https://www.microsoft.com/download/details.aspx?id=47323).
* Eine Übersicht über den Azure Automation Hybrid Runbook Worker finden Sie unter [Automatisieren von Ressourcen im Datencenter oder in der Cloud mit Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).

---
title: Problembehandlung für häufige Fehler
description: Erfahren Sie, wie Sie Probleme beim Erstellen von Richtliniendefinitionen, mit dem jeweiligen SDK und dem Add-On für Kubernetes beheben.
ms.date: 10/05/2020
ms.topic: troubleshooting
ms.openlocfilehash: 98b5f1658a7d3fc7c4a7db7145b92bb6065befc5
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91999895"
---
# <a name="troubleshoot-errors-using-azure-policy"></a>Problembehandlung mit Azure Policy

Beim Erstellen von Richtliniendefinitionen, dem Arbeiten mit dem SDK oder beim Einrichten des Add-Ons [Azure Policy für Kubernetes](../concepts/policy-for-kubernetes.md) können Fehler auftreten. In diesem Artikel wird beschrieben, welche Fehler auftreten und wie diese behoben werden können.

## <a name="finding-error-details"></a>Ermitteln von Fehlerdetails

Der Ort, an dem Sie Fehlerdetails finden können, hängt von der Aktion ab, die den Fehler verursacht hat.

- Wenn Sie mit einer benutzerdefinierten Richtlinie arbeiten, verwenden Sie das Azure-Portal, um Lintingfeedback zum Schema zu erhalten, oder überprüfen Sie die resultierenden [Konformitätsdaten](../how-to/get-compliance-data.md), um festzustellen, wie Ressourcen ausgewertet wurden.
- Wenn Sie mit dem jeweiligen SDK arbeiten, bietet das SDK Details dazu, warum bei der Funktion ein Fehler auftrat.
- Wenn Sie mit dem Add-On für Kubernetes arbeiten, beginnen Sie mit dem [Protokollierung](../concepts/policy-for-kubernetes.md#logging) im Cluster.

## <a name="general-errors"></a>Allgemeine Fehler

### <a name="scenario-alias-not-found"></a>Szenario: Alias nicht gefunden

#### <a name="issue"></a>Problem

Azure Policy verwendet [Aliase](../concepts/definition-structure.md#aliases) für die Zuordnung zu Azure Resource Manager-Eigenschaften.

#### <a name="cause"></a>Ursache

In einer Richtliniendefinition wird ein falscher oder nicht vorhandener Alias verwendet.

#### <a name="resolution"></a>Lösung

Überprüfen Sie zunächst, ob die Resource Manager-Eigenschaft über einen Alias verfügt. Verwenden Sie die [Azure Policy-Erweiterung für Visual Studio Code](../how-to/extension-for-vscode.md), [Azure Resource Graph](../../resource-graph/samples/starter.md#distinct-alias-values) oder das SDK, um nach verfügbaren Aliasen zu suchen. Wenn der Alias für eine Resource Manager-Eigenschaft nicht vorhanden ist, erstellen Sie ein Supportticket.

### <a name="scenario-evaluation-details-not-up-to-date"></a>Szenario: Auswertungsdetails nicht auf dem neuesten Stand

#### <a name="issue"></a>Problem

Eine Ressource weist den Status „Nicht gestartet“ auf, oder die Konformitätsdetails sind nicht aktuell.

#### <a name="cause"></a>Ursache

Das Zuweisen einer neuen Richtlinie oder Initiative dauert etwa 30 Minuten. Neue oder aktualisierte Ressourcen im Bereich einer vorhandenen Zuweisung werden etwa 15 Minuten später verfügbar. Ein standardmäßige Konformitätsprüfung erfolgt alle 24 Stunden. Weitere Informationen finden Sie unter [Auswertungsauslöser](../how-to/get-compliance-data.md#evaluation-triggers).

#### <a name="resolution"></a>Lösung

Warten Sie zuerst den entsprechenden Zeitraum ab, bis eine Auswertung abgeschlossen ist und die Konformitätsergebnisse im Azure-Portal oder SDK zur Verfügung stehen. Informationen zum Starten einer neuen Konformitätsprüfung mit Azure PowerShell oder der REST-API finden Sie unter [Bedarfsgesteuerter Auswertungsscan](../how-to/get-compliance-data.md#on-demand-evaluation-scan).

### <a name="scenario-compliance-not-as-expected"></a>Szenario: Compliance nicht wie erwartet

#### <a name="issue"></a>Problem

Eine Ressource weist nicht den Auswertungszustand ( _Konform_ oder _Nicht konform_ ) auf, der für diese Ressource erwartet wird.

#### <a name="cause"></a>Ursache

Die Ressource befindet sich nicht im richtigen Bereich für die Richtlinienzuweisung, oder die Richtliniendefinition funktioniert nicht wie vorgesehen.

#### <a name="resolution"></a>Lösung

Befolgen Sie diese Schritte zur Problembehandlung bei der Definition Ihrer Richtlinie:

1. Warten Sie zuerst den entsprechenden Zeitraum ab, bis eine Auswertung abgeschlossen ist und die Konformitätsergebnisse im Azure-Portal oder SDK zur Verfügung stehen. Informationen zum Starten einer neuen Konformitätsprüfung mit Azure PowerShell oder der REST-API finden Sie unter [Bedarfsgesteuerter Auswertungsscan](../how-to/get-compliance-data.md#on-demand-evaluation-scan).
1. Überprüfen Sie, ob die Zuordnungsparameter und der Zuordnungsbereich ordnungsgemäß festgelegt sind.
1. Überprüfen Sie den [Modus der Richtliniendefinition](../concepts/definition-structure.md#mode):
   - Modus „all“ für alle Ressourcentypen
   - Modus „indexed“, wenn bei der Richtliniendefinition eine Überprüfung auf Tags oder den Ort durchgeführt wird
1. Überprüfen Sie, dass der Bereich der Ressource nicht [ausgeschlossen](../concepts/assignment-structure.md#excluded-scopes) oder [ausgenommen](../concepts/exemption-structure.md) ist.
1. Wenn die Konformität für eine Richtlinienzuweisung mit `0/0` Ressourcen angezeigt wird, wurden keine Ressourcen für die Anwendung innerhalb des Zuweisungsbereichs ermittelt. Überprüfen Sie die Richtliniendefinition und den Zuweisungsbereich.
1. Bei einer nicht konformen Ressource, bei der Konformität erwartet wurde, [überprüfen Sie die Ermittlung der Ursachen für Nichtkonformität](../how-to/determine-non-compliance.md). Der Vergleich der Definition mit dem ausgewerteten Eigenschaftswert gibt an, warum eine Ressource nicht konform war.
   - Wenn der **Zielwert** falsch ist, überarbeiten Sie die Richtliniendefinition.
   - Wenn der **aktuelle Wert** falsch ist, überprüfen Sie die Ressourcennutzlast mit `resources.azure.com`.
1. Überprüfen Sie [Problembehandlung: Erzwingung nicht wie erwartet](#scenario-enforcement-not-as-expected) auf andere häufige Probleme und Lösungen.

Wenn Sie immer noch ein Problem mit Ihrer duplizierten und angepassten integrierten Richtliniendefinition oder benutzerdefinierten Definition haben, erstellen Sie ein Supportticket unter **Erstellen einer Richtlinie** , um das Problem ordnungsgemäß weiterzuleiten.

### <a name="scenario-enforcement-not-as-expected"></a>Szenario: Erzwingung nicht wie erwartet

#### <a name="issue"></a>Problem

Für eine Ressource, für die eine Aktion von Azure Policy erwartet wird, wird keine Aktion ausgeführt, und es gibt keinen Eintrag im [Azure-Aktivitätsprotokoll](../../../azure-monitor/platform/platform-logs-overview.md).

#### <a name="cause"></a>Ursache

Für die Richtlinienzuweisung ist [enforcementMode](../concepts/assignment-structure.md#enforcement-mode) als _Disabled_ konfiguriert. Bei deaktiviertem Erzwingungsmodus wird die Richtlinienauswirkung nicht erzwungen, und es erfolgt kein Eintrag im Aktivitätsprotokoll.

#### <a name="resolution"></a>Lösung

Befolgen Sie diese Schritte zur Problembehandlung bei der Erzwingung Ihrer Richtlinienzuweisung:

1. Warten Sie zuerst den entsprechenden Zeitraum ab, bis eine Auswertung abgeschlossen ist und die Konformitätsergebnisse im Azure-Portal oder SDK zur Verfügung stehen. Informationen zum Starten einer neuen Konformitätsprüfung mit Azure PowerShell oder der REST-API finden Sie unter [Bedarfsgesteuerter Auswertungsscan](../how-to/get-compliance-data.md#on-demand-evaluation-scan).
1. Prüfen Sie, ob die Zuweisungsparameter und der Zuweisungsbereich ordnungsgemäß festgelegt und **enforcementMode** auf _Enabled_ eingestellt ist. 
1. Überprüfen Sie den [Modus der Richtliniendefinition](../concepts/definition-structure.md#mode):
   - Modus „all“ für alle Ressourcentypen
   - Modus „indexed“, wenn bei der Richtliniendefinition eine Überprüfung auf Tags oder den Ort durchgeführt wird
1. Überprüfen Sie, dass der Bereich der Ressource nicht [ausgeschlossen](../concepts/assignment-structure.md#excluded-scopes) oder [ausgenommen](../concepts/exemption-structure.md) ist.
1. Stellen Sie sicher, dass die Ressourcennutzlast mit der Richtlinienlogik übereinstimmt. Dies ist möglich, indem Sie eine [HAR-Ablaufverfolgung](../../../azure-portal/capture-browser-trace.md) erfassen oder die Eigenschaften der ARM-Vorlage überprüfen.
1. Überprüfen Sie [Problembehandlung: Compliance nicht wie erwartet](#scenario-compliance-not-as-expected) auf andere häufige Probleme und Lösungen.

Wenn Sie immer noch ein Problem mit Ihrer duplizierten und angepassten integrierten Richtliniendefinition oder benutzerdefinierten Definition haben, erstellen Sie ein Supportticket unter **Erstellen einer Richtlinie** , um das Problem ordnungsgemäß weiterzuleiten.

### <a name="scenario-denied-by-azure-policy"></a>Szenario: Von Azure Policy abgelehnt

#### <a name="issue"></a>Problem

Das Erstellen oder Aktualisieren einer Ressource wird abgelehnt.

#### <a name="cause"></a>Ursache

Eine Richtlinienzuweisung für den Bereich, in dem sich die neue oder aktualisierte Ressource befindet, erfüllt die Kriterien einer Richtliniendefinition mit einer [deny](../concepts/effects.md#deny)-Auswirkung. Ressourcen, die diesen Definitionen entsprechen, können nicht erstellt oder aktualisiert werden.

#### <a name="resolution"></a>Lösung

Die Fehlermeldung bei einer Richtlinienzuweisung vom Typ „deny“ umfasst die Richtliniendefinition und die Richtlinienzuweisungs-IDs. Wenn die Fehlerinformationen in der Nachricht fehlen, stehen diese auch im [Aktivitätsprotokoll](../../../azure-monitor/platform/activity-log.md#view-the-activity-log) zur Verfügung. Diese Informationen geben Ihnen weitere Details, um die Ressourceneinschränkungen zu verstehen und die Ressourceneigenschaften in der Anforderung so anzupassen, dass sie zulässigen Werten entsprechen.

## <a name="template-errors"></a>Vorlagenfehler

### <a name="scenario-policy-supported-functions-processed-by-template"></a>Szenario: Von Policy unterstützte Funktionen werden von der Vorlage verarbeitet

#### <a name="issue"></a>Problem

Azure Policy unterstützt eine Reihe von Azure Resource Manager-Vorlagenfunktionen und Funktionen, die nur in einer Richtliniendefinition verfügbar sind. Resource Manager verarbeitet diese Funktionen im Rahmen einer Bereitstellung und nicht als Teil einer Richtliniendefinition.

#### <a name="cause"></a>Ursache

Die Verwendung unterstützter Funktionen wie `parameter()` oder `resourceGroup()` führt dazu, dass das verarbeitete Ergebnis der Funktion zum Zeitpunkt der Bereitstellung festgelegt wird, anstatt die Funktion von der Richtliniendefinition und der Azure Policy-Engine verarbeiten zu lassen.

#### <a name="resolution"></a>Lösung

Um eine Funktion so zu übergeben, dass sie Teil einer Richtliniendefinition ist, versehen Sie die gesamte Zeichenfolge mit dem Escapezeichen `[`, sodass die Eigenschaft folgendermaßen aussieht: `[[resourceGroup().tags.myTag]`. Das Escapezeichen bewirkt, dass Resource Manager den Wert beim Verarbeiten der Vorlage als Zeichenfolge behandelt. Azure Policy fügt die Funktion dann in die Richtliniendefinition ein, sodass sie wie erwartet dynamisch ist. Weitere Informationen finden Sie unter [Syntax und Ausdrücke in Azure Resource Manager-Vorlagen](../../../azure-resource-manager/templates/template-expressions.md).

## <a name="add-on-installation-errors"></a>Add-On-Installationsfehler

### <a name="scenario-install-using-helm-chart-fails-on-password"></a>Szenario: Fehler beim Installieren per Helm-Chart beim Kennwort

#### <a name="issue"></a>Problem

Der Befehl `helm install azure-policy-addon` schlägt mit einer der folgenden Meldungen fehl:

- `!: event not found`
- `Error: failed parsing --set data: key "<key>" has no value (cannot end with ,)`

#### <a name="cause"></a>Ursache

Das generierte Kennwort enthält ein Komma (`,`), das beim Helm-Chart eine Aufteilung bewirkt.

#### <a name="resolution"></a>Lösung

Versehen Sie das Komma (`,`) im Kennwortwert beim Ausführen von `helm install azure-policy-addon` mit einem umgekehrten Schrägstrich (`\`) als Escapezeichen.

### <a name="scenario-install-using-helm-chart-fails-as-name-already-exists"></a>Szenario: Fehler beim Installieren per Helm-Chart, da der Name bereits vorhanden ist

#### <a name="issue"></a>Problem

Der Befehl `helm install azure-policy-addon` schlägt mit folgender Meldung fehl:

- `Error: cannot re-use a name that is still in use`

#### <a name="cause"></a>Ursache

Das Helm-Chart mit dem Namen `azure-policy-addon` wurde bereits installiert oder teilweise installiert.

#### <a name="resolution"></a>Lösung

Befolgen Sie die Anweisungen zum [Entfernen des Azure Policy für Kubernetes-Add-Ons](../concepts/policy-for-kubernetes.md#remove-the-add-on), und führen Sie dann den Befehl `helm install azure-policy-addon` erneut aus.

### <a name="scenario-azure-virtual-machine-user-assigned-identities-are-replaced-by-system-assigned-managed-identities"></a>Szenario: Die vom Benutzer zugewiesenen Identitäten auf dem virtuellen Azure-Computer werden durch vom System zugewiesene, verwaltete Identitäten ersetzt.

#### <a name="issue"></a>Problem

Nach der Zuweisung von Richtlinieninitiativen für die Gastkonfiguration zu Überwachungseinstellungen innerhalb von Computern werden dem Computer zugewiesene verwaltete Identitäten, die dem Benutzer zugewiesen wurden, nicht mehr zugewiesen. Es wird nur eine vom System zugewiesene verwaltete Identität zugewiesen.

#### <a name="cause"></a>Ursache

Die Richtliniendefinitionen, die zuvor in den Definitionen von DeployIfNotExists für die Gastkonfiguration verwendet wurden, stellten sicher, dass dem Computer eine vom System zugewiesene Identität zugewiesen wird, entfernten aber auch vom Benutzer zugewiesene Identitätszuweisungen.

#### <a name="resolution"></a>Lösung

Die Definitionen, die zuvor dieses Problem verursacht haben, werden als \[Veraltet\] angezeigt und durch Richtliniendefinitionen ersetzt, die die Voraussetzungen verwalten, ohne die vom Benutzer zugewiesene verwaltete Identität zu entfernen. Ein manueller Schritt ist erforderlich. Löschen Sie alle vorhandenen Richtlinienzuweisungen, die mit \[Veraltet\] gekennzeichnet sind, und ersetzen Sie sie durch die aktualisierte erforderliche Richtlinieninitiative und Richtliniendefinitionen, die denselben Namen wie das Original aufweisen.

Eine ausführliche Beschreibung finden Sie im folgenden Blogbeitrag:

[Wichtige Änderung für Überwachungsrichtlinien für Gastkonfigurationen](https://techcommunity.microsoft.com/t5/azure-governance-and-management/important-change-released-for-guest-configuration-audit-policies/ba-p/1655316)

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

- Erhalten Sie Antworten von Experten über [Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-policy.html).
- Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
- Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus.

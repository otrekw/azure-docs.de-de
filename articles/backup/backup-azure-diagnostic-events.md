---
title: Verwenden von Diagnoseeinstellungen für Recovery Services-Tresore
description: In diesem Artikel wird beschrieben, wie die alten und neuen Diagnoseereignisse für Azure Backup verwendet werden.
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: d10bedf3818559971eff12624152d0e797f6c3cc
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672787"
---
# <a name="using-diagnostics-settings-for-recovery-services-vaults"></a>Verwenden von Diagnoseeinstellungen für Recovery Services-Tresore

Azure Backup sendet Diagnoseereignisse, die gesammelt und dann für die Analyse, Warnung und Berichterstattung verwendet werden können. 

Sie können Diagnoseereignisse für einen Recovery Services-Tresor über das Azure-Portal konfigurieren, indem Sie zum Tresor navigieren und auf das Menüelement **Diagnoseeinstellungen** klicken. Wenn Sie auf **Add Diagnostic Setting** (Diagnoseeinstellung hinzufügen) klicken, können Sie ein oder mehrere Diagnoseereignisse an ein Speicherkonto, einen Event Hub, oder einen Log Analytics-Arbeitsbereich (LA) senden.

![Blatt „Diagnoseeinstellungen“](./media/backup-azure-diagnostics-events/diagnostics-settings-blade.png)

## <a name="diagnostics-events-available-for-azure-backup-users"></a>Diagnoseereignisse für Azure Backup-Benutzer verfügbar

Azure Backup bietet die folgenden Diagnoseereignisse, die jeweils ausführliche Daten zu einer bestimmten Reihe von sicherungsbezogenen Artefakten bereitstellen:

* CoreAzureBackup
* AddonAzureBackupAlerts
* AddonAzureBackupProtectedInstance
* AddonAzureBackupJobs
* AddonAzureBackupPolicy
* AddonAzureBackupStorage

[Datenmodell für Azure Backup-Diagnoseereignisse](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)

Daten für diese Ereignisse können an ein Speicherkonto, einen Log Analytics-Arbeitsbereich oder einen Event Hub gesendet werden. Wenn Sie diese Daten an einen Log Analytics-Arbeitsbereich senden, müssen Sie auf den Umschalter **Resource Specific** (Ressourcenspezifisch) auf dem Bildschirm **Diagnoseeinstellungen** klicken (weitere Informationen in den folgenden Abschnitten).

## <a name="using-diagnostics-settings-with-log-analytics-la"></a>Verwenden von Diagnoseeinstellungen mit Log Analytics (LA)

Mit der Ausrichtung auf die Azure Log Analytics-Roadmap ermöglicht Azure Backup für die Sicherung jetzt das Senden von Tresordiagnosedaten an dedizierte Log Analytics-Tabellen. Diese werden als [ressourcenspezifische Tabellen](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace#resource-specific) bezeichnet.

So senden Sie Tresordiagnosedaten an Log Analytics:

1.    Navigieren Sie zum Tresor, und klicken Sie auf **Diagnoseeinstellungen**. Klicken Sie auf **+ Add Diagnostic Setting** (Diagnoseeinstellung hinzufügen).
2.    Geben Sie einen Namen für die Diagnoseeinstellung ein.
3.    Aktivieren Sie das Kontrollkästchen **Send to Log Analytics** (An Log Analytics senden), und wählen Sie einen Log Analytics-Arbeitsbereich aus.
4.    Klicken Sie beim Umschalter auf **Resource Specific** (Ressourcenspezifisch), und aktivieren Sie die folgenden sechs Ereignisse: **CoreAzureBackup**, **AddonAzureBackupAlerts**, **AddonAzureBackupProtectedInstance**, **AddonAzureBackupJobs**, **AddonAzureBackupPolicy** und **AddonAzureBackupStorage**.
5.    Klicken Sie auf **Speichern**.

![Modus „Ressourcenspezifisch“](./media/backup-azure-diagnostics-events/resource-specific-blade.png)

Sobald die Daten in den Log Analytics-Arbeitsbereich fließen, werden im Arbeitsbereich für jedes dieser Ereignisse dedizierte Tabellen erstellt. Sie können jede dieser Tabellen direkt abfragen und für diese Tabellen ggf. Joins oder Vereinigungen durchführen.

> [!IMPORTANT]
> Die oben genannten sechs Ereignisse (CoreAzureBackup, AddonAzureBackupAlerts, AddonAzureBackupProtectedInstance, AddonAzureBackupJobs, AddonAzureBackupPolicy und AddonAzureBackupStorage) werden **nur** im Modus „Resource Specific“ (Ressourcenspezifisch) in [Sicherungsberichten](https://docs.microsoft.com/azure/backup/configure-reports) unterstützt. **Beachten Sie, dass beim Versuch, Daten für diese sechs Ereignisse im Modus „Azure-Diagnose“ zu senden, keine Daten in Sicherungsberichten angezeigt werden.**

## <a name="legacy-event"></a>Legacyereignis

Bisher waren alle sicherungsbezogenen Diagnosedaten für einen Tresor in einem einzelnen Ereignis namens „AzureBackupReport“ enthalten. Die sechs oben beschriebenen Ereignisse sind im Wesentlichen eine Aufschlüsselung aller Daten, die in „AzureBackupReport“ enthalten sind. 

Auch aktuell unterstützen wir die Abwärtskompatibilität für das „AzureBackupReport“-Ereignis, wenn Benutzer über benutzerdefinierte Abfragen wie beispielsweise benutzerdefinierte Protokollwarnungen oder Visualisierungen für dieses Ereignis verfügen. **Es wird jedoch empfohlen, so früh wie möglich zu den [neuen Ereignissen](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#diagnostics-events-available-for-azure-backup-users) zu wechseln**, da so die Arbeit mit den Daten in Protokollabfragen erleichtert wird, eine bessere Auffindbarkeit von Schemas und deren Struktur geboten wird und die Leistung sowohl in Hinsicht auf die Erfassungslatenz als auch die Abfragezeiten verbessert werden kann. 

**Das Legacy-Ereignis im Modus „Azure-Diagnose“ wird schließlich als veraltet gekennzeichnet, und es ist daher hilfreich, die neuen Ereignisse zu verwenden, um komplexe Migrationsvorgänge zu einem späteren Zeitpunkt zu vermeiden.** Für unsere [Berichterstellungslösung](https://docs.microsoft.com/azure/backup/configure-reports), die Log Analytics nutzt, wird die Unterstützung von Daten aus dem Legacy-Ereignis ebenfalls eingestellt.

### <a name="steps-to-move-to-new-diagnostics-settings-to-log-analytics-workspace"></a>Schritte zum Wechseln zu den neuen Diagnoseeinstellungen (zum Log Analytics-Arbeitsbereich)

1. Ermitteln Sie mithilfe des Legacy-Ereignisses, welche Tresore Daten an die Log Analytics-Arbeitsbereiche senden und zu welchen Abonnements sie gehören. Führen Sie die folgenden Arbeitsbereiche aus, um diese Tresore und Abonnements zu ermitteln:

    ````Kusto
    let RangeStart = startofday(ago(3d));
    let VaultUnderAzureDiagnostics = (){
        AzureDiagnostics
        | where TimeGenerated >= RangeStart | where Category == "AzureBackupReport" and OperationName == "Vault" and SchemaVersion_s == "V2"
        | summarize arg_max(TimeGenerated, *) by ResourceId    
        | project ResourceId, Category};
    let VaultUnderResourceSpecific = (){
        CoreAzureBackup
        | where TimeGenerated >= RangeStart | where OperationName == "Vault" 
        | summarize arg_max(TimeGenerated, *) by ResourceId
        | project ResourceId, Category};
        // Some Workspaces will not have AzureDiagnostics Table, hence you need to use isFuzzy
    let CombinedVaultTable = (){
        CombinedTable | union isfuzzy = true 
        (VaultUnderAzureDiagnostics() ),
        (VaultUnderResourceSpecific() )
        | distinct ResourceId, Category};
    CombinedVaultTable | where Category == "AzureBackupReport"
    | join kind = leftanti ( 
    CombinedVaultTable | where Category == "CoreAzureBackup"
    ) on ResourceId
    | parse ResourceId with * "SUBSCRIPTIONS/" SubscriptionId:string "/RESOURCEGROUPS" * "MICROSOFT.RECOVERYSERVICES/VAULTS/" VaultName:string
    | project ResourceId, SubscriptionId, VaultName
    ````

2. Verwenden Sie die [integrierte Azure-Richtlinie](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics) für Azure Backup, um für alle Tresore in einem angegebenen Bereich eine neue Diagnoseeinstellung hinzuzufügen. Diese Richtlinie fügt diesen Tresoren, die entweder nicht über eine Diagnoseeinstellung verfügen oder nur eine Einstellung für die Legacy-Diagnose aufweisen, eine neue Diagnoseeinstellung hinzu. Diese Richtlinie kann jeweils nur einem gesamten Abonnement oder einer Ressourcengruppe zugewiesen werden. Beachten Sie, dass Sie für jedes Abonnement, dem die Richtlinie zugewiesen wird, die Zugriffsberechtigung „Besitzer“ benötigen.

Sie können separate Diagnoseeinstellungen für „AzureBackupReport“ und die sechs neuen Ereignisse verwenden, bis Sie alle benutzerdefinierten Abfragen migriert haben, um Daten aus den neuen Tabellen zu verwenden. Die folgende Abbildung zeigt ein Beispiel für einen Tresor mit zwei Diagnoseeinstellungen. Die erste Einstellung mit dem Namen **Setting1** sendet Daten des „AzureBackupReport“-Ereignisses an einen Log Analytics-Arbeitsbereich im Modus „Azure-Diagnose“. Die zweite Einstellung mit dem Namen **Setting2** sendet Daten der sechs neuen Azure Backup-Ereignisse an einen Log Analytics-Arbeitsbereich im Modus „Resource Specific“ (Ressourcenspezifisch).

![Beide Einstellungen](./media/backup-azure-diagnostics-events/two-settings-example.png)

> [!IMPORTANT]
> Das „AzureBackupReport“-Ereignis wird **nur** im Modus „Azure-Diagnose“ unterstützt. **Beachten Sie, dass beim Versuch, Daten für dieses Ereignis im Modus „Resource Specific“ (Ressourcenspezifisch) zu senden, keine Daten an den Log Analytics-Arbeitsbereich übermittelt werden.**

> [!NOTE]
> Der Umschalter für Azure-Diagnose/Resource Specific (Ressourcenspezifisch) wird nur angezeigt, wenn der Benutzer die Option **Send to Log Analytics** (An Log Analytics senden) aktiviert hat. Ein Benutzer kann ohne zusätzliche Eingaben einfach das erforderliche Ziel auswählen und alle gewünschten Ereignisse auswählen, um Daten an ein Speicherkonto oder einen Event Hub zu senden. Es wird auch empfohlen, das Vorgängerereignis „AzureBackupReport“ nicht auszuwählen.

## <a name="users-sending-azure-site-recovery-events-to-log-analytics-la"></a>Benutzerseitiges Senden von Azure Site Recovery-Ereignissen an Log Analytics (LA)

Azure Backup- und Azure Site Recovery-Ereignisse werden von demselben Recovery Services-Tresor gesendet. Da Azure Site Recovery zurzeit nicht in ressourcenspezifische Tabellen integriert ist, können Benutzer, die Azure Site Recovery-Ereignisse an Log Analytics senden möchten, **nur** den Modus „Azure-Diagnose“ verwenden (siehe Abbildung unten). **Wenn Sie für die Azure Site Recovery-Ereignisse den Modus „Resource Specific“ (Ressourcenspezifisch) auswählen, wird verhindert, dass die erforderlichen Daten an den Log Analytics-Arbeitsbereich gesendet werden.**

![Site Recovery-Ereignisse](./media/backup-azure-diagnostics-events/site-recovery-settings.png)

Zusammenfassung der obigen Schritte:

* Wenn Sie die Log Analytics-Diagnose bereits mit der Azure-Diagnose eingerichtet haben und zusätzlich über benutzerdefinierte Abfragen verfügen, behalten Sie diese **interact**-Einstellung bei, bis Sie Ihre Abfragen migrieren, um Daten aus den neuen Ereignissen zu verwenden.
* Erstellen Sie eine **neue** Diagnoseeinstellung, klicken Sie auf **Resource Specific** (Ressourcenspezifisch), und wählen Sie wie oben angegeben die sechs neuen Ereignisse aus, wenn Sie diese außerdem (wie empfohlen) in neue Tabellen integrieren möchten.
* Wenn Sie zurzeit Azure Site Recovery-Ereignisse an Log Analytics senden, sollten Sie für diese Ereignisse **nicht** den Modus „Resource Specific“ (Ressourcenspezifisch) auswählen, da für diese Ereignisse sonst keine Daten in Ihren Log Analytics-Arbeitsbereich übertragen werden. Erstellen Sie stattdessen eine **zusätzliche Diagnoseeinstellung**, klicken Sie auf **Azure-Diagnose**, und wählen Sie die relevanten Azure Site Recovery-Ereignisse aus.

Die folgende Abbildung zeigt ein Beispiel für einen Benutzer, der über drei Diagnoseeinstellungen für einen Tresor verfügt. Die erste Einstellung mit dem Namen **Setting1** sendet Daten des „AzureBackupReport“-Ereignisses an einen Log Analytics-Arbeitsbereich im Modus „Azure-Diagnose“. Die zweite Einstellung mit dem Namen **Setting2** sendet Daten der sechs neuen Azure Backup-Ereignisse an einen Log Analytics-Arbeitsbereich im Modus „Resource Specific“ (Ressourcenspezifisch). Die dritte Einstellung mit dem Namen **Setting3** sendet Daten der Azure Site Recovery-Ereignisse an einen Log Analytics-Arbeitsbereich im Modus „Azure-Diagnose“.

![Drei Einstellungen](./media/backup-azure-diagnostics-events/three-settings-example.png)

## <a name="next-steps"></a>Nächste Schritte

[Informationen zum Log Analytics-Datenmodell für Diagnoseereignisse](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)

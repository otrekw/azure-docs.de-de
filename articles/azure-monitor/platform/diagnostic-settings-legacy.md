---
title: Erfassen des Azure-Aktivitätsprotokolls mit Diagnoseeinstellungen (Vorschau) – Azure Monitor | Microsoft-Dokumentation
description: Verwenden Sie Diagnoseeinstellungen, um Azure-Aktivitätsprotokolle an Azure Monitor-Protokolle, Azure Storage oder Azure Event Hubs weiterzuleiten.
author: bwren
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
ms.author: bwren
ms.date: 12/20/2019
ms.openlocfilehash: 55efdfe2bb1b37e566654b8041f2cf5ed411cc3f
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75977566"
---
# <a name="collect-azure-activity-log-with-legacy-settings"></a>Erfassen des Azure-Aktivitätsprotokolls mit Legacyeinstellungen
Das [Azure-Aktivitätsprotokoll](platform-logs-overview.md) ist ein [Plattformprotokoll](platform-logs-overview.md), das einen Einblick in Ereignisse auf Abonnementebene bietet, die in Azure aufgetreten sind. Bis vor Kurzem haben Sie ein Protokollprofil erstellt, um Aktivitätsprotokolleinträge an [einen Event Hub oder ein Speicherkonto](activity-log-export.md) zu senden, und einen Connector verwendet, um sie in einem [Log Analytics-Arbeitsbereich](activity-log-collect.md) zu erfassen. In diesem Artikel werden die Unterschiede zwischen den Methoden, das Arbeiten mit vorhandenen Legacyeinstellungen und das Löschen von Legacyeinstellungen in Vorbereitung auf Diagnoseeinstellungen beschrieben.


## <a name="differences-between-methods"></a>Unterschiede zwischen den Methoden

### <a name="advantages"></a>Vorteile
Das Verwenden von Diagnoseeinstellungen bietet gegenüber den derzeitigen Methoden die folgenden Vorteile:

- Konsistente Methode zum Erfassen aller Plattformprotokolle
- Erfassen des Aktivitätsprotokolls übergreifend für mehrere Abonnements und Mandanten
- Filtern der Erfassung, um nur Protokolle für bestimmte Kategorien zu erfassen
- Sammeln aller Aktivitätsprotokollkategorien. Einige Kategorien werden mit der Legacymethode nicht erfasst.
- Niedrigere Latenz für Protokollerfassung. Die vorherige Methode weist eine Latenz von ungefähr 15 Minuten auf, während sich die Zeit bei Diagnoseeinstellungen nur um etwa eine Minute verlängert.

### <a name="considerations"></a>Überlegungen
Beachten Sie die folgenden Details der Aktivitätsprotokollerfassung mithilfe von Diagnoseeinstellungen, bevor Sie diese Funktion aktivieren.

- Die Aufbewahrungseinstellung für die Erfassung des Aktivitätsprotokolls in Azure Storage wurde entfernt. Das bedeutet, dass Daten unbegrenzt gespeichert werden, bis Sie sie entfernen.
- Derzeit können Sie eine Diagnoseeinstellung auf Abonnementebene nur über das Azure-Portal erstellen. Wenn Sie andere Methoden wie PowerShell oder die CLI verwenden möchten, können Sie eine Resource Manager-Vorlage erstellen.


### <a name="differences-in-data"></a>Unterschiede bei den Daten
Mit Diagnoseeinstellungen werden dieselben Daten erfasst wie mit den vorherigen Methoden zur Erfassung des Aktivitätsprotokolls, jedoch derzeit mit folgenden Unterschieden:

Die folgenden Eigenschaften wurden entfernt:

- ActivityStatus
- ActivitySubstatus
- Vorgangsname
- ResourceProvider

Die folgenden Eigenschaften wurden hinzugefügt:

- Authorization_d
- Claims_d
- Properties_d

## <a name="work-with-legacy-settings"></a>Arbeiten mit Legacyeinstellungen
Legacyeinstellungen für das Erfassen des Aktivitätsprotokolls funktionieren weiterhin, wenn Sie diese nicht durch eine Diagnoseeinstellung ersetzen. Verwenden Sie die folgende Methode, um das Protokollprofil für ein Abonnement zu verwalten.

1. Wählen Sie im Azure-Portal im Menü **Azure Monitor** die Option **Aktivitätsprotokoll** aus.
3. Klicken Sie auf **Diagnoseeinstellungen**.

   ![Diagnoseeinstellungen](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Klicken Sie auf das lila Banner, um die Legacybenutzeroberfläche anzuzeigen.

    ![Legacybenutzeroberfläche](media/diagnostic-settings-subscription/legacy-experience.png)


In den folgenden Artikeln finden Sie ausführliche Informationen zur Verwendung der Legacy-Erfassungsmethoden.

- [Erfassen und Analysieren von Azure-Aktivitätsprotokollen im Log Analytics-Arbeitsbereich in Azure Monitor](activity-log-collect.md)
- [Erfassen von Azure-Aktivitätsprotokollen in Azure Monitor über Azure Active Directory-Mandanten hinweg](activity-log-collect-tenants.md)
- [Exportieren des Aktivitätsprotokolls in den Speicher oder in Azure Event Hubs](activity-log-export.md)

## <a name="disable-existing-settings"></a>Deaktivieren vorhandener Einstellungen
Sie sollten die vorhandene Erfassung der Aktivität deaktivieren, bevor Sie die Erfassung mithilfe von Diagnoseeinstellungen aktivieren. Wenn beide Erfassungsmethoden aktiviert sind, kann dies zu doppelten Daten führen.

### <a name="disable-collection-into-log-analytics-workspace"></a>Deaktivieren der Erfassung im Log Analytics-Arbeitsbereich

1. Öffnen Sie im Azure-Portal das Menü **Log Analytics-Arbeitsbereiche**, und wählen Sie den Arbeitsbereich zum Erfassen des Aktivitätsprotokolls aus.
2. Wählen Sie im Menü des Arbeitsbereichs im Abschnitt **Arbeitsbereichsdatenquellen** die Option **Azure-Aktivitätsprotokoll** aus.
3. Klicken Sie auf das Abonnement, das Sie trennen möchten.
4. Klicken Sie auf **Trennen** und dann auf **Ja**, wenn Sie zum Bestätigen Ihrer Auswahl aufgefordert werden.

### <a name="disable-log-profile"></a>Deaktivieren des Protokollprofils

1. Verwenden Sie das unter [Arbeiten mit Legacyeinstellungen](#work-with-legacy-settings) beschriebene Verfahren, um Legacyeinstellungen zu öffnen.
2. Deaktivieren Sie alle derzeitigen Erfassungen im Speicher oder in Event Hubs.



## <a name="activity-log-monitoring-solution"></a>Überwachungslösung des Aktivitätsprotokolls
Die Azure Log Analytics-Überwachungslösung umfasst mehrere Protokollabfragen und Ansichten zum Analysieren der Aktivitätsprotokolldatensätze in Ihrem Log Analytics-Arbeitsbereich. Diese Lösung verwendet Protokolldaten, die in einem Log Analytics-Arbeitsbereich erfasst werden, und kann weiterhin ohne Änderungen verwendet werden, wenn Sie das Aktivitätsprotokoll mithilfe von Diagnoseeinstellungen erfassen. Ausführliche Informationen zu dieser Lösung finden Sie unter [Überwachungslösung der Aktivitätsprotokollanalyse](activity-log-collect.md#activity-logs-analytics-monitoring-solution).

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Aktivitätsprotokoll](../../azure-resource-manager/management/view-activity-logs.md)
* [Weitere Informationen zu Diagnoseeinstellungen](diagnostic-settings.md)

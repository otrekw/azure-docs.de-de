---
title: Erstellen von Warnungen für die Azure Automation-Updateverwaltung
description: In diesem Artikel erfahren Sie, wie Sie Azure-Warnungen konfigurieren, um über den Status von Updatebewertungen oder -bereitstellungen benachrichtigt zu werden.
services: automation
ms.subservice: update-management
ms.date: 03/15/2021
ms.topic: conceptual
ms.openlocfilehash: 224a7b5457a099fd763ac657349fc5497824ab76
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104601414"
---
# <a name="how-to-create-alerts-for-update-management"></a>Erstellen von Warnungen für die Updateverwaltung

Warnungen in Azure benachrichtigen Sie proaktiv über Ergebnisse von Runbookaufträgen, Probleme mit der Dienstintegrität oder andere Szenarien im Zusammenhang mit Ihrem Automation-Konto. Azure Automation enthält keine vorkonfigurierten Warnungsregeln, Sie können jedoch eigene Regeln basierend auf den generierten Daten erstellen. Dieser Artikel enthält Anleitungen zum Erstellen von Warnungsregeln mithilfe der in der Updateverwaltung enthaltenen Metriken.

## <a name="available-metrics"></a>Verfügbare Metriken

Azure Automation erstellt zwei unterschiedliche Plattformmetriken im Zusammenhang mit der Updateverwaltung, die gesammelt und an Azure Monitor weitergeleitet werden. Diese Metriken sind für die Analyse (über [Metrik-Explorer](../../azure-monitor/essentials/metrics-charts.md)) und für Warnungen (über eine [Metrikwarnungsregel](../../azure-monitor/alerts/alerts-metric.md)) verfügbar.

Die beiden folgenden Metriken werden ausgegeben:

* Gesamtzahl von Updatebereitstellungsausführungen auf dem Computer
* Gesamtzahl von Updatebereitstellungsausführungen

Bei der Verwendung für Warnungen unterstützen beide Metriken Dimensionen mit zusätzlichen Informationen, um den Bereich der Warnung auf ein bestimmtes Updatebereitstellungsdetail zu beschränken. In der folgenden Tabelle werden die Details zur Metrik und zu den Dimensionen angezeigt, die beim Konfigurieren einer Warnung verfügbar sind:

|Signalname|Dimensionen|Beschreibung
|---|---|---|
|`Total Update Deployment Runs`|- Name der Updatebereitstellung<br>- Status | Warnungen zum allgemeinen Status einer Updatebereitstellung|
|`Total Update Deployment Machine Runs`|- Name der Updatebereitstellung</br>- Status</br>- Zielcomputer</br>- ID der Updatebereitstellungsausführung    |Warnungen zum Status einer Updatebereitstellung für bestimmte Computer.|

## <a name="create-alert"></a>Erstellen einer Warnung

Gehen Sie wie folgt vor, um Warnungen einzurichten, die Sie über den Status einer Updatebereitstellung informieren: Wenn Sie noch nicht mit Azure-Warnungen vertraut sind, lesen Sie die Informationen unter [Überblick über Warnungen in Microsoft Azure](../../azure-monitor/alerts/alerts-overview.md).

1. Wählen Sie in Ihrem Automation-Konto unter **Überwachung** die Option **Warnungen** und dann **Neue Warnungsregel** aus.

1. Auf der Seite **Warnungsregel erstellen** ist Ihr Automation-Konto bereits als Ressource ausgewählt. Falls Sie es ändern möchten, wählen Sie **Ressource bearbeiten** aus.

1. Wählen Sie auf der Seite „Ressource auswählen“ in der Dropdownliste **Nach Ressourcentyp filtern** die Option **Automation-Konten** aus.

1. Wählen Sie das Automation-Konto, das Sie verwenden möchten, und dann **Fertig** aus.

1. Klicken Sie auf **Bedingung hinzufügen**, um das geeignete Signal für Ihre Anforderungen auszuwählen.

1. Wählen Sie als Dimension einen gültigen Wert aus der Liste aus. Falls der gewünschte Wert nicht in der Liste enthalten ist, wählen Sie neben der Dimension **\+** aus, und geben Sie den benutzerdefinierten Namen ein. Wählen Sie anschließend den gewünschten Suchwert aus. Wenn Sie alle Werte für eine Dimension auswählen möchten, klicken Sie auf die Schaltfläche **Auswählen \*** . Wenn Sie für eine Dimension keinen Wert auswählen, wird diese Dimension von der Updateverwaltung ignoriert.

    ![Konfigurieren der Signallogik](./media/manage-updates-for-vm/signal-logic.png)

1. Geben Sie unter **Warnungslogik** Werte in den Feldern **Zeitaggregation** und **Schwellenwert** ein, und wählen Sie dann **Fertig** aus.

1. Geben Sie auf der nächsten Seite einen Namen und eine Beschreibung für die Warnung ein.

1. Legen Sie das Feld **Schweregrad** für eine erfolgreiche Ausführung auf **Information (Schweregrad 2)** bzw. für eine fehlerhafte Ausführung auf **Information (Schweregrad 1)** fest.

    ![Screenshot: Abschnitt „Details der Warnungsregel definieren“ mit Hervorhebung der Felder „Name der Warnungsregel“, „Beschreibung“ und „Schweregrad“](./media/manage-updates-for-vm/define-alert-details.png)

1. Wählen Sie zum Aktivieren der Warnungsregel **Ja** aus.

## <a name="configure-action-groups-for-your-alerts"></a>Konfigurieren von Aktionsgruppen für Ihre Warnungen

Nachdem Sie Ihre Warnungen konfiguriert haben, können Sie eine Aktionsgruppe einrichten. Dabei handelt es sich um eine Gruppe von Aktionen, die für mehrere Warnungen verwendet werden sollen. Dabei kann es sich beispielsweise um E-Mail-Benachrichtigungen, Runbooks oder Webhooks handeln. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../../azure-monitor/alerts/action-groups.md).

1. Wählen Sie eine Warnung und dann unter **Aktionen** die Option **Aktionsgruppen hinzufügen** aus. Anschließend wird der Bereich **Aktionsgruppe zum Anfügen an diese Warnungsregel auswählen** angezeigt.

   :::image type="content" source="./media/manage-updates-for-vm/select-an-action-group.png" alt-text="Nutzung und geschätzte Kosten":::

1. Aktivieren Sie das Kontrollkästchen für die anzufügende Aktionsgruppe, und klicken Sie auf „Auswählen“.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Warnungen in Azure Monitor](../../azure-monitor/alerts/alerts-overview.md).

* Erfahren Sie mehr über [Protokollabfragen](../../azure-monitor/logs/log-query-overview.md) zum Abrufen und Analysieren von Daten aus einem Log Analytics-Arbeitsbereich.

* Verwalten von [Nutzung und Kosten mit Azure Monitor-Protokollen](../../azure-monitor/logs/manage-cost-storage.md) beschreibt, wie Sie Ihre Kosten durch Ändern der Datenaufbewahrungsdauer steuern sowie Ihre Datennutzung analysieren und Warnungen dazu ausgeben.
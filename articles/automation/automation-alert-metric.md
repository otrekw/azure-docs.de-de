---
title: Überwachen von Azure Automation-Runbooks mit Metrikwarnungen
description: In diesem Artikel wird beschrieben, wie Sie eine Metrikwarnung auf dem Abschlussstatus des Runbooks basierend einrichten.
services: automation
ms.date: 08/10/2020
ms.topic: article
ms.openlocfilehash: 3a52824a7030d78647d8a664819f439d92d7296d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100593945"
---
# <a name="monitor-runbooks-with-metric-alerts"></a>Überwachen von Runbooks mit Metrikwarnungen

In diesem Artikel erfahren Sie, wie Sie eine [Metrikwarnung](../azure-monitor/alerts/alerts-metric-overview.md) auf dem Abschlussstatus des Runbooks basierend erstellen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

## <a name="create-alert"></a>Erstellen einer Warnung

Mit Warnungen können Sie eine zu überwachende Bedingung und eine auszuführende Aktion definieren, die ausgeführt wird, sobald diese Bedingung erfüllt ist.

1. Starten Sie den Azure Automation-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Automation-Konten** suchen und die entsprechende Option auswählen.

2. Wählen Sie in der Liste der Automation-Konten das Konto aus, für das Sie eine Warnung erstellen möchten. 

3. Wählen Sie unter **Überwachen** **Warnungen** und dann **+ Neue Warnungsregel** aus. Der Zielbereich ist bereits in Ihrem Automation-Konto definiert und damit verknüpft.

### <a name="configure-alert-criteria"></a>Konfigurieren der Warnungskriterien

1. Klicken Sie auf **Bedingung auswählen**. Wählen Sie **Metriken** für den **Signaltyp** aus, und wählen Sie in der Liste **Aufträge gesamt** aus.

2. Auf der Seite **Signallogik konfigurieren** definieren Sie die Logik, die die Warnung auslöst. Unter dem Verlaufsdiagramm werden Ihnen zwei Dimensionen angezeigt, **Runbookname** und **Status**. Dimensionen sind verschiedene Eigenschaften für eine Metrik, die zum Filtern von Ergebnissen verwendet werden kann. Wählen Sie unter **Runbookname** das Runbook aus, für das eine Warnung ausgegeben werden soll, oder lassen Sie das Feld leer, um für alle Runbooks Warnungen auszugeben. Wählen Sie unter **Status** in der Dropdownliste einen Status aus, den Sie überwachen möchten. Der Name des Runbooks und die Statuswerte, die in der Dropdownliste angezeigt werden, gelten nur für Aufträge, die in der letzten Woche ausgeführt wurden.

   Wenn Sie eine Warnung zu einem Status oder einem Runbook ausgeben möchten, die nicht in der Dropdownliste angezeigt wird, klicken Sie auf die Option **Benutzerdefinierten Wert hinzufügen** neben der Dimension. Daraufhin wird ein Dialogfeld geöffnet, in dem Sie einen benutzerdefinierten Wert angeben können, der in letzter Zeit für diese Dimension nicht ausgegeben wurde. Wenn Sie einen Wert eingeben, der für eine Eigenschaft nicht existiert, wird Ihre Warnung nicht ausgelöst.

   > [!NOTE]
   > Wenn Sie keinen Namen für die **Runbookname**-Dimension angeben, erhalten Sie eine Warnung, wenn Runbooks vorhanden sind, die die Statuskriterien erfüllen, was auch ausgeblendete System-Runbooks umfasst.

    Um beispielsweise eine Warnung zu erhalten, wenn ein Runbook einen _Fehler_-Status zurückgibt, fügen Sie der Angabe des Runbooknamens für die **Status**-Dimension den benutzerdefinierten Dimensionswert **Fehler** hinzu.

    :::image type="content" source="./media/automation-alert-metric/specify-dimension-custom-value.png" alt-text="Benutzerdefinierten Dimensionswert angeben" border="false":::

3. Definieren Sie unter **Warnungslogik** die Bedingung und den Schwellenwert für Ihre Warnung. Unten sehen Sie eine Vorschau Ihrer definierten Bedingung.

4. Wählen Sie unter **Auswertung basierend auf** den Zeitraum und die Ausführungshäufigkeit für die Abfrage aus. Wenn Sie beispielsweise **In den letzten 5 Minuten** für **Zeitraum** und **Jede Minute** für **Häufigkeit** wählen, sucht die Warnung nach der Anzahl der Runbooks, die Ihre Kriterien in den letzten 5 Minuten erfüllt haben. Diese Abfrage wird jede Minute ausgeführt, und sobald die von Ihnen definierten Warnungskriterien nicht mehr in einem 5-Minuten-Fenster gefunden werden, löst sich die Warnung von selbst auf. Klicken Sie abschließend auf **Fertig**.

   ![Auswählen einer Ressource für die Warnung](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-the-action-to-take"></a>Definieren der auszuführenden Aktion

1. Wählen Sie unter **Aktionsgruppe** die Option **Aktionsgruppe auswählen** aus. Eine Aktionsgruppe ist eine Gruppe von Aktionen, die Sie für mehrere Warnungen verwenden können. Dies können beispielsweise E-Mail-Benachrichtigungen, Runbooks und Webhooks sein. Weitere Informationen zu Aktionsgruppen und Schritte zum Erstellen einer Aktionsgruppe, die eine E-Mail-Benachrichtigung sendet, finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../azure-monitor/alerts/action-groups.md).

### <a name="define-alert-details"></a>Definieren von Warnungsdetails

1. Geben Sie unter **Warnungsdetails definieren** einen Anzeigenamen und eine Beschreibung für die Warnung ein. Stellen Sie den **Schweregrad** entsprechend Ihrer Warnungsbedingung ein. Es gibt fünf Schweregrade von 0 bis 5. Die Warnungen werden unabhängig vom Schweregrad gleich behandelt. Sie können den Schweregrad an Ihre Geschäftslogik anpassen.

1. Standardmäßig werden Regeln bei der Erstellung aktiviert, es sei denn, Sie wählen **Nein** für die Option **Warnungsregel beim Erstellen aktivieren**. Warnungen, die in einem deaktivierten Zustand erstellt wurden, können Sie in Zukunft aktivieren, wenn Sie dazu bereit sind. Wählen Sie **Warnungsregel erstellen** aus, um Ihre Änderungen zu speichern.

## <a name="receive-notification"></a>Empfangen von Benachrichtigungen

Wenn die Warnungskriterien erfüllt sind, führt die Aktionsgruppe die definierte Aktion aus. Im Beispiel dieses Artikels wird eine E-Mail verschickt. In der folgenden Abbildung sehen Sie ein Beispiel für eine E-Mail, die Sie nach dem Auslösen der Warnung empfangen:

![E-Mail-Warnung](./media/automation-alert-activity-log/alert-email.png)

Wenn die Metrik nicht mehr außerhalb des definierten Schwellenwerts liegt, wird die Warnung deaktiviert und die Aktionsgruppe führt die definierte Aktion aus. Wenn ein E-Mail-Aktionstyp ausgewählt ist, wird eine Auflösungs-E-Mail gesendet, die angibt, dass sie aufgelöst wurde.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks](automation-create-alert-triggered-runbook.md).

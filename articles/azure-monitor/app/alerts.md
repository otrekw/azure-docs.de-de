---
title: Einrichten von Warnungen in Azure Application Insights
description: Legen Sie fest, dass Sie über längere Reaktionszeiten, Ausnahmen und andere Leistungs- oder Nutzungsänderungen in Ihrer Web-App informiert werden.
ms.topic: conceptual
ms.date: 01/23/2019
ms.reviewer: lagayhar
ms.subservice: alerts
ms.openlocfilehash: 28fd59556a586b85a6d3caf188d9e02c11d31e3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295079"
---
# <a name="set-alerts-in-application-insights"></a>Einrichten von Warnungen in Application Insights

[Azure Application Insights][start] kann Sie bei Änderungen der Leistung oder der Nutzungsmetriken in Ihrer Web-App benachrichtigen. 

Application Insights überwacht die Live-App auf einer [Vielzahl von Plattformen][platforms], um Sie bei der Diagnose von Leistungsproblemen und beim Auswerten von Nutzungsmustern zu unterstützen.

Es gibt mehrere Warnungstypen:

* [**Metrikwarnungen**](../../azure-monitor/platform/alerts-metric-overview.md) informieren Sie darüber, wenn eine Metrik für einen bestimmten Zeitraum einen Schwellenwert überschreitet – z. B. in Bezug auf Reaktionszeiten, Anzahl von Ausnahmen, CPU-Nutzung oder Seitenaufrufe.
* [**Protokollwarnungen**](../../azure-monitor/platform/alerts-unified-log.md) werden dazu verwendet, Warnungen zu beschreiben, bei denen das Warnsignal auf einer benutzerdefinierten Kusto-Abfrage basiert.
* Bei [**Webtests**][availability] werden Sie informiert, wenn Ihre Website im Internet nicht verfügbar ist oder langsam reagiert. [Weitere Informationen][availability]
* Die [**Proaktive Diagnose**](../../azure-monitor/app/proactive-diagnostics.md) wird automatisch konfiguriert, damit Benachrichtigungen über ungewöhnliche Leistungsmuster gesendet werden.

## <a name="how-to-set-an-exception-alert-using-custom-log-search"></a>Festlegen einer Ausnahmewarnung mithilfe von benutzerdefinierter Protokollsuche

In diesem Abschnitt erfahren Sie, wie eine abfragebasierte Ausnahmewarnung festgelegt wird. Nehmen wir für dieses Beispiel an, dass eine Warnung ausgegeben werden soll, wenn die Fehlerrate innerhalb der letzten 24 Stunden 10 % übersteigt.

1. Navigieren Sie im Azure-Portal zu Ihrer Application Insights-Ressource.
2. Klicken Sie links unter „Konfigurieren“ auf **Warnung**.

    ![Klicken Sie links unter „Konfigurieren“ auf „Warnung“.](./media/alerts/1appinsightalert.png)

3. Wählen Sie oben auf der Registerkarte „Warnung“ die Option **Neue Warnungsregel** aus.

     ![Wählen Sie oben auf der Registerkarte „Warnung“ die Option „Neue Warnungsregel“ aus.](./media/alerts/2createalert.png)

4. Ihre Ressource sollte automatisch ausgewählt werden. Klicken Sie auf **Bedingung hinzufügen**, um eine Bedingung festzulegen.

    ![Klicken Sie auf „Bedingung hinzufügen“.](./media/alerts/3addcondition.png)

5. Klicken Sie auf der Registerkarte „Signallogik konfigurieren“ auf **Benutzerdefinierte Protokollsuche**.

    ![Klicken Sie auf „Benutzerdefinierte Protokollsuche“.](./media/alerts/4customlogsearch.png)

6. Geben Sie Ihre Abfrage auf der Registerkarte „Benutzerdefinierte Protokollsuche“ im Feld „Suchabfrage“ ein. In diesem Beispiel verwenden wir die nachfolgende Kusto-Abfrage.
    ```kusto
    let percentthreshold = 10;
    let period = 24h;
    requests
    | where timestamp >ago(period)
    | summarize requestsCount = sum(itemCount)
    | project requestsCount, exceptionsCount = toscalar(exceptions | where timestamp >ago(period) | summarize sum(itemCount))
    | extend exceptionsRate = toreal(exceptionsCount)/toreal(requestsCount) * 100
    | where exceptionsRate > percentthreshold

    ```

    ![Eingeben einer Abfrage in das Suchabfragefeld](./media/alerts/5searchquery.png)
    
    > [!NOTE]
    > Sie können diese Schritte auch für andere abfragebasierte Warnungstypen durchführen. Weitere Informationen zur Kusto-Abfragesprache finden Sie in der Dokumentation [Erste Schritte mit Kusto](https://docs.microsoft.com/azure/kusto/concepts/) oder im Spickzettel [SQL to Kusto](https://docs.microsoft.com/azure/kusto/query/sqlcheatsheet).

7. Wählen Sie unter „Warnungslogik“ aus, ob diese auf der Anzahl von Ergebnissen oder metrischer Maßeinheit basiert. Wählen Sie dann die Bedingung (größer als, gleich, kleiner als) und einen Schwellenwert aus. Während Sie diese Werte ändern, stellen Sie möglicherweise eine Änderung des Satzes für die Bedingungsvorschau fest. In diesem Beispiel verwenden wir „gleich“.

    ![Wählen Sie unter „Warnungslogik“ aus den vorhandenen Optionen für „Basierend auf“ und „Bedingung“ aus, und geben Sie dann einen Schwellenwert ein.](./media/alerts/6alertlogic.png)

8. Legen Sie unter „Auswertung basierend auf“ den Zeitraum und die Häufigkeit fest. Der Zeitraum muss hier mit dem Wert übereinstimmen, den wir in der Abfrage oben für den Zeitraum festgelegt haben. Klicken Sie anschließend auf **Fertig**.

    ![Legen Sie unten den Zeitraum und die Häufigkeit fest, und klicken Sie dann auf „Fertig“.](./media/alerts/7evaluate.png)

9. Wir sehen jetzt die Bedingung, die wir mit den voraussichtlichen monatlichen Kosten erstellt haben. Unter [Aktionsgruppen](../platform/action-groups.md) können Sie eine neue Gruppe erstellen oder eine vorhandene Gruppe auswählen. Wenn Sie möchten, können Sie die Aktionen anpassen.

    ![Klicken Sie unter „Aktionsgruppe“ auf die Schaltflächen „Auswählen“ oder „Erstellen“.](./media/alerts/8actiongroup.png)

10. Fügen Sie anschließend Ihre Warnungsdetails (Name der Warnungsregel, Beschreibung, Schweregrad) hinzu. Klicken Sie zuletzt unten auf **Warnungsregel erstellen**.

    ![Geben Sie unter „Warnungsdetails“ Ihren Namen für die Warnungsregel und eine Beschreibung ein, und legen Sie einen Schweregrad fest.](./media/alerts/9alertdetails.png)

## <a name="how-to-unsubscribe-from-classic-alert-e-mail-notifications"></a>Abbestellen von E-Mail-Benachrichtigungen zu klassischen Warnungen

Dieser Abschnitt bezieht sich auf **klassische Verfügbarkeitswarnungen**, **klassische Application Insights-Metrikwarnungen** und **klassische Warnungen zu Fehleranomalien**.

Sie erhalten E-Mail-Benachrichtigungen für diese klassischen Warnungen, wenn Folgendes zutrifft:

* Ihre E-Mail-Adresse ist im Feld „Empfänger von E-Mail-Benachrichtigungen“ in den Einstellungen für Warnungsregeln aufgeführt.

* Die Option zum Senden von E-Mail-Benachrichtigungen an Benutzer mit bestimmten Rollen für das Abonnement ist aktiviert, und Sie haben eine entsprechende Rolle für dieses bestimmte Azure-Abonnement.

![Screenshot für Warnungsbenachrichtigungen](./media/alerts/alert-notification.png)

Um Sicherheit und Datenschutz besser steuern zu können, wird generell empfohlen, dass Sie im Feld **Empfänger von E-Mail-Benachrichtigungen** die Benachrichtigungsempfänger für die klassischen Warnungen explizit angeben. Die Option zum Benachrichtigen aller Benutzer mit bestimmten Rollen ist für die Abwärtskompatibilität vorgesehen.

Wenn Sie die über eine bestimmte Warnungsregel generierten E-Mail-Benachrichtigungen abbestellen möchten, entfernen Sie Ihre E-Mail-Adresse im Feld **Empfänger von E-Mail-Benachrichtigungen**.

Wenn Ihre E-Mail-Adresse nicht explizit aufgeführt ist, wird empfohlen, dass Sie die Option zur automatischen Benachrichtigung aller Benutzer mit bestimmten Rollen deaktivieren und stattdessen im Feld „Empfänger von E-Mail-Benachrichtigungen“ die E-Mail-Adressen aller Benutzer aufführen, die Benachrichtigungen für die Warnungsregel erhalten sollen.

## <a name="who-receives-the-classic-alert-notifications"></a>Wer erhält die (klassischen) Warnungsbenachrichtigungen?

Dieser Abschnitt gilt nur für klassische Benachrichtigungen und hilft Ihnen, Ihre Warnungsbenachrichtigungen zu optimieren, um sicherzustellen, dass nur die gewünschten Empfänger Benachrichtigungen erhalten. Lesen Sie den [Übersichtsartikel zu Warnungen](../platform/alerts-classic.overview.md), um mehr über den Unterschied zwischen [klassischen Warnungen](../platform/alerts-overview.md) und den neuen Warnungen zu erfahren. Verwenden Sie [Aktionsgruppen](../platform/action-groups.md), um die Warnungsbenachrichtigung in der neuen Benutzeroberfläche für Warnungen zu steuern.

* Wir empfehlen die Verwendung bestimmter Empfänger für klassische Warnungsbenachrichtigungen.

* Für Warnungen zu allen Application Insights-Metriken (einschließlich Verfügbarkeitsmetriken) sendet die Option **Masse/Gruppe**, falls aktiviert, Benachrichtigungen an Benutzer mit der Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ im Abonnement. Tatsächlich sind _alle_ Benutzer mit Zugriff auf das Abonnement der Application Insights-Ressource im Umfang enthalten und erhalten Benachrichtigungen.

> [!NOTE]
> Wenn Sie aktuell die Option **Massenversand/Gruppe** verwenden und diese deaktivieren, können Sie die Änderung nicht rückgängig machen.

Verwenden Sie die neue Benutzeroberfläche für Warnungen/Warnungen nahezu in Echtzeit, wenn Sie Benutzer basierend auf ihren Rollen benachrichtigen müssen. Mit [Aktionsgruppen](../platform/action-groups.md) können Sie E-Mail-Benachrichtigungen für Benutzer mit den Rollen „Mitwirkender“, „Besitzer“, „Leser“ konfigurieren (nicht kombiniert als eine einzige Option).

## <a name="automation"></a>Automation
* [Verwenden von PowerShell zum Automatisieren der Einrichtung von Warnungen](../../azure-monitor/app/powershell-alerts.md)
* [Verwenden von Webhooks zum Automatisieren der Reaktion auf Warnungen](../../azure-monitor/platform/alerts-webhooks.md)

## <a name="see-also"></a>Weitere Informationen
* [Verfügbarkeitswebtests](../../azure-monitor/app/monitor-web-app-availability.md)
* [Automatisieren der Einrichtung von Warnungen](../../azure-monitor/app/powershell-alerts.md)
* [Proaktive Diagnose](../../azure-monitor/app/proactive-diagnostics.md) 

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[platforms]: ../../azure-monitor/app/platforms.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[start]: ../../azure-monitor/app/app-insights-overview.md


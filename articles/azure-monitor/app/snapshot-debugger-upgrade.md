---
title: Aktualisieren des Azure Application Insights-Momentaufnahmedebuggers
description: Upgrade des Momentaufnahmedebuggers für .NET-Apps in Azure App Services oder über NuGet-Pakete auf die neueste Version
ms.topic: conceptual
author: pharring
ms.author: pharring
ms.date: 03/28/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6991db6055ca8556adff67f194bac0f4aa04cb06
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "77671391"
---
# <a name="upgrading-the-snapshot-debugger"></a>Upgraden des Momentaufnahmedebuggers

Um die bestmögliche Sicherheit für Ihre Daten zu gewährleisten, rückt Microsoft von TLS 1.0 und TLS 1.1 ab, da sich beide Versionen als anfällig für Angriffe erwiesen haben. Sollten Sie eine ältere Version der Websiteerweiterung verwenden, muss diese aktualisiert werden, damit sie weiter funktioniert. In diesem Dokument erfahren Sie, wie Sie den Momentaufnahmedebugger auf die neueste Version upgraden. Es gibt zwei primäre Upgradepfade. Diese sind abhängig davon, ob Sie den Momentaufnahmedebugger mithilfe einer Websiteerweiterung aktiviert oder Ihrer Anwendung ein SDK/NuGet-Paket hinzugefügt haben. Im Anschluss werden beide Upgradepfade beschrieben. 

## <a name="upgrading-the-site-extension"></a>Upgraden der Websiteerweiterung

> [!IMPORTANT]
> Bei älteren Versionen von Application Insights wurde eine private Websiteerweiterung mit dem Namen _Application Insights-Erweiterung für Azure App Service_ verwendet. Die aktuelle Application Insights-Oberfläche wird aktiviert, indem die App-Einstellungen auf das Anzeigen einer vorinstallierten Websiteerweiterung festgelegt werden.
> Um Konflikte zu vermeiden, durch die Ihre Website möglicherweise nicht mehr funktioniert, muss zuerst die private Websiteerweiterung gelöscht werden. Informationen dazu finden Sie in Schritt 4 weiter unten.

Wenn Sie den Momentaufnahmedebugger mithilfe der Websiteerweiterung aktiviert haben, können Sie das Upgrade wie folgt durchführen:

1. Melden Sie sich beim Azure-Portal an.
2. Navigieren Sie zu Ihrer Ressource, für die Application Insights und der Momentaufnahmedebugger aktiviert sind. Navigieren Sie also beispielsweise bei einer Web-App zur App Service-Ressource:

   ![Screenshot: App Service-Ressource namens „DiagService01“](./media/snapshot-debugger-upgrade/app-service-resource.png)

3. Nachdem Sie zu Ihrer Ressource navigiert sind, klicken Sie auf das Blatt „Erweiterungen“, und warten Sie, bis die Liste der Erweiterungen aufgefüllt wurde:

   ![Screenshot: App Service-Erweiterungen mit installierter Application Insights-Erweiterung für Azure App Service](./media/snapshot-debugger-upgrade/application-insights-site-extension-to-be-deleted.png)

4. Wenn eine Version von _Application Insights-Erweiterung für Azure App Service_ installiert ist, wählen Sie diese aus, und klicken Sie auf „Löschen“. Bestätigen Sie das Löschen der Erweiterung mit **Ja**, und warten Sie, bis der Löschvorgang abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren.

   ![Screenshot: App Service-Erweiterungen mit hervorgehobener Schaltfläche „Löschen“ für Application Insights-Erweiterung für Azure App Service](./media/snapshot-debugger-upgrade/application-insights-site-extension-delete.png)

5. Wechseln Sie zum Blatt „Übersicht“ für Ihre Ressource, und klicken Sie auf „Application Insights“:

   ![Screenshot mit drei Schaltflächen. Mittlere Schaltfläche namens „Application Insights“ ausgewählt.](./media/snapshot-debugger-upgrade/application-insights-button.png)

6. Wenn Sie das Blatt „Application Insights“ zum ersten Mal für diese App Service-Instanz anzeigen, werden Sie aufgefordert, Application Insights zu aktivieren. Wählen Sie **Application Insights aktivieren** aus.
 
   ![Screenshot: Erstmalige Darstellung des Blatts „Application Insights“ mit hervorgehobener Schaltfläche „Application Insights aktivieren“](./media/snapshot-debugger-upgrade/turn-on-application-insights.png)

7. Die aktuellen Application Insights-Einstellungen werden angezeigt. Hier können Sie bei Bedarf Ihre Einstellungen ändern. Die Schaltfläche **Übernehmen** am unteren Rand des Blatts wird erst aktiv, wenn Sie eine der Einstellungen bearbeitet haben. Sie müssen allerdings keine Einstellung ändern, sondern können die Änderung gleich wieder zurücksetzen. Wir empfehlen, die Profiler-Einstellung zu bearbeiten und anschließend **Übernehmen** auszuwählen.

   ![Screenshot: App Service-Konfigurationsseite von Application Insights mit rot hervorgehobener Schaltfläche „Übernehmen“](./media/snapshot-debugger-upgrade/view-application-insights-data.png)

8. Nach dem Klicken auf **Übernehmen** werden Sie aufgefordert, die Änderungen zu bestätigen.

    > [!NOTE]
    > Die Website wird im Rahmen des Upgradeprozesses neu gestartet.

   ![Screenshot: App Service-Aufforderung zur Bestätigung der Übernahme der Überwachungseinstellungen. Das Textfeld enthält folgende Meldung: „Wir werden jetzt Änderungen an Ihren App-Einstellungen vornehmen und unsere Tools installieren, um Ihre Application Insights-Ressource mit der Web-App zu verknüpfen. Dies führt zu einem Neustart der Website. Möchten Sie den Vorgang fortsetzen?“](./media/snapshot-debugger-upgrade/apply-monitoring-settings.png)

9. Klicken Sie auf **Ja**, um die Änderungen zu übernehmen, und warten Sie, bis der Vorgang abgeschlossen ist.

Die Website ist nun aktualisiert und verwendungsbereit.

## <a name="upgrading-snapshot-debugger-using-sdknuget"></a>Upgraden des Momentaufnahmedebuggers per SDK/NuGet

Falls die Anwendung nicht mindestens die Version 1.3.1 von `Microsoft.ApplicationInsights.SnapshotCollector` verwendet, ist ein Upgrade auf eine [neuere Version](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) erforderlich, damit sie weiter funktioniert.

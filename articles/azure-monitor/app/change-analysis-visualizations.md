---
title: Visualisierungen für die Anwendungsänderungsanalyse – Azure Monitor
description: Erfahren Sie, wie Sie Visualisierungen für die Anwendungsänderungsanalyse in Azure Monitor verwenden.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 02/11/2021
ms.openlocfilehash: 838a48aa11a1cb36c3a7d822ce88f58936aa976d
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734620"
---
# <a name="visualizations-for-application-change-analysis-preview"></a>Visualisierungen für die Anwendungsänderungsanalyse (Vorschau)

## <a name="standalone-ui"></a>Eigenständige Benutzeroberfläche

In Azure Monitor gibt es einen eigenständigen Bereich für die Änderungsanalyse, in dem alle Änderungen mit Erkenntnissen zu Anwendungsabhängigkeiten und Ressourcen angezeigt werden können.

Suchen Sie im Azure-Portal in der Suchleiste nach „Änderungsanalyse“, um die Erfahrung zu starten.

![Screenshot der Suche nach „Änderungsanalyse“ im Azure-Portal](./media/change-analysis/search-change-analysis.png)

Alle Ressourcen unter einem ausgewählten Abonnement werden mit Änderungen der letzten 24 Stunden angezeigt. Zur Optimierung der Seitenladeleistung zeigt der Dienst jeweils 10 Ressourcen gleichzeitig an. Wählen Sie die nächste Seite aus, um weitere Ressourcen anzuzeigen. Wir arbeiten derzeit daran, diese Einschränkung zu beseitigen.

![Screenshot des Blatts „Änderungsanalyse“ im Azure-Portal](./media/change-analysis/change-analysis-standalone-blade.png)

Klicken Sie auf eine Ressource, um alle ihre Änderungen anzuzeigen. Führen Sie bei Bedarf einen Drilldown in eine Änderung durch, um Änderungsdetails und Erkenntnisse im JSON-Format anzuzeigen.

![Screenshot von Änderungsdetails](./media/change-analysis/change-details.png)

Wenn Sie Feedback abgeben möchten, verwenden Sie die entsprechende Schaltfläche, oder senden Sie eine E-Mail an changeanalysisteam@microsoft.com.

![Screenshot der Feedbackschaltfläche auf der Registerkarte „Änderungsanalyse“](./media/change-analysis/change-analysis-feedback.png)

### <a name="multiple-subscription-support"></a>Unterstützung mehrerer Abonnements

Die Benutzeroberfläche unterstützt das Auswählen mehrerer Abonnements zum Anzeigen von Ressourcenänderungen. Verwenden Sie den Abonnementfilter:

![Screenshot: Abonnementfilter, der das Auswählen mehrerer Abonnements unterstützt](./media/change-analysis/multiple-subscriptions-support.png)

### <a name="web-app-diagnose-and-solve-problems"></a>Diagnose und Behandlung von Problemen bei Web-Apps

In Azure Monitor ist die Änderungsanalyse auch in die Self-Service-Umgebung **Diagnose und Problembehandlung** integriert. Der Zugriff auf diese Umgebung erfolgt von der Seite **Übersicht** Ihrer App Service-Anwendung.

![Screenshot der Schaltflächen „Übersicht“ und „Diagnose und Problembehandlung“](./media/change-analysis/change-analysis.png)

## <a name="application-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Anwendungsänderungsanalyse im Tool „Diagnose und Problembehandlung“

Bei der Anwendungsänderungsanalyse handelt es sich um ein eigenständiges Erkennungsmodul in den Web-App-Tools für Diagnose und Problembehandlung. Es ist auch in den Erkennungsmodulen für **Anwendungsabstürze** und  **Web-App-Ausfälle** aggregiert. Wenn Sie das Diagnose- und Problembehandlungstool öffnen, wird der Ressourcenanbieter **Microsoft.ChangeAnalysis** automatisch registriert. Befolgen Sie diese Anweisungen, um die Nachverfolgung von Web-App-Änderungen des Gastsystems zu aktivieren.

1. Wählen Sie **Verfügbarkeit und Leistung** aus.

    ![Screenshot der Problembehandlungsoptionen von „Verfügbarkeit und Leistung“](./media/change-analysis/availability-and-performance.png)

2. Wählen Sie **Anwendungsänderungen** aus. Diese Funktion ist ebenfalls unter **Anwendungsabstürze** verfügbar.

   ![Screenshot der Schaltfläche „Anwendungsabstürze“](./media/change-analysis/application-changes.png)

3. Der Link führt zur Benutzeroberfläche der Anwendungsänderungsanalyse bezogen auf die Web-App. Wenn die Nachverfolgung von Web-App-Änderungen des Gastsystems nicht aktiviert ist, befolgen Sie die Anweisung im Banner, um Änderungen an Datei- und App-Einstellungen abzurufen.

   ![Screenshot der Optionen von „Anwendungsabstürze“](./media/change-analysis/enable-changeanalysis.png)

4. Aktivieren Sie **Änderungsanalyse**, und wählen Sie **Speichern** aus. Das Tool zeigt alle Web-Apps unter einem App Service-Plan an. Sie können den Schalter auf Planebene verwenden, um die Änderungsanalyse für alle Web-Apps in einem Plan zu aktivieren.

    ![Screenshot der Benutzeroberfläche zum Aktivieren der Änderungsanalyse](./media/change-analysis/change-analysis-on.png)

5. Änderungsdaten sind auch in ausgewählten Erkennungsmodulen für **Web-App-Ausfälle** und **Anwendungsabstürze** verfügbar. Im angezeigten Diagramm sind die Arten der Änderungen im Laufe der Zeit und Details zu diesen Änderungen zusammengefasst. Standardmäßig werden Änderungen der letzten 24 Stunden angezeigt, um bei unmittelbaren Problemen sofort reagieren zu können.

     ![Screenshot der Gegenüberstellung der Änderungen](./media/change-analysis/change-view.png)

## <a name="virtual-machine-diagnose-and-solve-problems"></a>Diagnose und Problembehandlung bei virtuellen Computern

Wechseln Sie zum Diagnose- und Problembehandlungstool für einen virtuellen Computer.  Wechseln Sie zu **Problembehandlungstools**, scrollen Sie auf der Seite nach unten, und wählen Sie **Aktuelle Änderungen analysieren** aus, um Änderungen an dem virtuellen Computer anzuzeigen.

![Screenshot von „Diagnose und Problembehandlung bei virtuellen Computern“](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![Änderungsanalyse in Problembehandlungstools](./media/change-analysis/analyze-recent-changes.png)

## <a name="activity-log-change-history"></a>Änderungsverlauf im Aktivitätsprotokoll

Die Funktion zum [Anzeigen des Änderungsverlaufs](../essentials/activity-log.md#view-change-history) im Aktivitätsprotokoll ruft das Back-End des Anwendungsänderungsanalyse-Diensts auf, um Änderungen im Zusammenhang mit einem Vorgang abzurufen. Mit dem **Änderungsverlauf** wurde sonst [Azure Resource Graph](../../governance/resource-graph/overview.md) direkt aufgerufen, doch wurde das Back-End gegen einen Aufruf der Anwendungsänderungsanalyse getauscht, sodass in den zurückgegebenen Änderungen auch Änderungen auf Ressourcenebene aus [Azure Resource Graph](../../governance/resource-graph/overview.md), Ressourceneigenschaften aus [Azure Resource Manager](../../azure-resource-manager/management/overview.md) und Änderungen auf Gastsystemen aus PaaS-Diensten wie der App Services-Web-App enthalten sind. Damit der Anwendungsänderungsanalyse-Dienst nach Änderungen in den Abonnements der Benutzer suchen kann, muss ein Ressourcenanbieter registriert sein. Wenn Sie die Registerkarte **Änderungsverlauf** zum ersten Mal öffnen, beginnt das Tool automatisch mit der Registrierung des **Microsoft.ChangeAnalysis**-Ressourcenanbieters. Nach der Registrierung sind Änderungen aus **Azure Resource Graph** sofort verfügbar und decken die letzten 14 Tage ab. Änderungen aus anderen Quellen stehen ca. vier Stunden nach dem Onboarding des Abonnements zur Verfügung.

![Integration des Änderungsverlaufs im Aktivitätsprotokoll](./media/change-analysis/activity-log-change-history.png)

## <a name="vm-insights-integration"></a>VM Insights-Integration

Benutzer, die [VM Insights](../vm/vminsights-overview.md) aktiviert haben, können Änderungen bei ihren VMs anzeigen, die möglicherweise zu Spitzen in einem Metrikdiagramm, z. B. CPU oder Arbeitsspeicher, geführt haben. Änderungsdaten werden in die Seitennavigationsleiste von VM Insights eingefügt. Der Benutzer kann sehen, ob Änderungen bei der VM aufgetreten sind, und **Änderungen untersuchen** auswählen, um Änderungsdetails auf der eigenständigen Benutzeroberfläche der Anwendungsänderungsanalyse anzuzeigen.

[![VM Insights-Integration](./media/change-analysis/vm-insights.png)](./media/change-analysis/vm-insights.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Behandeln von Problemen bei der Änderungsanalyse](change-analysis-troubleshoot.md)
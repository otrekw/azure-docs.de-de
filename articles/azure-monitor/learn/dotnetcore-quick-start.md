---
title: Schnellstart ASP.NET Core – Azure Monitor Application Insights
description: Dieser Artikel enthält Informationen zum schnellen Einrichten einer ASP.NET Core-Web-App für die Überwachung mit Azure Monitor Application Insights.
ms.subservice: application-insights
ms.topic: quickstart
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/26/2019
ms.custom: mvc
ms.openlocfilehash: a12fe837bfd9606d07c7be71851880d2cebb9d61
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89055480"
---
# <a name="start-monitoring-your-aspnet-core-web-application"></a>Starten der Überwachung Ihrer ASP.NET Core-Webanwendung

Mit Azure Application Insights können Sie die Verfügbarkeit, Leistung und Nutzung Ihrer Webanwendung ganz einfach überwachen. Sie können auch Fehler in Ihrer Anwendung schnell erkennen und diagnostizieren, ohne darauf warten zu müssen, dass diese Fehler von Benutzern gemeldet werden. 

Diese Schnellstartanleitung führt Sie durch die notwendigen Schritte, um das Application Insights SDK zu einer vorhandenen ASP.NET Core-Webanwendung hinzuzufügen. Informationen zum Konfigurieren von Application Insights ohne Visual Studio finden Sie in diesem [Artikel](../app/asp-net-core.md).

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

- [Installieren Sie Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) mit den folgenden Workloads:
  - ASP.NET und Webentwicklung
  - Azure-Entwicklung
- [Installieren Sie das .NET Core 2.0 SDK](https://dotnet.microsoft.com/download).
- Sie benötigen ein Azure-Abonnement und eine vorhandene .NET Core-Webanwendung.

Wenn Sie keine ASP.NET Core-Webanwendung haben, können Sie anhand unserer detaillierten Anleitung [eine ASP.NET Core-App erstellen und Application Insights hinzufügen](../app/asp-net-core.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="enable-application-insights"></a>Aktivieren von Application Insights

Application Insights kann Telemetriedaten von jeder mit dem Internet verbundenen Anwendung erfassen, unabhängig davon, ob die Anwendung lokal oder in der Cloud ausgeführt wird. Geben Sie folgendermaßen vor, um diese Daten anzuzeigen.

1. Klicken Sie auf **Ressource erstellen** > **Entwicklertools** > **Application Insights**.

   > [!NOTE]
   >Wenn Sie zum ersten Mal eine Application Insights-Ressource erstellen, können Sie mehr dazu im Dokument [Erstellen einer Application Insights-Ressource](../app/create-new-resource.md) erfahren.

    Ein Dialogfeld für die Konfiguration wird geöffnet. Füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus:

   | Einstellungen        |  Wert           | BESCHREIBUNG  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert. |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen Ressourcengruppe, die Application Insights-Daten hosten soll. Sie können eine neue Ressourcengruppe erstellen oder eine bereits vorhandene Ressourcengruppe verwenden. |
   | **Location** | East US | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe des Standorts, in dem Ihre App gehostet wird. |

2. Klicken Sie auf **Erstellen**.



## <a name="configure-app-insights-sdk"></a>Konfigurieren des Application Insights SDK

1. Öffnen Sie das **Projekt** Ihrer ASP.NET Core-Web-App in Visual Studio. Klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den Namen Ihrer App, und wählen Sie **Hinzufügen** > **Application Insights-Telemetrie** aus.

    ![Hinzufügen von Application Insights-Telemetrie](./media/dotnetcore-quick-start/2vsaddappinsights.png)

2. Klicken Sie auf die Schaltfläche **Erste Schritte**.

3. Wählen Sie Ihr Konto, Ihr Abonnement und die **vorhandene Ressource** aus, die Sie im Azure-Portal erstellt haben. Klicken Sie dann auf **Registrieren**.

4. Wählen Sie **Projekt** > **NuGet-Pakete verwalten** > **Paketquelle: nuget.org** >  **,** und aktualisieren Sie die Application Insights SDK-Pakete auf die neueste stabile Version.

5. Wählen Sie **Debuggen** > **Ohne Debuggen starten**, oder drücken Sie STRG+F5, um Ihre App zu starten.

    ![Application Insights – Menü „Übersicht“](./media/dotnetcore-quick-start/3debug.png)

> [!NOTE]
> Es dauert ca. 3-5 Minuten, bis die ersten Daten im Portal angezeigt werden. Wenn es sich um eine Test-App mit geringem Datenverkehr handelt, denken Sie daran, dass die meisten Metriken nur erfasst werden, wenn aktive Anforderungen oder Vorgänge vorhanden sind.

## <a name="start-monitoring-in-the-azure-portal"></a>Starten der Überwachung im Azure-Portal

1. Öffnen Sie jetzt erneut die Seite **Übersicht** für Application Insights im Azure-Portal, indem Sie **Start** auswählen. Wählen Sie dann in den zuletzt verwendeten Ressourcen die zuvor erstellte Ressource aus, um Details zur aktuell ausgeführten Anwendung anzuzeigen.

   ![Application Insights – Menü „Übersicht“](./media/dotnetcore-quick-start/4overview.png)

2. Klicken Sie auf **Anwendungsübersicht**, um ein visuelles Layout der Abhängigkeitsbeziehungen zwischen den Komponenten Ihrer Anwendung zu erhalten. Jede Komponente zeigt KPIs wie z.B. Last, Leistung, Fehler und Warnungen an.

   ![Anwendungszuordnung](./media/dotnetcore-quick-start/5appmap.png)

3. Klicken Sie auf das **App-Analyse**-Symbol ![Symbol „Anwendungsübersicht“](./media/dotnetcore-quick-start/006.png) **In Analytics anzeigen**. Dadurch wird die **Application Insights-Analyse** geöffnet, die eine erweiterte Abfragesprache zum Analysieren aller Daten bereitstellt, die von Application Insights gesammelt werden. In diesem Fall wird eine Abfrage für Sie generiert, die die Anzahl von Anforderungen als Diagramm darstellt. Sie können selbst Abfragen zum Analysieren anderer Daten schreiben.

   ![Analysediagramm der Benutzeranforderungen in einem bestimmten Zeitraum](./media/dotnetcore-quick-start/6analytics.png)

4. Kehren Sie zur Seite **Übersicht** zurück, und untersuchen Sie die KPI-Dashboards.  Dieses Dashboard zeigt Statistiken zur Integrität Ihrer Anwendung, einschließlich der Anzahl von eingehenden Anforderungen, der Dauer dieser Anforderungen und aller auftretenden Fehler. 

   ![Diagramm der Übersichtszeitachse für die Integrität](./media/dotnetcore-quick-start/7kpidashboards.png)

5. Klicken Sie auf der linken Seite auf **Metriken**. Untersuchen Sie mithilfe des Metrik-Explorers die Integrität und Auslastung Ihrer Ressource. Sie können auf **Neues Diagramm hinzufügen** klicken, um zusätzliche benutzerdefinierte Ansichten zu erstellen, oder auf **Bearbeiten** klicken, um Typ, Höhe, Farbpalette, Gruppierungen und Metriken der vorhandenen Diagramme zu ändern. Sie können beispielsweise ein Diagramm für die durchschnittliche Seitenladezeit des Browsers erstellen, indem Sie in der Metrik-Dropdownliste die Option für die Browser-Seitenladezeit und unter „Aggregation“ die Durchschnittsoption auswählen. Weitere Informationen zum Azure-Metrik-Explorer finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../platform/metrics-getting-started.md).

     ![Registerkarte „Metriken“: Diagramm der durchschnittlichen Ladezeit der Browserseite](./media/dotnetcore-quick-start/8metrics.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie die Tests abgeschlossen haben, können Sie die Ressourcengruppe und alle dazugehörigen Ressourcen löschen. Gehen Sie dazu wie folgt vor:

> [!NOTE]
> Wenn Sie eine vorhandene Ressourcengruppe verwendet haben, funktionieren die unten aufgeführten Anweisungen nicht, und Sie müssen einfach die einzelne Application Insights-Ressource löschen. Denken Sie daran: Wenn Sie eine Ressourcengruppe löschen, werden alle zugrunde liegenden Ressourcen, die Mitglieder dieser Gruppe sind, gelöscht.

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myResourceGroup**.
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Suchen nach und Diagnostizieren von Laufzeitausnahmen](./tutorial-runtime-exceptions.md)


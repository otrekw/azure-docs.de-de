---
title: Erkunden von Azure Stream Analytics-Aufträgen mit Visual Studio Code
description: In diesem Artikel wird veranschaulicht, wie ein Azure Stream Analytics-Auftrag in ein lokales Projekt exportiert wird, Aufträge aufgelistet und Auftragsentitäten angezeigt werden können.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/15/2019
ms.topic: how-to
ms.openlocfilehash: 00705e40ca17959701af325ed52a4c3754d35122
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86039057"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Erkunden von Azure Stream Analytics mit Visual Studio Code (Vorschauversion)

Die Azure Stream Analytics-Erweiterung für Visual Studio Code bietet Entwicklern eine einfache Verwaltung Ihrer Stream Analytics-Aufträge. Sie funktioniert unter Windows, Mac und Linux. Mit der Azure Stream Analytics-Erweiterung ist Folgendes möglich:

- [Erstellen](quick-create-vs-code.md), Starten und Aufträge beenden
- Exportieren vorhandener Aufträge in ein lokales Projekt
- Auflisten von Aufträgen und Anzeigen von Auftragsentitäten

## <a name="export-a-job-to-a-local-project"></a>Exportieren eines Auftrags in ein lokales Projekt

Suchen Sie den zu exportierenden Auftrag im **Stream Analytics-Explorer** in Visual Studio Code, um einen Auftrag in ein lokales Projekt zu exportieren. Wählen Sie dann einen Ordner für Ihr Projekt aus. Das Projekt wird in den von Ihnen ausgewählten Ordner exportiert, und Sie können den Auftrag weiterhin über Visual Studio Code verwalten. Weitere Informationen zur Verwendung von Visual Studio Code zum Verwalten von Stream Analytics-Aufträgen finden Sie im Visual Studio Code-[Schnellstartanleitung](quick-create-vs-code.md).

![Exportieren von ASA-Aufträgen in Visual Studio Code](./media/vscode-explore-jobs/export-job.png)

## <a name="list-job-and-view-job-entities"></a>Auflisten von Aufträgen und Anzeigen von Auftragsentitäten

In der Auftragsansicht können Sie mit Azure Stream Analytics-Aufträgen aus Visual Studio arbeiten.


1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das **Azure**-Symbol, und erweitern Sie dann den Knoten **Stream Analytics**. Ihre Aufträge sollten unter Ihren Abonnements angezeigt werden.

   ![Öffnen von Stream Analytics-Explorer](./media/vscode-explore-jobs/open-explorer.png)

2. Erweitern Sie Ihren Auftragsknoten, und Sie können die Auftragsabfrage, Konfiguration, Eingaben, Ausgaben und Funktionen öffnen und anzeigen. 

3. Klicken Sie mit der rechten Maustaste auf Ihren Auftragsknoten, und wählen Sie den Knoten **Auftragsansicht im Portal öffnen** aus, um die Auftragsansicht im Azure-Portal zu öffnen.

   ![Auftragsansicht im Portal öffnen](./media/vscode-explore-jobs/open-job-view.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Azure Stream Analytics-Cloudauftrags in Visual Studio Code (Vorschauversion)](quick-create-vs-code.md)

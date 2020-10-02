---
title: Oberfläche für einfache Protokolle in Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Mithilfe der Oberfläche für einfache Protokolle können Sie grundlegende Abfragen in Azure Monitor erstellen, ohne direkt mit KQL zu interagieren.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/12/2019
ms.openlocfilehash: 4926e18aa6b00fe36608843ea5253903ace774e2
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90089107"
---
# <a name="simple-logs-experience-in-azure-monitor-preview"></a>Oberfläche für einfache Protokolle in Azure Monitor (Vorschauversion)
Azure Monitor bietet [umfassende Möglichkeiten](get-started-portal.md) für das Erstellen von [Protokollabfragen](log-query-overview.md) mithilfe der KQL-Sprache. Doch möglicherweise benötigen Sie gar nicht den vollen Funktionsumfang von KQL, sondern bevorzugen eine vereinfachte Oberfläche für grundlegende Abfrageanforderungen. Mithilfe der Oberfläche für einfache Protokolle können Sie grundlegende Abfragen erstellen, ohne direkt mit KQL zu interagieren. Sie können einfache Protokolle auch als Lerntool für KQL verwenden, wenn Sie anspruchsvollere Abfragen benötigen.

> [!NOTE]
> Die Oberfläche für einfache Protokolle wird derzeit nur zu Testzwecken für Cosmos DB und Key Vault implementiert. Teilen Sie uns Ihre Erfahrungen über die [User Voice](https://feedback.azure.com/forums/913690-azure-monitor)-Feedbackseite mit, damit wir entscheiden können, ob dieses Feature erweitert und freigegeben werden soll.


## <a name="scope"></a>`Scope`
Die Oberfläche für einfache Protokolle ruft Daten aus den Tabellen *AzureDiagnostics*, *AzureMetrics* und *AzureActivity* für die ausgewählte Ressource ab. 

## <a name="using-simple-logs"></a>Verwenden der Oberfläche für einfache Protokolle
Navigieren Sie zu einer beliebigen Cosmos DB- oder Key Vault-Instanz in Ihrem Azure-Abonnement, für das [Diagnoseeinstellungen zum Sammeln von Protokollen in einem Log Analytics-Arbeitsbereich konfiguriert sind](../platform/resource-logs.md#send-to-azure-storage). Klicken Sie im Menü **Überwachung** auf **Protokolle**, um die Oberfläche für einfache Protokolle zu öffnen.

![Screenshot: Menü „Überwachung“ mit ausgewählter Option „Protokolle“](media/simple-logs/menu.png)

Wählen Sie ein **Feld** und einen **Operator** aus, und geben Sie einen **Wert** für den Vergleich an. Klicken Sie auf **+** , und geben Sie **Und/Oder** an, um weitere Kriterien hinzuzufügen.

![Screenshot: Bereich „Search in the logs“ (In Protokollen suchen) mit ausgewählter Option „Einfache Protokolle“](media/simple-logs/criteria.png)

Klicken Sie auf **Ausführen**, um die Abfrageergebnisse anzuzeigen.

## <a name="view-and-edit-kql"></a>Anzeigen und Bearbeiten von KQL
Wählen Sie **Abfrage-Editor** aus, um die von der Abfrage für einfache Protokolle generierte KQL in der umfassenden Log Analytics-Oberfläche zu öffnen. 

![Abfrage-Editor](media/simple-logs/query-editor.png)

Sie können die KQL direkt bearbeiten und andere Funktionen in Log Analytics wie Filter verwenden, um die Ergebnisse weiter zu verfeinern.

![Bearbeiten von KQL](media/simple-logs/edit-kql.png)


## <a name="next-steps"></a>Nächste Schritte

- Arbeiten Sie ein Tutorial zum [Verwenden von Log Analytics im Azure-Portal](get-started-portal.md) durch.
- Arbeiten Sie ein Tutorial zum [Schreiben von Protokollabfragen](get-started-portal.md) durch.

---
title: Visualisieren Ihrer Daten mithilfe von Azure Monitor-Arbeitsmappen in Azure Sentinel | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Ihre Daten mithilfe von Arbeitsmappen in Azure Sentinel visualisieren.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2020
ms.author: yelevin
ms.openlocfilehash: 3f59fea9f6d96f6e259a47c75bcfb0fd0be76d34
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91826567"
---
# <a name="tutorial-visualize-and-monitor-your-data"></a>Tutorial: Visualisieren und Überwachen Ihrer Daten



Nachdem Sie Ihre [Datenquellen mit Azure Sentinel verbunden](quickstart-onboard.md)  haben, können Sie die Daten mithilfe der Azure Sentinel-Einführung von Azure Monitor-Arbeitsmappen visualisieren und überwachen, die für Vielseitigkeit bei der Erstellung benutzerdefinierter Dashboards sorgt. Zwar werden Arbeitsmappen in Azure Sentinel anders angezeigt, dennoch kann es für Sie hilfreich sein, zu erfahren, wie Sie [interaktive Berichte mit Azure Monitor-Arbeitsmappen erstellen](../azure-monitor/platform/workbooks-overview.md) können. Azure Sentinel ermöglicht das Erstellen von benutzerdefinierten Arbeitsmappen für Ihre Daten und enthält auch integrierte Arbeitsmappenvorlagen, mit denen Sie schnell Erkenntnisse zu den Daten erhalten, sobald Sie eine Verbindung mit einer Datenquelle herstellen.


In diesem Tutorial erfahren Sie, wie Sie Ihre Daten in Azure Sentinel visualisieren.
> [!div class="checklist"]
> * Verwenden integrierter Arbeitsmappen
> * Erstellen neuer Arbeitsmappen

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen mindestens über die Berechtigung „Arbeitsmappenleser“ oder „Arbeitsmappenmitwirkender“ für die Ressourcengruppe des Azure Sentinel-Arbeitsbereichs verfügen.

> [!NOTE]
> Die Arbeitsmappen, die in Azure Sentinel angezeigt werden, werden in der Ressourcengruppe des Azure Sentinel-Arbeitsbereichs gespeichert und sind durch den Arbeitsbereich markiert, in dem Sie erstellt wurden.

## <a name="use-built-in-workbooks"></a>Verwenden integrierter Arbeitsmappen

1. Wechseln Sie zu **Workbooks** (Arbeitsmappen), und wählen Sie dann **Templates** (Vorlagen) aus, um die vollständige Liste der in Azure Sentinel integrierten Arbeitsmappen anzuzeigen. Wenn Sie feststellen möchten, welche für die Datentypen relevant sind, mit denen Sie eine Verbindung hergestellt haben, wird Ihnen im Feld **Required data types** (Erforderliche Datentypen) in jeder Arbeitsmappe der Datentyp neben einem grünen Häkchen aufgelistet, wenn Sie bereits relevante Daten an Azure Sentinel streamen.
  ![Go to workbooks](./media/tutorial-monitor-data/access-workbooks.png) (Zu Arbeitsmappen wechseln)
1. Klicken Sie auf **View template** (Vorlage anzeigen), um die mit Ihren Daten aufgefüllte Vorlage anzuzeigen.
  
1. Wenn Sie die Arbeitsmappe bearbeiten möchten, klicken Sie auf **Save** (Speichern), und wählen Sie dann den Speicherort aus, an dem Sie die JSON-Datei für die Vorlage speichern möchten. 

   > [!NOTE]
   > Dadurch wird eine Azure-Ressource erstellt, die auf der relevanten Vorlage basiert, und die JSON-Datei der Arbeitsmappe, nicht die Daten, gespeichert.


1. Wählen Sie **View saved workbook** (Gespeicherte Arbeitsmappe anzeigen) aus. Klicken Sie dann oben auf **Edit** (Bearbeiten). Sie können die Arbeitsmappe jetzt bearbeiten und entsprechend Ihren Anforderungen anpassen. Weitere Informationen zum Anpassen der Arbeitsmappe finden Sie unter [Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen](../azure-monitor/platform/workbooks-overview.md).
![View workbooks](./media/tutorial-monitor-data/workbook-graph.png) (Arbeitsmappen anzeigen)
1. Nachdem Sie die Änderungen vorgenommen haben, können Sie die Arbeitsmappe speichern. 

1. Sie können die Arbeitsmappe auch klonen: Wählen Sie **Edit** (Bearbeiten) und dann **Save as** (Speichern unter) aus, und speichern Sie sie unter einem anderen Namen in demselben Abonnement und in derselben Ressourcengruppe. Diese geklonten Arbeitsmappen werden auf der Registerkarte **My workbooks** (Meine Arbeitsmappen) angezeigt.


## <a name="create-new-workbook"></a>Neue Arbeitsmappe erstellen

1. Klicken Sie auf **Workbooks** (Arbeitsmappen), und wählen Sie dann **Add workbook** (Arbeitsmappe hinzufügen), um eine komplett neue Arbeitsmappe zu erstellen.
  ![Go to workbooks](./media/tutorial-monitor-data/create-workbook.png) (Zu Arbeitsmappen wechseln)

1. Um die Arbeitsmappe zu bearbeiten, wählen Sie **Edit** (Bearbeiten) aus, und fügen Sie dann nach Bedarf Text, Abfragen und Parameter hinzu. Weitere Informationen zum Anpassen der Arbeitsmappe finden Sie unter [Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen](../azure-monitor/platform/workbooks-overview.md). 

1. Stellen Sie bei der Erstellung einer Abfrage sicher, dass **Data source** (Datenquelle) auf **Logs** (Protokolle) und **Resource type** (Ressourcentyp) auf **Log Analytics** (Protokollanalyse) festgelegt ist, und wählen Sie dann die relevanten Arbeitsbereiche aus. 

1. Nachdem Sie die Arbeitsmappe erstellt haben, speichern Sie diese in dem Abonnement und in der Ressourcengruppe Ihres Azure Sentinel-Arbeitsbereichs.

1. Wenn Sie möchten, dass andere Personen in Ihrer Organisation die Arbeitsmappe verwenden, wählen Sie unter **Save to** (Speichern in) die Option **Shared reports** (Freigegebene Berichte) aus. Wenn Sie möchten, dass diese Arbeitsmappe nur für Sie verfügbar ist, wählen Sie **My reports** (Meine Berichte) aus.

1. Um zwischen Arbeitsmappen in Ihrem Arbeitsbereich zu wechseln, können Sie **Öffnen** ![Arbeitsmappen wechseln](./media/tutorial-monitor-data/switch.png) im oberen Bereich jeder Arbeitsmappe auswählen. Wechseln Sie im Fenster, das rechts geöffnet wird, zwischen Arbeitsmappen.

   ![Arbeitsmappen wechseln](./media/tutorial-monitor-data/switch-workbooks.png)


## <a name="how-to-delete-workbooks"></a>Löschen von Arbeitsmappen

Um eine gespeicherte Arbeitsmappe (entweder eine gespeicherte Vorlage oder eine angepasste Arbeitsmappe) zu löschen, wählen Sie diese auf der Seite „Arbeitsmappen“ aus, und wählen Sie **Löschen** aus. Dadurch wird die gespeicherte Arbeitsmappe entfernt.

> [!NOTE]
> Dadurch werden sowohl die Arbeitsmappenressource als auch alle Änderungen entfernt, die Sie an der Vorlage vorgenommen haben. Die ursprüngliche Vorlage bleibt verfügbar.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie Ihre Daten in Azure Sentinel über Azure-Arbeitsmappen visualisieren.

Informationen zum Automatisieren Ihrer Reaktionen auf Bedrohungen finden Sie unter [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](tutorial-respond-threats-playbook.md).

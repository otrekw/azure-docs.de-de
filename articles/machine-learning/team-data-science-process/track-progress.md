---
title: Nachverfolgen des Fortschritts von Team Data Science-Prozessprojekten
description: Erfahren Sie, wie Data Science-Gruppenmanager, -Teamleiter und -Projektleiter den Status eines Data Science-Projekts nachverfolgen können.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8cf1e5a4d97b882e7a8d0c81041bbcde709760d1
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75864195"
---
# <a name="track-the-progress-of-data-science-projects"></a>Nachverfolgen des Fortschritts von Data Science-Projekten

Data Science-Gruppenmanager, Teamleiter und Projektleiter können den Fortschritt ihrer Projekte nachverfolgen.  Manager möchten wissen, welche Arbeit durchgeführt wurde, wer die Arbeit erledigt hat und welche Aufgaben verbleiben.   Die Verwaltung von Erwartungen ist ein wichtiges Element für den Erfolg.

## <a name="azure-devops-dashboards"></a>Azure DevOps-Dashboards

Bei Verwendung von Azure DevOps können Sie Dashboards zum Nachverfolgen der Aktivitäten und der einem bestimmten Agile-Projekt zugeordneten Arbeitselemente erstellen. Weitere Informationen zu Dashboards finden Sie unter [Dashboards, Berichte und Widgets](/azure/devops/report/dashboards/).

Anweisungen zum Erstellen und Anpassen von Dashboards und Widgets in Azure DevOps finden Sie in den folgenden Schnellstarts:

- [Hinzufügen und Verwalten von Dashboards](/azure/devops/report/dashboards/dashboards)
- [Hinzufügen von Widgets zu einem Dashboard](/azure/devops/report/dashboards/add-widget-to-dashboard)

## <a name="example-dashboard"></a>Beispieldashboard

Dies ist ein einfaches Beispieldashboard zum Nachverfolgen der Sprintaktivitäten in einem Agile-Data Science-Projekt, einschließlich der Anzahl von Commits in den zugeordneten Repositorys. 

- Die Kachel **Countdown** zeigt die Anzahl der verbleibenden Tage im aktuellen Sprint an. 

- Die beiden **Codekacheln** zeigen die Anzahl von Commits in den beiden Projektrepositorys in den letzten sieben Tagen an. 

- **Arbeitselemente für das TDSP-Kundenprojekt** zeigt die Ergebnisse einer Abfrage für alle Arbeitsaufgaben und deren Status an. 

- Ein **kumulatives Flussdiagramm** zeigt die Anzahl der geschlossenen und aktiven Arbeitselemente an.

- Das **Burndowndiagramm** zeigt Arbeit an, die in der verbleibenden Zeit im Sprint noch fertiggestellt werden muss.

- Das **Burnupdiagramm** zeigt abgeschlossene Arbeit im Vergleich zur Gesamtmenge an Arbeit im Sprint an.

![Dashboard](./media/track-progress/dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

Unter [Exemplarischen Vorgehensweisen der Ausführung des Team Data Science-Prozesses](walkthroughs.md) werden exemplarische Vorgehensweisen aufgeführt, in denen alle Schritte des Prozesses veranschaulicht werden. Die verknüpften Szenarien veranschaulichen, wie die Cloud und die lokalen Ressourcen in intelligenten Anwendungen verwaltet werden können. 

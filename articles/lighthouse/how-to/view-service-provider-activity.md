---
title: Anzeigen der Dienstanbieteraktivität
description: Kunden können anhand von protokollierten Aktivitäten überprüfen, welche Aktionen Dienstanbieter im Rahmen der delegierten Azure-Ressourcenverwaltung ausgeführt haben.
ms.date: 12/6/2019
ms.topic: conceptual
ms.openlocfilehash: 69517e942aa9f82be16fa3d0e7d6f9252de44d4c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932247"
---
# <a name="view-service-provider-activity"></a>Anzeigen der Dienstanbieteraktivität

Kunden, die Abonnements für die delegierte Azure-Ressourcenverwaltung delegiert haben, können [Daten des Azure-Aktivitätsprotokolls anzeigen](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview), um sich alle ausgeführten Aktionen anzusehen. Dadurch erhalten Kunden nicht nur einen umfassenden Einblick in Vorgänge, die von Dienstanbietern im Rahmen der delegierten Azure-Ressourcenverwaltung ausgeführt werden, sondern auch in Vorgänge, die von Benutzern innerhalb des eigenen Azure AD-Mandanten (Azure Active Directory) des Kunden ausgeführt werden.

## <a name="view-activity-log-data"></a>Anzeigen von Aktivitätsprotokolldaten

Zum [Anzeigen des Aktivitätsprotokolls](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview#view-the-activity-log) können Sie das Menü **Monitor** im Azure-Portal verwenden. Wenn Sie die Ergebnisse auf ein bestimmtes Abonnement beschränken möchten, können Sie mithilfe der Filter ein bestimmtes Abonnement auswählen. Außerdem haben Sie die Möglichkeit zum programmgesteuerten [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view).

> [!NOTE]
> Benutzer im Mandanten eines Dienstanbieters können die Ergebnisse des Aktivitätsprotokolls für ein delegiertes Abonnement in einem Kundenmandanten anzeigen, wenn ihnen beim Onboarding des Abonnements für die delegierte Azure-Ressourcenverwaltung die Rolle [Leser](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) (oder eine andere integrierte Rolle mit Lesezugriff) erteilt wurde.

Im Aktivitätsprotokoll werden der Name des Vorgangs und dessen Status sowie Datum und Uhrzeit der Ausführung angezeigt. Die Spalte **Ereignis initiiert von** gibt Aufschluss darüber, welcher Benutzer den Vorgang ausgeführt hat und ob der Benutzer ein Benutzer im Mandanten eines Dienstanbieters war, der im Rahmen der delegierten Azure-Ressourcenverwaltung gehandelt hat, oder ein Benutzer im eigenen Mandanten des Kunden. Beachten Sie, dass der Name des Benutzers angezeigt wird – nicht der Mandant oder die Rolle, die dem Benutzer für dieses Abonnement zugewiesen wurde.

Im Azure-Portal sind protokollierte Aktivitäten der letzten 90 Tage verfügbar. Unter [Erfassen und Analysieren von Azure-Aktivitätsprotokollen im Log Analytics-Arbeitsbereich in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect) erfahren Sie, wie Sie diese Daten länger als 90 Tage speichern können.

## <a name="set-alerts-for-critical-operations"></a>Festlegen von Warnungen für kritische Vorgänge

Wir empfehlen die Erstellung von [Aktivitätsprotokollwarnungen](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-alerts), um über kritische Vorgänge informiert zu sein, die von Dienstanbietern (oder Benutzern in Ihrem eigenen Mandanten) ausgeführt werden. So kann es beispielsweise empfehlenswert sein, alle administrativen Aktionen für ein Abonnement nachzuverfolgen oder sich benachrichtigen zu lassen, wenn in einer bestimmten Ressourcengruppe ein virtueller Computer gelöscht wird. Die erstellten Warnungen gelten sowohl für Aktionen von Benutzern im eigenen Mandanten des Kunden als auch für Benutzer in Verwaltungsmandanten.

Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

## <a name="create-log-queries"></a>Erstellen von Protokollabfragen

Sie können Abfragen erstellen, um Ihre protokollierten Aktivitäten zu analysieren oder sich auf bestimmte Elemente zu konzentrieren. Beispielsweise kann es vorkommen, dass im Rahmen einer Überprüfung ein Bericht mit sämtlichen Aktionen auf Administratorebene benötigt wird, die für ein Abonnement ausgeführt wurden. Sie können eine Abfrage erstellen, um nur nach diesen Aktionen zu filtern und die Ergebnisse nach Benutzer, Datum oder nach einem anderen Wert zu sortieren.

Weitere Informationen finden Sie in der [Übersicht über Protokollabfragen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Monitor finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/).
- Informationen zum Anzeigen und Verwalten von Dienstanbietern im Azure-Portal finden Sie [hier](view-manage-service-providers.md).
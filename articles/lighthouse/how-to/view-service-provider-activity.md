---
title: Anzeigen der Dienstanbieteraktivität
description: Kunden können anhand von protokollierten Aktivitäten überprüfen, welche Aktionen Dienstanbieter im Rahmen der delegierten Azure-Ressourcenverwaltung ausgeführt haben.
ms.date: 10/12/2020
ms.topic: how-to
ms.openlocfilehash: 8ec9d49de953a0fed3a5afc669e9297148aadf32
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91974310"
---
# <a name="view-service-provider-activity"></a>Anzeigen der Dienstanbieteraktivität

Kunden, die delegierte Abonnements für [Azure Lighthouse](../overview.md) haben, können [Daten des Azure-Aktivitätsprotokolls anzeigen](../../azure-monitor/platform/platform-logs-overview.md), um sich alle ausgeführten Aktionen anzusehen. Dadurch erhalten Kunden nicht nur einen umfassenden Einblick in Vorgänge, die von Dienstanbietern im Rahmen der [delegierten Azure-Ressourcenverwaltung](../concepts/azure-delegated-resource-management.md) ausgeführt werden, sondern auch in Vorgänge, die von Benutzern innerhalb des eigenen Azure AD-Mandanten (Azure Active Directory) des Kunden ausgeführt werden.

> [!TIP]
> Wir bieten in Azure Policy integrierte Richtliniendefinitionen, um die [Delegierung an bestimmte verwaltende Mandanten einzuschränken](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Lighthouse/AllowCertainManagingTenantIds_Deny.json) und die [Delegierung von Bereichen an einen verwaltenden Mandanten zu überwachen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/Lighthouse_Delegations_Audit.json). Weitere Informationen finden Sie unter [Überwachen von Delegierungen in Ihrer Umgebung](view-manage-service-providers.md#audit-delegations-in-your-environment).

## <a name="view-activity-log-data"></a>Anzeigen von Aktivitätsprotokolldaten

Zum [Anzeigen des Aktivitätsprotokolls](../../azure-monitor/platform/activity-log.md#view-the-activity-log) können Sie das Menü **Monitor** im Azure-Portal verwenden. Wenn Sie die Ergebnisse auf ein bestimmtes Abonnement beschränken möchten, wählen Sie mithilfe der Filter ein bestimmtes Abonnement aus. Außerdem haben Sie die Möglichkeit zum programmgesteuerten [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](../../azure-monitor/platform/activity-log.md#view-the-activity-log).

> [!NOTE]
> Benutzer im Mandanten eines Dienstanbieters können die Ergebnisse des Aktivitätsprotokolls für ein delegiertes Abonnement in einem Kundenmandanten anzeigen, wenn diesem Abonnement beim Onboarding für Azure Lighthouse die Rolle [Leser](../../role-based-access-control/built-in-roles.md#reader) (oder eine andere integrierte Rolle mit Lesezugriff) erteilt wurde.

Im Aktivitätsprotokoll werden der Name des Vorgangs und dessen Status sowie Datum und Uhrzeit der Ausführung angezeigt. Die Spalte **Ereignis initiiert von** gibt Aufschluss darüber, welcher Benutzer den Vorgang ausgeführt hat und ob der Benutzer ein Benutzer im Mandanten eines Dienstanbieters war, der im Rahmen von Azure Lighthouse gehandelt hat, oder ein Benutzer im eigenen Mandanten des Kunden. Beachten Sie, dass der Name des Benutzers angezeigt wird – nicht der Mandant oder die Rolle, die dem Benutzer für dieses Abonnement zugewiesen wurde.

Im Azure-Portal sind protokollierte Aktivitäten der letzten 90 Tage verfügbar. Unter [Erfassen und Analysieren von Azure-Aktivitätsprotokollen im Log Analytics-Arbeitsbereich](../../azure-monitor/platform/activity-log.md) erfahren Sie, wie Sie diese Daten länger als 90 Tage speichern können.

> [!NOTE]
> Benutzer des Dienstanbieters werden im Aktivitätsprotokoll angezeigt, aber diese Benutzer und Ihre Rollenzuweisungen werden weder in **Access Control (IAM)** noch beim Abrufen von Rollenzuweisungsinformationen über APIs angezeigt.

## <a name="set-alerts-for-critical-operations"></a>Festlegen von Warnungen für kritische Vorgänge

Wir empfehlen die Erstellung von [Aktivitätsprotokollwarnungen](../../azure-monitor/platform/activity-log-alerts.md), um über kritische Vorgänge informiert zu sein, die von Dienstanbietern (oder Benutzern in Ihrem eigenen Mandanten) ausgeführt werden. So kann es beispielsweise empfehlenswert sein, alle administrativen Aktionen für ein Abonnement nachzuverfolgen oder sich benachrichtigen zu lassen, wenn in einer bestimmten Ressourcengruppe ein virtueller Computer gelöscht wird. Die erstellten Warnungen gelten sowohl für Aktionen von Benutzern im eigenen Mandanten des Kunden als auch für Benutzer in Verwaltungsmandanten.

Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor](../../azure-monitor/platform/alerts-activity-log.md).

## <a name="create-log-queries"></a>Erstellen von Protokollabfragen

Sie können Abfragen erstellen, um Ihre protokollierten Aktivitäten zu analysieren oder sich auf bestimmte Elemente zu konzentrieren. Beispielsweise kann es vorkommen, dass im Rahmen einer Überprüfung ein Bericht mit sämtlichen Aktionen auf Administratorebene benötigt wird, die für ein Abonnement ausgeführt wurden. Sie können eine Abfrage erstellen, um nur nach diesen Aktionen zu filtern und die Ergebnisse nach Benutzer, Datum oder nach einem anderen Wert zu sortieren.

Weitere Informationen finden Sie in der [Übersicht über Protokollabfragen in Azure Monitor](../../azure-monitor/log-query/log-query-overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Monitor finden Sie [hier](../../azure-monitor/index.yml).
- Informationen zum Anzeigen und Verwalten von Dienstanbietern im Azure-Portal finden Sie [hier](view-manage-service-providers.md).

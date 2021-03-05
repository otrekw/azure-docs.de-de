---
title: Problembehandlung für das Feature „Gastintegrität“ von VM Insights (Vorschau)
description: Enthält eine Beschreibung der Problembehandlungsschritte, die Sie ausführen können, wenn Probleme mit der Integrität in VM Insights auftreten.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/08/2020
ms.openlocfilehash: d8b37569ebaa8e75be601a1efd65a23a61aeaa75
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051938"
---
# <a name="troubleshoot-vm-insights-guest-health-preview"></a>Problembehandlung für das Feature „Gastintegrität“ von VM Insights (Vorschau)
Dieser Artikel enthält eine Beschreibung der Problembehandlungsschritte, die Sie ausführen können, wenn Probleme mit der Integrität in VM Insights auftreten.

## <a name="error-message-that-no-data-is-available"></a>Fehlermeldung: Keine Daten verfügbar 

![Keine Daten](media/vminsights-health-troubleshoot/no-data.png)


### <a name="verify-that-the-virtual-machine-meets-configuration-requirements"></a>Überprüfen, ob der virtuelle Computer die Konfigurationsanforderungen erfüllt

1. Vergewissern Sie sich, dass die VM ein virtueller Azure-Computer ist. Azure Arc für Server wird derzeit nicht unterstützt.
2. Vergewissern Sie sich, dass auf dem virtuellen Computer ein [unterstütztes Betriebssystem](vminsights-health-enable.md?current-limitations.md) ausgeführt wird.
3. Stellen Sie sicher, dass der virtuelle Computer in einer [unterstützten Region](vminsights-health-enable.md?current-limitations.md) installiert ist.
4. Stellen Sie sicher, dass der Log Analytics-Arbeitsbereich in einer [unterstützten Region](vminsights-health-enable.md?current-limitations.md) installiert ist.

### <a name="verify-that-the-vm-is-properly-onboarded"></a>Überprüfen, ob das Onboarding für die VM korrekt durchgeführt wurde
Vergewissern Sie sich, dass die Bereitstellung der Azure Monitor-Agent-Erweiterung und des Agents für die Integrität der Gast-VM auf dem virtuellen Computer erfolgreich war. Wählen Sie im Azure-Portal im Menü des virtuellen Computers die Option **Erweiterungen** aus, und vergewissern Sie sich, dass die beiden Agents aufgelistet sind.

![VM-Erweiterungen](media/vminsights-health-troubleshoot/extensions.png)

### <a name="verify-the-system-assigned-identity-is-enabled-on-the-virtual-machine"></a>Überprüfen, ob auf dem virtuellen Computer die systemseitig zugewiesene Identität aktiviert ist
Überprüfen Sie, ob auf dem virtuellen Computer die systemseitig zugewiesene Identität aktiviert ist. Wählen Sie im Azure-Portal im Menü des virtuellen Computers die Option **Identität** aus. Wenn die vom Benutzer verwaltete Identität aktiviert ist, kann der Azure Monitor-Agent, unabhängig vom Status der vom System verwalteten Identität, nicht mit dem Konfigurationsdienst kommunizieren, und die Erweiterung für die Gastintegrität funktioniert nicht.

![Systemzugewiesene Identität](media/vminsights-health-troubleshoot/system-identity.png)

### <a name="verify-data-collection-rule"></a>Überprüfen der Datensammlungsregel
Vergewissern Sie sich, dass die Datensammlungsregel, mit der die Integritätserweiterung als Datenquelle angegeben wird, dem virtuellen Computer zugeordnet ist.

## <a name="error-message-for-bad-request-due-to-insufficient-permissions"></a>Fehlermeldung: Ungültige Anforderung aufgrund von unzureichenden Berechtigungen
Dieser Fehler ist ein Hinweis darauf, dass der Ressourcenanbieter **Microsoft.WorkloadMonitor** unter dem Abonnement nicht registriert wurde. Ausführliche Informationen zum Registrieren dieses Ressourcenanbieters finden Sie unter [Azure-Ressourcenanbieter und -typen](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider). 

![Ungültige Anforderung](media/vminsights-health-troubleshoot/bad-request.png)

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über das Feature „Gastintegrität“ von VM Insights](vminsights-health-overview.md)
---
title: Verwalten von Warnungen von System Center Operations Manager, Zabbix und Nagios in Azure Monitor
description: Verwalten von Warnungen von System Center Operations Manager, Zabbix und Nagios in Azure Monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.subservice: alerts
ms.openlocfilehash: 9fbad920bdfa26419e4b865db5d4e53dad97acce
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539447"
---
# <a name="manage-alerts-from-system-center-operations-manager-zabbix-and-nagios-in-azure-monitor"></a>Verwalten von Warnungen von System Center Operations Manager, Zabbix und Nagios in Azure Monitor

Sie können jetzt Ihre Warnungen aus Nagios, Zabbix und System Center Operations Manager in [Azure Monitor](https://aka.ms/azure-alerts-overview) anzeigen. Diese Warnungen stammen von der Integration mit Nagios-/Zabbix-Servern oder System Center Operations Manager in Log Analytics. 

## <a name="prerequisites"></a>Voraussetzungen
Alle Datensätze im Log Analytics-Repository mit einem Warnungstyp werden in Azure Monitor importiert, daher müssen Sie die Konfiguration durchführen, die zum Erfassen dieser Datensätze erforderlich ist.
1. Für Warnungen aus **Nagios** und **Zabbix** müssen Sie [diese Server konfigurieren](../learn/quick-collect-linux-computer.md), sodass sie [Warnungen](./data-sources-custom-logs.md?toc=/azure/azure-monitor/toc.json) an Log Analytics senden.
1. Für **System Center Operations Manager**-Warnungen müssen Sie Ihre [Operations Manager-Verwaltungsgruppe mit Ihrem Log Analytics-Arbeitsbereich verbinden](./om-agents.md). Stellen Sie anschließend die Lösung [Warnungsverwaltung](./alert-management-solution.md) aus dem Marketplace für Azure-Lösungen bereit. Nach der Bereitstellung werden alle in System Center Operations Manager erstellten Warnungen in Log Analytics importiert.

## <a name="view-your-alert-instances"></a>Anzeigen Ihrer Warnungsinstanzen
Nachdem Sie den Import in Log Analytics konfiguriert haben, können Sie mit der Anzeige von Warnungsinstanzen aus diesen Überwachungsdiensten in [Azure Monitor](https://aka.ms/azure-alerts-overview) beginnen. Sobald sie in Azure Monitor vorhanden sind, können Sie [Ihre Warnungsinstanzen verwalten](https://aka.ms/managing-alert-instances), [die für diese Warnungen erstellten intelligenten Gruppen verwalten](https://aka.ms/managing-smart-groups) und [den Zustand der Warnungen und intelligenten Gruppen ändern](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  1. Mit dieser Lösung können Sie nur von System Center Operations Manager/Zabbix/Nagios ausgelöste Warnungsinstanzen in Azure Monitor anzeigen. Warnungsregelkonfiguration können nur in den entsprechenden Überwachungstools angezeigt/bearbeitet werden. 
>  1. Alle ausgelösten Warnungsinstanzen sind sowohl in Azure Monitor als auch Azure Log Analytics verfügbar. Derzeit gibt es keine Möglichkeit, zwischen den beiden Optionen zu wählen oder nur bestimmte ausgelöste Warnungen zu erfassen.
>  1. Alle Warnungen aus System Center Operations Manager, Zabbix und Nagios weisen den Signaltyp „Unbekannt“ auf, da der zugrunde liegende Telemetrietyp nicht verfügbar ist.
>  1. Nagios-Warnungen sind nicht zustandsbehaftet. Die [Überwachungsbedingung](https://aka.ms/azure-alerts-overview) einer Warnung wechselt z.B. nicht von „Ausgelöst“ zu „Gelöst“. Stattdessen werden sowohl „Ausgelöst“ als auch „Gelöst“ als separate Warnungsinstanzen angezeigt. 

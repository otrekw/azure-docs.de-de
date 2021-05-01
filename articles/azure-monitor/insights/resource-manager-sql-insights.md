---
title: Beispiele für Resource Manager-Vorlagen für SQL Insights
description: Hier finden Sie Beispiele für Azure Resource Manager-Vorlagen zum Bereitstellen und Konfigurieren von SQL Insights.
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 03/25/2021
ms.openlocfilehash: dafb7335ef211cb9173ec2fb4565243d603d35fe
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107730190"
---
# <a name="resource-manager-template-samples-for-sql-insights"></a>Beispiele für Resource Manager-Vorlagen für SQL Insights
Dieser Artikel enthält [Azure Resource Manager-Beispielvorlagen](../../azure-resource-manager/templates/template-syntax.md) zum Aktivieren von SQL Insights für die Überwachung der in Azure ausgeführten SQL-Instanz.  Details zu den von uns unterstützten SQL-Angeboten und -Versionen finden Sie in der [Dokumentation zu SQL Insights](sql-insights-overview.md). Jedes Beispiel umfasst eine Vorlagendatei und eine Parameterdatei mit Beispielwerten für die Vorlage.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]


## <a name="create-a-sql-insights-monitoring-profile"></a>Erstellen eines Überwachungsprofils für SQL Insights
Im folgenden Beispiel wird ein SQL Insights-Überwachungsprofil erstellt, das die zu sammelnden SQL-Überwachungsdaten und die Häufigkeit der Datensammlung enthält und den Arbeitsbereich angibt, an den die Daten gesendet werden.


### <a name="template-file"></a>Vorlagendatei

Sehen Sie sich die [Vorlagendatei auf GitHub](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Workloads/SQL/Create%20new%20profile/CreateNewProfile.armtemplate) an.

### <a name="parameter-file"></a>Parameterdatei

Sehen Sie sich die [Parameterdatei auf GitHub](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Workloads/SQL/Create%20new%20profile/CreateNewProfile.parameters.json) an.


## <a name="add-a-monitoring-vm-to-a-sql-insights-monitoring-profile"></a>Hinzufügen eines virtuellen Überwachungscomputers zu einem SQL Insights-Überwachungsprofil
Nach der Erstellung eines Überwachungsprofils müssen Sie virtuelle Azure-Computer zuordnen. Diese werden für die Remotesammlung von Daten von den SQL-Ressourcen konfiguriert, die Sie in der Konfiguration für diesen virtuellen Computer angeben.  Ausführlichere Informationen finden Sie in der Dokumentation zum Aktivieren von SQL Insights.

Im folgenden Beispiel wird eine Überwachungs-VM konfiguriert, um die Daten von den angegebenen SQL-Ressourcen zu sammeln.


### <a name="template-file"></a>Vorlagendatei

Sehen Sie sich die [Vorlagendatei auf GitHub](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Workloads/SQL/Add%20monitoring%20virtual%20machine/AddMonitoringVirtualMachine.armtemplate) an.

### <a name="parameter-file"></a>Parameterdatei

Sehen Sie sich die [Parameterdatei auf GitHub](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Workloads/SQL/Add%20monitoring%20virtual%20machine/AddMonitoringVirtualMachine.parameters.json) an.


## <a name="create-an-alert-rule-for-sql-insights"></a>Erstellen einer Warnungsregel für SQL Insights
Im folgenden Beispiel wird eine Warnungsregel für die SQL-Ressourcen innerhalb des Bereichs des angegebenen Überwachungsprofils erstellt.  Diese Warnungsregel wird auf der Benutzeroberfläche von SQL Insights im Kontextbereich der Warnungsbenutzeroberfläche angezeigt.  

Die Parameterdatei enthält Werte aus einer der in SQL Insights bereitgestellten Warnungsvorlagen. Sie können sie ändern, um Warnungen für andere Daten zu erhalten, die für SQL gesammelt werden.  Die Vorlage gibt keine Aktionsgruppe für die Warnungsregel an.


#### <a name="template-file"></a>Vorlagendatei

Sehen Sie sich die [Vorlagendatei auf GitHub](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Workloads/Alerts/log-metric-noag.armtemplate) an.

### <a name="parameter-file"></a>Parameterdatei

Sehen Sie sich die [Parameterdatei auf GitHub](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Workloads/Alerts/sql-cpu-utilization-percent.parameters.json) an.





## <a name="next-steps"></a>Nächste Schritte

* [Rufen Sie weitere Beispielvorlagen für Azure Monitor ab](../resource-manager-samples.md).
* [Weitere Informationen zu SQL Insights](sql-insights-overview.md)

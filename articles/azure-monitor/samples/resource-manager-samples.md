---
title: Beispiele für Resource Manager-Vorlagen für Azure Monitor
description: Bereitstellen und Konfigurieren von Azure Monitor-Features mithilfe von Resource Manager-Vorlagen
author: bwren
ms.author: bwren
services: azure-monitor
ms.topic: sample
ms.date: 05/18/2020
ms.subservice: ''
ms.openlocfilehash: ab869fc8577d4d1934be96404ded5a2051237bbf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87922727"
---
# <a name="resource-manager-template-samples-for-azure-monitor"></a>Beispiele für Resource Manager-Vorlagen für Azure Monitor

Azure Monitor kann mithilfe von [Azure Resource Manager-Vorlagen](../../azure-resource-manager/templates/template-syntax.md) im gewünschten Umfang bereitgestellt und konfiguriert werden. Die folgenden Artikel enthalten Beispielvorlagen für verschiedene Azure Monitor-Features. Diese Beispiele können entsprechend Ihren speziellen Anforderungen geändert und anhand einer beliebigen Standardmethode zum Bereitstellen von Resource Manager-Vorlagen bereitgestellt werden. 

## <a name="deploying-the-sample-templates"></a>Bereitstellen der Beispielvorlagen
Hier sind die grundlegenden Schritte zur Verwendung der Beispiele aufgeführt:

1. Kopieren Sie die Vorlage, und speichern Sie sie als JSON-Datei.
2. Ändern Sie die Parameter für Ihre Umgebung, und speichern Sie sie als JSON-Datei.
4. Stellen Sie die Vorlage mithilfe einer [beliebigen Bereitstellungsmethode für Resource Manager-Vorlagen](../../azure-resource-manager/templates/deploy-powershell.md) bereit. 

Verwenden Sie beispielsweise die folgenden Befehle, um die Vorlagen- und Parameterdatei für eine Ressourcengruppe mithilfe von PowerShell oder der Azure CLI bereitzustellen.


```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName my-subscription
New-AzResourceGroupDeployment -Name AzureMonitorDeployment -ResourceGroupName my-resource-group -TemplateFile azure-monitor-deploy.json -TemplateParameterFile azure-monitor-deploy.parameters.json
```

```azurecli
az login
az deployment group create \
    --name AzureMonitorDeployment \
    --resource-group ResourceGroupofTargetResource \
    --template-file azure-monitor-deploy.json \
    --parameters azure-monitor-deploy.parameters.json
```

## <a name="list-of-sample-templates"></a>Liste der Beispielvorlagen

- [Agents](resource-manager-agent.md): Bereitstellen und Konfigurieren des Log Analytics-Agents und der Diagnoseerweiterung
- Alerts
  - [Protokollwarnungsregeln](resource-manager-alerts-log.md): Warnungen von Protokollabfragen und Azure-Aktivitätsprotokoll
  - [Metrikwarnungsregeln](resource-manager-alerts-metric.md): Warnungen von Metriken unter Verwendung verschiedener Arten von Logik
- [Application Insights](resource-manager-app-resource.md)
- [Diagnoseeinstellungen](resource-manager-diagnostic-settings.md): Erstellen von Diagnoseeinstellungen zum Weiterleiten von Protokollen und Metriken von verschiedenen Ressourcentypen
- [Protokollabfragen](resource-manager-log-queries.md): Erstellen gespeicherter Protokollabfragen in einem Log Analytics-Arbeitsbereich
- [Log Analytics-Arbeitsbereich](resource-manager-workspace.md): Erstellen eines Log Analytics-Arbeitsbereichs und Konfigurieren der Erfassung verschiedener Datenquellen vom Log Analytics Agent
- [Arbeitsmappen](resource-manager-workbooks.md): Erstellen von Arbeitsmappen
- [Azure Monitor für Container](resource-manager-container-insights.md): Integrieren von Clustern in Azure Monitor für Container
- [Azure Monitor für VMs](resource-manager-vminsights.md): Integrieren virtueller Computer in Azure Monitor für VMs



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Resource Manager-Vorlagen](../../azure-resource-manager/templates/overview.md).
---
title: Erstellen einer neuen Azure Application Insights-Ressource | Microsoft Docs
description: Richten Sie manuell die Application Insights-Überwachung für eine neue Liveanwendung ein.
ms.topic: conceptual
ms.date: 02/10/2021
ms.openlocfilehash: 6158b5604046897e20053c67321f26d650c21b7f
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107566221"
---
# <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource

Azure Application Insights zeigt Daten über Ihre Anwendung in einer Microsoft Azure-*Ressource* an. Die Erstellung einer neuen Ressource gehört daher zur [Einrichtung von Application Insights zur Überwachung einer neuen Anwendung][start]. Nach dem Erstellen der neuen Ressource erhalten Sie den Instrumentierungsschlüssel, den Sie zum Konfigurieren des Application Insights SDK verwenden. Der Instrumentierungsschlüssel verknüpft Ihre Telemetriedaten mit der Ressource.

> [!IMPORTANT]
> [Klassisches Application Insights ist veraltet](https://azure.microsoft.com/updates/we-re-retiring-classic-application-insights-on-29-february-2024/). Befolgen Sie diese [Anweisungen zur Vorgehensweise beim Upgrade auf arbeitsbereichsbasierte Application Insights-Ressourcen](convert-classic-resource.md).

## <a name="sign-in-to-microsoft-azure"></a>Anmelden bei Microsoft Azure

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und erstellen Sie eine Application Insights-Ressource:

![Klicken Sie links oben auf das Pluszeichen (+). Auswählen von „Entwicklertools“ und „Application Insights“](./media/create-new-resource/new-app-insights.png)

   | Einstellungen        |  Wert           | Beschreibung  |
   | ------------- |:-------------|:-----|
   | **Name**      | `Unique value` | Der Name, der die zu überwachende App identifiziert |
   | **Ressourcengruppe**     | `myResourceGroup`      | Der Name der neuen oder vorhandenen Ressourcengruppe, die Application Insights-Daten hosten soll |
   | **Region** | `East US` | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe des Standorts aus, an dem Ihre App gehostet wird. |
   | **Ressourcenmodus** | `Classic` oder `Workspace-based` | Arbeitsbereichsbasierte Ressourcen ermöglichen Ihnen das Senden Ihrer Application Insights-Telemetrie an einen gemeinsamen Log Analytics-Arbeitsbereich. Weitere Informationen finden Sie im [Artikel zu arbeitsbereichsbasierten Ressourcen](create-workspace-resource.md).

> [!NOTE]
> Obwohl Sie denselben Ressourcennamen für verschiedene Ressourcengruppen verwenden können, kann es vorteilhaft sein, einen global eindeutigen Namen zu verwenden. Dies kann hilfreich sein, wenn Sie planen, [ressourcenübergreifende Abfragen auszuführen](../logs/cross-workspace-query.md#identifying-an-application), da dadurch die erforderliche Syntax vereinfacht wird.

Geben Sie die entsprechenden Werte in die erforderlichen Felder ein, und wählen Sie dann **Überprüfen + erstellen** aus.

> [!div class="mx-imgBorder"]
> ![Geben Sie Werte in die erforderlichen Felder ein, und wählen Sie dann „Überprüfen + erstellen“ aus.](./media/create-new-resource/review-create.png)

Wenn Ihre App erstellt wurde, wird ein neuer Bereich geöffnet. In diesem Bereich werden die Leistungs- und Nutzungsdaten Ihrer überwachten Anwendung angezeigt. 

## <a name="copy-the-instrumentation-key"></a>Kopieren des Instrumentationsschlüssels

Der Instrumentierungsschlüssel identifiziert die Ressource, der Sie Ihre Telemetriedaten zuordnen möchten. Sie müssen den Instrumentierungsschlüssel kopieren und Ihrem Anwendungscode hinzufügen.

> [!IMPORTANT]
> Neue Azure-Regionen **erfordern** die Verwendung von Verbindungszeichenfolgen anstelle von Instrumentierungsschlüsseln. Die [Verbindungszeichenfolge](./sdk-connection-string.md?tabs=net) identifiziert die Ressource, der Sie Ihre Telemetriedaten zuordnen möchten. Hier können Sie die Endpunkte ändern, die Ihre Ressource als Ziel für die Telemetrie verwendet. Sie müssen die Verbindungszeichenfolge kopieren und dem Code Ihrer Anwendung oder einer Umgebungsvariable hinzufügen.

## <a name="install-the-sdk-in-your-app"></a>Installieren des SDK in Ihrer App

Installieren Sie das Application Insights-SDK in Ihrer App. Dieser Schritt hängt stark von der Art der Anwendung ab.

Konfigurieren Sie [das SDK, das Sie in Ihrer Anwendung installieren][start] mithilfe des Instrumentierungsschlüssels.

Das SDK enthält die Standardmodule, die Telemetriedaten senden, ohne dass Sie zusätzlichen Code schreiben müssen. Um Benutzeraktionen nachzuverfolgen oder Probleme im Detail zu diagnostizieren, [verwenden Sie die API][api] zum Senden eigener Telemetriedaten.

## <a name="creating-a-resource-automatically"></a>Automatisches Erstellen einer Ressource

### <a name="powershell"></a>PowerShell

Erstellen einer neuen Application Insights-Ressource

```powershell
New-AzApplicationInsights [-ResourceGroupName] <String> [-Name] <String> [-Location] <String> [-Kind <String>]
 [-Tag <Hashtable>] [-DefaultProfile <IAzureContextContainer>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="example"></a>Beispiel

```powershell
New-AzApplicationInsights -Kind java -ResourceGroupName testgroup -Name test1027 -location eastus
```
#### <a name="results"></a>Ergebnisse

```powershell
Id                 : /subscriptions/{subid}/resourceGroups/testgroup/providers/microsoft.insights/components/test1027
ResourceGroupName  : testgroup
Name               : test1027
Kind               : web
Location           : eastus
Type               : microsoft.insights/components
AppId              : 8323fb13-32aa-46af-b467-8355cf4f8f98
ApplicationType    : web
Tags               : {}
CreationDate       : 10/27/2017 4:56:40 PM
FlowType           :
HockeyAppId        :
HockeyAppToken     :
InstrumentationKey : 00000000-aaaa-bbbb-cccc-dddddddddddd
ProvisioningState  : Succeeded
RequestSource      : AzurePowerShell
SamplingPercentage :
TenantId           : {subid}
```

Die vollständige PowerShell-Dokumentation für dieses Cmdlet und Informationen zum Abrufen des Instrumentierungsschlüssels finden Sie in der [Azure PowerShell-Dokumentation](/powershell/module/az.applicationinsights/new-azapplicationinsights).

### <a name="azure-cli-preview"></a>Azure CLI (Vorschauversion)

Für den Zugriff auf die Azure CLI-Befehle (Vorschauversion) von Application Insights müssen Sie zuerst Folgendes ausführen:

```azurecli
 az extension add -n application-insights
```

Wenn Sie den Befehl `az extension add` nicht ausführen, wird eine Fehlermeldung mit dem folgenden Hinweis angezeigt: `az : ERROR: az monitor: 'app-insights' is not in the 'az monitor' command group. See 'az monitor --help'.`

Jetzt können Sie Folgendes ausführen, um Ihre Application Insights-Ressource zu erstellen:

```azurecli
az monitor app-insights component create --app
                                         --location
                                         --resource-group
                                         [--application-type]
                                         [--kind]
                                         [--tags]
```

#### <a name="example"></a>Beispiel

```azurecli
az monitor app-insights component create --app demoApp --location westus2 --kind web -g demoRg --application-type web
```

#### <a name="results"></a>Ergebnisse

```azurecli
az monitor app-insights component create --app demoApp --location eastus --kind web -g demoApp  --application-type web
{
  "appId": "87ba512c-e8c9-48d7-b6eb-118d4aee2697",
  "applicationId": "demoApp",
  "applicationType": "web",
  "creationDate": "2019-08-16T18:15:59.740014+00:00",
  "etag": "\"0300edb9-0000-0100-0000-5d56f2e00000\"",
  "flowType": "Bluefield",
  "hockeyAppId": null,
  "hockeyAppToken": null,
  "id": "/subscriptions/{subid}/resourceGroups/demoApp/providers/microsoft.insights/components/demoApp",
  "instrumentationKey": "00000000-aaaa-bbbb-cccc-dddddddddddd",
  "kind": "web",
  "location": "eastus",
  "name": "demoApp",
  "provisioningState": "Succeeded",
  "requestSource": "rest",
  "resourceGroup": "demoApp",
  "samplingPercentage": null,
  "tags": {},
  "tenantId": {tenantID},
  "type": "microsoft.insights/components"
}
```

Die vollständige Azure CLI-Dokumentation zu diesem Befehl und Informationen zum Abrufen des Instrumentierungsschlüssels finden Sie in der [Azure CLI-Dokumentation](/cli/azure/ext/application-insights/monitor/app-insights/component#ext-application-insights-az-monitor-app-insights-component-create).

## <a name="next-steps"></a>Nächste Schritte
* [Diagnosesuche](./diagnostic-search.md)
* [Untersuchen von Metriken](../essentials/metrics-charts.md)
* [Schreiben von Analytics-Abfragen](../logs/log-query-overview.md)

<!--Link references-->

[api]: ./api-custom-events-metrics.md
[diagnostic]: ./diagnostic-search.md
[metrics]: ../essentials/metrics-charts.md
[start]: ./app-insights-overview.md


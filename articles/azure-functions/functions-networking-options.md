---
title: Netzwerkoptionen von Azure Functions
description: Enthält eine Übersicht über alle Netzwerkoptionen, die in Azure Functions verfügbar sind.
ms.topic: conceptual
ms.date: 4/11/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 271730e57a2d7ef8324420744b4bcd088b9809cc
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90530086"
---
# <a name="azure-functions-networking-options"></a>Netzwerkoptionen von Azure Functions

In diesem Artikel werden die Netzwerkfunktionen beschrieben, die für die Hostingoptionen von Azure Functions zur Verfügung stehen. Alle folgenden Netzwerkoptionen geben Ihnen die Möglichkeit zum Zugreifen auf Ressourcen ohne Verwendung von Adressen, die über das Internet geroutet werden können, oder zum Einschränken des Internetzugriffs auf eine Funktions-App.

Die Hostingmodelle weisen verschiedene verfügbare Ebenen der Netzwerkisolation auf. Durch Auswahl der jeweils richtigen Ebene lassen sich Ihre Anforderungen an die Netzwerkisolation erfüllen.

Sie können Funktions-Apps auf verschiedene Arten hosten:

* Sie können unter Tarifoptionen wählen, die in einer mehrinstanzenfähigen Infrastruktur ausgeführt werden und über unterschiedliche Ebenen von Konnektivitäts- und Skalierungsoptionen für virtuelle Netzwerke verfügen:
    * Im [Verbrauchstarif](functions-scale.md#consumption-plan) wird als Reaktion auf Last dynamisch skaliert, und der Tarif bietet Optionen für eine minimale Netzwerkisolation.
    * Im [Premium-Tarif](functions-scale.md#premium-plan) wird ebenfalls dynamisch skaliert, der Tarif ermöglicht aber eine umfassendere Netzwerkisolation.
    * Beim [Azure App Service-Plan](functions-scale.md#app-service-plan) wird eine feste Skalierung verwendet, und er weist eine ähnliche Netzwerkisolation wie der Premium-Tarif auf.
* Sie können Funktionen in einer [App Service-Umgebung](../app-service/environment/intro.md) ausführen. Mit dieser Methode werden die Funktionen in Ihrem virtuellen Netzwerk bereitgestellt und eine umfassende Netzwerksteuerung und -isolation ermöglicht.

## <a name="matrix-of-networking-features"></a>Matrix der Netzwerkfunktionen

[!INCLUDE [functions-networking-features](../../includes/functions-networking-features.md)]

## <a name="inbound-ip-restrictions"></a>IP-Einschränkungen für eingehenden Datenverkehr

Mit IP-Einschränkungen können Sie eine nach Priorität sortierte Liste mit IP-Adressen definieren, über die der Zugriff auf Ihre App zugelassen bzw. abgelehnt wird. Die Liste kann IPv4- und IPv6-Adressen enthalten. Wenn mindestens ein Eintrag vorhanden ist, enthält die Liste am Ende einen impliziten Eintrag vom Typ „Alle ablehnen“. IP-Einschränkungen können für alle Hostingoptionen für Funktionen verwendet werden.

> [!NOTE]
> Mit den geltenden Netzwerkbeschränkungen können Sie den Portal-Editor nur in Ihrem virtuellen Netzwerk verwenden, oder wenn Sie die IP-Adresse des Computers, mit dem Sie auf das Azure-Portal zugreifen, der Liste der sicheren Empfänger hinzugefügt haben. Sie können aber weiterhin von jedem Computer aus auf alle Funktionen der Registerkarte **Plattformfeatures** zugreifen.

Weitere Informationen finden Sie unter [Azure App Service – statische Zugriffseinschränkungen](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>Privater Websitezugriff

[!INCLUDE [functions-private-site-access](../../includes/functions-private-site-access.md)]

## <a name="virtual-network-integration"></a>Integration in ein virtuelles Netzwerk

Durch die Integration des virtuellen Netzwerks kann die Funktions-App auf Ressourcen eines virtuellen Netzwerks zugreifen.
Azure Functions unterstützt zwei Arten der Integration virtueller Netzwerke:

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

Bei der Integration virtueller Netzwerke in Azure Functions wird die gemeinsame Infrastruktur mit App Service-Web-Apps verwendet. Weitere Informationen zu den beiden Arten der Integration virtueller Netzwerke finden Sie unter:

* [Regionale Integration des virtuellen Netzwerks](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Integration des virtuellen Netzwerks mit erforderlichem Gateway](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Informationen zum Einrichten der Integration virtueller Netzwerke finden Sie unter [Tutorial: Integrieren von Functions in ein virtuelles Azure-Netzwerk](functions-create-vnet.md).

## <a name="regional-virtual-network-integration"></a>Regionale Integration des virtuellen Netzwerks

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

## <a name="connect-to-service-endpoint-secured-resources"></a>Herstellen einer Verbindung mit durch den Dienstendpunkt gesicherten Ressourcen

Um eine höhere Sicherheitsstufe zu gewährleisten, können Sie eine Reihe von Azure-Diensten auf ein virtuelles Netzwerk beschränken, indem Sie Dienstendpunkte verwenden. Sie müssen Ihre Funktions-App dann in dieses virtuelle Netzwerk integrieren, um auf die Ressource zugreifen zu können. Diese Konfiguration wird für alle Pläne unterstützt, die die Integration in ein virtuelles Netzwerk unterstützen.

Weitere Informationen finden Sie unter [VNET-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="restrict-your-storage-account-to-a-virtual-network"></a>Einschränken Ihres Speicherkontos auf ein virtuelles Netzwerk

Beim Erstellen einer Funktions-App müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob-, Queue- und Table Storage unterstützt. Für dieses Konto können derzeit keine Einschränkungen für virtuelle Netzwerke verwendet werden. Wenn Sie einen Dienstendpunkt des virtuellen Netzwerks für das Speicherkonto konfigurieren, das Sie für ihre Funktions-App verwenden, funktioniert die App nicht mehr.

Weitere Informationen finden Sie unter [Speicherkontoanforderungen](./functions-create-function-app-portal.md#storage-account-requirements).

## <a name="use-key-vault-references"></a>Verwenden von Key Vault-Verweisen

Sie können mithilfe von Azure Key Vault-Verweisen Geheimnisse aus Ihrer Azure Key Vault-Instanz in Ihrer Azure Functions-Anwendung verwenden, ohne dass Codeänderungen erforderlich sind. Azure Key Vault ist ein Dienst, der eine zentralisierte Verwaltung von Geheimnissen mit voller Kontrolle über Zugriffsrichtlinien und Überprüfungsverlauf ermöglicht.

Derzeit funktionieren [Key Vault-Verweise](../app-service/app-service-key-vault-references.md) nicht, wenn Ihre Key Vault-Instanz mit Dienstendpunkten gesichert ist. Zum Herstellen einer Verbindung mit einer Key Vault-Instanz mithilfe der Integration von virtuellen Netzwerken müssen Sie Key Vault im Anwendungscode aufrufen.

## <a name="virtual-network-triggers-non-http"></a>Trigger für virtuelle Netzwerke (nicht HTTP)

HTTP-fremde Triggerfunktionen können aktuell auf zwei Arten von einem virtuellen Netzwerk aus verwendet werden:

+ Sie können Ihre Funktions-App in einem Premium-Plan ausführen und die Triggerunterstützung für virtuelle Netzwerke aktivieren.
+ Sie können Ihre Funktions-App in einem App Service-Plan oder in einer App Service-Umgebung ausführen.

### <a name="premium-plan-with-virtual-network-triggers"></a>Premium-Plan mit Triggern für virtuelle Netzwerke

Bei Verwendung eines Premium-Plans können Sie HTTP-fremde Triggerfunktionen mit Diensten verbinden, die innerhalb eines virtuellen Netzwerks ausgeführt werden. Hierzu müssen Sie für Ihre Funktions-App die Triggerunterstützung für virtuelle Netzwerke aktivieren. Die Einstellung **Runtime Scale Monitoring** (Überwachung der Runtimeskalierung) finden Sie im [Azure-Portal](https://portal.azure.com) unter **Konfiguration** > **Einstellungen der Funktionsruntime**.

:::image type="content" source="media/functions-networking-options/virtual-network-trigger-toggle.png" alt-text="VNETToggle":::

Trigger für virtuelle Netzwerke können auch mithilfe des folgenden Azure CLI-Befehls aktiviert werden:

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

Trigger für virtuelle Netzwerke werden ab Version 2.x der Functions-Runtime unterstützt. Folgende HTTP-fremde Triggertypen werden unterstützt:

| Durchwahl | Mindestversion |
|-----------|---------| 
|[Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) | 3.0.10 oder höher |
|[Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs)| 4.1.0 oder höher|
|[Microsoft.Azure.WebJobs.Extensions.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus)| 3.2.0 oder höher|
|[Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB)| 3.0.5 oder höher|
|[Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)| 2.0.0 oder höher|

> [!IMPORTANT]
> Wenn Sie die Triggerunterstützung für virtuelle Netzwerke aktivieren, werden nur die in der obigen Tabelle aufgeführten Triggertypen dynamisch mit Ihrer Anwendung skaliert. Sie können zwar weiterhin Trigger verwenden, die nicht in der Tabelle aufgeführt sind, diese werden jedoch nicht über die Anzahl vorab aufgewärmter Instanzen hinaus skaliert. Die vollständige Triggerliste finden Sie unter [Trigger und Bindungen](./functions-triggers-bindings.md#supported-bindings).

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>App Service-Plan und App Service-Umgebung mit Triggern für virtuelle Netzwerke

Wenn Ihre Funktions-App entweder in einem App Service-Plan oder in einer App Service-Umgebung ausgeführt wird, können Sie HTTP-fremde Triggerfunktionen verwenden. Damit Ihre Funktionen ordnungsgemäß ausgelöst werden, muss eine Verbindung mit einem virtuellen Netzwerk bestehen. Außerdem muss auf die in der Triggerverbindung definierte Ressource zugegriffen werden können.

Angenommen, Sie möchten Azure Cosmos DB so konfigurieren, dass nur Datenverkehr aus einem virtuellen Netzwerk akzeptiert wird. In diesem Fall müssen Sie Ihre Funktions-App in einem App Service-Plan bereitstellen, der die VNET-Integration mit diesem virtuellen Netzwerk ermöglicht. Die Integration ermöglicht das Auslösen einer Funktion durch diese Azure Cosmos DB-Ressource.

## <a name="hybrid-connections"></a>Hybridverbindungen

[Hybrid Connections](../azure-relay/relay-hybrid-connections-protocol.md) ist ein Feature von Azure Relay, das Sie zum Zugreifen auf Anwendungsressourcen in anderen Netzwerken verwenden können. Es ermöglicht den Zugriff von Ihrer App auf einen Anwendungsendpunkt. Sie können es nicht verwenden, um auf Ihre Anwendung zuzugreifen. Hybrid Connections steht für Funktionen unter Windows in allen Tarifen (mit Ausnahme des Verbrauchstarifs) zur Verfügung.

Bei der Verwendung in Azure Functions entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port. Dies bedeutet, dass sich der Hybridverbindungsendpunkt in einem beliebigen Betriebssystem und einer beliebigen Anwendung befinden kann, solange der Zugriff über einen TCP-Überwachungsport erfolgt. Das Feature „Hybrid Connections“ verfügt nicht über Informationen zum Anwendungsprotokoll oder zum abzurufenden Inhalt und benötigt diese Informationen auch nicht. Es ermöglicht lediglich den Netzwerkzugriff.

Weitere Informationen finden Sie in der [App Service-Dokumentation zu Hybrid Connections](../app-service/app-service-hybrid-connections.md). Diese Konfigurationsschritte unterstützen auch Azure Functions.

>[!IMPORTANT]
> Hybrid Connections wird nur in Windows-Tarifen unterstützt. Linux wird nicht unterstützt.

## <a name="outbound-ip-restrictions"></a>IP-Einschränkungen für ausgehenden Datenverkehr

IP-Einschränkungen für ausgehenden Datenverkehr sind in einem Premium-Plan, einem App Service-Plan oder in einer App Service-Umgebung verfügbar. Sie können ausgehende Einschränkungen für das virtuelle Netzwerk konfigurieren, wo Ihre App Service-Umgebung bereitgestellt wird.

Wenn Sie eine Funktions-App in einen Premium-Tarif oder einen App Service-Plan mit einem virtuellen Netzwerk integrieren, kann die App standardmäßig weiterhin ausgehende Aufrufe ins Internet vornehmen. Durch Hinzufügen der Anwendungseinstellung `WEBSITE_VNET_ROUTE_ALL=1` erzwingen Sie, dass der gesamte ausgehende Datenverkehr an das virtuelle Netzwerk gesendet wird. Dort können Netzwerksicherheitsgruppen-Regeln verwendet werden, um den Datenverkehr einzuschränken.

## <a name="automation"></a>Automation
Die folgenden APIs ermöglichen es Ihnen, regionale Integrationen virtueller Netzwerke programmgesteuert zu verwalten:

+ **Azure CLI**: Verwenden Sie die Befehle [`az functionapp vnet-integration`](/cli/azure/functionapp/vnet-integration), um Integrationen regionaler virtueller Netzwerk hinzuzufügen, aufzulisten oder zu entfernen.  
+ **ARM-Vorlagen**: Die Integration regionaler virtueller Netzwerke kann mithilfe einer Azure Resource Manager-Vorlage aktiviert werden. Ein vollständiges Beispiel finden Sie in [dieser Functions-Schnellstartvorlage](https://azure.microsoft.com/resources/templates/101-function-premium-vnet-integration/).

## <a name="troubleshooting"></a>Problembehandlung

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Netzwerk und zu Azure Functions:

* [Tutorial zum Einstieg in die Integration des virtuellen Netzwerks](./functions-create-vnet.md)
* [Häufig gestellte Fragen zu Netzwerken in Functions](./functions-networking-faq.md)
* [Weitere Informationen zur Integration des virtuellen Netzwerks mit App Service und Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Weitere Informationen zu virtuellen Netzwerken in Azure](../virtual-network/virtual-networks-overview.md)
* [Aktivieren weiterer Netzwerkfunktionen und Steuerung mit App Service-Umgebungen](../app-service/environment/intro.md)
* [Verbinden mit einzelnen lokalen Ressourcen ohne Änderungen an der Firewall mithilfe von Hybrid Connections](../app-service/app-service-hybrid-connections.md)

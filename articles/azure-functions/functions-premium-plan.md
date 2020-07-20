---
title: Premium-Tarif für Azure Functions
description: Details und Konfigurationsoptionen (VNet, kein Kaltstart, unbegrenzte Ausführungsdauer) für den Premium-Plan (Premium-Tarif) für Azure Functions.
author: jeffhollan
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: jehollan
ms.openlocfilehash: d59335c5c4ebd2688097539594f11ea349939eff
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85298513"
---
# <a name="azure-functions-premium-plan"></a>Premium-Tarif für Azure Functions

Der Premium-Plan für Azure Functions (manchmal auch als elastischer Premium-Plan bezeichnet) ist eine Hostingoption für Funktions-Apps. Der Premium-Plan bietet Features wie VNet-Konnektivität, keinen Kaltstart und Premium-Hardware.  Mehrere Funktions-Apps können im selben Premium-Plan bereitgestellt werden, und der Tarif ermöglicht es Ihnen, die Compute-Instanzgröße, Basisplangröße und maximale Plangröße zu konfigurieren.  Einen Vergleich des Premium-Plans und anderer Pläne und Hostingtypen finden Sie unter [Skalierung und Hosting von Azure Functions](functions-scale.md).

## <a name="create-a-premium-plan"></a>Erstellen eines Premium-Plans

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

Sie können auch einen Premium-Plan erstellen. Verwenden Sie dazu [az functionapp plan create](/cli/azure/functionapp/plan#az-functionapp-plan-create) in der Azure-Befehlszeilenschnittstelle. Im folgenden Beispiel wird ein Plan vom Typ _Elastic Premium 1_ erstellt:

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

Ersetzen Sie in diesem Beispiel `<RESOURCE_GROUP>` durch Ihre Ressourcengruppe und `<PLAN_NAME>` durch einen Namen für Ihren Plan, der in der Ressourcengruppe eindeutig ist. Geben Sie eine [unterstützte `<REGION>`](https://azure.microsoft.com/global-infrastructure/services/?products=functions) an. Wenn Sie einen Premium-Plan mit Linux-Unterstützung erstellen möchten, schließen Sie die Option `--is-linux` ein.

Nach Erstellung des Plans können Sie mithilfe von [az functionapp create](/cli/azure/functionapp#az-functionapp-create) Ihre Funktions-App erstellen. Im Portal werden Plan und App gleichzeitig erstellt. Ein Beispiel für ein vollständiges Azure CLI-Skript finden Sie unter [Erstellen einer Funktions-App in einem Premium-Tarif](scripts/functions-cli-create-premium-plan.md).

## <a name="features"></a>Features

Die folgenden Features sind für Funktions-Apps verfügbar, die für einen Premium-Plan bereitgestellt werden.

### <a name="pre-warmed-instances"></a>Vorab aufgewärmte Instanzen

Wenn für einen bestimmten Tag keine Ereignisse und Ausführungen im Verbrauchsplan aufgeführt werden, wird Ihre App möglicherweise auf null Instanzen abskaliert. Wenn neue Ereignisse eingehen, muss eine neue Instanz zum Ausführen Ihrer App eingerichtet werden.  Das Einrichten einer neuen Instanz kann je nach App einige Zeit dauern.  Diese zusätzliche Wartezeit beim ersten Aufruf wird häufig als App-Kaltstart bezeichnet.

Im Premium-Plan können Sie Ihre App in einem „vorab aufgewärmten“ Zustand für eine angegebene Anzahl von Instanzen vorhalten, bis zur Größe Ihres Mindestplans.  Mit vorab aufgewärmten Instanzen können Sie eine App auch vor hoher Last vorab skalieren. Erfolgt eine Erweiterung für die App, wird die App zunächst in die vorab aufgewärmten Instanzen skaliert. In Vorbereitung auf den nächsten Skalierungsvorgang werden sofort weitere Instanzen gepuffert und aufgewärmt. Dadurch, dass es einen Puffer mit vorab aufgewärmten Instanzen gibt, können Sie Kaltstartwartezeiten effektiv vermeiden.  Vorab aufgewärmte Instanzen ist ein Feature für den Premium-Plan, und Sie müssen während der gesamten Zeit, in der der Plan aktiv ist, mindestens eine Instanz aktiv und verfügbar halten.

Sie können die Anzahl der vorab aufgewärmten Instanzen im Azure-Portal konfigurieren, indem Sie Ihre **Funktions-App** auswählen, zur Registerkarte **Plattformfeatures** wechseln und die **Aufskalieren**-Optionen auswählen. Im Bearbeitungsfenster für eine Funktions-App gelten vorab aufgewärmte Instanzen speziell für diese App, aber die Mindestanzahl und die maximale Anzahl von Instanzen gelten für Ihren gesamten Plan.

![Einstellungen für elastisches Skalieren](./media/functions-premium-plan/scale-out.png)

Sie können vorab aufgewärmte Instanzen für eine App auch mit der Azure CLI konfigurieren.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Private Netzwerkkonnektivität

Für Azure Functions, bereitgestellt in einem Premium-Plan, wird die [neue VNET-Integration für Web-Apps](../app-service/web-sites-integrate-with-vnet.md) genutzt.  Ist diese Integration konfiguriert, kann Ihre App mit Ressourcen in Ihrem VNET oder geschützt über Dienstendpunkte kommunizieren.  IP-Einschränkungen sind ebenfalls für die App verfügbar, um eingehenden Datenverkehr zu beschränken.

Wenn Sie Ihrer Funktions-App in einem Premium-Plan ein Subnetz zuweisen, benötigen Sie ein Subnetz mit genügend IP-Adressen für jede mögliche-Instanz. Wir benötigen einen IP-Block mit mindestens 100 verfügbaren Adressen.

Weitere Informationen finden Sie unter [Integrieren einer Funktions-App in ein Azure Virtual Network](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Schnelle elastische Skalierung

Weitere Compute-Instanzen werden automatisch für Ihre App hinzugefügt. Dazu wird die gleiche Logik für schnelle Skalierung verwendet wie für den Verbrauchsplan. Apps im gleichen App Service-Plan werden unabhängig voneinander auf der Grundlage der Anforderungen der jeweiligen App skaliert. Von Funktions-Apps im gleichen App Service-Plan werden VM-Ressourcen jedoch nach Möglichkeit gemeinsam genutzt, um Kosten zu sparen. Die Anzahl von Apps, die einem virtuellen Computer zugeordnet sind, hängt vom Speicherbedarf der einzelnen Apps sowie von der Größe des virtuellen Computers ab.

Weitere Informationen zur Funktionsweise von Skalierung finden Sie unter [Skalierung und Hosting von Azure Functions](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="longer-run-duration"></a>Längere Ausführungsdauer

Azure Functions in einem Verbrauchsplan sind auf 10 Minuten für eine einzelne Ausführung beschränkt.  Im Premium-Plan wird die Ausführungsdauer standardmäßig auf 30 Minuten festgelegt, um Endlosausführungen zu verhindern. Sie können jedoch [die Konfiguration „host.json“ ändern](./functions-host-json.md#functiontimeout), um die Ausführungsdauer für Apps im Premium-Tarif auf unbegrenzt (garantierte 60 Minuten) festzulegen.

## <a name="plan-and-sku-settings"></a>Plan- und SKU-Einstellungen

Wenn Sie den Plan erstellen, konfigurieren Sie zwei Einstellungen: die Mindestanzahl von Instanzen (oder Plangröße) und den maximalen Burstgrenzwert.  So viele Instanzen, wie die Mindestanzahl von Instanzen angibt, werden reserviert und immer ausgeführt.

> [!IMPORTANT]
> Ihnen wird jede Instanz, die entsprechend der Mindestanzahl von Instanzen zugeordnet ist, in Rechnung gestellt, unabhängig davon, ob Funktionen ausgeführt werden oder nicht.

Wenn Ihre App mehr Instanzen erfordert, als Ihre Plangröße vorgibt, kann diese aufskaliert werden, bis die Anzahl von Instanzen den maximalen Burstgrenzwert erreicht hat.  Instanzen, die sich außerhalb Ihrer Plangröße befinden, werden Ihnen nur in Rechnung gestellt, während sie ausgeführt werden und für Sie bereitgestellt sind.  Es wird versucht, ein Erweitern Ihrer App bis zu deren definiertem maximalen Grenzwert bestmöglich vorzunehmen, während die im Plan festgelegte Mindestanzahl von Instanzen für Ihre App garantiert ist.

Sie können die Plangröße und die Maximalwerte im Azure-Portal konfigurieren, indem Sie die **Aufskalieren**-Optionen im Plan oder eine Funktions-App auswählen, die für diesen Plan bereitgestellt ist (unter **Plattformfeatures**).

Sie können auch den maximalen Burstgrenzwert über die Azure-Befehlszeilenschnittstelle erhöhen:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>Verfügbare Instanz-SKUs

Wenn Sie Ihren Plan erstellen oder skalieren, können Sie zwischen drei Instanzgrößen wählen.  Ihnen werden die Gesamtanzahl von Kernen und der Arbeitsspeicher in Rechnung gestellt, die pro Sekunde genutzt werden.  Ihre App kann automatisch nach Bedarf auf mehrere Instanzen aufskaliert werden.  

|SKU|Kerne|Arbeitsspeicher|Storage|
|--|--|--|--|
|EP1|1|3,5 GB|250 GB|
|EP2|2|7 GB|250 GB|
|EP3|4|14 GB|250 GB|

### <a name="memory-utilization-considerations"></a>Überlegungen zur Arbeitsspeichernutzung
Das Ausführen auf einem Computer mit mehr Arbeitsspeicher bedeutet nicht immer, dass Ihre Funktions-App den gesamten verfügbaren Arbeitsspeicher auch verwendet.

Beispielsweise wird eine JavaScript-Funktions-App durch das standardmäßige Arbeitsspeicherlimit in „Node.js“ eingeschränkt. Um dieses feste Arbeitsspeicherlimit zu erhöhen, fügen Sie die App-Einstellung `languageWorkers:node:arguments` mit dem Wert `--max-old-space-size=<max memory in MB>` hinzu.

## <a name="region-max-scale-out"></a>Maximale horizontale Hochskalierung der Region

Im Folgenden werden die derzeit unterstützten maximalen Werte für horizontales Hochskalieren für einen einzelnen Tarif in jeder Region und Betriebssystemkonfiguration aufgeführt. Wenn Sie eine Erhöhung anfordern möchten, öffnen Sie ein Supportticket.

Sehen Sie sich die gesamte regionale Verfügbarkeit von Functions an: [Azure.com](https://azure.microsoft.com/global-infrastructure/services/?products=functions)

|Region| Windows | Linux |
|--| -- | -- |
|Australien, Mitte| 20 | Nicht verfügbar. |
|Australien, Mitte 2| 20 | Nicht verfügbar. |
|Australien (Osten)| 100 | 20 |
|Australien, Südosten | 100 | 20 |
|Brasilien Süd| 60 | 20 |
|Kanada, Mitte| 100 | 20 |
|USA (Mitte)| 100 | 20 |
|Asien, Osten| 100 | 20 |
|East US | 100 | 20 |
|USA (Ost) 2| 100 | 20 |
|Frankreich, Mitte| 100 | 20 |
|Deutschland, Westen-Mitte| 100 | Nicht verfügbar. |
|Japan, Osten| 100 | 20 |
|Japan, Westen| 100 | 20 |
|Korea, Mitte| 100 | 20 |
|USA Nord Mitte| 100 | 20 |
|Nordeuropa| 100 | 20 |
|Norwegen, Osten| 20 | 20 |
|USA Süd Mitte| 100 | 20 |
|Indien (Süden) | 100 | Nicht verfügbar. |
|Asien, Südosten| 100 | 20 |
|UK, Süden| 100 | 20 |
|UK, Westen| 100 | 20 |
|Europa, Westen| 100 | 20 |
|Indien, Westen| 100 | 20 |
|USA, Westen-Mitte| 20 | 20 |
|USA (Westen)| 100 | 20 |
|USA, Westen 2| 100 | 20 |

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Grundlegendes zu Skalierung und Hosting von Azure Functions](functions-scale.md)

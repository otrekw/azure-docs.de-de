---
title: Dediziertes Hosten von Azure Functions
description: Lernen Sie die Vorteile kennen, von denen Sie profitieren, wenn Sie Azure Functions in einem dedizierten App Service-Hostingplan ausführen.
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: 0ebf83aa919d91f161b247539ae20873242a8ed8
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97937483"
---
# <a name="dedicated-hosting-plans-for-azure-functions"></a>Dedizierte Hostingpläne für Azure Functions

Dieser Artikel beschreibt das Hosten einer Funktions-App in einem App Service-Plan bzw. einer App Service-Umgebung (App Service Environment, ASE). Andere Hostingoptionen finden Sie im [Artikel zu Hostingplänen](functions-scale.md).

Mit einem App Service-Plan wird ein Satz mit Computeressourcen für eine auszuführende App definiert. Diese Computeressourcen entsprechen der [_Serverfarm_](https://wikipedia.org/wiki/Server_farm) beim herkömmlichen Hosting. Funktions-Apps können so konfiguriert werden, dass sie mit denselben Computeressourcen (App Service-Plan) ausgeführt werden wie andere App Service-Apps wie z. B. Web-Apps. Zu diesen Plänen gehören folgende SKUs: „Basic“, „Standard“, „Premium“ und „Isoliert“. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/overview-hosting-plans.md).

Ziehen Sie einen App Service-Plan in folgenden Situationen in Betracht:

* Sie verfügen über nicht ausgelastete virtuelle Computer, auf denen bereits andere App Service-Instanzen ausgeführt werden.
* Sie möchten ein benutzerdefiniertes Image bereitstellen, auf dem Ihre Funktionen ausgeführt werden sollen.

## <a name="billing"></a>Abrechnung

Sie zahlen für Funktions-Apps in einem App Service-Plan wie auch für andere App Service-Ressourcen. Dies ist ein Unterschied zum Hosten in einem [Verbrauchsplan](consumption-plan.md) oder [Premium-Plan](functions-premium-plan.md) für Azure Functions, verbrauchsbasierte Komponenten bieten. Ihnen wird nur der Plan in Rechnung gestellt, unabhängig davon, wie viele Funktions-App oder Web-Apps im Plan ausgeführt werden. Weitere Informationen finden Sie auf der [Preisseite für App Service](https://azure.microsoft.com/pricing/details/app-service/windows/). 

## <a name="always-on"></a><a name="always-on"></a> Always On

Wenn Sie einen App Service-Plan verwenden, müssen Sie die Einstellung **Always On** aktivieren, damit Ihre Funktions-App ordnungsgemäß ausgeführt wird. Bei einem App Service-Plan geht die Functions-Runtime nach wenigen Minuten der Inaktivität in den Leerlauf über, sodass nur HTTP-Trigger Ihre Funktionen tatsächlich „reaktivieren“ können. Die Einstellung **Always On** ist nur in einem App Service-Plan verfügbar. Bei einem Verbrauchsplan aktiviert die Plattform Funktions-Apps automatisch.

Auch bei aktiviertem Always On wird das Ausführungstimeout für einzelne Funktionen durch die Einstellung `functionTimeout` in der [host.json](functions-host-json.md#functiontimeout)-Projektdatei gesteuert.

## <a name="scaling"></a>Skalierung

Mit einem App Service-Plan können Sie manuell aufskalieren, indem Sie weitere VM-Instanzen hinzufügen. Sie können auch die Autoskalierung aktivieren, allerdings ist diese langsamer als die elastische Skalierung des Premium-Plans. Weitere Informationen finden Sie unter [Manuelles oder automatisches Skalieren der Instanzenzahl](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Sie können auch hochskalieren, indem Sie einen anderen App Service-Plan auswählen. Weitere Informationen finden Sie unter [Hochskalieren einer App in Azure](../app-service/manage-scale-up.md). 

> [!NOTE] 
> Wenn Sie JavaScript-Funktionen (Node.js) in einem App Service-Plan ausführen möchten, sollten Sie einen Plan mit weniger vCPUs wählen. Weitere Informationen finden Sie unter [Auswählen von Einzelkern-App Service-Plänen](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

## <a name="app-service-environments"></a>App Service-Umgebungen

Durch die Ausführung in einer [App Service-Umgebung](../app-service/environment/intro.md) können Sie Ihre Funktionen vollständig isolieren und von einer höheren Anzahl von Instanzen als in einem App Service-Plan profitieren. Informationen zu ersten Schritten finden Sie unter .

Wenn Sie nur Ihre Funktions-App in einem virtuellen Netzwerk ausführen möchten, können Sie den [Premium-Plan](functions-premium-plan.md) nutzen. Weitere Informationen finden Sie unter [Einrichten von privatem Websitezugriff für Azure Functions](functions-create-private-site-access.md). 

## <a name="next-steps"></a>Nächste Schritte

+ [Azure Functions-Hostingoptionen](functions-scale.md)
+ [Azure App Service-Plan – Übersicht](../app-service/overview-hosting-plans.md)
---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: da43e639779dd4946cb014be777e3a6db0366e24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671507"
---
* Mehrinstanzenfähige Systeme, die den gesamten Bereich der Tarife unterstützen, mit Ausnahme von Isoliert
* Die App Service-Umgebung (App Service Environment, ASE), die in Ihrem VNET bereitgestellt wird und Apps im Tarif „App Service (isoliert)“ unterstützt

In diesem Dokument wird das VNET-Integrationsfeature verwendet, das für die Verwendung in mehrinstanzenfähigen Apps vorgesehen ist. In der [App Service-Umgebung][ASEintro] befindet sich Ihre App bereits in einem VNET und benötigt das VNET-Integrationsfeature nicht, um Ressourcen in demselben VNET zu erreichen. Ausführliche Informationen zu allen Netzwerkfunktionen finden Sie unter [App Service-Netzwerkfunktionen][Networkingfeatures].

Die VNET-Integration ermöglicht Ihrer App den Zugriff auf Ressourcen in Ihrem virtuellen Netzwerk, gewährt aber keinen eingehenden privaten Zugriff auf Ihre App aus dem VNET. Privater Websitezugriff bezieht sich darauf, den Zugriff auf die App nur über ein privates Netzwerk zuzulassen, z. B. innerhalb eines virtuellen Azure-Netzwerks. Die VNET-Integration dient nur zum Ausführen ausgehender Aufrufe von Ihrer App in Ihr VNET. Die VNET-Integrationsfunktion verhält sich unterschiedlich, wenn Sie mit VNETs in derselben Region und mit VNETs in anderen Regionen verwendet wird. Die VNET-Integrationsfunktion weist zwei Variationen auf.

* Regionale VNET-Integration: Beim Herstellen einer Verbindung mit Resource Manager-VNETs in derselben Region müssen Sie über ein dediziertes Subnetz im VNET verfügen, das Sie integrieren. 
* VNET-Integration mit erforderlichem Gateway: Wenn eine Verbindung mit VNETs in anderen Regionen oder mit einem klassischen VNET in derselben Region hergestellt wird, benötigen Sie ein Virtual Network Gateway, das im Ziel-VNET bereitgestellt wird.

Für die Funktionen für die VNET-Integration gilt Folgendes:

* Sie erfordern einen Tarifplan des Typs Standard, Premium, PremiumV2 oder Elastisch Premium. 
* Sie unterstützen TCP und UDP.
* Sie funktionieren mit App Service-Apps und Funktions-Apps.

Einige Dinge werden von der VNET-Integration nicht unterstützt, z.B.:

* Bereitstellung eines Laufwerks
* AD-Integration 
* NetBios

Die VNET-Integration mit erforderlichem Gateway ermöglicht nur den Zugriff auf Ressourcen im Ziel-VNET oder in Netzwerken, die mit dem Ziel-VNET über Peering oder VPNs verbunden sind. Die VNET-Integration mit erforderlichem Gateway ermöglicht keinen Zugriff auf Ressourcen, die über ExpressRoute-Verbindungen verfügbar sind oder mit Dienstendpunkten funktionieren. 

Unabhängig von der verwendeten Version erhält Ihre App durch die VNET-Integration Zugriff auf Ressourcen in Ihrem virtuellen Netzwerk, es wird aber kein eingehender privater Zugriff auf Ihre App aus dem virtuellen Netzwerk gewährt. Privater Websitezugriff bezieht sich darauf, den Zugriff auf Ihre App nur über ein privates Netzwerk zuzulassen, z.B. aus einem virtuellen Azure-Netzwerk heraus. Die VNET-Integration dient nur zum Ausführen ausgehender Aufrufe von Ihrer App in Ihr VNET. 

<!--Links-->
[ASEintro]: https://docs.microsoft.com/azure/app-service/environment/intro
[Networkingfeatures]: https://docs.microsoft.com/azure/app-service/networking-features
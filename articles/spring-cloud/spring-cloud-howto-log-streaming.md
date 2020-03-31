---
title: Streamen von Azure Spring Cloud-App-Protokollen in Echtzeit
description: Verwenden des Protokollstreamings zum sofortigen Anzeigen von Anwendungsprotokollen
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.openlocfilehash: fc208a3542528fb4554a365a02e13c2da3055cf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78192199"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Streamen von Azure Spring Cloud-App-Protokollen in Echtzeit
Azure Spring Cloud ermöglicht das Protokollstreaming in der Azure-Befehlszeilenschnittstelle, um Anwendungskonsolenprotokolle in Echtzeit für die Problembehandlung zu erhalten. Sie können auch [Protokolle und Metriken mit Diagnoseeinstellungen analysieren](./diagnostic-services.md).

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie die [Azure CLI-Erweiterung](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#install-the-azure-cli-extension) für Spring Cloud (mindestens Version 0.2.0).
* Eine Instanz von **Azure Spring Cloud** mit einer laufenden Anwendung, z. B. der [Spring Cloud-App](./spring-cloud-quickstart-launch-app-cli.md).

> [!NOTE]
>  Die ASC CLI-Erweiterung wird von Version 0.2.0 auf 0.2.1 aktualisiert. Diese Änderung wirkt sich auf die Syntax des Befehls für das Protokollstreaming aus: `az spring-cloud app log tail` wird ersetzt durch: `az spring-cloud app logs`. Der Befehl `az spring-cloud app log tail` wird in einer zukünftigen Version als veraltet markiert. Wenn Sie Version 0.2.0 verwendet haben, können Sie ein Upgrade auf 0.2.1 durchführen. Entfernen Sie zunächst mithilfe des Befehls `az extension remove -n spring-cloud` die alte Version.  Installieren Sie dann Version 0.2.1 mit dem folgenden Befehl: `az extension add -n spring-cloud`.

## <a name="use-cli-to-tail-logs"></a>Verwenden der Befehlszeilenschnittstelle zum Verfolgen von Protokollen

Legen Sie den Namen der Standardressourcengruppe und den Clusternamen fest, um die wiederholte Angabe von Ressourcengruppen- und Dienstinstanznamen zu vermeiden.
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```
In den folgenden Beispielen werden die Ressourcengruppen- und Dienstnamen in den Befehlen ausgelassen.

### <a name="tail-log-for-app-with-single-instance"></a>Protokollfragment für eine App mit einer einzelnen Instanz
Wenn eine App mit dem Namen „auth-service“ nur über eine Instanz verfügt, können Sie das Protokoll der App-Instanz mit dem folgenden Befehl anzeigen:
```
az spring-cloud app logs -n auth-service
```
Damit werden die Protokolle zurückgegeben:
```
...
2020-01-15 01:54:40.481  INFO [auth-service,,,] 1 --- [main] o.apache.catalina.core.StandardService  : Starting service [Tomcat]
2020-01-15 01:54:40.482  INFO [auth-service,,,] 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.22]
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/uaa]  : Initializing Spring embedded WebApplicationContext
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.s.web.context.ContextLoader  : Root WebApplicationContext: initialization completed in 7203 ms

...
```

### <a name="tail-log-for-app-with-multiple-instances"></a>Protokollfragment für eine App mit mehreren Instanzen
Wenn mehrere Instanzen für eine App mit dem Namen `auth-service` vorhanden sind, können Sie das Instanzprotokoll mithilfe der Option `-i/--instance` anzeigen. 

Sie können zunächst die Namen der App-Instanzen mit folgendem Befehl abrufen.

```
az spring-cloud app show -n auth-service --query properties.activeDeployment.properties.instances -o table
```
Ergebnisse:

```
Name                                         Status    DiscoveryStatus
-------------------------------------------  --------  -----------------
auth-service-default-12-75cc4577fc-pw7hb  Running   UP
auth-service-default-12-75cc4577fc-8nt4m  Running   UP
auth-service-default-12-75cc4577fc-n25mh  Running   UP
``` 
Anschließend können Sie Protokolle einer App-Instanz mit der Option `-i/--instance` streamen:

```
az spring-cloud app logs -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

Sie können auch Details zu App-Instanzen im Azure-Portal anzeigen.  Nachdem Sie im linken Navigationsbereich Ihres Azure Spring Cloud-Diensts **Apps** ausgewählt haben, wählen Sie **App-Instanzen** aus.

### <a name="continuously-stream-new-logs"></a>Fortlaufendes Streamen neuer Protokolle
Standardmäßig gibt `az spring-cloud ap log tail` nur vorhandene Protokolle aus, die an die App-Konsole gestreamt werden, und wird dann beendet. Wenn Sie neue Protokolle streamen möchten, fügen Sie „-f“ („--follow“) an:  

```
az spring-cloud app logs -n auth-service -f
``` 
So überprüfen Sie alle unterstützten Protokollierungsoptionen
``` 
az spring-cloud app logs -h 
```

## <a name="next-steps"></a>Nächste Schritte

* [Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen](./diagnostic-services.md)

 






---
title: Streamen von Azure Spring Cloud-App-Protokollen in Echtzeit
description: Verwenden des Protokollstreamings zum sofortigen Anzeigen von Anwendungsprotokollen
author: karlerickson
ms.author: karler
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: cc34e87823e11b2c80d669edb0afe50703e38527
ms.sourcegitcommit: 7f3ed8b29e63dbe7065afa8597347887a3b866b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122350067"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Streamen von Azure Spring Cloud-App-Protokollen in Echtzeit

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

Azure Spring Cloud ermöglicht das Protokollstreaming in der Azure-Befehlszeilenschnittstelle, um Anwendungskonsolenprotokolle in Echtzeit für die Problembehandlung zu erhalten. Sie können auch [Protokolle und Metriken mit Diagnoseeinstellungen analysieren](./diagnostic-services.md).

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie die [Azure CLI-Erweiterung](/cli/azure/install-azure-cli) für Spring Cloud (mindestens Version 0.2.0).
* Eine Instanz von **Azure Spring Cloud** mit einer laufenden Anwendung, z. B. der [Spring Cloud-App](./quickstart.md).

> [!NOTE]
>  Die ASC CLI-Erweiterung wird von Version 0.2.0 auf 0.2.1 aktualisiert. Diese Änderung wirkt sich auf die Syntax des Befehls für das Protokollstreaming aus: `az spring-cloud app log tail` wird ersetzt durch: `az spring-cloud app logs`. Der Befehl `az spring-cloud app log tail` wird in einer zukünftigen Version als veraltet markiert. Wenn Sie Version 0.2.0 verwendet haben, können Sie ein Upgrade auf 0.2.1 durchführen. Entfernen Sie zunächst mithilfe des Befehls `az extension remove -n spring-cloud` die alte Version.  Installieren Sie dann Version 0.2.1 mit dem folgenden Befehl: `az extension add -n spring-cloud`.

## <a name="use-cli-to-tail-logs"></a>Verwenden der Befehlszeilenschnittstelle zum Verfolgen von Protokollen

Legen Sie den Namen der Standardressourcengruppe und den Clusternamen fest, um die wiederholte Angabe von Ressourcengruppen- und Dienstinstanznamen zu vermeiden.

```azurecli
az config set defaults.group=<service group name>
az config set defaults.spring-cloud=<service instance name>
```

In den folgenden Beispielen werden die Ressourcengruppen- und Dienstnamen in den Befehlen ausgelassen.

### <a name="tail-log-for-app-with-single-instance"></a>Protokollfragment für eine App mit einer einzelnen Instanz

Wenn eine App mit dem Namen „auth-service“ nur über eine Instanz verfügt, können Sie das Protokoll der App-Instanz mit dem folgenden Befehl anzeigen:

```azurecli
az spring-cloud app logs -n auth-service
```

Damit werden die Protokolle zurückgegeben:

```output
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

```azurecli
az spring-cloud app show -n auth-service --query properties.activeDeployment.properties.instances -o table
```

Ergebnisse:

```output
Name                                         Status    DiscoveryStatus
-------------------------------------------  --------  -----------------
auth-service-default-12-75cc4577fc-pw7hb  Running   UP
auth-service-default-12-75cc4577fc-8nt4m  Running   UP
auth-service-default-12-75cc4577fc-n25mh  Running   UP
```

Anschließend können Sie Protokolle einer App-Instanz mit der Option `-i/--instance` streamen:

```azurecli
az spring-cloud app logs -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

Sie können auch Details zu App-Instanzen im Azure-Portal anzeigen.  Nachdem Sie im linken Navigationsbereich Ihres Azure Spring Cloud-Diensts **Apps** ausgewählt haben, wählen Sie **App-Instanzen** aus.

### <a name="continuously-stream-new-logs"></a>Fortlaufendes Streamen neuer Protokolle

Standardmäßig gibt `az spring-cloud app logs` nur vorhandene Protokolle aus, die an die App-Konsole gestreamt werden, und wird dann beendet. Wenn Sie neue Protokolle streamen möchten, fügen Sie „-f“ („--follow“) an:

```azurecli
az spring-cloud app logs -n auth-service -f
```

So überprüfen Sie alle unterstützten Protokollierungsoptionen

```azurecli
az spring-cloud app logs -h
```

### <a name="format-json-structured-logs"></a>Formatieren strukturierter JSON-Protokolle

> [!NOTE]
> Dies erfordert die Spring Cloud-Erweiterungsversion 2.4.0 oder höher.

Wenn das [strukturierte Anwendungsprotokoll](./structured-app-log.md) für die App aktiviert ist, werden die Protokolle im JSON-Format ausgegeben. In diesem Format sind die Protokolle jedoch schwer zu lesen. Mit dem Argument `--format-json` können Sie die JSON-Protokolle in ein lesbares Format formatieren.

```shell
# Raw JSON log
$ az spring-cloud app logs -n auth-service
{"timestamp":"2021-05-26T03:35:27.533Z","logger":"com.netflix.discovery.DiscoveryClient","level":"INFO","thread":"main","mdc":{},"message":"Disable delta property : false"}
{"timestamp":"2021-05-26T03:35:27.533Z","logger":"com.netflix.discovery.DiscoveryClient","level":"INFO","thread":"main","mdc":{},"message":"Single vip registry refresh property : null"}

# Formatted JSON log
$ az spring-cloud app logs -n auth-service --format-json
2021-05-26T03:35:27.533Z  INFO [           main] com.netflix.discovery.DiscoveryClient   : Disable delta property : false
2021-05-26T03:35:27.533Z  INFO [           main] com.netflix.discovery.DiscoveryClient   : Single vip registry refresh property : null
```

Mit dem Argument `--format-json` ist es auch möglich, die Protokolle optional mithilfe des Schlüsselwortarguments [format string syntax](https://docs.python.org/3/library/string.html#format-string-syntax) in ein benutzerdefiniertes Format zu konvertieren.

```shell
# Custom format
$ az spring-cloud app logs -n auth-service --format-json="{message}{n}"
Disable delta property : false
Single vip registry refresh property : null
```

> Das verwendete Standardformat lautet:
>
> ```format
> {timestamp} {level:>5} [{thread:>15.15}] {logger{39}:<40.40}: {message}{n}{stackTrace}
> ```

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Überwachen von Azure Spring Cloud-Apps mit Protokollen, Metriken und Ablaufverfolgung](./quickstart-logs-metrics-tracing.md)
* [Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen](./diagnostic-services.md)

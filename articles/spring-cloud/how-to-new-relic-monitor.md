---
title: Überwachen mit dem New Relic-Agent von Java
titleSuffix: Azure Spring Cloud
description: Erfahren Sie, wie Sie Azure Spring Cloud-Apps mithilfe des New Relic-Agents von Java überwachen.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 04/07/2021
ms.custom: devx-track-java
ms.openlocfilehash: 61796641dbba6b02e8298f084f6c36908b053669
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111758076"
---
# <a name="how-to-monitor-with-new-relic-java-agent-preview"></a>Überwachen mit dem New Relic-Agent von Java (Vorschau)

Dieses Feature ermöglicht die Überwachung von Azure Spring Cloud-Apps mit dem **New Relic**-Agent von Java.

Mit dem **New Relic**-Agent von Java können Sie die folgenden Funktionen verwenden:
* Nutzen des **New Relic**-Agents von Java.
* Konfigurieren des **New Relic**-Agents von Java mit **Umgebungsvariablen**.
* Überprüfen aller Überwachungsdaten aus dem **New Relic**-Dashboard.

## <a name="prerequisites"></a>Voraussetzungen

* Ein **New Relic**-Konto.
* [Azure CLI-Version 2.0.67 oder höher](/cli/azure/install-azure-cli).

## <a name="leverage-the-new-relic-java-in-process-agent"></a>Nutzen des prozessinternen New Relic-Java-Agents

Verwenden Sie das folgende Verfahren, um auf den Agent zuzugreifen:

1. Erstellen Sie eine Instanz von Azure Spring Cloud.

2. Erstellen einer Anwendung.

    ```shell
      az spring-cloud app create --name "appName" --is-public true \
      -s "resourceName" -g "resourceGroupName"
    ```

3. Erstellen Sie eine Bereitstellung mit dem **New Relic**-Agent und **Umgebungsvariablen**.

    ```shell
    az spring-cloud app deploy --name "appName" --jar-path app.jar \
       -s "resourceName" -g "resourceGroupName" \
       --jvm-options="-javaagent:/opt/agents/newrelic/java/newrelic-agent.jar" \
       --env NEW_RELIC_APP_NAME=appName NEW_RELIC_LICENSE_KEY=newRelicLicenseKey
    ```

  Azure Spring Cloud installiert den **New Relic**-Java-Agent im folgenden Pfad vor. Kunden können den Agent aus den **JVM-Optionen** von Anwendungen nutzen und den Agent mithilfe der [Umgebungsvariablen des New Relic-Java-Agents](https://docs.newrelic.com/docs/agents/java-agent/configuration/java-agent-configuration-config-file/#Environment_Variables) konfigurieren.

  ```shell
  /opt/agents/newrelic/java/newrelic-agent.jar
  ```

## <a name="portal"></a>Portal

Sie können diesen Agent auch über das Portal mit dem folgenden Verfahren nutzen. 

1. Suchen die App unter **Einstellungen**/**Apps** im Navigationsbereich.

   [ ![Suchen nach der zu überwachenden App](media/new-relic-monitoring/find-app.png) ](media/new-relic-monitoring/find-app.png)

2. Klicken Sie auf die Anwendung, um zur Seite **Übersicht** zu navigieren.

   [ ![Seite „Übersicht“](media/new-relic-monitoring/overview-page.png) ](media/new-relic-monitoring/overview-page.png)

3. Klicken Sie im linken Navigationsbereich auf **Konfiguration**, um die **Umgebungsvariablen** der Anwendung hinzuzufügen, zu aktualisieren oder zu löschen.

   [ ![Umgebung aktualisieren](media/new-relic-monitoring/configurations-update-environment.png) ](media/new-relic-monitoring/configurations-update-environment.png)

4. Klicken Sie auf **Allgemeine Einstellungen**, um die **JVM-Option** der Anwendung hinzuzufügen, zu aktualisieren oder zu löschen.

   [ ![Aktualisieren der JVM-Option](media/new-relic-monitoring/update-jvm-option.png) ](media/new-relic-monitoring/update-jvm-option.png)

5. Zeigen Sie die **Zusammenfassungsseite** der Anwendungs-API bzw. des Gateways im **New Relic**-Dashboard an.

   [ ![Seite „Zusammenfassung“ der App](media/new-relic-monitoring/app-summary-page.png) ](media/new-relic-monitoring/app-summary-page.png)

6. Zeigen Sie die Seite **Zusammenfassung** des Kundendiensts der Anwendung im **New Relic**-Dashboard an.
 
   [ ![Seite „Kundendienst“](media/new-relic-monitoring/customers-service.png) ](media/new-relic-monitoring/customers-service.png)  

7. Zeigen Sie die Seite **Dienstzuordnung** im **New Relic**-Dashboard an.  

   [ ![ Seite „Dienstzuordnung“](media/new-relic-monitoring/service-map.png) ](media/new-relic-monitoring/service-map.png) 

8. Zeigen Sie die **JVM**-Optionen der Anwendung über das **New Relic**-Dashboard an.

   [ ![Seite „JVM“](media/new-relic-monitoring/jvm-page.png) ](media/new-relic-monitoring/jvm-page.png) 

9. Zeigen Sie das Anwendungsprofil im **New Relic**-Dashboard an.

   [ ![Anwendungsprofil](media/new-relic-monitoring/profile-app.png) ](media/new-relic-monitoring/profile-app.png) 

## <a name="new-relic-java-agent-logging"></a>Protokollierung des New Relic-Java-Agents

Standardmäßig gibt Azure Spring Cloud die Protokolle des **New Relic**-Java-Agents in `STDOUT` aus. Sie werden mit den Anwendungsprotokollen kombiniert. Sie können die explizite Agent-Version aus den Anwendungsprotokollen abrufen.

Sie können die Protokolle des **New Relic**-Agents auch aus den folgenden Quellen abrufen:

* Azure Spring Cloud-Protokolle.
* Azure Spring Cloud Application Insights.
* Azure Spring Cloud-LogStream.

Sie können einige von **New Relic** bereitgestellte Umgebungsvariablen nutzen, um die Protokollierung des **neuen Agents** zu konfigurieren, z. B. `NEW_RELIC_LOG_LEVEL`, um den Protokolliergrad zu steuern. Weitere Informationen finden Sie unter [New Relic-Umgebungsvariablen](https://docs.newrelic.com/docs/agents/java-agent/configuration/java-agent-configuration-config-file/#Environment_Variables).

> [!CAUTION]
> Es wird dringend empfohlen, das Standardverhalten der Protokollierung *nicht* außer Kraft zu setzen, das von Azure Spring Cloud für New Relic bereitgestellt wird. Andernfalls werden die Protokollierungsszenarien in den oben beschriebenen Szenarien blockiert, und die Protokolldateien gehen möglicherweise verloren. Beispielsweise sollten Sie die folgenden Umgebungsvariablen nicht an Ihre Anwendungen übergeben. Protokolldateien können nach einem Neustart oder einer erneuten Bereitstellung von Anwendungen verloren gehen.
>
> * NEW_RELIC_LOG
> * NEW_RELIC_LOG_FILE_PATH

## <a name="new-relic-java-agent-updateupgrade"></a>Update/Upgrade des New Relic-Java-Agents

Der **New Relic**-Java-Agent führt regelmäßig ein Update/Upgrade des JDK aus. Das Update/Upgrade des Agents kann sich auf die folgenden Szenarien auswirken.

* Vorhandene Anwendungen, die den **New Relic**-Java-Agent vor dem Update/Upgrade verwenden, bleiben unverändert.
* Vorhandene Anwendungen, die den **New Relic**-Java-Agent vor dem Update/Upgrade verwenden, erfordern einen Neustart oder eine erneute Bereitstellung, um die neue Version des **New Relic**-Java-Agents zu nutzen.
* Neue Anwendungen, die nach dem Update/Upgrade erstellt werden, verwenden die neue Version des **New Relic**-Java-Agents.

## <a name="vnet-injection-instance-outbound-traffic-configuration"></a>Konfiguration des ausgehenden Datenverkehrs der VNET-Injektionsinstanz

Für eine VNET-Injektionsinstanz von Azure Spring Cloud müssen Sie sicherstellen, dass der ausgehende Datenverkehr für den **New Relic**-Java-Agent ordnungsgemäß konfiguriert ist. Weitere Informationen finden Sie unter [Netzwerke von New Relic](https://docs.newrelic.com/docs/using-new-relic/cross-product-functions/install-configure/networks/#agents).

## <a name="next-steps"></a>Nächste Schritte
*  [Verteilte Ablaufverfolgung und App Insights](how-to-distributed-tracing.md)
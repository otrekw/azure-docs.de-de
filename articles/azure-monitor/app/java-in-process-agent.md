---
title: Überwachen von Java-Anwendungen in beliebiger Umgebung – Azure Monitor Application Insights
description: Überwachen der Anwendungsleistung für Java-Anwendungen, die in einer beliebigen Umgebung ausgeführt werden, ohne die App zu instrumentieren. Verteilte Ablaufverfolgung und Anwendungszuordnung.
ms.topic: conceptual
ms.date: 03/29/2020
ms.openlocfilehash: 5a62be45320523ee0577d56eb557a4f87a58a1cc
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886856"
---
# <a name="java-codeless-application-monitoring-azure-monitor-application-insights---public-preview"></a>Java-Anwendungsüberwachung ohne Code mit Azure Monitor Application Insights – Public Preview

Bei der Java-Anwendungsüberwachung ohne Code geht es um Einfachheit. Es gibt keine Codeänderungen, und der Java-Agent kann durch nur wenige Konfigurationsänderungen aktiviert werden.

 Der Java-Agent kann in jeder Umgebung verwendet werden und ermöglicht Ihnen die Überwachung aller Ihrer Java-Anwendungen. Anders ausgedrückt: Ganz gleich, ob Sie Ihre Java-Apps auf VMs, lokal, in AKS, unter Windows oder unter Linux ausführen – Sie nennen die App, und der Java 3.0-Agent überwacht sie.

Das Application Insights Java SDK muss nicht mehr zu Ihrer Anwendung hinzugefügt werden, da der 3.0-Agent Anforderungen, Abhängigkeiten und Protokolle eigenständig und automatisch sammelt.

Sie können weiterhin benutzerdefinierte Telemetriedaten aus Ihrer Anwendung senden. Der 3.0-Agent verfolgt und korreliert diese zusammen mit allen automatisch gesammelten Telemetriedaten.

## <a name="quickstart"></a>Schnellstart

**1. Herunterladen des Agents**

Laden Sie [applicationinsights-agent-3.0.0-PREVIEW.2.jar](https://github.com/microsoft/ApplicationInsights-Java/releases/download/3.0.0-PREVIEW.2/applicationinsights-agent-3.0.0-PREVIEW.2.jar) herunter.

**2. Verweisen der JVM auf den Agent**

Fügen Sie den JVM-Argumenten Ihrer Anwendung den Eintrag `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.2.jar` hinzu.

Typische JVM-Argumente sind `-Xmx512m` und `-XX:+UseG1GC`. Wenn Sie wissen, wo diese hinzugefügt werden, wissen Sie auch, wo dieses Argument hinzuzufügen ist.

Weitere Informationen zum Konfigurieren der JVM-Argumente Ihrer Anwendung finden Sie unter [3.0 Preview: Tipps zum Aktualisieren Ihrer JVM-Argumente](https://github.com/microsoft/ApplicationInsights-Java/wiki/3.0-Preview:-Tips-for-updating-your-JVM-args).

**3. Verweisen des Agents auf die Application Insights-Ressource**

Wenn Sie noch nicht über eine Application Insights-Ressource verfügen, können Sie eine neue Ressource erstellen, indem Sie die Schritte im [Leitfaden zum Erstellen einer Ressource](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) ausführen.
Erstellen Sie eine Konfigurationsdatei mit dem Namen `ApplicationInsights.json`, und legen Sie sie im gleichen Verzeichnis wie `applicationinsights-agent-3.0.0-PREVIEW.2.jar` mit folgendem Inhalt ab:

```json
{
  "instrumentationSettings": {
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000"
  }
}
```

Die Verbindungszeichenfolge finden Sie in der Application Insights-Ressource:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights-Verbindungszeichenfolge":::

**4. Das war's schon!**

Starten Sie nun Ihre Anwendung, und wechseln Sie zur Application Insights-Ressource im Azure-Portal, um die Überwachungsdaten anzuzeigen.

> [!NOTE]
> Es kann einige Minuten dauern, bis die Überwachungsdaten im Portal angezeigt werden.


## <a name="configuration-options"></a>Konfigurationsoptionen

In der Datei `ApplicationInsights.json` können Sie zusätzlich Folgendes konfigurieren:

* Cloudrollenname
* Cloudrolleninstanz
* Erfassung des Anwendungsprotokolls
* JMX-Metriken
* Micrometer
* Heartbeat
* Stichproben
* HTTP-Proxy
* Selbstdiagnose

Ausführliche Informationen finden Sie unter [3.0 Public Preview: Konfigurationsoptionen](https://github.com/microsoft/ApplicationInsights-Java/wiki/3.0-Preview:-Configuration-Options).

## <a name="autocollected-requests-dependencies-logs-and-metrics"></a>Automatisch gesammelte Anforderungen, Abhängigkeiten, Protokolle und Metriken

### <a name="requests"></a>Anforderungen

* JMS-Consumer
* Kafka-Consumer
* Netty/WebFlux
* Servlets
* Spring-Zeitplanung

### <a name="dependencies-with-distributed-trace-propagation"></a>Abhängigkeiten mit Weitergabe der verteilten Ablaufverfolgung

* Apache HttpClient und HttpAsyncClient
* gRPC
* java.net.HttpURLConnection
* JMS
* Kafka
* Netty-Client
* OkHttp

### <a name="other-dependencies"></a>Andere Abhängigkeiten

* Cassandra
* JDBC
* MongoDB (asynchron und synchron)
* Redis (Lettuce und Jedis)

### <a name="logs"></a>Protokolle

* java.util.logging
* Log4j
* SLF4J/Logback

### <a name="metrics"></a>Metriken

* Micrometer
* JMX-Metriken

## <a name="sending-custom-telemetry-from-your-application"></a>Senden benutzerdefinierter Telemetriedaten aus Ihrer Anwendung

Unser Ziel in Version 3.0 und höher besteht darin, Ihnen das Senden benutzerdefinierter Telemetriedaten mithilfe von Standard-APIs zu ermöglichen.

Es werden Micrometer, OpenTelemetry-API und die gängigen Protokollierungsframeworks unterstützt. Mit Application Insights Java 3.0 werden die Telemetriedaten automatisch erfasst und mit allen automatisch gesammelten Telemetriedaten korreliert.

Aus diesem Grund ist es zum jetzigen Zeitpunkt nicht geplant, ein SDK mit Application Insights 3.0 zu veröffentlichen.

Application Insights Java 3.0 lauscht bereits auf Telemetriedaten, die an das Application Insights Java SDK 2.x gesendet werden. Diese Funktionalität ist ein wichtiger Bestandteil der Upgrades für vorhandene 2.x-Benutzer und füllt eine wichtige Lücke in der Unterstützung benutzerdefinierter Telemetriedaten bis zur allgemeinen Verfügbarkeit der OpenTelemetry-API.

## <a name="sending-custom-telemetry-using-application-insights-java-sdk-2x"></a>Senden benutzerdefinierter Telemetriedaten mit Application Insights Java SDK 2.x

Fügen Sie `applicationinsights-core-2.6.0.jar` zu Ihrer Anwendung hinzu (alle 2.x-Versionen werden von Application Insights Java 3.0 unterstützt, aber es lohnt sich, die neueste Version zu verwenden, wenn Sie die Wahl haben):

```xml
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-core</artifactId>
    <version>2.6.0</version>
  </dependency>
```

Erstellen Sie einen TelemetryClient:

  ```java
private static final TelemetryClient telemetryClient = new TelemetryClient();
```

und verwenden Sie diesen zum Senden benutzerdefinierter Telemetriedaten.

### <a name="events"></a>Ereignisse

  ```java
telemetryClient.trackEvent("WinGame");
```
### <a name="metrics"></a>Metriken

Sie können Metriktelemetrie über [Micrometer](https://micrometer.io) senden:

```java
  Counter counter = Metrics.counter("test_counter");
  counter.increment();
```

Sie können aber auch Application Insights Java SDK 2.x verwenden:

```java
  telemetryClient.trackMetric("queueLength", 42.0);
```

### <a name="dependencies"></a>Abhängigkeiten

```java
  boolean success = false;
  long startTime = System.currentTimeMillis();
  try {
      success = dependency.call();
  } finally {
      long endTime = System.currentTimeMillis();
      RemoteDependencyTelemetry telemetry = new RemoteDependencyTelemetry();
      telemetry.setTimestamp(new Date(startTime));
      telemetry.setDuration(new Duration(endTime - startTime));
      telemetryClient.trackDependency(telemetry);
  }
```

### <a name="logs"></a>Protokolle
Sie können benutzerdefinierte Protokolltelemetriedaten über Ihr bevorzugtes Protokollierungsframework senden.

Sie können aber auch Application Insights Java SDK 2.x verwenden:

```java
  telemetryClient.trackTrace(message, SeverityLevel.Warning, properties);
```

### <a name="exceptions"></a>Ausnahmen
Sie können benutzerdefinierte Ausnahmetelemetriedaten über Ihr bevorzugtes Protokollierungsframework senden.

Sie können aber auch Application Insights Java SDK 2.x verwenden:

```java
  try {
      ...
  } catch (Exception e) {
      telemetryClient.trackException(e);
  }
```

## <a name="upgrading-from-application-insights-java-sdk-2x"></a>Upgrade von Application Insights Java SDK 2.x

Wenn Sie bereits Application Insights Java SDK 2.x in Ihrer Anwendung verwenden, müssen Sie es nicht entfernen. Der Java 3.0-Agent erkennt es und erfasst und korreliert alle benutzerdefinierten Telemetriedaten, die über das Java SDK 2.x gesendet werden, während gleichzeitig alle vom Java SDK 2.x durchgeführten automatischen Sammlungen unterdrückt werden, um eine doppelte Erfassung zu verhindern.

> [!NOTE]
> Hinweis: TelemetryInitializers und TelemetryProcessors von Java SDK 2.x werden bei Verwendung des 3.0-Agents nicht ausgeführt.

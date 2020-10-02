---
title: Überwachen von Java-Anwendungen an einem beliebigen Standort – Azure Monitor Application Insights
description: Überwachen Sie die Anwendungsleistung für Java-Anwendungen in einer beliebigen Umgebung – ohne Code und ohne App-Instrumentierung. Ermitteln Sie die Grundursache für Probleme mithilfe einer verteilten Ablaufverfolgung und einer Anwendungsübersicht.
ms.topic: conceptual
ms.date: 04/16/2020
ms.custom: devx-track-java
ms.openlocfilehash: 561a6405a49d8f15affbf6d8d4de1a7f4886826a
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90056097"
---
# <a name="configuration-options---java-standalone-agent-for-azure-monitor-application-insights"></a>Konfigurationsoptionen: Eigenständige Java-Agents für Azure Monitor Application Insights



## <a name="connection-string-and-role-name"></a>Verbindungszeichenfolge und Rollenname

```json
{
  "instrumentationSettings": {
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
      "roleName": "my cloud role name"
    }
  }
}
```

Die Verbindungszeichenfolge ist erforderlich, und der Rollenname ist immer dann relevant, wenn Sie Daten unterschiedlicher Anwendungen an dieselbe Application Insights-Ressource senden.

Weitere Informationen und zusätzliche Konfigurationsoptionen finden Sie unten.

## <a name="configuration-file-path"></a>Pfad der Konfigurationsdatei

Standardmäßig erwartet Application Insights Java 3.0 Preview eine Konfigurationsdatei mit dem Namen `ApplicationInsights.json`, die sich im gleichen Verzeichnis wie `applicationinsights-agent-3.0.0-PREVIEW.5.jar` befindet.

Verwenden Sie eines der folgenden Elemente, um einen eigenen Pfad für Ihre Konfigurationsdatei anzugeben:

* Umgebungsvariable `APPLICATIONINSIGHTS_CONFIGURATION_FILE` oder
* Java-Systemeigenschaft `applicationinsights.configurationFile`

Wenn Sie einen relativen Pfad angeben, wird dieser relativ zum Verzeichnis von `applicationinsights-agent-3.0.0-PREVIEW.5.jar` aufgelöst.

## <a name="connection-string"></a>Verbindungszeichenfolge

Dies ist erforderlich. Die Verbindungszeichenfolge finden Sie in der Application Insights-Ressource:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights-Verbindungszeichenfolge":::

Sie können auch die Verbindungszeichenfolge mit der Umgebungsvariablen `APPLICATIONINSIGHTS_CONNECTION_STRING` festlegen.

## <a name="cloud-role-name"></a>Cloudrollenname

Der Name der Cloudrolle wird verwendet, um die Komponente in der Anwendungsübersicht zu beschriften.

Wenn Sie den Cloudrollennamen festlegen möchten:

```json
{
  "instrumentationSettings": {
    "preview": {   
      "roleName": "my cloud role name"
    }
  }
}
```

Falls der Cloudrollenname nicht festgelegt ist, wird der Name der Application Insights-Ressource zur Beschriftung der Komponente in der Anwendungsübersicht verwendet.

Sie können den Namen der Cloudrolle auch mithilfe der Umgebungsvariablen `APPLICATIONINSIGHTS_ROLE_NAME` festlegen.

## <a name="cloud-role-instance"></a>Cloudrolleninstanz

Der Name der Cloudrolleninstanz entspricht standardmäßig dem Computernamen.

Wenn Sie für die Cloudrolleninstanz einen anderen Namen festlegen möchten:

```json
{
  "instrumentationSettings": {
    "preview": {
      "roleInstance": "my cloud role instance"
    }
  }
}
```

Sie können den Namen der Cloudrolleninstanz auch mithilfe der Umgebungsvariablen `APPLICATIONINSIGHTS_ROLE_INSTANCE` festlegen.

## <a name="application-log-capture"></a>Erfassung des Anwendungsprotokolls

Application Insights Java 3.0 Preview führt mithilfe von Log4j, Logback und java.util.logging eine automatische Anwendungsprotokollierung durch.

Die gesamte Protokollierung erfolgt standardmäßig auf `WARN`-Ebene oder höher.

Wenn Sie diesen Schwellenwert ändern möchten:

```json
{
  "instrumentationSettings": {
    "preview": {
      "instrumentation": {
        "logging": {
          "threshold": "ERROR"
        }
      }
    }
  }
}
```

Nachfolgend werden die gültigen `threshold`-Werte, die Sie in der Datei `ApplicationInsights.json` angeben können, und deren Zuordnung zu den Protokolliergraden in verschiedenen Frameworks für die Protokollierung gezeigt:

| Schwellenwert   | Log4j  | Logback | JUL     |
|-------------------|--------|---------|---------|
| OFF               | OFF    | OFF     | OFF     |
| FATAL             | FATAL  | ERROR   | SEVERE  |
| ERROR (oder SEVERE) | ERROR  | ERROR   | SEVERE  |
| WARN (oder WARNING) | WARN   | WARN    | WARNING |
| INFO              | INFO   | INFO    | INFO    |
| CONFIG            | DEBUG  | DEBUG   | CONFIG  |
| DEBUG (oder FINE)   | DEBUG  | DEBUG   | FINE    |
| FINER             | DEBUG  | DEBUG   | FINER   |
| TRACE (oder FINEST) | TRACE  | TRACE   | FINEST  |
| ALL               | ALL    | ALL     | ALL     |

## <a name="jmx-metrics"></a>JMX-Metriken

Wenn Sie einige für Sie relevante JMX-Metriken erfassen möchten:

```json
{
  "instrumentationSettings": {
    "preview": {
      "jmxMetrics": [
        {
          "objectName": "java.lang:type=ClassLoading",
          "attribute": "LoadedClassCount",
          "display": "Loaded Class Count"
        },
        {
          "objectName": "java.lang:type=MemoryPool,name=Code Cache",
          "attribute": "Usage.used",
          "display": "Code Cache Used"
        }
      ]
    }
  }
}
```

## <a name="micrometer-including-metrics-from-spring-boot-actuator"></a>Micrometer (einschließlich Metriken vom Spring Boot-Aktor)

Wenn Ihre Anwendung [Micrometer](https://micrometer.io) nutzt, erfasst Application Insights 3.0 (ab Preview 2) jetzt Metriken, die an die globale Micrometer-Registrierung gesendet werden.

Wenn Ihre Anwendung den [Spring Boot-Aktor](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html) verwendet, erfasst Application Insights 3.0 (ab Preview 4) jetzt Metriken, die vom Spring Boot-Aktor konfiguriert werden (der Micrometer, jedoch nicht die globale Micrometer-Registrierung verwendet).

Wenn Sie diese Features deaktivieren möchten:

```json
{
  "instrumentationSettings": {
    "preview": {
      "instrumentation": {
        "micrometer": {
          "enabled": false
        }
      }
    }
  }
}
```

## <a name="heartbeat"></a>Heartbeat

Application Insights Java 3.0 Preview sendet standardmäßig alle 15 Minuten eine Taktmetrik. Wenn Sie die Taktmetrik zum Auslösen von Warnungen verwenden, können Sie die Frequenz für den Takt erhöhen:

```json
{
  "instrumentationSettings": {
    "preview": {
      "heartbeat": {
        "intervalSeconds": 60
      }
    }
  }
}
```

> [!NOTE]
> Es ist nicht möglich, die Takthäufigkeit zu verringern, weil die Taktdaten auch zum Nachverfolgen der Application Insights-Nutzung verwendet werden.

## <a name="sampling"></a>Stichproben

Stichproben sind nützlich, wenn Sie die Kosten senken müssen.
Stichproben werden als Funktion für die Vorgangs-ID (auch als Nachverfolgungs-ID bezeichnet) ausgeführt, sodass dieselbe Vorgangs-ID immer zur selben Stichprobenentscheidung führt. So wird sichergestellt, dass Stichproben nicht nur für Teile einer verteilten Transaktion erfasst werden, während andere Teile ausgelassen werden.

Wenn Sie beispielsweise die Stichprobenentnahme auf 10 % festlegen, werden nur 10 % Ihrer Transaktionen angezeigt, aber in diesen 10 % sind vollständige End-to-End-Transaktionsdetails enthalten.

Hier ist ein Beispiel, wie die Stichprobenentnahme auf **10 % aller Transaktionen** festgelegt werden kann. Stellen Sie sicher, dass Sie die für Ihren Anwendungsfall richtige Stichprobenrate festlegen:

```json
{
  "instrumentationSettings": {
    "preview": {
      "sampling": {
        "fixedRate": {
          "percentage": 10
        }
      }
    }
  }
}
```

## <a name="http-proxy"></a>HTTP-Proxy

Wenn sich Ihre Anwendung hinter einer Firewall befindet und nicht direkt mit Application Insights verbunden werden kann (siehe [Von Application Insights verwendete IP-Adressen](./ip-addresses.md)), können Sie Application Insights Java 3.0 Preview zur Verwendung eines HTTP-Proxys konfigurieren:

```json
{
  "instrumentationSettings": {
    "preview": {
      "httpProxy": {
        "host": "myproxy",
        "port": 8080
      }
    }
  }
}
```

## <a name="self-diagnostics"></a>Selbstdiagnose

Als „Selbstdiagnose“ wird eine interne Protokollierung von Application Insights Java 3.0 Preview bezeichnet.

Diese kann hilfreich sein, um Probleme mit Application Insights selbst zu identifizieren und zu diagnostizieren.

Standardmäßig erfolgt die Protokollierung an die Konsole auf Ebene `warn`, was dieser Konfiguration entspricht:

```json
{
  "instrumentationSettings": {
    "preview": {
      "selfDiagnostics": {
        "destination": "console",
        "level": "WARN"
      }
    }
  }
}
```

Gültige Ebenen sind `OFF`, `ERROR`, `WARN`, `INFO`, `DEBUG` und `TRACE`.

Wenn die Protokollierung nicht an die Konsole, sondern an eine Datei erfolgen soll:

```json
{
  "instrumentationSettings": {
    "preview": {
      "selfDiagnostics": {
        "destination": "file",
        "directory": "/var/log/applicationinsights",
        "level": "WARN",
        "maxSizeMB": 10
      }
    }
  }
}
```

Bei Verwendung der Dateiprotokollierung findet ein Rollover statt, wenn die Dateigröße `maxSizeMB` erreicht. Es werden jeweils nur die aktuelle Protokolldatei und die letzte vorherige Protokolldatei beibehalten.

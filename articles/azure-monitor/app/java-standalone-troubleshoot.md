---
title: Problembehandlung bei Azure Monitor Application Insights für Java
description: Erfahren Sie, wie Sie Probleme mit dem Java-Agent für Azure Monitor Application Insights beheben.
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-java
ms.openlocfilehash: 14644f76b7de53b2b6ee3f04131daaf59267a5ff
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97507641"
---
# <a name="troubleshooting-guide-azure-monitor-application-insights-for-java"></a>Handbuch zur Problembehandlung: Azure Monitor Application Insights für Java

In diesem Artikel werden einige der häufigsten Probleme behandelt, die beim Instrumentieren einer Java-Anwendung mit dem Java-Agent für Application Insights auftreten können. Außerdem werden die Schritte zum Beheben dieser Probleme beschrieben. Application Insights ist ein Feature des Azure Monitor-Plattformdiensts.

## <a name="check-the-self-diagnostic-log-file"></a>Überprüfen der Protokolldatei zur Selbstdiagnose

Standardmäßig wird vom Java 3.0-Agent für Application Insights eine Protokolldatei mit dem Namen `applicationinsights.log` in dem Verzeichnis erstellt, in dem sich auch die Datei `applicationinsights-agent-3.0.0.jar` befindet.

Diese Protokolldatei sollten Sie als Erstes auf Hinweise auf aufgetretene Probleme untersuchen.

## <a name="upgrade-from-the-application-insights-java-2x-sdk"></a>Durchführen eines Upgrades vom Application Insights SDK für Java 2.x

Wenn Sie bereits das Application Insights Java 2.x SDK in Ihrer Anwendung nutzen, können Sie es weiterhin verwenden. Der Java 3.0-Agent wird es erkennen. Weitere Informationen finden Sie unter [Upgrade des Application Insights Java 2.x SDK](./java-standalone-upgrade-from-2x.md).

## <a name="upgrade-from-application-insights-java-30-preview"></a>Durchführen eines Upgrades von Application Insights für Java 3.0 (Vorschau)

Wenn Sie ein Upgrade vom Agent der Vorschauversion von Java 3.0 durchführen, überprüfen Sie alle [Konfigurationsoptionen](./java-standalone-config.md) sorgfältig. Die JSON-Struktur wurde im Release 3.0 für die allgemeine Verfügbarkeit (General Availability, GA) grundlegend geändert.

Zu diesen Änderungen zählen die folgenden:

-  Der Name der Konfigurationsdatei wurde von `ApplicationInsights.json` in `applicationinsights.json` geändert.
-  Der Knoten `instrumentationSettings` ist nicht mehr vorhanden. Alle Inhalte in `instrumentationSettings` wurden auf die Stammebene verschoben. 
-  Konfigurationsknoten wie `sampling`, `jmxMetrics`, `instrumentation` und `heartbeat` wurden aus `preview` auf die Stammebene verschoben.

## <a name="some-logging-is-not-auto-collected"></a>Keine automatische Erfassung bestimmter Protokolle

Eine Protokollierung erfolgt nur, wenn erstens der für das Protokollierungsframework konfigurierte Schwellenwert und zweitens auch der in Application Insights konfigurierte Schwellenwert erreicht wird.

Wenn Sie wissen möchten, ob eine bestimmte Protokollierungsanweisung den konfigurierten Schwellenwert des Protokollierungsframeworks erreicht, überprüfen Sie am besten, ob er in Ihrem normalen Anwendungsprotokoll (z. B. in einer Datei oder an der Konsole) angezeigt wird.

Weitere Informationen finden Sie unter [Automatisch gesammelte Protokolle](./java-standalone-config.md#auto-collected-logging).

## <a name="import-ssl-certificates"></a>Importieren von SSL-Zertifikaten

Wenn Sie den standardmäßigen Java-Keystore verwenden, verfügt dieser bereits über alle Stammzertifikate der Zertifizierungsstelle. Es sollte daher nicht erforderlich sein, weitere SSL-Zertifikate zu importieren.

Wenn Sie einen benutzerdefinierten Java-Keystore verwenden, müssen Sie ggf. die SSL-Zertifikate für den Application Insights-Endpunkt importieren.

### <a name="key-terminology"></a>Wesentliche Terminologie
Ein *Keystore* ist ein Repository mit Zertifikaten sowie öffentlichen und privaten Schlüsseln. Normalerweise verfügen JDK-Distributionen (Java Development Kit) über eine ausführbare Datei für ihre Verwaltung: `keytool`.

Das folgende Beispiel ist ein einfacher Befehl zum Importieren eines SSL-Zertifikats in den Keystore:

`keytool -importcert -alias your_ssl_certificate -file "your downloaded SSL certificate name".cer -keystore "Your KeyStore name" -storepass "Your keystore password" -noprompt`

### <a name="steps-to-download-and-add-an-ssl-certificate"></a>Schritte zum Herunterladen und Hinzufügen eines SSL-Zertifikats

1.  Öffnen Sie Ihren bevorzugten Browser, und navigieren Sie zur `IngestionEndpoint`-URL in der Verbindungszeichenfolge, die zum Instrumentieren Ihrer Anwendung verwendet wird.

    :::image type="content" source="media/java-ipa/troubleshooting/ingestion-endpoint-url.png" alt-text="Screenshot einer Application Insights-Verbindungszeichenfolge":::

2.  Wählen Sie im Browser das Symbol **Websiteinformationen anzeigen** (Schlosssymbol) und dann die Option **Zertifikat** aus.

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-icon-capture.png" alt-text="Screenshot der Option „Zertifikat“ in den Websiteinformationen":::

3.  Wechseln Sie zur Registerkarte **Details**, und wählen Sie **In Datei kopieren** aus.
4.  Wählen Sie die Schaltfläche **Weiter**, anschließend das Format **Base64-codiert X.509 (.CER)** und dann erneut **Weiter** aus.

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-export-wizard.png" alt-text="Screenshot des Zertifikatexport-Assistenten mit ausgewähltem Format":::

5.  Geben Sie die Datei an, in der Sie das SSL-Zertifikat speichern möchten. Wählen Sie anschließend **Weiter** > **Fertig stellen** aus. Die Meldung „Der Exportvorgang wurde erfolgreich abgeschlossen“ sollte angezeigt werden.
6.  Wenn Sie über das Zertifikat verfügen, können Sie es in einen Java-Keystore importieren. Verwenden Sie den [obigen Befehl](#key-terminology), um Zertifikate zu importieren.

> [!WARNING]
> Sie müssen diese Schritte erneut ausführen, um vor Ablauf des aktuellen Zertifikats ein neues Zertifikat zu erhalten. Informationen zum Ablauf finden Sie im Dialogfeld **Zertifikat** auf der Registerkarte **Details**.
>
> :::image type="content" source="media/java-ipa/troubleshooting/certificate-details.png" alt-text="Screenshot mit Details zum SSL-Zertifikat":::

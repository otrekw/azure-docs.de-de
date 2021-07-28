---
title: Datei einfügen
description: include file
services: azure-communication-services
author: jbeauregardb
manager: vravikumar
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 06/01/2021
ms.topic: include
ms.custom: include file
ms.author: jbeauregardb
ms.openlocfilehash: f473b3dbc898a19ad9ef3118f86c7e0ab0e23229
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111593107"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Python](https://www.python.org/downloads/) 2.7 bzw. 3.6 oder höher.
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource](../create-communication-resource.md).
- Erstellen Sie eine [Application Insights-Ressource](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) im Azure-Portal.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

1. Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

   ```console
   mkdir application-insights-quickstart && cd application-insights-quickstart
   ```

1. Verwenden Sie einen Text-Editor zum Erstellen einer Datei mit dem Namen **application-insights-quickstart.py** im Stammverzeichnis des Projekts, und fügen Sie die Struktur für das Programm hinzu, einschließlich einer einfachen Ausnahmebehandlung. In den folgenden Abschnitten wird dieser Datei der gesamte Quellcode für diese Schnellstartanleitung hinzugefügt.

   ```python
    import os
    from opentelemetry import trace
    from opentelemetry.sdk.trace import TracerProvider
    from opentelemetry.sdk.trace.export import BatchSpanProcessor
    from azure.communication.identity import CommunicationIdentityClient, CommunicationUserIdentifier

    from azure.monitor.opentelemetry.exporter import AzureMonitorTraceExporter

   try:
      print("Azure Communication Services - Access Tokens Quickstart")
      # Quickstart code goes here
   except Exception as ex:
      print("Exception:")
      print(ex)
   ```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie – immer noch im Anwendungsverzeichnis – das Paket Azure Communication Services Identity SDK für Python und den Microsoft Opentelemetry Exporter für Azure Monitor.

```console
pip install azure-communication-identity
pip install azure-monitor-opentelemetry-exporter --pre
```

## <a name="setting-up-the-telemetry-tracer-with-communication-identity-sdk-calls"></a>Einrichten der Telemetrie-Ablaufverfolgung mit SDK-Aufrufen für die Kommunikationsidentität

Instanziieren Sie einen Kommunikationsidentitätsclient (`CommunicationIdentityClient`) mit Ihrer Verbindungszeichenfolge. Im folgenden Code wird die Verbindungszeichenfolge für die Ressource aus einer Umgebungsvariablen namens `COMMUNICATION_SERVICES_CONNECTION_STRING` abgerufen. Informationen zur [Verwaltung der Verbindungszeichenfolge Ihrer Ressource](../create-communication-resource.md#store-your-connection-string).

Fügen Sie diesen Code im `try`-Block hinzu:

```python
connection_string = os.environ["COMMUNICATION_SERVICES_CONNECTION_STRING"]
client = CommunicationIdentityClient.from_connection_string(connection_string)
```

Zunächst müssen Sie eine Instanz eines `AzureMonitorTraceExporter`-Objekts erstellen, um die Spanne zu erstellen, mit der Sie die Anforderungen in Azure Monitor nachverfolgen können. Sie müssen die Verbindungszeichenfolge aus Ihrer Application Insights-Ressource angeben.

```python
exporter = AzureMonitorTraceExporter.from_connection_string(
    "<APPLICATION-INSIGHTS-RESOURCE-CONNECTION-STRING>"
)
```

Mit diesem Exporter können Sie anschließend die folgenden Instanzen erstellen, um die Ablaufverfolgung der Anforderung zu ermöglichen. Fügen Sie nach dem Erstellen von `AzureMonitorTraceExporter` den folgenden Code hinzu:

```python
    trace.set_tracer_provider(TracerProvider())
    tracer = trace.get_tracer(__name__)
    span_processor = BatchSpanProcessor(exporter)
    trace.get_tracer_provider().add_span_processor(span_processor)
```
Nachdem die Ablaufverfolgung initialisiert wurde, können Sie die Spanne erstellen, die für die Ablaufverfolgung Ihrer Anforderungen zuständig ist.

```python
with tracer.start_as_current_span(name="MyIdentityApplication"):
    user = identity_client.create_user()
```

## <a name="run-the-code"></a>Ausführen des Codes

Navigieren Sie in einer Konsoleneingabeaufforderung zu dem Verzeichnis, das die Datei **application-insights-quickstart.py** enthält, und führen Sie dann den folgenden `python`-Befehl aus, um die App auszuführen.

```console
python application-insights-quickstart.py
```

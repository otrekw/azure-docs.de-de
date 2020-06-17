---
title: Senden oder Empfangen von Ereignissen an bzw. von Azure Event Hubs unter Verwendung von Python (altes Paket)
description: Diese exemplarische Vorgehensweise zeigt, wie Sie Python-Skripts erstellen und ausführen, die unter Verwendung des alten Pakets „azure-eventhub“ (Version 1) Ereignisse an Azure Event Hubs senden bzw. von dort empfangen.
services: event-hubs
author: spelluru
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.custom: tracking-python
ms.openlocfilehash: 96c95efdc8f2154e0586fec59d1af66496acb101
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84558967"
---
# <a name="quickstart-send-and-receive-events-with-event-hubs-using-python-azure-eventhub-version-1"></a>Schnellstart: Senden und Empfangen von Ereignissen mit Event Hubs mithilfe von Python (Version 1 von „azure-eventhub“)
In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des Python-Pakets **azure-eventhub** (Version 1) Ereignisse an einen Event Hub senden bzw. von dort empfangen. 

> [!WARNING]
> In dieser Schnellstartanleitung wird das alte Paket „azure-eventhub“ (Version 1) verwendet. Eine Schnellstartanleitung, in der die aktuelle **Version 5** des Pakets verwendet wird, finden Sie unter [Senden oder Empfangen von Ereignissen an bzw. von Event Hubs unter Verwendung von Python (Version 5 von „azure-eventhub“)](get-started-python-send-v2.md). Informationen zur Umstellung Ihrer Anwendung vom alten auf das neue Paket finden Sie im [Leitfaden zur Migration von Version 1 von „azure-eventhub“ zu Version 5](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md).
 

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie mit Azure Event Hubs noch nicht vertraut sind, lesen Sie vor dem Durcharbeiten dieser Schnellstartanleitung die Informationen unter [Übersicht über Event Hubs](event-hubs-about.md). 

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- **Microsoft Azure-Abonnement.** Für die Verwendung von Azure-Diensten benötigen Sie ein Abonnement. Das gilt auch für Azure Event Hubs.  Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/free/) registrieren oder beim [Erstellen eines Kontos](https://azure.microsoft.com) Ihre MSDN-Abonnentenvorteile nutzen.
- Python 3.4 oder höher mit installiertem und aktualisiertem `pip`.
- Das Python-Paket für Event Hubs. Um das Paket zu installieren, führen Sie diesen Befehl in einer Eingabeaufforderung aus, in deren Pfad Python enthalten ist: 
  
  ```cmd
  pip install azure-eventhub==1.3.*
  ```
- **Erstellen Sie einen Event Hubs-Namespace und einen Event Hub**. Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Erstellen Sie anhand der Anleitung in [diesem Artikel](event-hubs-create.md) einen Namespace und einen Event Hub. Gehen Sie anschließend wie im Artikel beschrieben vor, um den Wert des Zugriffsschlüssels für den Event Hub abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Sie verwenden den Zugriffsschlüssel im Code, den Sie später in dieser Schnellstartanleitung schreiben. Der Standardschlüsselname lautet: **RootManageSharedAccessKey**. 


## <a name="send-events"></a>Senden von Ereignisse

Um eine Python-Anwendung zu erstellen, die Ereignisse an einen Event Hub sendet:

> [!NOTE]
> Anstatt den Schnellstart durchzuarbeiten, können Sie auch die [Beispiel-Apps](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) von GitHub herunterladen und ausführen. Ersetzen Sie die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch Ihre Event Hub-Werte.

1. Öffnen Sie Ihren bevorzugten Python-Editor, z. B. [Visual Studio Code](https://code.visualstudio.com/).
2. Erstellen Sie eine neue Datei mit dem Namen *send.py*. Dieses Skript sendet 100 Ereignisse an Ihren Event Hub.
3. Fügen Sie den folgenden Code in *send.py* ein, indem Sie die Angaben \<namespace>, \<eventhub>, \<AccessKeyName> und \<primary key value> für Event Hubs durch Ihre Werte ersetzen: 
   
   ```python
   import sys
   import logging
   import datetime
   import time
   import os
   
   from azure.eventhub import EventHubClient, Sender, EventData
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<namespace>.servicebus.windows.net/eventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   try:
       if not ADDRESS:
           raise ValueError("No EventHubs URL supplied.")
   
       # Create Event Hubs client
       client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
       sender = client.add_sender(partition="0")
       client.run()
       try:
           start_time = time.time()
           for i in range(100):
               print("Sending message: {}".format(i))
               message = "Message {}".format(i)
               sender.send(EventData(message))
       except:
           raise
       finally:
           end_time = time.time()
           client.stop()
           run_time = end_time - start_time
           logger.info("Runtime: {} seconds".format(run_time))
   
   except KeyboardInterrupt:
       pass
   ```
   
4. Speichern Sie die Datei . 

Um das Skript auszuführen, führen Sie den folgenden Befehl aus dem Verzeichnis aus, in dem Sie *send.py* gespeichert haben:

```cmd
start python send.py
```

Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

## <a name="receive-events"></a>Empfangen von Ereignissen

Um eine Python-Anwendung zu erstellen, die Ereignisse von einem Event Hub empfängt:

1. Erstellen Sie in Ihrem Python-Editor eine Datei namens *recv.py*.
2. Fügen Sie den folgenden Code in *recv.p*  ein, und ersetzen Sie die Angaben \<namespace>, \<eventhub>, \<AccessKeyName> und \<primary key value> von Event Hubs durch Ihre Werte: 
   
   ```python
   import os
   import sys
   import logging
   import time
   from azure.eventhub import EventHubClient, Receiver, Offset
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   
   CONSUMER_GROUP = "$default"
   OFFSET = Offset("-1")
   PARTITION = "0"
   
   total = 0
   last_sn = -1
   last_offset = "-1"
   client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
   try:
       receiver = client.add_receiver(
           CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
       client.run()
       start_time = time.time()
       for event_data in receiver.receive(timeout=100):
           print("Received: {}".format(event_data.body_as_str(encoding='UTF-8')))
           total += 1
   
       end_time = time.time()
       client.stop()
       run_time = end_time - start_time
       print("Received {} messages in {} seconds".format(total, run_time))
   
   except KeyboardInterrupt:
       pass
   finally:
       client.stop()
   ```
   
4. Speichern Sie die Datei .

Um das Skript auszuführen, führen Sie den folgenden Befehl aus dem Verzeichnis aus, in dem Sie *recv.py* gespeichert haben:

```cmd
start python recv.py
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie in den folgenden Artikeln:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Features und Terminologie in Azure Event Hubs](event-hubs-features.md)
- [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)


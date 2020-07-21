---
title: Integrieren von RabbitMQ in Azure Service Bus
description: Schritt-für-Schritt-Anleitung zur Integration von RabbitMQ in Azure Service Bus
services: Azure Service Bus
author: videlalvaro
ms.topic: how-to
ms.service: service-bus
ms.date: 07/02/2020
ms.author: alvidela
ms.openlocfilehash: df0541802dfc331ffc94e95be112ea7e005960b5
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86049373"
---
# <a name="how-to-integrate-rabbitmq-with-azure-service-bus"></a>Integrieren von RabbitMQ in Azure Service Bus

In dieser Anleitung erfahren Sie, wie Nachrichten von RabbitMQ an Azure Service Bus gesendet werden.

Im Folgenden werden einige Szenarios beschrieben, in denen diese Funktionen genutzt werden können:

- **Edge-Einrichtungen**: In einer Edge-Einrichtung werden Nachrichten an RabbitMQ gesendet. Wir möchten diese Nachrichten jedoch zur weiteren Verarbeitung an [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) weiterleiten, sodass wir viele [Azure-Funktionen für Big Data](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/big-data) nutzen können.
- **Hybrid Cloud**: Ihr Unternehmen hat soeben einen Drittanbieter gefunden, der für die eigenen Messaginganforderungen RabbitMQ verwendet. Er befindet sich in einer anderen Cloud. Während er zu Azure wechselt, können Sie bereits Daten freigeben, indem Sie RabbitMQ mit Azure Service Bus überbrücken.
- **Drittanbieterintegration**: Ein Drittanbieter verwendet RabbitMQ als Broker, und möchte uns seine Daten senden. Er befindet sich jedoch außerhalb unserer Organisation. Wir können ihm einen SAS-Schlüssel bereitstellen, mit dem er Zugriff auf bestimmte Azure Service Bus-Warteschlangen hat, an die er seine Nachrichten weiterleiten kann.

Die Liste lässt sich beliebig fortführen. Bei den meisten dieser Anwendungsfälle ist eine Weiterleitung mittels Bridging von RabbitMQ zu Azure möglich.

Zunächst müssen Sie ein kostenloses Azure-Konto erstellen, indem Sie sich [hier](https://azure.microsoft.com/free/) registrieren.

Nachdem Sie sich bei Ihrem Konto angemeldet haben, wechseln Sie zum [Azure-Portal](https://portal.azure.com/). Erstellen Sie dort einen neuen Azure Service Bus-[Namespace](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal). Bei Namespaces handelt es sich um die Bereichscontainer, in denen sich unsere Messagingkomponenten wie Warteschlangen und Themen befinden.

## <a name="adding-a-new-azure-service-bus-namespace"></a>Hinzufügen eines neuen Azure Service Bus-Namespace

Klicken Sie im Azure-Portal auf das große Plussymbol, um eine neue Ressource hinzuzufügen.

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/create-resource.png" alt-text="Ressource erstellen":::

Wählen Sie anschließend „Integration“ aus, und klicken Sie auf „Azure Service Bus“, um einen Messagingnamespace zu erstellen:

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/integration.png" alt-text="Azure Service Bus auswählen":::

Sie werden aufgefordert, die Namespaceinformationen einzugeben. Wählen Sie das Azure-Abonnement aus, das Sie verwenden möchten. Wenn keine [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal) vorhanden ist, können Sie eine neue erstellen.

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/create-namespace.png" alt-text="Namespace erstellen":::

Verwenden Sie `rabbitmq` für `Namespace name`. Sie können jedoch einen beliebigen Namen verwenden. Legen Sie `East US` als Speicherort fest. Wählen Sie `Basic` als Tarif aus.

Wenn alles ordnungsgemäß verläuft, wird der folgende Bestätigungsbildschirm angezeigt:

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/create-namespace-confirm.png" alt-text="Bestätigung zur Erstellung des Namespace":::

Ihr neuer `rabbitmq`-Namespace wird nun auch im Azure-Portal angezeigt. Klicken Sie darauf, um auf die Ressource zuzugreifen, damit Sie eine Warteschlange hinzufügen können.

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/resource-view-with-namespace.png" alt-text="Ressourcenliste mit neuem Namespace":::

## <a name="creating-our-azure-service-bus-queue"></a>Erstellen einer Azure Service Bus-Warteschlange

Nachdem Sie den Azure Service Bus-Namespace erstellt haben, klicken Sie links unter `Entities` auf die Schaltfläche `Queues`, sodass Sie eine neue Warteschlange hinzufügen können:

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/create-queue.png" alt-text="Warteschlange erstellen":::

Der Name der Warteschlange lautet `from-rabbitmq` – nur zur Erinnerung, woher die Nachrichten stammen. Für alle anderen Optionen können Sie die Standardeinstellungen übernehmen. Sie können sie jedoch auch an die Anforderungen Ihrer App anpassen.

## <a name="enabling-the-rabbitmq-shovel-plugin"></a>Aktivieren des RabbitMQ Shovel-Plugins

Für die Übertragung von Nachrichten von RabbitMQ zu Azure Service Bus verwenden wir das [Shovel-Plugin](https://www.rabbitmq.com/shovel.html), das in RabbitMQ enthalten ist. Sie können das Plugin und die zugehörige grafische Benutzeroberfläche mit dem folgenden Befehl aktivieren:

```bash
rabbitmq-plugins enable rabbitmq_shovel_management
```

>Möglicherweise müssen Sie diesen Befehl als Root-Benutzer ausführen.

Nun müssen Sie die Anmeldeinformationen zum Herstellen der Verbindung zwischen RabbitMQ und Azure abrufen.

## <a name="connecting-rabbitmq-to-azure-service-bus"></a>Herstellen einer Verbindung zwischen RabbitMQ und Azure Service Bus

Sie müssen für Ihre Warteschlange eine [SAS-Richtlinie](https://docs.microsoft.com/azure/storage/common/storage-sas-overview) erstellen, damit diese von RabbitMQ zum Veröffentlichen von Nachrichten verwendet werden kann. Mithilfe einer SAS-Richtlinie können Sie angeben, welche Aktionen eine externe Partei für Ihre Ressource durchführen darf. Damit soll sichergestellt werden, dass RabbitMQ Nachrichten senden, die Warteschlange jedoch nicht belauschen oder verwalten kann.

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/create-sas-policy.png" alt-text="SAS-Richtlinie hinzufügen":::

Aktivieren Sie das Kontrollkästchen `Send`, und klicken Sie auf `Create`, um unsere SAS-Richtlinie hinzuzufügen.

Nachdem Sie die Richtlinie erstellt haben, klicken Sie darauf, um die **primäre Verbindungszeichenfolge** anzuzeigen. Diese wird verwendet, damit RabbitMQ mit Azure Service Bus kommunizieren kann:

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/sas-policy-key.png" alt-text="SAS-Richtlinie abrufen":::

Diese Verbindungszeichenfolge können Sie erst verwenden, nachdem Sie sie in das AMQP-Verbindungsformat von RabbitMQ konvertiert haben. Wechseln Sie also zum [Verbindungszeichenfolgenkonverter](https://red-mushroom-0f7446a0f.azurestaticapps.net/), fügen Sie im Formular Ihre Verbindungszeichenfolge ein, und klicken Sie auf „Konvertieren“. Daraufhin erhalten Sie eine Verbindungszeichenfolge, die Sie für RabbitMQ verwenden können. (Auf dieser Website wird alles lokal in Ihrem Browser ausgeführt, d. h., Ihre Daten werden nicht versendet.) Sie können den entsprechenden Quellcode auf [GitHub](https://github.com/videlalvaro/connstring_to_amqp) aufrufen.

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/converter.png" alt-text="Verbindungszeichenfolge konvertieren":::

Öffnen Sie nun das RabbitMQ-Management-Plugin in einem unserer Browser [http://localhost:15672/#/dynamic-shovels](http://localhost:15672/#/dynamic-shovels), und wechseln Sie zu `Admin -> Shovel Management`. Hier können Sie die neue Shovel-Instanz hinzufügen, mit der Nachrichten aus einer RabbitMQ-Warteschlange an die Azure Service Bus-Warteschlange gesendet werden.

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/add-shovel.png" alt-text="RabbitMQ Shovel hinzufügen":::

Rufen Sie hier die Shovel-Instanz `azure` auf, und wählen Sie `AMQP 0.9.1` als Quellenprotokoll aus. Im Screenshot wird `amqp://` angezeigt. Hierbei handelt es sich um den Standard-URI, der eine Verbindung mit einem lokalen RabbitMQ-Server herstellt. Passen Sie diesen an Ihre aktuelle Bereitstellung an.

Auf der Seite der Warteschlange können Sie `azure` als Namen für Ihre Warteschlange verwenden. Wenn diese Warteschlange nicht vorhanden ist, wird sie von RabbitMQ erstellt. Sie können auch den Namen einer Warteschlange auswählen, die bereits vorhanden ist. Die anderen Optionen können Sie unverändert übernehmen.

Wählen Sie auf der `destination`-Seite `AMQP 1.0` als Protokoll aus. Geben Sie im Feld `URI` die Verbindungszeichenfolge ein, die Sie im vorherigen Schritt abgerufen haben, als Sie die Azure-Verbindungszeichenfolge in das RabbitMQ-Format konvertiert haben. Diese sollte wie folgt aussehen:

```
amqps://rabbitmq-shovel:StringOfRandomChars@rabbitmq.servicebus.windows.net:5671/?sasl=plain
```

Geben Sie im Feld `Address` den Namen der **Azure Service Bus-Warteschlange** ein. In unserem Beispiel lautet er `from-rabbitmq`. Klicken Sie auf `Add Shovel`. Danach können Sie mit Ihrer Einrichtung Nachrichten empfangen.

## <a name="publishing-messages-from-rabbitmq-to-azure-service-bus"></a>Veröffentlichen von Nachrichten auf RabbitMQ in Azure Service Bus

Wechseln Sie in der RabbitMQ-Management-Schnittstelle zu `Queues`. Wählen Sie die Warteschlange `azure` aus, und suchen Sie nach dem Bereich `Publish message`. Es wird ein Formular angezeigt, in dem Sie Nachrichten direkt in der Warteschlange veröffentlichen können. In unserem Beispiel fügen wir `fist message` als `Payload` hinzu und klicken auf `Publish Message`:

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/first-message.png" alt-text="Erste Nachricht veröffentlichen":::

Kehren Sie zu Azure zurück, und rufen Sie Ihre Warteschlange auf. Klicken Sie im linken Bereich auf `Service Bus Explorer`. Wenn alles ordnungsgemäß verlaufen ist, enthält Ihre Warteschlange nun zwei Nachrichten. Glückwunsch!

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/service-bus-queue.png" alt-text="Azure Service Bus-Warteschlange":::

Überprüfen Sie, ob es sich bei der Nachricht tatsächlich um die Nachricht handelt, die Sie über RabbitMQ gesendet haben. Wählen Sie die Registerkarte `Peek` aus, und klicken Sie auf die Schaltfläche `Peek`, um die letzten Nachrichten in der Warteschlange abzurufen. Klicken Sie auf die Nachricht, um deren Inhalt zu überprüfen. Die Anzeige sollte wie in der folgenden Abbildung aussehen und Ihre `first message` sollte angezeigt werden.

:::image type="content" source="./media/service-bus-integrate-with-rabbitmq/peek.png" alt-text="Warteschlange einsehen":::

## <a name="lets-recap"></a>Zusammenfassung

Glückwunsch! Sie haben viel erreicht! Sie haben Ihre Nachrichten mit den folgenden Schritten von RabbitMQ an Azure Service Bus gesendet:

1. Erstellen eines Azure Service Bus-Namespace
2. Hinzufügen einer Warteschlange zum Namespace
3. Hinzufügen einer SAS-Richtlinie zu Ihrer Warteschlange
4. Abrufen der Verbindungszeichenfolge für die Warteschlange
5. Aktivieren des RabbitMQ Shovel-Plugins und der Management-Schnittstelle
6. Konvertieren der Azure Service Bus-Verbindungszeichenfolge in das AMQP-Format von RabbitMQ
7. Hinzufügen einer neuen Shovel-Instanz zu RabbitMQ und Herstellen einer Verbindung mit Azure Service Bus
8. Veröffentlichen von Nachrichten

Nachdem Sie die vorherigen Schritte ausgeführt haben, haben Sie Bereiche Ihrer Organisation integriert, die außerhalb von Azure liegen. Mithilfe des Shovel-Plugins konnten Sie Nachrichten von RabbitMQ an Azure Service Bus übertragen. Dies hat enorme Vorteile, da Sie jetzt vertrauenswürdigen Drittanbietern gestatten können, ihre Apps mit Ihrer Azure-Bereitstellung zu verbinden.

Am Ende geht es beim Messaging um das Aktivieren von Verbindungen. Mit dieser Technik haben wir gerade eine neue Möglichkeit eröffnet.
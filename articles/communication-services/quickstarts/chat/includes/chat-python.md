---
title: include file
description: include file
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 9/1/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 0225c948fddf65b9312c689144ecc567a70aa27e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750307"
---
## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

- Erstellen Sie ein Azure-Konto mit einem aktiven Abonnement. Details finden Sie auf der [Seite zum Erstellen eines kostenloses Azure-Kontos](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- [Installieren Sie Python.](https://www.python.org/downloads/)
- Erstellen Sie eine Azure Communication Services-Ressource. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure Communication Services-Ressource](../../create-communication-resource.md). Für diese Schnellstartanleitung müssen Sie den **Endpunkt** Ihrer Ressource aufzeichnen.
- Ein [Benutzerzugriffstoken](../../access-tokens.md). Achten Sie darauf, dass Sie den Bereich auf „Chat“ festlegen, und notieren Sie sich die Tokenzeichenfolge und die userId-Zeichenfolge.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Öffnen Sie Ihr Terminal- bzw. Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und greifen Sie auf das Verzeichnis zu.

```console
mkdir chat-quickstart && cd chat-quickstart
```

Verwenden Sie einen Text-Editor zum Erstellen einer Datei mit dem Namen **start-chat.py** im Stammverzeichnis des Projekts, und fügen Sie die Struktur für das Programm hinzu, z. B. eine einfache Ausnahmebehandlung. In den folgenden Abschnitten fügen Sie der Datei den gesamten Quellcode für diese Schnellstartanleitung hinzu.

```python
import os
# Add required client library components from quickstart here

try:
    print('Azure Communication Services - Chat Quickstart')
    # Quickstart code goes here
except Exception as ex:
    print('Exception:')
    print(ex)
```

### <a name="install-client-library"></a>Installieren der Clientbibliothek

```console

pip install azure-communication-chat

```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen werden für einige der wichtigsten Features der Python-Clientbibliothek für Chats von Azure Communication Services verwendet.

| Name                                  | Beschreibung                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Diese Klasse wird für die Chatfunktionalität benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Erstellen, Abrufen und Löschen von Threads. |
| ChatThreadClient | Diese Klasse wird für die Chatthreadfunktionalität benötigt. Sie rufen eine Instanz über den ChatClient ab und verwenden sie zum Senden/Empfangen/Aktualisieren/Löschen von Nachrichten, Hinzufügen/Entfernen/Abrufen von Benutzern und Senden von Eingabebenachrichtigungen und Lesebestätigungen. |

## <a name="create-a-chat-client"></a>Erstellen eines Chatclients

Zum Erstellen eines Chatclients verwenden Sie den Communication Services-Endpunkt und das `Access Token` (Zugriffstoken), das im Rahmen der Schritte zur Erfüllung der Voraussetzungen generiert wurde. Informieren Sie sich über [Benutzerzugriffstoken](../../access-tokens.md).

Die Erstellung einer Dienstebene zum Verwalten von Token für Ihre Chatanwendung wird zwar empfohlen, aber in dieser Schnellstartanleitung nicht behandelt. Ausführlichere Informationen finden Sie in der Dokumentation zur [Chatarchitektur](../../../concepts/chat/concepts.md).

```console
pip install azure-communication-administration
```

```python
from azure.communication.chat import ChatClient, CommunicationTokenCredential, CommunicationTokenRefreshOptions

endpoint = "https://<RESOURCE_NAME>.communication.azure.com"
refresh_options = CommunicationTokenRefreshOptions(<Access Token>)
chat_client = ChatClient(endpoint, CommunicationTokenCredential(refresh_options))
```

## <a name="start-a-chat-thread"></a>Starten eines Chatthreads

Verwenden Sie die `create_chat_thread`-Methode, um einen Chatthread zu erstellen.

- Verwenden Sie `topic`, um ein Threadthema anzugeben. Nach der Erstellung des Chatthreads kann das Thema mit der Funktion `update_thread` aktualisiert werden.
- Verwenden Sie `thread_participants`, um das `ChatThreadParticipant`-Element aufzulisten, das dem Chatthread hinzugefügt werden soll. Für `ChatThreadParticipant` wird für `CommunicationUserIdentifier` der Typ `user` verwendet. Dies war das Ergebnis, das Sie unter [Erstellen eines Benutzers](../../access-tokens.md#create-an-identity) erhalten haben.
- Verwenden Sie `repeatability_request_id`, um anzugeben, dass die Anforderung wiederholbar ist. Der Client kann die Anforderung mehrmals mit der gleichen Wiederholbarkeitsanforderungs-ID ausführen und eine geeignete Antwort erhalten, ohne dass der Server die Anforderung mehrmals ausführt.

Die Antwort `chat_thread_client` wird verwendet, um Vorgänge für den neu erstellten Chatthread durchzuführen, beispielsweise Hinzufügen von Teilnehmern zum Chatthread, Senden einer Nachricht, Löschen einer Nachricht oder Ähnliches. Sie enthält die `thread_id`-Eigenschaft, bei der es sich um die eindeutige ID des Chatthreads handelt.

#### <a name="without-repeatability-request-id"></a>Ohne Wiederholbarkeitsanforderungs-ID
```python
from datetime import datetime
from azure.communication.chat import ChatThreadParticipant

topic="test topic"
participants = [ChatThreadParticipant(
    user=user,
    display_name='name',
    share_history_time=datetime.utcnow()
)]

chat_thread_client = chat_client.create_chat_thread(topic, participants)
```

#### <a name="with-repeatability-request-id"></a>Mit Wiederholbarkeitsanforderungs-ID
```python
from datetime import datetime
from azure.communication.chat import ChatThreadParticipant

topic="test topic"
participants = [ChatThreadParticipant(
    user=user,
    display_name='name',
    share_history_time=datetime.utcnow()
)]

repeatability_request_id = 'b66d6031-fdcc-41df-8306-e524c9f226b8' # some unique identifier
chat_thread_client = chat_client.create_chat_thread(topic, participants, repeatability_request_id)
```

## <a name="get-a-chat-thread-client"></a>Abrufen eines Clients für den Chatthread
Die `get_chat_thread_client`-Methode gibt einen Threadclient für einen bereits vorhandenen Thread zurück. Er kann verwendet werden, um Vorgänge wie das Hinzufügen von Teilnehmern oder das Senden einer Nachricht im erstellten Thread auszuführen. „thread_id“ ist die eindeutige ID des vorhandenen Chatthreads.

```python
thread_id = chat_thread_client.thread_id
chat_thread_client = chat_client.get_chat_thread_client(thread_id)
```

## <a name="list-all-chat-threads"></a>Auflisten aller Chatthreads
Von der Methode `list_chat_threads` wird ein Iterator vom Typ `ChatThreadInfo` zurückgegeben. Sie kann zum Auflisten aller Chatthreads verwendet werden.

- Verwenden Sie `start_time`, um den frühesten Zeitpunkt anzugeben, bis zu dem Chatthreads abgerufen werden sollen.
- Verwenden Sie `results_per_page`, um die maximale Anzahl von Chatthreads anzugeben, die pro Seite zurückgegeben werden sollen.

```python
from datetime import datetime, timedelta
import pytz

start_time = datetime.utcnow() - timedelta(days=2)
start_time = start_time.replace(tzinfo=pytz.utc)
chat_thread_infos = chat_client.list_chat_threads(results_per_page=5, start_time=start_time)

for chat_thread_info_page in chat_thread_infos.by_page():
    for chat_thread_info in chat_thread_info_page:
        # Iterate over all chat threads
        print("thread id:", chat_thread_info.id)
```

## <a name="delete-a-chat-thread"></a>Löschen eines Chatthreads
`delete_chat_thread` wird verwendet, um einen Chatthread zu löschen.

- Verwenden Sie `thread_id`, um die Thread-ID eines vorhandenen Chatthreads anzugeben, der gelöscht werden soll.

```python
thread_id = chat_thread_client.thread_id
chat_client.delete_chat_thread(thread_id)
```

## <a name="send-a-message-to-a-chat-thread"></a>Senden einer Nachricht an einen Chatthread

Verwenden Sie die Methode `send_message`, um eine Nachricht an einen per Thread-ID identifizierten Chatthread zu senden, den Sie gerade erstellt haben.

- Verwenden Sie `content`, um den Inhalt der Chatnachricht anzugeben.
- Verwenden Sie `chat_message_type`, um den Inhaltstyp der Nachricht anzugeben. Mögliche Werte sind „text“ und „html“. Ohne Angabe wird der Standardwert „text“ zugewiesen.
- Verwenden Sie `sender_display_name`, um den Anzeigenamen des Absenders anzugeben.

Die Antwort ist eine ID vom Typ `str`, bei der es sich um die eindeutige ID der Nachricht handelt.

#### <a name="message-type-not-specified"></a>Ohne angegebenen Nachrichtentyp
```python
chat_thread_client = chat_client.create_chat_thread(topic, participants)

content='hello world'
sender_display_name='sender name'

send_message_result_id = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name)
```

#### <a name="message-type-specified"></a>Mit angegebenem Nachrichtentyp
```python
from azure.communication.chat import ChatMessageType

content='hello world'
sender_display_name='sender name'

# specify chat message type with pre-built enumerations
send_message_result_id_w_enum = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name, chat_message_type=ChatMessageType.TEXT)

# specify chat message type as string
send_message_result_id_w_str = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name, chat_message_type='text')
```

## <a name="get-a-specific-chat-message-from-a-chat-thread"></a>Abrufen einer bestimmten Chatnachricht aus einem Chatthread
Mithilfe der Funktion `get_message` können Sie eine bestimmte, per Nachrichten-ID identifizierte Nachricht abrufen.

- Verwenden Sie `message_id`, um die Nachrichten-ID anzugeben.

Die Antwort vom Typ `ChatMessage` enthält alle Informationen, die mit der einzelnen Nachricht zusammenhängen.

```python
message_id = send_message_result_id
chat_message = chat_thread_client.get_message(message_id)
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Empfangen von Chatnachrichten aus einem Chatthread

Sie können Chatnachrichten abrufen, indem Sie die `list_messages`-Methode jeweils nach dem angegebenen Intervallzeitraum abfragen.

- Verwenden Sie `results_per_page`, um die maximale Anzahl von Nachrichten anzugeben, die pro Seite zurückgegeben werden sollen.
- Verwenden Sie `start_time`, um den frühesten Zeitpunkt anzugeben, bis zu dem Nachrichten abgerufen werden sollen.

```python
chat_messages = chat_thread_client.list_messages(results_per_page=1, start_time=start_time)
for chat_message_page in chat_messages.by_page():
    for chat_message in chat_message_page:
        print('ChatMessage: ', chat_message)
        print('ChatMessage: ', chat_message.content.message)
```

`list_messages` gibt die aktuelle Version der Nachricht zurück, einschließlich aller Bearbeitungen oder Löschungen, die für die Nachricht mit `update_message` und `delete_message` durchgeführt wurden. Für gelöschte Nachrichten wird von `ChatMessage.deleted_on` ein datetime-Wert zurückgegeben, mit dem der Löschzeitpunkt der Nachricht angegeben wird. Für bearbeitete Nachrichten gibt `ChatMessage.edited_on` einen datetime-Wert zurück, mit dem der Bearbeitungszeitpunkt der Nachricht angegeben wird. Auf den ursprünglichen Zeitpunkt der Nachrichtenerstellung kann mit `ChatMessage.created_on` zugegriffen werden. Dieses Element kann zum Sortieren der Nachrichten genutzt werden.

Mit `list_messages` werden unterschiedliche Nachrichtentypen zurückgegeben, die mit `ChatMessage.type` identifiziert werden können. Diese Typen lauten:

- `ChatMessageType.TEXT`: Reguläre Chatnachricht, die von einem Threadteilnehmer gesendet wurde

- `ChatMessageType.HTML`: HTML-Chatnachricht, die von einem Threadteilnehmer gesendet wurde

- `ChatMessageType.TOPIC_UPDATED`: Systemnachricht, die angibt, dass das Thema aktualisiert wurde

- `ChatMessageType.PARTICIPANT_ADDED`: Systemnachricht mit dem Hinweis, dass dem Chatthread mindestens ein Teilnehmer hinzugefügt wurde

- `ChatMessageType.PARTICIPANT_REMOVED`: Systemnachricht mit dem Hinweis, dass ein Teilnehmer aus dem Chatthread entfernt wurde

Weitere Details finden Sie unter [Nachrichtentypen](../../../concepts/chat/concepts.md#message-types).

## <a name="update-topic-of-a-chat-thread"></a>Aktualisieren des Themas eines Chatthreads
Das Thema eines Chatthreads kann mithilfe der Methode `update_topic` aktualisiert werden.

```python
topic = "updated thread topic"
chat_thread_client.update_topic(topic=topic)
updated_topic = chat_client.get_chat_thread(chat_thread_client.thread_id).topic
print('Updated topic: ', updated_topic)
```

## <a name="update-a-message"></a>Aktualisieren einer Nachricht
Mithilfe der Methode `update_message` können Sie den Inhalt einer vorhandenen, per Nachrichten-ID identifizierten Nachricht aktualisieren.

- Verwenden Sie `message_id`, um die Nachrichten-ID anzugeben.
- Verwenden Sie `content`, um den neuen Inhalt der Nachricht festzulegen.

```python
content = 'Hello world!'
send_message_result_id = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name)

content = 'Hello! I am updated content'
chat_thread_client.update_message(message_id=send_message_result_id, content=content)

chat_message = chat_thread_client.get_message(send_message_result_id)
print('Updated message content: ', chat_message.content.message)
```

## <a name="send-read-receipt-for-a-message"></a>Senden einer Lesebestätigung für eine Nachricht
Die Methode `send_read_receipt` kann verwendet werden, um im Auftrag eines Benutzers ein Lesebestätigungsereignis in einem Thread zu veröffentlichen.

- Verwenden Sie `message_id`, um die ID der letzten vom aktuellen Benutzer gelesenen Nachricht anzugeben.

```python
message_id=send_message_result_id
chat_thread_client.send_read_receipt(message_id=message_id)
```

## <a name="list-read-receipts-for-a-chat-thread"></a>Auflisten der Lesebestätigungen für einen Chatthread
Mithilfe der Methode `list_read_receipts` können Lesebestätigungen für einen Thread abgerufen werden.

- Verwenden Sie `results_per_page`, um die maximale Anzahl von Lesebestätigungen für Chatnachrichten anzugeben, die pro Seite zurückgegeben werden sollen.
- Verwenden Sie `skip`, um anzugeben, dass Lesebestätigungen von Chatnachrichten in der Antwort bis zu einer bestimmten Position übersprungen werden sollen.

```python
read_receipts = chat_thread_client.list_read_receipts(results_per_page=2, skip=0)

for read_receipt_page in read_receipts.by_page():
    for read_receipt in read_receipt_page:
        print('ChatMessageReadReceipt: ', read_receipt)
```

## <a name="send-typing-notification"></a>Senden einer Eingabebenachrichtigung
Die Methode `send_typing_notification` kann verwendet werden, um im Auftrag eines Benutzers ein Eingabeereignis in einem Thread zu veröffentlichen.

```python
chat_thread_client.send_typing_notification()
```

## <a name="delete-message"></a>Löschen einer Nachricht
Mithilfe der Methode `delete_message` können Sie eine per Nachrichten-ID identifizierte Nachricht löschen.

- Verwenden Sie `message_id`, um die Nachrichten-ID anzugeben.

```python
message_id=send_message_result_id
chat_thread_client.delete_message(message_id=message_id)
```

## <a name="add-a-user-as-participant-to-the-chat-thread"></a>Hinzufügen eines Benutzers als Teilnehmer des Chatthreads

Nach der Erstellung eines Chatthreads können Sie dafür Benutzer hinzufügen und entfernen. Durch Hinzufügen von Benutzern gewähren Sie ihnen Zugriff zum Senden von Nachrichten an den Chatthread und zum Hinzufügen/Entfernen anderer Teilnehmer. Stellen Sie vor dem Aufrufen der `add_participant`-Methode sicher, dass Sie ein neues Zugriffstoken und eine Identität für den Benutzer beschafft haben. Der Benutzer benötigt dieses Zugriffstoken, um den Chatclient initialisieren zu können.

Verwenden Sie die Methode `add_participant`, um dem per Thread-ID identifizierten Thread Teilnehmer hinzuzufügen.

- Verwenden Sie `thread_participant`, um den Teilnehmer anzugeben, der dem Chatthread hinzugefügt werden soll.
- `user` (erforderlich) ist der `CommunicationUserIdentifier`, den Sie mit `CommunicationIdentityClient` unter [Erstellen eines Benutzers](../../access-tokens.md#create-an-identity) erstellt haben.
- `display_name` (optional) ist der Anzeigename für den Threadteilnehmer.
- `share_history_time` (optional) ist der Zeitpunkt, ab dem der Chatverlauf für den Teilnehmer freigegeben wird. Sie können den Verlauf seit dem Beginn des Chatthreads freigeben, indem Sie diese Eigenschaft auf das Datum der Threaderstellung (oder früher) festlegen. Soll der Verlauf vor dem Hinzufügezeitpunkt des Teilnehmers nicht freigegeben werden, geben Sie das aktuelle Datum an. Falls Sie einen Teil des Verlaufs freigeben möchten, können Sie ein Datum angeben, das zwischen diesen Zeitpunkten liegt.

```python
new_user = identity_client.create_user()

from azure.communication.chat import ChatThreadParticipant
from datetime import datetime

new_chat_thread_participant = ChatThreadParticipant(
    user=new_user,
    display_name='name',
    share_history_time=datetime.utcnow())

chat_thread_client.add_participant(new_chat_thread_participant)
```

Mithilfe der Methode `add_participants` können dem Chatthread auch mehrere Benutzer hinzugefügt werden, sofern für alle Benutzer ein neues Zugriffstoken und eine Identifizierung verfügbar ist.

```python
from azure.communication.chat import ChatThreadParticipant
from datetime import datetime

new_chat_thread_participant = ChatThreadParticipant(
        user=self.new_user,
        display_name='name',
        share_history_time=datetime.utcnow())
thread_participants = [new_chat_thread_participant] # instead of passing a single participant, you can pass a list of participants
chat_thread_client.add_participants(thread_participants)
```


## <a name="remove-user-from-a-chat-thread"></a>Entfernen eines Benutzers aus einem Chatthread

Teilnehmer können auch aus einem Thread entfernt werden. Die Vorgehensweise ist dabei ähnlich wie beim Hinzufügen. Zum Entfernen benötigen Sie die IDs der von Ihnen hinzugefügten Teilnehmer.

Verwenden Sie die Methode `remove_participant`, um den per Thread-ID identifizierten Threadteilnehmer aus dem Thread zu entfernen.
- `user` ist der Bezeichner des Kommunikationsbenutzers (`CommunicationUserIdentifier`), der aus dem Thread entfernt werden soll.

```python
chat_thread_client.remove_participant(new_user)
```

## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie die Anwendung mit dem Befehl `python` aus dem Anwendungsverzeichnis aus.

```console
python start-chat.py
```

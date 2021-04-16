---
title: include file
description: include file
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 31704e705b828cc0070e3b79f5d527cfa9deb0c3
ms.sourcegitcommit: dddd1596fa368f68861856849fbbbb9ea55cb4c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107386881"
---
[!INCLUDE [Public Preview Notice](../../../includes/public-preview-include-chat.md)]

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
# Add required SDK components from quickstart here

try:
    print('Azure Communication Services - Chat Quickstart')
    # Quickstart code goes here
except Exception as ex:
    print('Exception:')
    print(ex)
```

### <a name="install-sdk"></a>Installieren des SDK

```console

pip install azure-communication-chat

```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen werden für einige der wichtigsten Features des Chat-SDKs von Azure Communication Services für Python verwendet.

| Name                                  | Beschreibung                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Diese Klasse wird für die Chatfunktionalität benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Erstellen, Abrufen und Löschen von Threads. |
| ChatThreadClient | Diese Klasse wird für die Chatthreadfunktionalität benötigt. Sie rufen eine Instanz über den ChatClient ab und verwenden sie zum Senden/Empfangen/Aktualisieren/Löschen von Nachrichten, Hinzufügen/Entfernen/Abrufen von Benutzern und Senden von Eingabebenachrichtigungen und Lesebestätigungen. |

## <a name="create-a-chat-client"></a>Erstellen eines Chatclients

Zum Erstellen eines Chatclients verwenden Sie den Communication Services-Endpunkt und das `Access Token` (Zugriffstoken), das im Rahmen der Schritte zur Erfüllung der Voraussetzungen generiert wurde. Informieren Sie sich über [Benutzerzugriffstoken](../../access-tokens.md).

Die Erstellung einer Dienstebene zum Verwalten von Token für Ihre Chatanwendung wird zwar empfohlen, aber in dieser Schnellstartanleitung nicht behandelt. Ausführlichere Informationen finden Sie in der Dokumentation zur [Chatarchitektur](../../../concepts/chat/concepts.md).

```console
pip install azure-communication-identity
```

```python
from azure.communication.chat import ChatClient, CommunicationTokenCredential

endpoint = "https://<RESOURCE_NAME>.communication.azure.com"
chat_client = ChatClient(endpoint, CommunicationTokenCredential("<Access Token>"))
```

## <a name="start-a-chat-thread"></a>Starten eines Chatthreads

Verwenden Sie die `create_chat_thread`-Methode, um einen Chatthread zu erstellen.

- Verwenden Sie `topic`, um ein Threadthema anzugeben. Nach der Erstellung des Chatthreads kann das Thema mit der Funktion `update_thread` aktualisiert werden.
- Verwenden Sie `thread_participants`, um das `ChatParticipant`-Element aufzulisten, das dem Chatthread hinzugefügt werden soll. Für `ChatParticipant` wird für `CommunicationUserIdentifier` der Typ `user` verwendet. Dies war das Ergebnis, das Sie unter [Erstellen eines Benutzers](../../access-tokens.md#create-an-identity) erhalten haben.

`CreateChatThreadResult` ist das Ergebnis, das beim Erstellen eines Threads zurückgegeben wurde. Sie können es verwenden, um die ID (`id`) des erstellten Chatthreads abzurufen. Diese ID (`id`) kann dann zum Abrufen eines `ChatThreadClient`-Objekts mithilfe der Methode `get_chat_thread_client` verwendet werden. Mit `ChatThreadClient` können weitere Chatvorgänge für diesen Chatthread ausgeführt werden.

```python
topic="test topic"

create_chat_thread_result = chat_client.create_chat_thread(topic)
chat_thread_client = chat_client.get_chat_thread_client(create_chat_thread_result.chat_thread.id)
```

## <a name="get-a-chat-thread-client"></a>Abrufen eines Clients für den Chatthread
Die `get_chat_thread_client`-Methode gibt einen Threadclient für einen bereits vorhandenen Thread zurück. Er kann verwendet werden, um Vorgänge wie das Hinzufügen von Teilnehmern oder das Senden einer Nachricht im erstellten Thread auszuführen. „thread_id“ ist die eindeutige ID des vorhandenen Chatthreads.

Mit `ChatThreadClient` können weitere Chatvorgänge für diesen Chatthread ausgeführt werden.

```python
thread_id = create_chat_thread_result.chat_thread.id
chat_thread_client = chat_client.get_chat_thread_client(thread_id)
```


## <a name="list-all-chat-threads"></a>Auflisten aller Chatthreads
Von der Methode `list_chat_threads` wird ein Iterator vom Typ `ChatThreadItem` zurückgegeben. Sie kann zum Auflisten aller Chatthreads verwendet werden.

- Verwenden Sie `start_time`, um den frühesten Zeitpunkt anzugeben, bis zu dem Chatthreads abgerufen werden sollen.
- Verwenden Sie `results_per_page`, um die maximale Anzahl von Chatthreads anzugeben, die pro Seite zurückgegeben werden sollen.

Beim Auflisten von Threads wird als Antwort ein Iterator vom Typ `[ChatThreadItem]` zurückgegeben.

```python
from datetime import datetime, timedelta

start_time = datetime.utcnow() - timedelta(days=2)

chat_threads = chat_client.list_chat_threads(results_per_page=5, start_time=start_time)
for chat_thread_item_page in chat_threads.by_page():
    for chat_thread_item in chat_thread_item_page:
        print(chat_thread_item)
        print('Chat Thread Id: ', chat_thread_item.id)
```


## <a name="send-a-message-to-a-chat-thread"></a>Senden einer Nachricht an einen Chatthread

Verwenden Sie die Methode `send_message`, um eine Nachricht an einen per Thread-ID identifizierten Chatthread zu senden, den Sie gerade erstellt haben.

- Verwenden Sie `content`, um den Inhalt der Chatnachricht anzugeben.
- Verwenden Sie `chat_message_type`, um den Inhaltstyp der Nachricht anzugeben. Mögliche Werte sind „text“ und „html“. Ohne Angabe wird der Standardwert „text“ zugewiesen.
- Verwenden Sie `sender_display_name`, um den Anzeigenamen des Absenders anzugeben.

`SendChatMessageResult` ist die Antwort, die nach dem Senden einer Nachricht zurückgegeben wird. Sie enthält eine ID, bei der es sich um die eindeutige ID der Nachricht handelt.

```python
from azure.communication.chat import ChatMessageType

topic = "test topic"
create_chat_thread_result = chat_client.create_chat_thread(topic)
thread_id = create_chat_thread_result.chat_thread.id
chat_thread_client = chat_client.get_chat_thread_client(create_chat_thread_result.chat_thread.id)


content='hello world'
sender_display_name='sender name'

# specify chat message type with pre-built enumerations
send_message_result_w_enum = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name, chat_message_type=ChatMessageType.TEXT)
print("Message sent: id: ", send_message_result_w_enum.id)
```


## <a name="receive-chat-messages-from-a-chat-thread"></a>Empfangen von Chatnachrichten aus einem Chatthread

Sie können Chatnachrichten abrufen, indem Sie die `list_messages`-Methode jeweils nach dem angegebenen Intervallzeitraum abfragen.

- Verwenden Sie `results_per_page`, um die maximale Anzahl von Nachrichten anzugeben, die pro Seite zurückgegeben werden sollen.
- Verwenden Sie `start_time`, um den frühesten Zeitpunkt anzugeben, bis zu dem Nachrichten abgerufen werden sollen.

Beim Auflisten von Nachrichten wird als Antwort ein Iterator vom Typ `[ChatMessage]` zurückgegeben.

```python
from datetime import datetime, timedelta

start_time = datetime.utcnow() - timedelta(days=1)

chat_messages = chat_thread_client.list_messages(results_per_page=1, start_time=start_time)
for chat_message_page in chat_messages.by_page():
    for chat_message in chat_message_page:
        print("ChatMessage: Id=", chat_message.id, "; Content=", chat_message.content.message)
```

`list_messages` gibt die aktuelle Version der Nachricht zurück, einschließlich aller Bearbeitungen oder Löschungen, die für die Nachricht mit `update_message` und `delete_message` durchgeführt wurden. Für gelöschte Nachrichten wird von `ChatMessage.deleted_on` ein datetime-Wert zurückgegeben, mit dem der Löschzeitpunkt der Nachricht angegeben wird. Für bearbeitete Nachrichten gibt `ChatMessage.edited_on` einen datetime-Wert zurück, mit dem der Bearbeitungszeitpunkt der Nachricht angegeben wird. Auf den ursprünglichen Zeitpunkt der Nachrichtenerstellung kann mit `ChatMessage.created_on` zugegriffen werden. Dieses Element kann zum Sortieren der Nachrichten genutzt werden.

Mit `list_messages` werden unterschiedliche Nachrichtentypen zurückgegeben, die mit `ChatMessage.type` identifiziert werden können. 

Weitere Informationen zu Nachrichtentypen finden Sie hier: [Nachrichtentypen](../../../concepts/chat/concepts.md#message-types).

## <a name="send-read-receipt"></a>Senden einer Lesebestätigung
Die Methode `send_read_receipt` kann verwendet werden, um im Auftrag eines Benutzers ein Lesebestätigungsereignis in einem Thread zu veröffentlichen.

- Verwenden Sie `message_id`, um die ID der letzten vom aktuellen Benutzer gelesenen Nachricht anzugeben.

```python
content='hello world'

send_message_result = chat_thread_client.send_message(content)
chat_thread_client.send_read_receipt(message_id=send_message_result.id)
```


## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Hinzufügen eines Benutzers als Teilnehmer des Chatthreads

Nach der Erstellung eines Chatthreads können Sie dafür Benutzer hinzufügen und entfernen. Durch Hinzufügen von Benutzern gewähren Sie ihnen Zugriff zum Senden von Nachrichten an den Chatthread und zum Hinzufügen/Entfernen anderer Teilnehmer. Stellen Sie vor dem Aufrufen der `add_participants`-Methode sicher, dass Sie ein neues Zugriffstoken und eine Identität für den Benutzer beschafft haben. Der Benutzer benötigt dieses Zugriffstoken, um den Chatclient initialisieren zu können.

Mithilfe der Methode `add_participants` können zum Chatthread einer oder mehrere Benutzer hinzugefügt werden, sofern für alle Benutzer ein neues Zugriffstoken und eine Identifizierung verfügbar ist.

Ein `list(tuple(ChatParticipant, CommunicationError))` wird zurückgegeben. Wenn der Teilnehmer erfolgreich hinzugefügt wurde, wird eine leere Liste erwartet. Tritt beim Hinzufügen des Teilnehmers ein Fehler auf, wird die Liste mit den Teilnehmern, bei denen ein Fehler auftrat, sowie den aufgetretenen Fehler aufgefüllt.

```python
from azure.communication.identity import CommunicationIdentityClient
from azure.communication.chat import ChatParticipant
from datetime import datetime

# create 2 users
identity_client = CommunicationIdentityClient.from_connection_string('<connection_string>')
new_users = [identity_client.create_user() for i in range(2)]

# # conversely, you can also add an existing user to a chat thread; provided the user_id is known
# from azure.communication.identity import CommunicationUserIdentifier
#
# user_id = 'some user id'
# user_display_name = "Wilma Flinstone"
# new_user = CommunicationUserIdentifier(user_id)
# participant = ChatParticipant(
#     user=new_user,
#     display_name=user_display_name,
#     share_history_time=datetime.utcnow())

participants = []
for _user in new_users:
  chat_thread_participant = ChatParticipant(
    user=_user,
    display_name='Fred Flinstone',
    share_history_time=datetime.utcnow()
  ) 
  participants.append(chat_thread_participant) 

response = chat_thread_client.add_participants(participants)

def decide_to_retry(error, **kwargs):
    """
    Insert some custom logic to decide if retry is applicable based on error
    """
    return True

# verify if all users has been successfully added or not
# in case of partial failures, you can retry to add all the failed participants 
retry = [p for p, e in response if decide_to_retry(e)]
if retry:
    chat_thread_client.add_participants(retry)
```


## <a name="list-thread-participants-in-a-chat-thread"></a>Liste der Threadteilnehmer eines Chatthreads

Threadteilnehmer können aufgelistet werden. Die Vorgehensweise ist dabei ähnlich wie beim Hinzufügen.

Verwenden Sie `list_participants`, um die Teilnehmer des Threads abzurufen.
- Verwenden Sie optional `results_per_page`: die maximale Anzahl von Teilnehmern, die pro Seite zurückgegeben werden sollen.
- Verwenden Sie `skip` optional, um die Teilnehmer bis zu angegebenen Position in der Antwort zu überspringen.

Beim Auflisten von Teilnehmern wird als Antwort ein Iterator vom Typ `[ChatParticipant]` zurückgegeben.

```python
chat_thread_participants = chat_thread_client.list_participants()
for chat_thread_participant_page in chat_thread_participants.by_page():
    for chat_thread_participant in chat_thread_participant_page:
        print("ChatParticipant: ", chat_thread_participant)
```

## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie die Anwendung mit dem Befehl `python` aus dem Anwendungsverzeichnis aus.

```console
python start-chat.py
```

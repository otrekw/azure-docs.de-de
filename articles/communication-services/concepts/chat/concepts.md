---
title: Chatkonzepte in Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Erfahren Sie mehr über Chatkonzepte in Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 2360cc8d202ed29051551231d14bef69c0e66ce4
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657807"
---
# <a name="chat-concepts"></a>Chatkonzepte

Die Clientbibliotheken für Chats von Azure Communication Services können verwendet werden, um Echtzeittextchat zu Ihren Anwendungen hinzuzufügen. Auf dieser Seite werden wichtige Chatkonzepte und -funktionen zusammengefasst.

Weitere Informationen zu den einzelnen Sprachen und Funktionen der Clientbibliothek finden Sie in der [Übersicht über die Clientbibliothek für Chats von Communication Services](./sdk-features.md).

## <a name="chat-overview"></a>Übersicht über den Chat

Chatkonversationen erfolgen innerhalb von Chatthreads. Ein Chatthread kann viele Nachrichten und Benutzer umfassen. Jede Nachricht gehört zu einem einzelnen Thread, und ein Benutzer kann einem oder mehreren Threads angehören.

Die einzelnen Benutzer im Chatthread werden als Mitglieder bezeichnet. In einem Chatthread sind bis zu 250 Mitglieder möglich. Nur Threadmitglieder können Nachrichten senden und empfangen oder Mitglieder zu einem Chatthread hinzufügen bzw. daraus entfernen. Die maximal zulässige Nachrichtengröße beträgt ca. 28 KB. Mithilfe des `List/Get Messages`-Vorgangs können Sie alle Nachrichten in einem Chatthread abrufen. Communication Services speichert den Chatverlauf, bis Sie einen Löschvorgang für den Chatthread oder die Nachricht ausführen oder bis keine Mitglieder mehr im Chatthread verbleiben – an diesem Punkt ist er verwaist und wird für die Löschung verarbeitet.

Für Chatthreads mit mehr als 20 Mitgliedern sind Lesebestätigungen und Eingabeindikatorfunktionen deaktiviert.

## <a name="chat-architecture"></a>Chatarchitektur

Die Chatarchitektur besteht aus zwei zentralen Teilen: 1) einem vertrauenswürdigen Dienst und 2) einer Clientanwendung.

:::image type="content" source="../../media/chat-architecture.png" alt-text="Diagramm, das die Chatarchitektur von Communication Services zeigt":::

 - **Vertrauenswürdiger Dienst:** Damit Sie eine Chatsitzung ordnungsgemäß verwalten können, benötigen Sie einen Dienst, der Ihnen hilft, mithilfe ihrer Ressourcen-Verbindungszeichenfolge eine Verbindung mit Communication Services herzustellen. Dieser Dienst ist für das Erstellen von Chatthreads, das Verwalten von Threadmitgliedschaften und das Bereitstellen von Zugriffstoken für Benutzer verantwortlich. Weitere Informationen zu Zugriffstoken finden Sie in unserem Schnellstart zu [Zugriffstoken](../../quickstarts/access-tokens.md).

 - **Client-App:**  Die Clientanwendung stellt eine Verbindung mit Ihrem vertrauenswürdigen Dienst her und empfängt die Zugriffstoken, die zum Herstellen einer direkten Verbindung mit Communication Services verwendet werden. Nachdem diese Verbindung hergestellt wurde, kann Ihre Client-App Nachrichten senden und empfangen.

Es wird empfohlen, Zugriffstoken unter Verwendung der vertrauenswürdigen Dienstebene zu erstellen. In diesem Szenario ist die Serverseite für die Erstellung und Verwaltung von Benutzern sowie für die Ausstellung ihrer Token zuständig.

## <a name="message-types"></a>Nachrichtentypen

Ein Communication Services-Chat teilt vom Benutzer sowie vom System generierte Nachrichten, die als **Threadaktivitäten** bezeichnet werden. Threadaktivitäten werden generiert, wenn ein Chatthread aktualisiert wird. Wenn Sie `List Messages` oder `Get Messages` in einem Chatthread aufrufen, enthält das Ergebnis die vom Benutzer generierten Textnachrichten sowie die Systemnachrichten in chronologischer Reihenfolge. Auf diese Weise können Sie ermitteln, wann ein Mitglied hinzugefügt oder entfernt wurde oder wann das Thema des Chatthreads aktualisiert wurde. Die folgende Nachrichtentypen werden unterstützt:

 - `Text`: Die von einem Benutzer als Teil der Chatkonversation verfasste und gesendete Nur-Text-Nachricht.
 - `RichText/HTML`: Eine formatierte Textnachricht. Beachten Sie, dass Communication Services-Benutzer derzeit keine RichText-Nachrichten senden können. Dieser Nachrichtentyp wird von Nachrichten unterstützt, die in Teams-Interoperabilitätsszenarien von Teams-Benutzern an Communication Services-Benutzer gesendet werden.

 - `ThreadActivity/ParticipantAdded`: Eine Systemnachricht mit dem Hinweis, dass dem Chatthread mindestens ein Teilnehmer hinzugefügt wurde. Beispiel:

```
{
            "id": "1613589626560",
            "type": "participantAdded",
            "sequenceId": "7",
            "version": "1613589626560",
            "content":
            {
                "participants":
                [
                    {
                        "id": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4df6-f40f-343a0d003226",
                        "displayName": "Jane",
                        "shareHistoryTime": "1970-01-01T00:00:00Z"
                    }
                ],
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"
            },
            "createdOn": "2021-02-17T19:20:26Z"
        }
```

- `ThreadActivity/ParticipantRemoved`: Systemnachricht mit dem Hinweis, dass ein Teilnehmer aus dem Chatthread entfernt wurde Beispiel:

```
{
            "id": "1613589627603",
            "type": "participantRemoved",
            "sequenceId": "8",
            "version": "1613589627603",
            "content":
            {
                "participants":
                [
                    {
                        "id": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4df6-f40f-343a0d003226",
                        "displayName": "Jane",
                        "shareHistoryTime": "1970-01-01T00:00:00Z"
                    }
                ],
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"
            },
            "createdOn": "2021-02-17T19:20:27Z"
        }
```

- `ThreadActivity/TopicUpdate`: Systemnachricht mit dem Hinweis, dass das Threadthema aktualisiert wurde. Beispiel:

```
{
            "id": "1613589623037",
            "type": "topicUpdated",
            "sequenceId": "2",
            "version": "1613589623037",
            "content":
            {
                "topic": "New topic",
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"
            },
            "createdOn": "2021-02-17T19:20:23Z"
        }
```

## <a name="real-time-signaling"></a>Echtzeitsignalisierung

Die JavaScript-Clientbibliothek für Chats enthält Echtzeitsignalisierung. Dadurch können Clients auf Echtzeitaktualisierungen und eingehende Nachrichten in einem Chatthread lauschen, ohne die APIs abfragen zu müssen. Verfügbare Ereignisse umfassen Folgendes:

 - `ChatMessageReceived`: wenn eine neue Nachricht an einen Chatthread gesendet wird, in dem der Benutzer Mitglied ist. Dieses Ereignis wird nicht für automatisch generierte Systemnachrichten gesendet, die im vorherigen Thema erläutert wurden.
 - `ChatMessageEdited`: wenn eine Nachricht in einem Chatthread bearbeitet wird, in dem der Benutzer Mitglied ist
 - `ChatMessageDeleted`: wenn eine Nachricht in einem Chatthread gelöscht wird, in dem der Benutzer Mitglied ist
 - `TypingIndicatorReceived`: wenn ein anderes Mitglied eine Nachricht in einem Chatthread eingibt, in dem der Benutzer Mitglied ist
 - `ReadReceiptReceived`: wenn ein anderes Mitglied die Nachricht gelesen hat, die der Benutzer in einem Chatthread gesendet hat

## <a name="chat-events"></a>Chatereignisse

Die Echtzeitsignalisierung ermöglicht es Ihren Benutzern, in Echtzeit zu chatten. Ihre Dienste können Azure Event Grid verwenden, um Ereignisse zu abonnieren, die für den Chat relevant sind. Weitere Informationen finden Sie unter [Konzepte für die Ereignisbehandlung](../event-handling.md).

## <a name="using-cognitive-services-with-chat-client-library-to-enable-intelligent-features"></a>Verwenden von Cognitive Services mit der Clientbibliothek für Chats zum Aktivieren intelligenter Features

Sie können [Azure Cognitive-APIs](../../../cognitive-services/index.yml) zusammen mit der Clientbibliothek für Chats verwenden, um intelligente Features zu Ihren Anwendungen hinzuzufügen. Beispielsweise können Sie folgende Aktionen ausführen:

- Ermöglichen Sie es Benutzern, miteinander in verschiedenen Sprachen zu chatten.
- Helfen Sie einem Support-Agent bei der Priorisierung von Tickets, indem Sie eine negative Stimmung in einem eingehenden Problem eines Kunden erkennen.
- Analysieren Sie die eingehenden Nachrichten zur Schlüssel- und Entitätserkennung, und fordern Sie von Ihrem Benutzer in Ihrer App basierend auf dem Nachrichteninhalt relevante Informationen an.

Eine Möglichkeit, um dies zu erreichen, besteht darin, dass Ihr vertrauenswürdiger Dienst als Mitglied eines Chatthreads fungiert. Angenommen, Sie möchten die Sprachübersetzung aktivieren. Dieser Dienst ist dafür zuständig, auf Nachrichten zu lauschen, die von anderen Mitgliedern ausgetauscht werden [1], Cognitive-APIs aufzurufen, um den Inhalt in die gewünschte Sprache zu übersetzen [2,3] und das übersetzte Ergebnis als Nachricht im Chatthread zu senden [4].

So enthält der Nachrichtenverlauf sowohl die ursprünglichen als auch übersetzte Nachrichten. In der Clientanwendung können Sie Logik hinzufügen, um die ursprüngliche oder die übersetzte Nachricht anzuzeigen. Informationen zur Verwendung von Cognitive APIs zum Übersetzen von Text in verschiedene Sprachen finden Sie in [diesem Schnellstart](../../../cognitive-services/translator/quickstart-translator.md).

:::image type="content" source="../media/chat/cognitive-services.png" alt-text="Diagramm, das zeigt, wie Cognitive Services mit Communication Services interagieren":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte mit dem Chat](../../quickstarts/chat/get-started.md)

Die folgenden Dokumente könnten Sie auch interessieren:

- Machen Sie sich mit der [Chatclientbibliothek](sdk-features.md) vertraut.

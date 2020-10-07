---
title: Übersicht über die Clientbibliothek für Chats von Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Hier finden Sie Informationen zur Clientbibliothek für Chats von Azure Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 3b66c962758afc28da3c6be938c13d768c4f2572
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91666142"
---
# <a name="chat-client-library-overview"></a>Übersicht über die Clientbibliothek für Chats

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Die Clientbibliotheken für Chats von Azure Communication Services können verwendet werden, um Ihren Anwendungen umfassende Echtzeitchatfunktionen hinzuzufügen.

## <a name="chat-client-library-capabilities"></a>Funktionen der Clientbibliothek für Chats

Die folgende Liste enthält die Features, die aktuell in den Clientbibliotheken für Chats von Communication Services verfügbar sind:

| Featuregruppe | Funktion                                                                                                          | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------------------------------------- | --- | ----- | ---- | -----  |
| Grundlegende Funktionen | Erstellen eines Chatthreads zwischen zwei oder mehr Benutzern (bis zu 250 Benutzer)                                                       | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Aktualisieren des Themas eines Chatthreads                                                                              | ✔️   | ✔️ | ✔️    | ✔️   |
|                   | Hinzufügen von Mitgliedern zu einem Chatthread oder Entfernen von Mitgliedern aus einem Chatthread                                                                           | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Auswählen, ob der Chatnachrichtenverlauf mit neu hinzugefügten Mitgliedern geteilt werden soll (*alle/keine/bis zu einer bestimmten Zeit*) | ✔️   | ✔️   | ✔️    | ✔️  |
|                   | Abrufen einer Liste mit allen Chatthreadmitgliedern                                                                          | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Löschen eines Chatthreads                                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Abrufen einer Liste mit den Chatthreadmitgliedschaften eines Benutzers                                                                  | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Abrufen von Informationen für einen bestimmten Chatthread                                                                              | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Senden und Empfangen von Nachrichten in einem Chatthread                                                                            | ✔️   | ✔️   | ✔️    | ✔️  |
|                   | Bearbeiten des Inhalts einer bereits gesendeten Nachricht                                                                   | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Löschen einer Nachricht                                                                                                       | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Markieren einer Nachricht mit normaler oder hoher Priorität beim Senden                                               | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Senden und Empfangen von Lesebestätigungen für Nachrichten, die von Mitgliedern gelesen wurden <br/> *Bei Chatthreads mit mehr als 20 Mitgliedern nicht verfügbar*    | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Senden und Empfangen von Eingabebenachrichtigungen, wenn ein Mitglied aktiv eine Nachricht in einem Chatthread eingibt <br/> *Bei Chatthreads mit mehr als 20 Mitgliedern nicht verfügbar*      | ✔️   | ✔️   | ✔️    | ✔️    |
|                   | Abrufen aller Nachrichten in einem Chatthread <br/> *Unicode-Emojis werden unterstützt.*                                                  | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Senden von Emojis im Nachrichteninhalt                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |
|Echtzeitsignalisierung (durch proprietäres Signalisierungspaket)| Erhalten einer Benachrichtigung, wenn ein Benutzer in einem Chatthread, dem er angehört, eine neue Nachricht erhält                                     | ✔️   | ❌    | ❌  | ❌  |
|                    | Erhalten einer Benachrichtigung, wenn in einem Chatthread, dem der Benutzer angehört, eine Nachricht durch einen anderen Benutzer bearbeitet wurde                | ✔️   | ❌    | ❌    | ❌  |
|                    | Erhalten einer Benachrichtigung, wenn in einem Chatthread, dem der Benutzer angehört, eine Nachricht durch einen anderen Benutzer gelöscht wurde                | ✔️   | ❌    | ❌    | ❌  |
|                    | Erhalten einer Benachrichtigung, wenn ein anderes Chatthreadmitglied etwas eingibt                                                             | ✔️   | ❌    | ❌    | ❌  |
|                    | Erhalten einer Benachrichtigung, wenn ein anderes Mitglied eine Nachricht im Chatthread gelesen hat (Lesebestätigung)                               | ✔️   | ❌    | ❌    | ❌  |
| Ereignisse             | Verwenden von Event Grid, um Benutzeraktivitäten in Chatthreads zu abonnieren und benutzerdefinierte Benachrichtigungsdienste oder Geschäftslogik zu integrieren     | ✔️   | ✔️  | ✔️    | ✔️  |
| Überwachung        | Überwachen der Nutzung (gesendete Nachrichten)                                                                               | ✔️   | ✔️  | ✔️    | ✔️  |
|                    | Überwachen der Qualität und des Status von API-Anforderungen Ihrer App sowie Konfigurieren von Warnungen über das Portal                                                          | ✔️   | ✔️  | ✔️    | ✔️  |
|Zusätzliche Features | Verwenden von [Cognitive Services-APIs](https://docs.microsoft.com/azure/cognitive-services/) zusammen mit der Clientbibliothek für Chats, um die Verwendung intelligenter Features zu ermöglichen: *Sprachübersetzung und Standpunktanalyse der eingehenden Nachricht auf einem Client, Spracherkennung zur Erstellung einer Nachricht, während der Teilnehmer spricht, usw.*                                                                                         | ✔️   | ✔️  | ✔️    | ✔️  |

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte mit dem Chat](../../quickstarts/chat/get-started.md)

Die folgenden Dokumente könnten Sie auch interessieren:

- Machen Sie sich mit [Chatkonzepten](../chat/concepts.md) vertraut.

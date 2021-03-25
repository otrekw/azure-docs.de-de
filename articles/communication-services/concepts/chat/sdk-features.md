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
ms.openlocfilehash: 705bd926c2ac6f414464254969b5c511c88891f0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104656106"
---
# <a name="chat-client-library-overview"></a>Übersicht über die Clientbibliothek für Chats  

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]    

Die Clientbibliotheken für Chats von Azure Communication Services können verwendet werden, um Ihren Anwendungen umfassende Echtzeitchatfunktionen hinzuzufügen.
    
## <a name="chat-client-library-capabilities"></a>Funktionen der Clientbibliothek für Chats 

Die folgende Liste enthält die Features, die aktuell in den Clientbibliotheken für Chats von Communication Services verfügbar sind:  

| Featuregruppe | Funktion | JavaScript  | Java | .NET | Python | iOS | Android |
|-----------------|-------------------|---|-----|----|-----|----|----|
| Grundlegende Funktionen | Erstellen eines Chatthreads zwischen zwei oder mehr Benutzern (bis zu 250 Benutzer)                                                       | ✔️   | ✔️  | ✔️    | ✔️   |  ✔️    | ✔️   |    
|                   | Aktualisieren des Themas eines Chatthreads                                                                              | ✔️   | ✔️ | ✔️    | ✔️   |  ✔️    | ✔️   |   
|                   | Hinzufügen oder Entfernen von Teilnehmern zu bzw. aus einem Chatthread                                                                           | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
|                   | Auswählen, ob der Chatnachrichtenverlauf mit dem hinzugefügten Teilnehmer geteilt werden soll                                   | ✔️   | ✔️   | ✔️    | ✔️  |  ✔️    | ✔️   | 
|                   | Abrufen einer Liste der Teilnehmer eines Chatthreads                                                                          | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   | 
|                   | Löschen eines Chatthreads                                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|                   | Abrufen der Liste der Chatthreads, denen der Benutzer angehört (bei Kommunikationsbenutzern)                                           | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |   
|                   | Abrufen von Informationen für einen bestimmten Chatthread                                                                              | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | Senden und Empfangen von Nachrichten in einem Chatthread                                                                            | ✔️   | ✔️   | ✔️    | ✔️  |  ✔️    | ✔️   |   
|                   | Bearbeiten des Inhalts einer gesendeten Nachricht                                                                                | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | Löschen einer Nachricht                                                                                                       | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | Lesebestätigungen für Nachrichten, die von anderen Teilnehmern in einem Chat gelesen wurden <br/> *Bei Chatthreads mit mehr als 20 Teilnehmern nicht verfügbar*    | ✔️   | ✔️  | ✔️    | ✔️   |  ✔️    | ✔️   |   
|                   | Erhalten einer Benachrichtigung, wenn Teilnehmer aktiv eine Nachricht in einem Chatthread eingeben <br/> *Bei Chatthreads mit mehr als 20 Mitgliedern nicht verfügbar*      | ✔️   | ✔️   | ✔️    | ✔️    |  ✔️    | ✔️   | 
|                   | Abrufen aller Nachrichten in einem Chatthread <br/>                                                                         | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
|                   | Senden von Unicode-Emojis im Nachrichteninhalt                                                                            | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|Echtzeitsignalisierung (durch proprietäres Signalisierungspaket**)|  Abonnieren von Echtzeitaktualisierungen für eingehende Nachrichten und andere Vorgänge in Ihrer Chat-App. Eine Liste der unterstützten Aktualisierungen für die Echtzeitsignalisierung finden Sie unter [Chatkonzepte](concepts.md#real-time-signaling).                                     | ✔️   | ❌    | ❌  | ❌  | ❌  | ❌  |    
| Event Grid-Unterstützung             | Nutzen der Integration in Azure Event Grid und Konfigurieren des Kommunikationsdiensts zum Ausführen von Geschäftslogik basierend auf Chataktivitäten oder zum Einbinden eines benutzerdefinierten Pushbenachrichtigungsdiensts   | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
| Überwachung        | Verwenden der im Azure-Portal ausgegebenen API-Anforderungsmetriken, um Dashboards zu erstellen, die Integrität Ihrer Chat-App zu überwachen und Warnungen zum Erkennen von Anomalien festzulegen      | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|                   | Konfigurieren der Communication Services-Ressource zum Empfangen von Chatbetriebsprotokollen zu Überwachungs- und Diagnosezwecken          | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  


**Das proprietäre Signalisierungspaket wird mithilfe von Websockets implementiert. Es wird ein Fallback auf ein langes Abrufintervall durchgeführt, falls Websockets nicht unterstützt werden.  

## <a name="javascript-chat-client-library-support-by-os-and-browser"></a>JavaScript-Chatclientbibliothek: Unterstützung nach Betriebssystemen und Browsern 

Die folgende Tabelle enthält die unterstützten Browser und Versionen, die derzeit verfügbar sind:
    
|                                  | Windows          | macOS          | Ubuntu | Linux  | Android | iOS    | iPad-Betriebssystem|
|--------------------------------|----------------|--------------|-------|------|------|------|-------|
| **Chatclientbibliothek** | Firefox *, Chrome*, Microsoft Edge (neu) | Firefox *, Chrome*, Safari* | Chrome*  | Chrome* | Chrome* | Safari* | Safari* |

\* Hinweis: Neben den beiden vorherigen Releases wird auch die neueste Version unterstützt.<br/>   

## <a name="next-steps"></a>Nächste Schritte   

> [!div class="nextstepaction"] 
> [Erste Schritte mit dem Chat](../../quickstarts/chat/get-started.md)    

Die folgenden Dokumente könnten Sie auch interessieren:  
- Machen Sie sich mit [Chatkonzepten](../chat/concepts.md) vertraut.
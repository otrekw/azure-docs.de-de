---
title: Preisszenarien für Anrufe (Sprache/Video) und Chats
titleSuffix: An Azure Communication Services concept document
description: Enthält eine Beschreibung des Preismodells für Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: fc5da3f4ac5bf9a08e16a931d54dfbf6a2fb9f48
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103495792"
---
# <a name="pricing-scenarios"></a>Preisszenarien

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]


Die Preise für Azure Communication Services basieren im Allgemeinen auf einem Modell mit nutzungsbasierter Bezahlung. Die Preise in den folgenden Beispielen dienen lediglich zu Veranschaulichung und entsprechen nicht unbedingt den aktuellen Azure-Preisen.

## <a name="voicevideo-calling-and-screen-sharing"></a>Sprach-/Videoanrufe und Bildschirmfreigabe

Azure Communication Services ermöglicht das Hinzufügen von Sprach-/Videoanrufen und das Freigeben des Bildschirms für Ihre Anwendungen. Sie können die Benutzeroberfläche in Ihre Anwendungen einbetten, indem Sie Clientbibliotheken vom Typ JavaScript, Objective-C (Apple), Java (Android) oder .NET verwenden. Weitere Informationen finden Sie in unserer [vollständigen Liste mit den verfügbaren Clientbibliotheken](./sdk-options.md).

### <a name="pricing"></a>Preise

Für die Dienste für Anrufe und die Bildschirmfreigabe werden die Gebühren pro Minute und Teilnehmer berechnet (0,004 US-Dollar pro Teilnehmer pro Minute für Gruppenanrufe). Grundlegende Informationen zu den unterschiedlichen Anrufabläufen, die möglich sind, finden Sie auf [dieser Seite](./call-flows.md).

Bei der Abrechnung zählt jeder Teilnehmer des Anrufs für jede Minute, in der für ihn eine Anrufverbindung bestanden hat. Dies gilt unabhängig davon, ob der Benutzer einen Videoanruf, Sprachanruf oder die Bildschirmfreigabe genutzt hat.

### <a name="pricing-example-group-audiovideo-call-using-js-and-ios-client-libraries"></a>Preisbeispiel: Gruppieren von Audio-/Videoanrufen mit JS- und iOS-Clientbibliotheken

Alice hat einen Gruppenanruf mit ihren Kollegen Bob und Charlie durchgeführt. Alice und Bob haben die JS-Clientbibliotheken verwendet, und Charlie hat die iOS-Clientbibliotheken genutzt.

- Der Anruf hat insgesamt 60 Minuten gedauert.
- Alice und Bob haben am gesamten Anruf teilgenommen. Alice hat die Videofunktion fünf Minuten lang aktiviert und ihren Bildschirm 23 Minuten lang freigegeben. Bob hatte die Videofunktion während der gesamten Anrufdauer (60 Minuten) aktiviert und seinen Bildschirm zwölf Minuten lang freigegeben.
- Charlie hat den Anruf nach 43 Minuten verlassen. Charlie hatte die Audio- und Videofunktion während der gesamten Dauer der Teilnahme (43 Minuten) aktiviert.

**Berechnung der Kosten**

- 2 Teilnehmer · 60 Minuten · 0,004 USD pro Teilnehmer pro Minute = 0,48 USD [gleicher Preis für Video und Audio]
- 1 Teilnehmer · 43 Minuten · 0,004 USD pro Teilnehmer pro Minute = 0,172 USD [gleicher Preis für Video und Audio]

**Gesamtkosten für den Gruppenanruf**: 0,48 USD + 0,172 USD = 0,652 USD

### <a name="pricing-example-a-user-of-the-communication-services-js-client-library-joins-a-scheduled-microsoft-teams-meeting"></a>Preisbeispiel: Benutzer der Communication Services-JS-Clientbibliothek tritt einer geplanten Microsoft Teams-Besprechung bei

Die Ärztin Alice hat eine Besprechung mit dem Patienten Bob. Alice tritt über die Teams-Desktopanwendung bei. Bob erhält einen Link, mit dem er der Besprechung über die Website des Gesundheitsdienstleisters beitreten kann. Die Verbindung wird dabei unter Verwendung der Communication Services-JS-Clientbibliothek hergestellt. Bob verwendet sein Mobiltelefon (iPhone mit Safari), um über einen Webbrowser an der Besprechung teilzunehmen. Während des virtuellen Besuchs ist Chat verfügbar.

- Der Anruf dauert insgesamt 30 Minuten.
- Alice und Bob nehmen am gesamten Anruf teil. Fünf Minuten nach Beginn des Anrufs schaltet Alice ihre Videoübertragung ein und teilt 13 Minuten lang ihren Bildschirm. Bob hat seine Videoübertragung während des gesamten Anrufs aktiviert.
- Alice sendet fünf Nachrichten, Bob antwortet mit drei Nachrichten.


**Berechnung der Kosten**

- 1 Teilnehmer (Bob) · 30 Minuten · 0,004 USD pro Teilnehmer und Minute = 0,12 USD [gleicher Preis für Video und Audio]
- 1 Teilnehmer (Alice) · 30 Minuten · 0,000 USD pro Teilnehmer und Minute = 0,0 USD*.
- 1 Teilnehmer (Bob) · 3 Chatnachrichten · 0,0008 USD = 0,0024 USD.
- 1 Teilnehmer (Alice) · 5 Chatnachrichten · 0,000 USD = 0,0 USD.

*Die Teilnahme von Alice ist durch ihre Teams-Lizenz abgedeckt. Der Einfachheit halber enthält Ihre Azure-Rechnung die Minuten und Chatnachrichten zwischen Teams- und Communication Services-Benutzern, die Minuten und Nachrichten des Teams-Clients sind jedoch kostenlos.

**Gesamtkosten für den Besuch:**
- Benutzer, der über die Communication Services-JS-Clientbibliothek teilnimmt: 0,12 USD + 0,0024 = 0,1224 USD
- Benutzer, der über die Teams-Desktopanwendung teilnimmt: 0 USD (abgedeckt durch die Teams-Lizenz)


## <a name="chat"></a>Chat

Mit Communication Services können Sie Ihre Anwendung um eine Funktion zum Senden und Empfangen von Chatnachrichten zwischen zwei oder mehr Benutzern erweitern. Chatclientbibliotheken sind für JavaScript, .NET, Python und Java verfügbar. Informieren Sie sich auf [dieser Seite](./sdk-options.md) über Clientbibliotheken.

### <a name="price"></a>Preis

Ihnen werden für jede gesendete Chatnachricht 0,0008 US-Dollar berechnet.

### <a name="pricing-example-chat-between-two-users"></a>Preisbeispiel: Chat zwischen zwei Benutzern

Geeta beginnt einen Chatthread mit Emily, um neue Informationen zu übermitteln, und sendet fünf Nachrichten. Der Chat dauert zehn Minuten. Geeta und Emily senden sich gegenseitig jeweils 15 Nachrichten.

**Berechnung der Kosten**
- Anzahl gesendeter Nachrichten (5 + 15 + 15) · 0,0008 USD = 0,028 USD

### <a name="pricing-example-group-chat-with-multiple-users"></a>Preisbeispiel: Gruppenchat mit mehreren Benutzern

Charlie beginnt einen Chatthread mit seinen Freunden Casey und Jasmine, um eine Urlaubsreise zu planen. Sie chatten eine Weile, und Charlie, Casey und Jasmine senden 20, 30 bzw. 18 Nachrichten. Ihnen fällt ein, dass ihre Freundin Rose ggf. auch Interesse an der Urlaubsreise haben könnte. Daher fügen Sie sie dem Chatthread hinzu und geben den gesamten Nachrichtenverlauf für sie frei.

Rose sieht die Nachrichten und beteiligt sich am Chat. In der Zwischenzeit erhält Casey einen Anruf und trifft die Entscheidung, die Unterhaltung später zu lesen. Charlie, Jasmine und Rose einigen sich auf die Reisetermine und senden 30, 25 bzw. 35 Nachrichten.

**Berechnung der Kosten**

- Anzahl gesendeter Nachrichten (20 + 30 + 18 + 30 + 25 + 35) · 0,0008 USD = 0,1264 USD


## <a name="telephony-and-sms"></a>Telefonie und SMS

## <a name="price"></a>Preis

Telefoniedienste werden auf Minutenbasis abgerechnet, während SMS auf Nachrichtenbasis abgerechnet werden. Die Preise richten sich nach der Art und dem Standort der von Ihnen verwendeten Nummer sowie nach dem Ziel Ihrer Anrufe und SMS-Nachrichten.

### <a name="telephone-number-leasing"></a>Leasing von Telefonnummern

Gebühren für das Leasing von Telefonnummern werden im Voraus erhoben und fallen dann monatlich wiederkehrend an:

|Zahlentyp   |Monatliche Gebühr   |
|--------------|-----------|
|Lokal (Nordamerika)     |1 USD/Monat        |
|Gebührenfrei (Nordamerika) |2 USD/Monat |


### <a name="telephone-calling"></a>Telefonanruf

Herkömmliche Telefonanrufe (Anrufe, die über das Telefonfestnetz getätigt werden) sind für Telefonnummern mit Sitz in den USA zu nutzungsbasierten Preisen erhältlich. Der Preis ist ein Minutenpreis, der von der Art der verwendeten Nummer und dem Ziel des Anrufs abhängt. Preisdetails für die beliebtesten Anrufziele sind in der folgenden Tabelle aufgeführt. Eine vollständige Liste der Ziele finden Sie in der [detaillierten Preisliste](https://github.com/Azure/Communication/blob/master/pricing/communication-services-pstn-rates.csv).


#### <a name="united-states-calling-prices"></a>Preise für Anrufe in den USA

Die folgenden Preise enthalten die erforderlichen Kommunikationssteuern und Gebühren bis zum 30. Juni 2021:

|Zahlentyp   |Anrufe tätigen   |Anrufe empfangen|
|--------------|-----------|------------|
|Lokal     |Ab 0,013 USD/Minute       |0,0085 USD/Minute        |
|Gebührenfrei |0,013 USD/Minute   |0,0220 USD/Minute |

#### <a name="other-calling-destinations"></a>Sonstige Anrufziele

Die folgenden Preise enthalten die erforderlichen Kommunikationssteuern und Gebühren bis zum 30. Juni 2021:

|Tätigen von Anrufen nach   |Preis pro Minute|
|-----------|------------|
|Kanada     |Ab 0,013 USD/Minute   |
|Vereinigtes Königreich     |Ab 0,015 USD/Minute   |
|Deutschland     |Ab 0,015 USD/Minute   |
|Frankreich     |Ab 0,016 USD/Minute   |


### <a name="sms"></a>sms

SMS bietet nutzungsbasierte Preise. Der Preis ist eine auf dem Ziel der Nachricht basierende Gebühr pro Nachricht. Nachrichten können über gebührenfreie Telefonnummern an Telefonnummern innerhalb der USA gesendet werden. Beachten Sie, dass lokale (geografische) Telefonnummern nicht zum Senden von SMS-Nachrichten verwendet werden können.

Die folgenden Preise enthalten die erforderlichen Kommunikationssteuern und Gebühren bis zum 30. Juni 2021:

|Land   |Senden von Nachrichten|Empfangen von Nachrichten|
|-----------|------------|------------|
|USA (gebührenfrei)    |0,0075 USD/Nachricht   | 0,0075 USD/Nachricht |

---
title: Übersicht über die Clientbibliothek für Telefonie von Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Hier finden Sie eine Übersicht über die Clientbibliothek für Telefonie.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 110607e9da68cf131a43651b97c599873ce33cb8
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103495265"
---
# <a name="calling-client-library-overview"></a>Übersicht über die Clientbibliothek für Telefonie

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]


Es gibt zwei separate Familien von Clientbibliotheken für Telefonie: eine für *Clients* und eine für *Dienste*. Die aktuell verfügbaren Clientbibliotheken sind für Endbenutzerumgebungen (Websites und native Apps) vorgesehen.

Die Clientbibliotheken für Dienste sind noch nicht verfügbar und ermöglichen den Zugriff auf unformatierte Sprach- und Videodaten für die Integration in Bots und andere Diensten.

## <a name="calling-client-library-capabilities"></a>Funktionen der Clientbibliothek für Telefonie

Die folgende Liste enthält die Features, die aktuell in den Clientbibliotheken für Telefonie von Communication Services verfügbar sind:

| Featuregruppe | Funktion                                                                                                          | JS  | Java (Android) | Objective-C (iOS)
| ----------------- | ------------------------------------------------------------------------------------------------------------------- | ---  | -------------- | -------------
| Grundlegende Funktionen | Tätigen eines 1:1-Anrufs zwischen zwei Benutzern                                                                           | ✔️   | ✔️            | ✔️
|                   | Tätigen eines Gruppenanrufs zwischen mehr als zwei Benutzern (bis zu 350 Benutzer)                                                       | ✔️   | ✔️            | ✔️
|                   | Höherstufen eines 1:1-Anrufs mit zwei Benutzern zu einem Gruppenanruf mit mehr als zwei Benutzern                                 | ✔️   | ✔️            | ✔️
|                   | Beitreten zu einem bereits gestarteten Gruppenanruf                                                                              | ✔️   | ✔️            | ✔️
|                   | Einladen eines weiteren VoIP-Teilnehmers zu einem laufenden Gruppenanruf                                                       | ✔️   | ✔️            | ✔️
|  Steuerung während des Anrufs | Aktivieren/Deaktivieren Ihres Videos                                                                                              | ✔️   | ✔️            | ✔️ 
|                   | Stummschalten des Mikrofons/Aufheben der Stummschaltung                                                                                                     | ✔️   | ✔️            | ✔️         
|                   | Wechseln zwischen Kameras                                                                                              | ✔️   | ✔️            | ✔️           
|                   | Lokales Halten/Aufheben des Haltens                                                                                                  | ✔️   | ✔️            | ✔️           
|                   | Aktiver Lautsprecher                                                                                                      | ✔️   | ✔️            | ✔️           
|                   | Auswählen des Lautsprechers für Anrufe                                                                                            | ✔️   | ✔️            | ✔️           
|                   | Auswählen des Mikrofons für Anrufe                                                                                         | ✔️   | ✔️            | ✔️           
|                   | Anzeigen des Status eines Teilnehmers<br/>*Beschäftigt, Early Media, Verbindungsaufbau, Verbunden, Gehalten, Im Wartebereich, Getrennt*         | ✔️   | ✔️            | ✔️           
|                   | Anzeigen des Zustands eines Anrufs<br/>*Early Media, Eingehend, Verbindungsaufbau, Klingeln, Verbunden, Halten, Trennung, Getrennt* | ✔️   | ✔️            | ✔️           
|                   | Anzeigen, ob ein Teilnehmer stummgeschaltet ist                                                                                      | ✔️   | ✔️            | ✔️           
|                   | Anzeigen des Grunds, warum ein Teilnehmer einen Anruf verlassen hat                                                                       | ✔️   | ✔️            | ✔️     
| Bildschirmfreigabe    | Freigeben des gesamten Bildschirms innerhalb der Anwendung                                                                 | ✔️   | ❌            | ❌           
|                   | Freigeben einer bestimmten Anwendung (aus der Liste aktiver Anwendungen)                                                | ✔️   | ❌            | ❌           
|                   | Freigeben eines Webbrowsertabs aus der Liste geöffneter Tabs                                                                  | ✔️   | ❌            | ❌           
|                   | Anzeigen von Remotebildschirmfreigabe durch Teilnehmer                                                                            | ✔️   | ✔️            | ✔️         
| Liste            | Auflisten von Teilnehmern                                                                                                   | ✔️   | ✔️            | ✔️           
|                   | Entfernen eines Teilnehmers                                                                                                | ✔️   | ✔️            | ✔️         
| PSTN              | Tätigen eines 1:1-Anrufs mit einem PSTN-Teilnehmer                                                                     | ✔️   | ✔️            | ✔️   
|                   | Tätigen eines Gruppenanrufs mit PSTN-Teilnehmern                                                                           | ✔️   | ✔️            | ✔️
|                   | Höherstufen eines 1:1-Anrufs mit einem PSTN-Teilnehmer zu einem Gruppenanruf                                                 | ✔️   | ✔️            | ✔️
|                   | Verlassen eines Gruppenanrufs als PSTN-Teilnehmer                                                                    | ✔️   | ✔️            | ✔️   
| Allgemein           | Testen von Mikrofon, Lautsprecher und Kamera mit einem Audiotestdienst (verfügbar durch Anrufen von 8:echo123)                   | ✔️   | ✔️            | ✔️ 
| -Geräteverwaltung | Bitten um Erlaubnis zur Verwendung der Audio-/Videofunktion                                                                       | ✔️   | ✔️            | ✔️
|                   | Abrufen einer Kameraliste                                                                                                     | ✔️   | ✔️            | ✔️ 
|                   | Festlegen der Kamera                                                                                                          | ✔️   | ✔️            | ✔️
|                   | Abrufen der ausgewählten Kamera                                                                                                 | ✔️   | ✔️            | ✔️
|                   | Abrufen der Mikrofonliste                                                                                                 | ✔️   | ✔️            | ✔️
|                   | Festlegen des Mikrofons                                                                                                      | ✔️   | ✔️            | ✔️
|                   | Abrufen des ausgewählten Mikrofons                                                                                             | ✔️   | ✔️            | ✔️
|                   | Abrufen der Lautsprecherliste                                                                                                   | ✔️   | ✔️            | ✔️
|                   | Festlegen des Lautsprechers                                                                                                         | ✔️   | ✔️            | ✔️
|                   | Abrufen des ausgewählten Lautsprechers                                                                                                | ✔️   | ✔️            | ✔️
| Videorendering   | Rendern eines Videos an mehreren Orten (lokale Kamera oder Remotedatenstrom)                                                  | ✔️   | ✔️            | ✔️
|                   | Festlegen/Aktualisieren des Skalierungsmodus                                                                                           | ✔️   | ✔️            | ✔️ 
|                   | Rendern des Remote-Videodatenstroms                                                                                          | ✔️   | ✔️            | ✔️



## <a name="javascript-calling-client-library-support-by-os-and-browser"></a>JavaScript-Clientbibliothek für Telefonie: Unterstützung nach Betriebssystemen und Browsern

Die folgende Tabelle enthält die unterstützten Browser, die derzeit verfügbar sind. Sofern nichts anderes angegeben ist, unterstützen wir die letzten drei Versionen des Browsers.

|                                  | Chrome | Safari*  | Edge (Chromium) | 
| -------------------------------- | -------| ------  | --------------  |
| Android-                          |  ✔️    | ❌     | ❌             |
| iOS                              |  ❌    | ✔️**** | ❌             |
| macOS***                         |  ✔️    | ✔️**   | ❌             |
| Windows***                       |  ✔️    | ❌     | ✔️             |
| Ubuntu/Linux                     |  ✔️    | ❌     | ❌             |

*Safari wird ab Version 13.1 unterstützt, 1:1-Anrufe werden in Safari nicht unterstützt. 

** Safari 14+/macOS 11+ erforderlich für die Unterstützung von ausgehenden Videos. 

*** Die ausgehende Bildschirmfreigabe wird unabhängig von der Browserversion nur auf Desktopplattformen (Windows, macOS und Linux) unterstützt, und nicht für mobile Plattformen (Android, iOS, iPad und Tablets).

**** Mit einer iOS-App in Safari können keine Mikrofon- und Lautsprechergeräte (z. B. Bluetooth) aufgelistet bzw. ausgewählt werden. Dies ist eine Einschränkung des Betriebssystems, und es ist immer nur ein Gerät vorhanden.


## <a name="calling-client---browser-security-model"></a>Anrufclient: Browsersicherheitsmodell

### <a name="user-webrtc-over-https"></a>Verwenden von WebRTC über HTTPS

WebRTC-APIs wie `getUserMedia` setzen voraus, dass die App, von der diese APIs aufgerufen werden, über HTTPS bedient wird.

Für die lokale Entwicklung können Sie `http://localhost` verwenden.

### <a name="embed-the-communication-services-calling-sdk-in-an-iframe"></a>Einbetten des Anruf-SDK für Communication Services in ein iframe-Element

Von verschiedenen Browsern wird eine neue [Berechtigungsrichtlinie (auch Featurerichtlinie genannt)](https://www.w3.org/TR/permissions-policy-1/#iframe-allow-attribute) eingeführt. Durch diese Richtlinie wird über ein ursprungsübergreifendes iframe-Element gesteuert, wie Anwendungen auf die Kamera und das Mikrofon eines Geräts zugreifen können, was Auswirkungen auf Anrufszenarien hat.

Wenn Sie ein iframe-Element verwenden möchten, um einen Teil der App über eine andere Domäne zu hosten, müssen Sie Ihrem iframe-Element das Attribut `allow` mit dem korrekten Wert hinzufügen.

Durch das folgende iframe-Element wird beispielsweise sowohl Kamera- als auch Mikrofonzugriff gewährt:

```html
<iframe allow="camera *; microphone *">
```

## <a name="calling-client-library-streaming-support"></a>Streamingunterstützung der Clientbibliothek für Telefonie
Die Clientbibliothek für Telefonie von Communication Services unterstützt die folgenden Streamingkonfigurationen:

|           |Web | Android/iOS|
|-----------|----|------------|
|**Anzahl der ausgehenden Datenströme, die gleichzeitig gesendet werden können.** |1 Video- und 1 Bildschirmfreigabe | 1 Video- und 1 Bildschirmfreigabe|
|**Anzahl der eingehenden Datenströme, die gleichzeitig gerendert werden können.** |1 Video- und 1 Bildschirmfreigabe| 6 Video- und 1 Bildschirmfreigabe |


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte mit Anrufen](../../quickstarts/voice-video-calling/getting-started-with-calling.md)

Weitere Informationen finden Sie in den folgenden Artikeln:
- Machen Sie sich mit allgemeinen [Anrufflows](../call-flows.md) vertraut.
- Informieren Sie sich über [Anruftypen](../voice-video-calling/about-call-types.md).
- [Planen Sie Ihre PSTN-Lösung.](../telephony-sms/plan-solution.md)

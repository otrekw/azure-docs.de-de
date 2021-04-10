---
title: Konfigurieren der Azure Industrial IoT-Komponenten
description: In diesem Tutorial erfahren Sie, wie Sie die Standardwerte der Konfiguration ändern.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: tutorial
ms.date: 3/22/2021
ms.openlocfilehash: 348276a71b2227063374da6455445118fcb00fcf
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104787229"
---
# <a name="tutorial-configure-the-industrial-iot-components"></a>Tutorial: Konfigurieren der Industrial IoT-Komponenten

Durch das Bereitstellungsskript werden alle Komponenten automatisch mit Standardwerten konfiguriert, damit sie miteinander verwendet werden können. Die Standardwerte können jedoch an Ihre Anforderungen angepasst werden.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Anpassen der Konfiguration der Komponenten


Im Anschluss finden Sie einige der relevanteren Anpassungseinstellungen für die Komponenten:
* IoT Hub
    * „Netzwerk“ > „Öffentlicher Zugriff“: Ermöglicht die Konfiguration des Internetzugriffs (beispielsweise IP-Filter).
    * „Netzwerk“ > „Private Endpunktverbindungen“: Ermöglicht die Erstellung eines Endpunkts, auf den nicht über das Internet zugegriffen und der intern von anderen Azure-Diensten oder lokalen Geräten genutzt werden kann (beispielsweise über eine VPN-Verbindung).
    * „IoT Edge“: Ermöglicht die Verwaltung der Konfiguration der Edge-Geräte, die mit den OPC UA-Servern verbunden sind. 
* Cosmos DB
    * „Daten global replizieren“: Ermöglicht die Konfiguration von Datenredundanz.
    * „Firewall und virtuelle Netzwerke“: Ermöglicht die Konfiguration von Internet- und VNET-Zugriff sowie von IP-Filtern.
    * „Private Endpunktverbindungen“: Ermöglicht die Erstellung eines Endpunkts, auf den nicht über das Internet zugegriffen werden kann. 
* Key Vault
    * „Geheimnisse“: Ermöglicht die Verwaltung von Plattformeinstellungen.
    * „Zugriffsrichtlinien“: Ermöglicht die Steuerung, welche Anwendungen und Benutzer auf die Daten in der Key Vault-Instanz zugreifen und welche Vorgänge (beispielsweise Lesen, Schreiben, Auflisten, Löschen) sie für das Netzwerk, die Firewall und das VNET sowie für private Endpunkte ausführen dürfen.
* „Azure Active Directory (AAD)“ > „App-Registrierungen“
    * „<APP-NAME>-web“ > „Authentifizierung“: Ermöglicht die Verwaltung von Antwort-URIs (die Liste der URIs, die nach erfolgreicher Authentifizierung als Landing Pages verwendet werden können). Dies kann unter Umständen in bestimmten Szenarien (beispielsweise im Falle fehlender AAD-Administratorrechte) nicht automatisch durch das Bereitstellungsskript konfiguriert werden. URIs sollten ggf. hinzugefügt oder geändert werden, wenn Sie den Hostnamen der Web-App ändern (beispielsweise die Portnummer, die vom Localhost zum Debuggen verwendet wird).
* App Service
    * „Konfiguration“: Ermöglicht die Verwaltung der Umgebungsvariablen zum Steuern der Dienste oder Benutzeroberfläche.
* Virtueller Computer
    * „Netzwerk“: Ermöglicht die Konfiguration unterstützter Netzwerke und Firewallregeln.
    * „Serielle Konsole“: SSH-Zugriff zum Abrufen von Erkenntnissen oder zum Debuggen, zum Abrufen der Anmeldeinformationen aus der Ausgabe des Bereitstellungsskripts oder zum Zurücksetzen des Kennworts
* „IoT Hub“ > „IoT Edge“
    * Ermöglicht die Verwaltung der Identitäten der IoT Edge-Geräte, die auf den Hub zugreifen dürfen. Außerdem können Sie konfigurieren, welche Module installiert werden und welche Konfiguration sie verwenden (beispielsweise Codierungsparameter für OPC Publisher).
* „IoT Hub“ > „IoT Edge“ > \<DEVICE> > „Module festlegen“ > „OpcPublisher“ (nur für eigenständige Verwendung von OPC Publisher)


## <a name="configuration-options"></a>Konfigurationsoptionen

|Konfigurationsoption (Kurzform/vollständiger Name)    |    Beschreibung   |
|----------------------------------------------|------------------|
pf/publishfile |Der Dateiname zum Konfigurieren der zu veröffentlichenden Knoten. Wenn diese Option angegeben ist, wird OPC Publisher in den eigenständigen Modus versetzt.
lf/logfile |Der Dateiname der zu verwendenden Protokolldatei.
ll/loglevel |Die zu verwendende Protokollebene. Zulässige Werte: „fatal“ (Schwerwiegend), „error“ (Fehler), „warn“ (Warnen), „info“ (Info), „debug“ (Debuggen), „verbose“ (Ausführlich)
me/messageencoding |Die Nachrichtencodierung für ausgehende Nachrichten. Zulässige Werte: „Json“ (JSON), „Uadp“ (UADP)
mm/messagingmode |Der Nachrichtenmodus für ausgehende Nachrichten. Zulässige Werte: „PubSub“, „Samples“ (Beispiele)
fm/fullfeaturedmessage |Der umfassende Modus für Nachrichten (alle Felder ausgefüllt). Standardmäßig auf „true“ festgelegt. Verwenden Sie „false“ für Legacykompatibilität.
aa/autoaccept |Vom Herausgeber wird automatisch allen Servern vertraut, mit denen eine Verbindung hergestellt wird.
bs/batchsize |Die Anzahl von OPC UA-Datenänderungsnachrichten, die für die Batchverarbeitung zwischengespeichert werden sollen.
si/iothubsendinterval |Das Intervall zum Auslösen der Batchverarbeitung (in Sekunden).
ms/iothubmessagesize |Die maximale Größe der (IoT D2C-)Nachricht.
om/maxoutgressmessages |Die maximale Größe des Ausgangspuffers für (IoT D2C-)Nachrichten.
di/diagnosticsinterval |Zeigt Herausgeberdiagnoseinformationen im angegebenen Sekundenintervall an. (Erfordert Informationen auf Protokollebene.) Mit „-1“ werden das Remotediagnoseprotokoll und die Diagnoseausgabe deaktiviert.
lt/logflugtimespan |Die Zeitspanne in Sekunden, nach der die Protokolldatei geleert werden soll.
ih/iothubprotocol |Zu verwendendes Protokoll für die Kommunikation mit dem Hub. Zulässige Werte: „AmqpOverTcp“, „AmqpOverWebsocket“, „MqttOverTcp“, „MqttOverWebsocket“, „Amqp“, „Mqtt“, „Tcp“, „Websocket“, „Any“
hb/heartbeatinterval |Wird vom Verleger als Standardwert (in Sekunden) für die Heartbeat-Intervalleinstellung von Knoten ohne Heartbeat-Intervalleinstellung verwendet.
ot/operationtimeout |Das Vorgangstimeout des OPC UA-Herausgeberclients (in ms).
ol/opcmaxstringlen |Die maximale Länge einer Zeichenfolge, die von OPC übertragen/empfangen kann.
oi/opcsamplinginterval |Standardwert in Millisekunden für das Anfordern einer Stichprobenentnahme von Werten durch die Server.
op/opcpublishinginterval |Standardwert in Millisekunden für die Veröffentlichungsintervalleinstellung der Abonnements für den OPC UA-Server.
ct/createsessiontimeout |Das Intervall (in Sekunden), mit dem der Herausgeber Keep-Alive-Nachrichten an die OPC-Server an den Endpunkten sendet, mit denen er verbunden ist.
kt/keepalivethresholt |Geben Sie die Anzahl von Keep-Alive-Paketen an, die ein Server verpassen kann, bevor die Sitzung getrennt wird.
tm/trustmyself |Das Herausgeberzertifikat wird automatisch im Vertrauensspeicher platziert.
at/appcertstoretype |Die Art des eigenen Anwendungszertifikatspeichers. Zulässige Werte: „Directory“, „X509Store“.


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun wissen, wie Sie die Standardwerte der Konfiguration ändern, können Sie sich als Nächstes mit Folgendem vertraut machen: 

> [!div class="nextstepaction"]
> [Tutorial: Pullen von Azure Industrial IoT-Daten in ADX](tutorial-industrial-iot-azure-data-explorer.md)

> [!div class="nextstepaction"]
> [Tutorial: Visualisieren von Daten mit Time Series Insights (TSI)](tutorial-visualize-data-time-series-insights.md)

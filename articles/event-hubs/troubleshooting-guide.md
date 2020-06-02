---
title: 'Leitfaden zur Problembehandlung: Azure Event Hubs | Microsoft-Dokumentation'
description: Dieser Artikel stellt eine Liste von Azure Event Hubs-Messagingausnahmen und vorgeschlagenen Aktionen zur Verfügung.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 01/16/2020
ms.author: shvija
ms.openlocfilehash: ab3cbdf938409f4eacffa10ae5cb20f8c36b5856
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124668"
---
# <a name="azure-event-hubs---troubleshooting-guide"></a>Azure Event Hubs: Leitfaden zur Problembehandlung
In diesem Artikel finden Sie Tipps zur Problembehandlung und Empfehlungen für einige Probleme, die bei der Verwendung von Azure EventHubs auftreten können.

## <a name="connectivity-certificate-or-timeout-issues"></a>Konnektivitäts-, Zertifikat- oder Timeoutprobleme
Die folgenden Schritte unterstützen Sie bei der Problembehandlung von Konnektivitäts-/Zertifikat-/Timeoutproblemen für alle Dienste unter *.servicebus.windows.net. 

- Navigieren Sie zu `https://<yournamespacename>.servicebus.windows.net/`, oder verwenden Sie [wget](https://www.gnu.org/software/wget/). Dies hilft bei der Überprüfung, ob Probleme mit der IP-Filterung oder dem virtuellen Netzwerk bzw. der Zertifikatkette vorliegen (häufiges Problem bei Verwendung des Java SDK).

    Beispiel für eine erfolgreiche Meldung:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Beispiel für eine Fehlermeldung:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Führen Sie den folgenden Befehl aus, um zu überprüfen, ob ein Port auf der Firewall blockiert ist. Die verwendeten Ports sind 443 (HTTPS), 5671 (AMQP) und 9093 (Kafka). Abhängig von der verwendeten Bibliothek werden auch andere Ports verwendet. Dies ist der Beispielbefehl, mit dem überprüft wird, ob Port 5671 blockiert ist.

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Unter Linux:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Führen Sie bei zeitweiligen Konnektivitätsproblemen den folgenden Befehl aus, um zu überprüfen, ob gelöschte Pakete vorhanden sind. Mit diesem Befehl wird versucht, jede Sekunde 25 verschiedene TCP-Verbindungen mit dem Dienst herzustellen. Anschließend können Sie überprüfen, wie viele davon erfolgreich/fehlerhaft waren, und außerdem die Latenz der TCP-Verbindung anzeigen. Sie können das `psping`-Tool [hier](/sysinternals/downloads/psping) herunterladen.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
    ```
    Sie können äquivalente Befehle verwenden, wenn Sie andere Tools wie `tnc`, `ping` usw. nutzen. 
- Rufen Sie eine Netzwerkablaufverfolgung ab, wenn die vorherigen Schritte nicht hilfreich sind, und analysieren Sie diese mit Tools wie [Wireshark](https://www.wireshark.org/). Wenden Sie sich bei Bedarf an den [Microsoft-Support](https://support.microsoft.com/). 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Mögliche Probleme im Zusammenhang mit Dienstupgrades/-neustarts
Upgrades und Neustarts des Back-End-Diensts können sich wie folgt auf Ihre Anwendungen auswirken:

- Anforderungen werden ggf. vorübergehend gedrosselt.
- Es gehen möglicherweise weniger Nachrichten/Anforderungen ein.
- Die Protokolldatei enthält unter Umständen Fehlermeldungen.
- Die Verbindung zwischen Anwendungen und dem Dienst wird möglicherweise für einige Sekunden getrennt.

Wenn im Anwendungscode das SDK verwendet wird, ist die Wiederholungsrichtlinie bereits integriert und aktiv. Die Verbindung wird ohne nennenswerte Auswirkungen auf die Anwendung bzw. den Workflow wiederhergestellt.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

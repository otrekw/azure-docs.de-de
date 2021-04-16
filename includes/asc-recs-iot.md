---
author: memildin
ms.service: security-center
ms.topic: include
ms.date: 03/22/2021
ms.author: memildin
ms.custom: generated
ms.openlocfilehash: 388798b9f1ada6921fb79363678ecd17a3041227
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104801464"
---
Diese Kategorie enthält **zwölf** Empfehlungen.

|Empfehlung |BESCHREIBUNG |severity |
|---|---|---|
|Die Standard-IP-Filterrichtlinie muss auf „Verweigern“ festgelegt sein |Die IP-Filterkonfiguration sollte Regeln für zulässigen Datenverkehr definieren und den gesamten übrigen Datenverkehr ablehnen<br />(Keine zugehörige Richtlinie) |Medium |
|In IoT Hub sollten Diagnoseprotokolle aktiviert sein. |Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.<br />(Zugehörige Richtlinie: [In IoT Hubs sollten Diagnoseprotokolle aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f383856f8-de7f-44a2-81fc-e5135b5c2aa4)) |Niedrig |
| Identische Anmeldeinformationen für Authentifizierung |Identische Authentifizierungsinformationen für den IoT Hub wurden von mehreren Geräten verwendet. Dies kann auf ein unzulässiges Gerät hinweisen, das die Identität eines legitimen Geräts vorgibt. Außerdem entsteht das Risiko, dass ein Angreifer die Identität eines Geräts vorgibt<br />(Keine zugehörige Richtlinie) |High |
| IoT-Geräte: Agent sendet Nachrichten zu Unterauslastung |Die Kapazität für die Nachrichtengröße des IoT-Agent wird zurzeit nicht ausgeschöpft. Dadurch erhöht sich die Anzahl gesendeter Nachrichten. Passen Sie die Nachrichtenintervalle an, um eine bessere Nutzung zu erzielen<br />(Keine zugehörige Richtlinie) |Niedrig |
| IoT-Geräte: Überwachter Prozess hat das Senden von Ereignissen beendet |Aus dem Auditd-Prozess stammende sicherheitsrelevante Ereignisse werden von diesem Gerät nicht mehr empfangen<br />(Keine zugehörige Richtlinie) |High |
| IoT-Geräte: offene Ports auf Gerät |Auf dem Gerät wurde ein lauschender Endpunkt gefunden<br />(Keine zugehörige Richtlinie) |Medium |
| IoT-Geräte: Fehler bei Validierung von Baseline von Betriebssystem |In der Systemkonfiguration wurden sicherheitsbezogene Probleme erkannt<br />(Keine zugehörige Richtlinie) |Medium |
| IoT-Geräte: In einer der Ketten wurde eine zu wenig einschränkende Firewallrichtlinie gefunden |In den Hauptfirewallketten (Eingabe/Ausgabe) wurde eine zugelassene Firewallrichtlinie gefunden. Die Richtlinie muss standardmäßig den gesamten Datenverkehr verweigern und Regeln definieren, die die notwendige Kommunikation zum/vom Gerät ermöglichen<br />(Keine zugehörige Richtlinie) |Medium |
| IoT-Geräte: In der Eingabekette wurde eine zu wenig einschränkende Firewallregel gefunden |In der Firewall wurde eine Regel gefunden, die ein zu wenig einschränkendes Muster für eine Vielzahl von IP-Adressen oder Ports enthält.<br />(Keine zugehörige Richtlinie) |Medium |
| IoT-Geräte: In der Ausgabekette wurde eine zu wenig einschränkende Firewallregel gefunden |In der Firewall wurde eine Regel gefunden, die ein zu wenig einschränkendes Muster für eine Vielzahl von IP-Adressen oder Ports enthält<br />(Keine zugehörige Richtlinie) |Medium |
| IoT-Geräte:Upgrade der TLS-Verschlüsselungssammlung erforderlich |Unsichere TLS-Konfigurationen wurden erkannt. Sofortiges Upgrade der TLS-Verschlüsselungssammlung empfohlen<br />(Keine zugehörige Richtlinie) |Medium |
| IP-Filterregel umfasst großen IP-Adressbereich |Der Quell-IP-Adressbereich einer Filterregel für zulässige IP-Adressen ist zu groß. Durch zu wenig einschränkende Regeln ist Ihr IoT Hub möglicherweise Personen mit böser Absicht ausgesetzt<br />(Keine zugehörige Richtlinie) |Medium |
|||

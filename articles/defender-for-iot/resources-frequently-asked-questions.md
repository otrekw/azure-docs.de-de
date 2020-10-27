---
title: Häufig gestellte Fragen zu Defender für IoT
description: Hier finden Sie Antworten auf die am häufigsten gestellten Fragen zu Features und Diensten zu Azure Defender für IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2020
ms.author: rkarlin
ms.openlocfilehash: 5e7eabd44ea8c56fbb102f9e48812745a31de62a
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92089195"
---
# <a name="azure-defender-for-iot-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Defender für IoT

Dieser Artikel enthält eine Liste häufig gestellter Fragen (FAQ) zu Defender für IoT sowie die zugehörigen Antworten.

## <a name="what-is-azures-unique-value-proposition-for-iot-security"></a>Was ist einzigartiges Wertangebot für IoT-Sicherheit von Azure?

Defender für IoT ermöglicht es Unternehmen, ihre bestehende Cybersicherheitsansicht auf ihre gesamte IoT-Lösung zu erweitern. Azure bietet eine ganzheitliche Ansicht Ihrer Geschäftslösung und ermöglicht es Ihnen, geschäftsbezogene Maßnahmen und Entscheidungen auf der Grundlage Ihrer Sicherheitslage und der gesammelten Daten zu treffen. Die Kombination verschiedener Sicherheitsfunktionen mit Azure IoT, Azure IoT Edge und Azure Security Center ermöglicht es Ihnen, die gewünschte Lösung mit der erforderlichen Sicherheit zu erstellen.

## <a name="our-organization-uses-proprietary-non-standard-industrial-protocols-are-they-supported"></a>Unsere Organisation verwendet proprietäre, nicht standardmäßige Branchenprotokolle. Werden sie unterstützt? 

Azure Defender für IoT bietet umfassende Protokollunterstützung. Zusätzlich zur integrierten Protokollunterstützung können Sie IoT- und OT-Geräte sichern, auf denen proprietäre, benutzerdefinierte oder von allen Standards abweichende Protokolle ausgeführt werden. Mit dem Horizon ODE SDK (Open Development Environment) können Entwickler Zergliederungs-Plug-Ins erstellen, die Netzwerkdatenverkehr basierend auf definierten Protokollen decodieren. Der Datenverkehr wird durch Dienste analysiert, die vollständige Überwachungs-, Warnungs- und Berichterstellungsfunktionen bieten. Verwenden Sie Horizon für Folgendes:
- Erweitern der Transparenz und Kontrolle, ohne ein Upgrade auf neue Versionen durchführen zu müssen
- Sichern proprietärer Informationen durch Vor-Ort-Entwicklung als externes Plug-In 
- Lokalisieren von Text für Warnungen, Ereignisse und Protokollparameter

Diese einmalige Lösung zum Entwickeln von Protokollen als Plug-Ins erfordert keine dedizierten Entwicklerteams oder Versionsfreigaben zum Unterstützen eines neuen Protokolls. Mit Horizon können Entwickler, Partner und Kunden Protokolle sicher entwickeln und Erkenntnisse und Wissen austauschen. 

## <a name="do-i-have-to-purchase-hardware-appliances-from-microsoft-partners"></a>Muss ich Hardwareappliances bei Microsoft-Partnern erwerben?
Der Azure Defender für IoT-Sensor wird auf Hardware mit bestimmten Spezifikationen ausgeführt, wie im [Hardware Specifications Guide](https://aka.ms/AzureDefenderforIoTBareMetalAppliance) (Leitfaden zu Hardwarespezifikationen) beschrieben. Kunden können zertifizierte Hardware bei Microsoft-Partnern erwerben oder anhand der bereitgestellten Stückliste selbst Hardware beschaffen. 

Zertifizierte Hardware wurde in unseren Labs auf Treiberstabilität, Paketverluste und Netzwerkgröße getestet.


## <a name="regulation-does-not-allow-us-to-connect-our-system-to-the-internet-can-we-still-utilize-defender-for-iot"></a>Aufgrund gesetzlicher Vorgaben ist es uns nicht möglich, unser System mit dem Internet zu verbinden. Können wir Defender für IoT trotzdem nutzen?

Ja, das ist möglich. Die lokale Lösung für die Azure Defender für IoT-Plattform wird als physische oder virtuelle Sensorappliance bereitgestellt. Diese erfasst Netzwerkdatenverkehr (über SPAN, RSPAN oder TAP) passiv und kann damit IT-, OT- und IoT-Netzwerke analysieren, entdecken und kontinuierlich überwachen. In größeren Unternehmen können mehrere Sensoren ihre Daten in einer lokalen Verwaltungskonsole aggregieren.

## <a name="where-in-the-network-should-i-connect-monitoring-ports"></a>An welcher Stelle im Netzwerk muss ich Überwachungsports verbinden?

Der Azure Defender für IoT-Sensor stellt eine Verbindung mit einem SPAN-Port oder einem Netzwerk-TAP her und beginnt per passiver Überwachung (ohne Agent) sofort mit der Erfassung von ICS-Netzwerkdatenverkehr. Diese Lösung hat keinerlei Auswirkungen auf OT-Netzwerke, da sie nicht im Datenpfad platziert wird und OT-Geräte nicht aktiv überprüft.

Zum Beispiel:
- Eine einzelne Appliance (virtuell oder physisch) kann sich in der DMZ-Schicht einer Produktionsstätte befinden. Dabei wird der gesamte Datenverkehr der Zelle dieser Produktionsstätte an diese Schicht weitergeleitet.
- Alternativ dazu können Sie Minisensoren in jeder Zelle der Produktionsstätte platzieren und eine cloudbasierte oder lokale Verwaltungslösung nutzen, die sich in der DMZ-Schicht der Produktionsstätte befindet. Eine andere Appliance (virtuell oder physisch) kann den Datenverkehr in der DMZ-Schicht der Produktionsstätte überwachen (für SCADA, Historian oder MES).

## <a name="how-does-defender-for-iot-compare-to-the-competition"></a>Wie hebt sich Defender für IoT vom Wettbewerb ab?

Andere Lösungen bieten eine Reihe von Funktionen, die es Kunden ermöglichen, ihre eigenen Lösungen zu erstellen. Defender für IoT hingegen bietet eine einzigartige End-to-End-IoT-Sicherheitslösung, die einen umfassenden Überblick über die Sicherheit aller Ihrer zugehörigen Azure-Ressourcen bietet. Azure ermöglicht eine schnelle Bereitstellung und vollständige Integration mit IoT Hub-Modulzwillingen für eine einfache Integration in bestehende Geräteverwaltungstools.


## <a name="do-i-have-to-be-an-azure-iot-customer"></a>Muss ich Azure-IoT-Kunde sein?

Ja. In mit der Cloud verbundenen Bereitstellungen nutzt Azure Defender für IoT die Azure IoT-Konnektivität und -Infrastruktur.
## <a name="can-i-create-my-own-alerts"></a>Kann ich meine eigenen Warnungen erstellen?

Ja. Sie können eine benutzerdefinierte Warnung für vordefinierte Verhaltensweisen wie IP-Adresse und offene Ports festlegen. Weitere Informationen zu benutzerdefinierten Warnungen und deren Ausführung finden Sie unter [Erstellen von benutzerdefinierten Warnungen](quickstart-create-custom-alerts.md).

## <a name="where-can-i-see-logs-can-i-customize-logs"></a>Wo kann ich Protokolle sehen? Kann ich Protokolle anpassen?

- Warnungen und Empfehlungen können Sie über Ihren angeschlossenen Log Analytics-Arbeitsbereich anzeigen. Im Arbeitsbereich können Sie die Speichergröße und die Dauern konfigurieren.

- Rohdaten von Ihrem Sicherheits-Agent können auch in Ihrem Log Analytics-Konto gespeichert werden. Berücksichtigen Sie Größe, Dauer, Speicherbedarf und damit verbundene Kosten, bevor Sie die Konfiguration dieser Option ändern.



## <a name="what-happens-when-the-internet-connection-stops-working"></a>Was geschieht, wenn die Internetverbindung nicht mehr funktioniert?

Solange das Gerät betrieben wird, werden die Sensoren und der Agent weiterhin ausgeführt und speichern Daten. Die Daten werden entsprechend der Größenkonfiguration im Cache für Sicherheitsmeldungen gespeichert. Wenn das Gerät wieder eine Verbindung herstellt, werden die Sicherheitsmeldungen wieder gesendet.





## <a name="next-steps"></a>Nächste Schritte

Um mehr darüber zu erfahren, wie Sie mit Defender für IoT beginnen können, lesen Sie die folgenden Artikel:

- Lesen der [Übersicht](overview.md) über Defender für IoT
- Überprüfen der [Dienstvoraussetzungen](service-prerequisites.md)
- Erfahren Sie mehr über die [Ersten Schritte](getting-started.md)
- Grundlegendes zu [Defender für IoT-Sicherheitswarnungen](concept-security-alerts.md)

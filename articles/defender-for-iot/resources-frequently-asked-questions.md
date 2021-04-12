---
title: Häufig gestellte Fragen zu Defender für IoT
description: Hier finden Sie Antworten auf die am häufigsten gestellten Fragen zu Features und Diensten zu Azure Defender für IoT.
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 0ce8ded3eea344d72679e0f8b805f45b00279b58
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104778592"
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
Der Azure Defender für IoT-Sensor wird auf Hardware mit bestimmten Spezifikationen ausgeführt, wie im [Hardware Specifications Guide](./how-to-identify-required-appliances.md) (Leitfaden zu Hardwarespezifikationen) beschrieben. Kunden können zertifizierte Hardware bei Microsoft-Partnern erwerben oder anhand der bereitgestellten Stückliste selbst Hardware beschaffen. 

Zertifizierte Hardware wurde in unseren Labs auf Treiberstabilität, Paketverluste und Netzwerkgröße getestet.


## <a name="regulation-does-not-allow-us-to-connect-our-system-to-the-internet-can-we-still-utilize-defender-for-iot"></a>Aufgrund gesetzlicher Vorgaben ist es uns nicht möglich, unser System mit dem Internet zu verbinden. Können wir Defender für IoT trotzdem nutzen?

Ja, das ist möglich. Die lokale Lösung für die Azure Defender für IoT-Plattform wird als physische oder virtuelle Sensorappliance bereitgestellt. Diese erfasst Netzwerkdatenverkehr (über SPAN, RSPAN oder TAP) passiv und kann damit IT-, OT- und IoT-Netzwerke analysieren, entdecken und kontinuierlich überwachen. In größeren Unternehmen können mehrere Sensoren ihre Daten in einer lokalen Verwaltungskonsole aggregieren.

## <a name="where-in-the-network-should-i-connect-monitoring-ports"></a>An welcher Stelle im Netzwerk muss ich Überwachungsports verbinden?

Der Azure Defender für IoT-Sensor stellt eine Verbindung mit einem SPAN-Port oder einem Netzwerk-TAP her und beginnt per passiver Überwachung (ohne Agent) sofort mit der Erfassung von ICS-Netzwerkdatenverkehr. Diese Lösung hat keinerlei Auswirkungen auf OT-Netzwerke, da sie nicht im Datenpfad platziert wird und OT-Geräte nicht aktiv überprüft.

Zum Beispiel:
- Eine einzelne Appliance (virtuell oder physisch) kann sich in der DMZ-Schicht einer Produktionsstätte befinden. Dabei wird der gesamte Datenverkehr der Zelle dieser Produktionsstätte an diese Schicht weitergeleitet.
- Alternativ dazu können Sie Minisensoren in jeder Zelle der Produktionsstätte platzieren und eine cloudbasierte oder lokale Verwaltungslösung nutzen, die sich in der DMZ-Schicht der Produktionsstätte befindet. Eine andere Appliance (virtuell oder physisch) kann den Datenverkehr in der DMZ-Schicht der Produktionsstätte überwachen (für SCADA, Historian oder MES).

## <a name="how-does-defender-for-iot-compare-to-the-competition"></a>Wie hebt sich Defender für IoT vom Wettbewerb ab?

Azure Defender für IoT bietet umfassende Sicherheit auf allen IoT-/OT-Geräten. Für **Endbenutzerorganisationen** bietet Azure Defender für IoT Sicherheit auf Netzwerkebene ohne Agents, die schnell bereitgestellt werden kann, mit verschiedenen proprietären OT-Geräten und Windows-Legacysystemen funktioniert und mit Azure Sentinel und anderen SOC-Tools interagiert. Die Bereitstellung kann vollständig lokal oder in mit Azure verbundenen Umgebungen erfolgen. Für **Hersteller von IoT-Geräten** bietet Azure Defender für IoT einfache Agents zum Integrieren von Sicherheit auf Geräteebene in neue IoT-/OT-Initiativen.

## <a name="do-i-have-to-be-an-azure-customer"></a>Muss ich Azure-Kunde sein?

Nein, für die Version von Azure Defender für IoT ohne Agents müssen Sie kein Azure-Kunde sein. Wenn Sie jedoch Warnungen an Azure Sentinel senden, Netzwerksensoren bereitstellen und deren Integrität über die Cloud überwachen sowie automatische Updates zu Software- und Bedrohungsinformationen erhalten möchten, müssen Sie die Sensoren über Azure IoT Hub mit Azure verbinden.

Für die Version von Azure Defender für IoT mit Agents müssen Sie in jedem Fall Azure-Kunde sein.

## <a name="can-i-create-my-own-alerts"></a>Kann ich meine eigenen Warnungen erstellen?

Ja, Sie können benutzerdefinierte Warnungen erstellen, die auf mehreren Parametern, z. B. IP-/MAC-Adresse, Protokolltyp, Klasse, Dienst, Funktion, Befehl usw., sowie auf in den Nutzdaten enthaltenen Werten benutzerdefinierter Tags basieren.  Weitere Informationen zu benutzerdefinierten Warnungen und deren Erstellung finden Sie unter [Erstellen benutzerdefinierter Warnungen](quickstart-create-custom-alerts.md).

## <a name="what-happens-when-the-internet-connection-stops-working"></a>Was geschieht, wenn die Internetverbindung nicht mehr funktioniert?

Solange das Gerät betrieben wird, werden die Sensoren und Agents weiterhin ausgeführt und speichern Daten. Die Daten werden entsprechend der Größenkonfiguration im Cache für Sicherheitsmeldungen gespeichert. Wenn das Gerät wieder eine Verbindung herstellt, werden die Sicherheitsmeldungen wieder gesendet.

## <a name="next-steps"></a>Nächste Schritte

Um mehr darüber zu erfahren, wie Sie mit Defender für IoT beginnen können, lesen Sie die folgenden Artikel:

- Lesen der [Übersicht](overview.md) über Defender für IoT
- Überprüfen der [Systemvoraussetzungen](quickstart-system-prerequisites.md)
- Weitere Informationen zu [Erste Schritte mit Defender für IoT](getting-started.md)
- Grundlegendes zu [Defender für IoT-Sicherheitswarnungen](concept-security-alerts.md)
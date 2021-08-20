---
title: Häufig gestellte Fragen zu Defender für IoT
description: Hier finden Sie Antworten auf die am häufigsten gestellten Fragen zu Features und Diensten zu Azure Defender für IoT.
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: e73096dbdee070a74daf700578e59f4ac181db42
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113016205"
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

## <a name="what-happens-when-the-internet-connection-stops-working"></a>Was geschieht, wenn die Internetverbindung nicht mehr funktioniert?

Solange das Gerät betrieben wird, werden die Sensoren und Agents weiterhin ausgeführt und speichern Daten. Die Daten werden entsprechend der Größenkonfiguration im Cache für Sicherheitsmeldungen gespeichert. Wenn das Gerät wieder eine Verbindung herstellt, werden die Sicherheitsmeldungen wieder gesendet.

## <a name="how-can-i-change-a-users-passwords"></a>Wie kann ich die Kennwörter eines Benutzers ändern?

Erfahren Sie, wie Sie das [Kennwort eines Benutzers](how-to-create-and-manage-users.md#change-a-users-password) für den Sensor oder die lokale Verwaltungskonsole ändern.

Sie können auch [das Kennwort für die lokale Verwaltungskonsole oder den Sensor wiederherstellen](how-to-create-and-manage-users.md#recover-the-password-for-the-on-premises-management-console-or-the-sensor).

## <a name="how-do-i-activate-the-sensor-and-on-premises-management-console"></a>Wie aktiviere ich den Sensor und die lokale Verwaltungskonsole?

Informationen zum Aktivieren Ihres Sensors finden Sie unter [Anmelden und Aktivieren des Sensors](how-to-activate-and-set-up-your-sensor.md#sign-in-and-activate-the-sensor).

Informationen zum Aktivieren Ihrer lokalen Verwaltungskonsole finden Sie unter [Aktivieren der lokalen Verwaltungskonsole](how-to-activate-and-set-up-your-on-premises-management-console.md#activate-the-on-premises-management-console).

## <a name="how-to-change-the-network-configuration"></a>Wie ändere ich die Netzwerkkonfiguration?

Sie können [die Netzwerkkonfiguration Ihres Sensors vor der Aktivierung aktualisieren](how-to-activate-and-set-up-your-sensor.md#update-sensor-network-configuration-before-activation).

Sie können die [Netzwerkkonfiguration des Sensors auch nach der Aktivierung aktualisieren](how-to-manage-individual-sensors.md#update-the-sensor-network-configuration).

Sie können mit CLI-[Befehlen](references-work-with-defender-for-iot-cli-commands.md#network-configuration) arbeiten, um [Netzwerkkonfigurationen zu ändern](references-work-with-defender-for-iot-cli-commands.md#network-configuration).

## <a name="how-do-i-check-the-sanity-of-my-deployment"></a>Wie überprüfe ich die Integrität meiner Bereitstellung?

Nachdem Sie die Software für Ihren Sensor oder die lokale Verwaltungskonsole installiert haben, müssen Sie die [Überprüfung nach der Installation](how-to-install-software.md#post-installation-validation) durchführen. Dort erfahren Sie, wie Sie die [Systemintegrität über die CLI überprüfen](how-to-install-software.md#check-system-health-by-using-the-cli), eine [Integritätsprüfung](how-to-install-software.md#sanity) durchführen und Ihre [Systemstatistik](how-to-install-software.md#system) insgesamt überprüfen.

Sie können diesen Links folgen, wenn [die Appliance nicht antwortet](how-to-install-software.md#the-appliance-isnt-responding) oder [Sie keine Verbindung über eine Webschnittstelle herstellen können](how-to-install-software.md#you-cant-connect-by-using-a-web-interface).

## <a name="next-steps"></a>Nächste Schritte

Um mehr darüber zu erfahren, wie Sie mit Defender für IoT beginnen können, lesen Sie die folgenden Artikel:

- Lesen der [Übersicht](overview.md) über Defender für IoT
- Überprüfen der [Systemvoraussetzungen](quickstart-system-prerequisites.md)
- Weitere Informationen zu [Erste Schritte mit Defender für IoT](getting-started.md)

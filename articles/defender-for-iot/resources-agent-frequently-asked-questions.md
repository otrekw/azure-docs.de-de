---
title: Häufig gestellte Fragen zum Azure Defender für IoT-Agent
description: Hier finden Sie Antworten auf die am häufigsten gestellten Fragen zum Azure Defender für IoT-Agent.
ms.topic: conceptual
ms.date: 10/07/2020
ms.openlocfilehash: 4bd7d3c9b76dfb37e53cc51e5e16b660545cb7f0
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104778660"
---
# <a name="azure-defender-for-iot-agent-frequently-asked-questions"></a>Häufig gestellte Fragen zum Azure Defender für IoT-Agent

Dieser Artikel enthält eine Liste häufig gestellter Fragen (FAQ) zum Defender für IoT-Agent und die zugehörigen Antworten.

## <a name="do-i-have-to-install-an-embedded-security-agent"></a>Muss ich einen eingebetteten Sicherheits-Agent installieren?

Die Installation des Agent auf Ihren IoT-Geräten ist nicht zwingend erforderlich, um Defender für IoT zu aktivieren. Sie können zwischen den folgenden drei Optionen wählen und erhalten je nach Ihrer Auswahl verschiedene Stufen der Sicherheitsüberwachung und -verwaltung:

- Passive, nicht-invasive (ohne Agents) Bereitstellung mithilfe von NTA-Sensoren (Network Traffic Analysis, Netzwerkdatenverkehrsanalyse) zum Überwachen und Bereitstellen umfassender Einblicke in IoT/OT-Risiken mit keinerlei Auswirkungen auf die Leistung im Netzwerk und auf Geräten
- Installieren des eingebetteten Defender für IoT-Sicherheits-Agents mit oder ohne Änderungen. Diese Option bietet ein Höchstmaß an verbesserten Sicherheitsinformationen über das Geräteverhalten und den Zugriff.

- Erstellen Ihres eigenen Agents und Implementieren des Defender für IoT-Sicherheit-Nachrichtenschemas. Diese Option ermöglicht die Verwendung von Defender für IoT-Analysetools auf Ihrem Gerätesicherheits-Agent.

- Keine Installation eines Sicherheits-Agent auf Ihren IoT-Geräten. Diese Option ermöglicht die Kommunikationsüberwachung des IoT Hubs mit reduzierten Sicherheitsüberwachungs- und -verwaltungsfunktionen.

## <a name="what-does-the-defender-for-iot-agent-do"></a>Was leistet der Defender für IoT-Agent?

Der Defender für IoT-Agent bietet Bedrohungsschutz auf Geräteebene für Gerätekonfiguration, -verhalten und -zugriff (durch Scannen der Konfiguration), Prozess und Konnektivität. Der Defender für IoT-Sicherheits-Agent scannt keine geschäftsrelevanten Daten oder Aktivitäten.

Der Defender für IoT-Sicherheits-Agent ist Open Source und auf GitHub in 32-Bit- und 64-Bit-Versionen für Windows und Linux verfügbar: https://github.com/Azure/Azure-IoT-Security.

## <a name="what-are-the-dependencies-and-prerequisites-of-the-agent"></a>Wie lauten die Abhängigkeiten und Voraussetzungen für den Agent?

Defender für IoT unterstützt eine Vielzahl von Plattformen. Informationen zur Unterstützung für Ihre spezifischen Geräte finden Sie unter [Unterstützte Geräteplattformen](how-to-deploy-agent.md).

## <a name="which-data-is-collected-by-the-agent"></a>Welche Daten sammelt der Agent?

Der Agent sammelt Daten zu Konnektivität, Zugriff, Firewallkonfiguration, Prozessliste und BS-Baseline.

## <a name="how-much-data-will-the-agent-generate"></a>Wie viele Daten generiert der Agent?

Die Generierung von Daten durch den Agent wird durch Gerät, Anwendung, Konnektivitätsart und Konfiguration des Kunde-Agent gesteuert. Aufgrund der hohen Variabilität zwischen Geräten und IoT-Lösungen empfehlen wir, den Agent zunächst in einem Labor oder einer Testumgebung einzusetzen, um die spezifische Konfiguration zu beobachten, zu lernen und einzustellen, die Ihren Bedürfnissen entspricht, und gleichzeitig die Menge der generierten Daten zu messen. Nach dem Start des Dienstes gibt der Defender für IoT-Agent operative Empfehlungen zur Optimierung des Agent-Durchsatzes, um Sie bei der Konfiguration und Anpassung zu unterstützen.

## <a name="do-agent-messages-use-up-quota-from-iot-hub"></a>Wird für Nachrichten des Agent Kontingent von IoT Hub verwendet?

Ja. Die vom Agent übertragenen Daten werden in Ihrem IoT Hub-Kontingent gezählt.

## <a name="what-next-ive-installed-an-agent-and-dont-see-any-activities-or-logs"></a>Nächste Schritte Ich habe einen Agent installiert, es werden aber keine Aktivitäten oder Protokolle angezeigt...

1. Überprüfen Sie, ob der [Typ des Agent mit der angegebenen Betriebssystemplattform Ihres Geräts übereinstimmt](how-to-deploy-agent.md).

1. Vergewissern Sie sich, dass der [Agent auf dem Gerät ausgeführt wird](how-to-agent-configuration.md).

1. Überprüfen Sie, ob der [Dienst erfolgreich](quickstart-onboard-iot-hub.md) für **Security** in Ihrem IoT Hub aktiviert wurde.

1. Überprüfen Sie, ob das Gerät im [IoT Hub mit dem Defender für IoT-Modul konfiguriert](quickstart-create-security-twin.md) ist.

Wenn die Aktivitäten oder Protokolle immer noch nicht verfügbar sind, wenden Sie sich an Ihren Defender für IoT-Partner, um zusätzliche Hilfe zu erhalten.

## <a name="what-happens-when-the-internet-connection-stops-working"></a>Was geschieht, wenn die Internetverbindung nicht mehr funktioniert?

Solange das Gerät betrieben wird, werden die Sensoren und Agents weiterhin ausgeführt und speichern Daten. Die Daten werden entsprechend der Größenkonfiguration im Cache für Sicherheitsmeldungen gespeichert. Wenn das Gerät wieder eine Verbindung herstellt, werden die Sicherheitsmeldungen wieder gesendet.

## <a name="can-the-agent-affect-the-performance-of-the-device-or-other-installed-software"></a>Kann der Agent die Leistung des Geräts oder einer anderen installierten Software beeinträchtigen?

Der Agent verbraucht wie jede andere Anwendung/jeder andere Prozess Computerressourcen und sollte die normale Geräteaktivität nicht stören. Der Ressourcenverbrauch auf dem Gerät, auf dem der Agent ausgeführt wird, ist mit dessen Einrichtung und Konfiguration gekoppelt. Wir empfehlen, Ihre Agentkonfiguration in einer geschlossenen Umgebung zusammen mit der Interoperabilität mit Ihren anderen IoT-Anwendungen und -Funktionen zu testen, bevor Sie versuchen, sie in einer Produktionsumgebung einzusetzen.

## <a name="im-making-some-maintenance-on-the-device-can-i-turn-off-the-agent"></a>Ich führe einige Wartungsmaßnahmen auf dem Gerät aus. Kann ich den Agent deaktivieren?

Der Agent kann nicht deaktiviert werden.

## <a name="is-there-a-way-to-test-if-the-agent-is-working-correctly"></a>Gibt es eine Möglichkeit zum Testen, ob der Agent ordnungsgemäß funktioniert?

Wenn der Agent die Kommunikation einstellt oder keine Sicherheitsmeldungen sendet, wird eine Warnung **Gerät ist still** generiert.

## <a name="next-steps"></a>Nächste Schritte

Um mehr darüber zu erfahren, wie Sie mit Defender für IoT beginnen können, lesen Sie die folgenden Artikel:

- Lesen der [Übersicht](overview.md) über Defender für IoT
- Überprüfen der [Systemvoraussetzungen](quickstart-system-prerequisites.md)
- Weitere Informationen zu [Erste Schritte mit Defender für IoT](getting-started.md)
- Grundlegendes zu [Defender für IoT-Sicherheitswarnungen](concept-security-alerts.md)

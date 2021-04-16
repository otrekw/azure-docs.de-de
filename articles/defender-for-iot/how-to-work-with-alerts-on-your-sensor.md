---
title: Informationen zu Sensorwarnungen
description: Arbeiten Sie mit Warnungen, um die Sicherheit und den Betrieb Ihres Netzwerks zu verbessern.
ms.date: 11/30/2020
ms.topic: how-to
ms.openlocfilehash: 178d3aedb44c29f53aab481894defeb4b9355d39
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104781346"
---
# <a name="about-sensor-alerts"></a>Informationen zu Sensorwarnungen

Mit Warnungen können Sie die Sicherheit und den Betrieb Ihres Netzwerks verbessern. Warnungen bieten Informationen zu folgenden Aspekten:

- Abweichungen von autorisierten Netzwerkaktivitäten

- Protokoll- und operative Anomalien

- Verdächtiger Malwaredatenverkehr

:::image type="content" source="media/how-to-work-with-alerts-sensor/address-scan-detected-screen.png" alt-text="Erkannte Adressenüberprüfung":::

Warnungsverwaltungsoptionen ermöglichen Benutzern Folgendes:

- Sie können Sensoren anweisen, erkannte Aktivitäten als autorisierten Datenverkehr zu erlernen.

- Sie können die Überprüfung der Warnung bestätigen.

- Sie können Sensoren anweisen, mit identischen Geräten und vergleichbarem Datenverkehr erkannte Ereignisse stumm zu schalten.

Es sind zusätzliche Tools verfügbar, mit denen Sie die Untersuchung von Warnungen verbessern und beschleunigen können. Beispiel:

  - Sie können Anweisungskommentare für Warnungsprüfer hinzufügen.

  - Sie können Warnungsgruppen für die Anzeige in SOC-Lösungen erstellen. 

  - Suchen nach bestimmten Warnungen, Überprüfen der zugehörigen PCAP-Dateien, Anzeigen der erkannten Geräte und anderer verbundener Geräte in der Gerätezuordnung oder Senden von Warnungsdetails an Partnersysteme.

  - Sie können Warnungen an Partneranbieter weiterleiten: SIEM-Systeme, MSSP-Systeme usw.

## <a name="alerts-and-engines"></a>Warnungen und Module

Warnungen werden ausgelöst, wenn Sensormodule Änderungen im Netzwerkdatenverkehr und Verhalten erkennen, die Ihre Aufmerksamkeit erfordern. In diesem Artikel werden die Typen von Warnungen beschrieben, die von den einzelnen Modulen ausgelöst werden.

| Warnungstyp | BESCHREIBUNG |
|-|-|
| Warnungen zu Richtlinienverstößen | Diese Warnungen werden ausgelöst, wenn das Richtlinienverstoßmodul eine Abweichung vom zuvor erlernten Datenverkehr erkennt. Beispiel: <br /> - Ein neues Gerät wird erkannt.  <br /> - Auf einem Gerät wird eine neue Konfiguration erkannt. <br /> - Ein nicht als Programmiergerät definiertes Gerät führt eine Programmieränderung aus. <br /> - Eine Firmwareversion wurde geändert. |
| Warnungen zu Protokollverletzungen | Diese Warnungen werden ausgelöst, wenn das Protokollverletzungsmodul Paketstrukturen oder Feldwerte erkennt, die nicht mit der Protokollspezifikation übereinstimmen. | 
| Vorgangswarnungen | Diese Warnungen werden ausgelöst, wenn das Vorgangsmodul Netzwerkbetriebsvorfälle erkennt oder ein Gerät nicht ordnungsgemäß funktioniert. Beispiele: Ein Netzwerkgerät wurde über einen Befehl zum Beenden der SPS beendet, oder eine Schnittstelle eines Sensors hat die Überwachung des Datenverkehrs beendet. |
| Malwarewarnungen | Diese Warnungen werden ausgelöst, wenn das Malwaremodul schädliche Netzwerkaktivitäten erkennt. Beispiel: Das Modul erkennt einen bekannten Angriff, z. B. Conficker. |
| Anomaliewarnungen | Diese Warnungen werden ausgelöst, wenn das Anomaliemodul eine Abweichung erkennt. Beispiel: Ein nicht als Scangerät definiertes Gerät führt Netzwerkscans aus. |

Es sind Tools zum Aktivieren und Deaktivieren von Sensormodulen verfügbar. Von deaktivierten Modulen werden keine Warnungen ausgelöst. Weitere Informationen finden Sie unter [Steuern, welcher Datenverkehr überwacht wird](how-to-control-what-traffic-is-monitored.md).

## <a name="alerts-and-sensor-reporting"></a>Warnungen und Sensorberichte

In Warnungen reflektierte Aktivitäten können beim Generieren von Data Mining-, Risikobewertungs- und Angriffsvektorberichten berechnet werden. Wenn Sie diese Ereignisse verwalten, aktualisiert der Sensor die Berichte entsprechend.

Beispiel:

  - Nicht autorisierte Verbindungen zwischen einem Gerät in einem definierten Subnetz und Geräten, die sich außerhalb des Subnetzes (öffentlich) befinden, werden im Data Mining-Bericht *Internetaktivität* und im Risikobewertungsabschnitt *Internetverbindungen* angezeigt. Nachdem diese Geräte autorisiert (erlernt) wurden, werden sie beim Generieren dieser Berichte berechnet.

  - Auf Netzwerkgeräten erkannte Malwareereignisse werden in Risikobewertungsberichten gemeldet. Wenn Warnungen zu Malwareereignissen *stummgeschaltet* werden, können betroffene Geräte nicht im Risikobewertungsbericht berechnet werden.

## <a name="see-also"></a>Weitere Informationen

- [Lernmodus und Smart IT-Lernmodus](how-to-control-what-traffic-is-monitored.md#learning-and-smart-it-learning-modes)
- [Anzeigen der in Warnungen angegebenen Informationen](how-to-view-information-provided-in-alerts.md)
- [Verwalten des Warnungsereignisses](how-to-manage-the-alert-event.md)
- [Beschleunigen des Warnungsworkflows](how-to-accelerate-alert-incident-response.md)

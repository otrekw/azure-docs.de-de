---
title: Informationen zur Splunk-Integration
titleSuffix: Azure Defender for IoT
description: 'Um eine fehlende Sichtbarkeit in Bezug auf die Sicherheit und Resilienz von OT-Netzwerken zu beheben, wurde für Defender für IoT die Defender für IoT-, IIoT- und ICS-Bedrohungsüberwachungsanwendung für Splunk entwickelt: eine native Integration von Defender für IoT und Splunk, die einen einheitlichen Ansatz für IT- und OT-Sicherheit ermöglicht.'
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/4/2021
ms.topic: article
ms.service: azure
ms.openlocfilehash: 91d877d644b4b5ca7231f5f81f9163a0fd3cbe25
ms.sourcegitcommit: 6628bce68a5a99f451417a115be4b21d49878bb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2021
ms.locfileid: "98556787"
---
# <a name="defender-for-iot-and-ics-threat-monitoring-application-for-splunk"></a>Defender für IoT und ICS-Bedrohungsüberwachungsanwendung für Splunk

Defender für IoT mindert IIoT-, ICS- und SCADA-Risiken mit patentierten, ICS-fähigen, selbstlernenden Engines, die in weniger als einer Imagestunde und ohne Agents, Regeln oder Signaturen und spezielle Skills oder Vorwissen der Umgebung unmittelbare Einblicke in ICS-Geräte, Sicherheitsrisiken und Bedrohungen liefern.

Um eine fehlende Sichtbarkeit in Bezug auf die Sicherheit und Resilienz von OT-Netzwerken zu beheben, wurde für Defender für IoT die Defender für IoT-, IIoT- und ICS-Bedrohungsüberwachungsanwendung für Splunk entwickelt: eine native Integration von Defender für IoT und Splunk, die einen einheitlichen Ansatz für IT- und OT-Sicherheit ermöglicht.

> [!Note]
> Verweise auf CyberX beziehen sich auf Azure Defender für IoT.

## <a name="about-the-splunk-application"></a>Informationen zur Splunk-Anwendung

Die Anwendung bietet SOC-Analysten einen mehrdimensionalen Einblick in die speziellen OT-Protokolle und IIoT-Geräte, die in Industrieumgebungen bereitgestellt werden, sowie ICS-fähige Verhaltensanalysen zur schnellen Erkennung von verdächtigem oder anomalem Verhalten. Außerdem ermöglicht die Anwendung die Reaktion auf IT- und OT-Vorfälle über ein zentrales Unternehmens-SOC. Im Hinblick auf die fortschreitende Verschmelzung von IT und OT zur Unterstützung neuer IIoT-Initiativen, z. B. intelligente Computer und Echtzeitintelligenz, stellt dies eine wichtige Weiterentwicklung dar.

Die Splunk-Anwendung kann lokal installiert oder in einer Cloud ausgeführt werden. Bei der Integration in Defender für IoT werden beide Bereitstellungen unterstützt.

## <a name="about-the-integration"></a>Informationen zur Integration

Die Integration von Defender für IoT und Splunk über die native Anwendung ermöglicht Benutzern Folgendes:

- Reduzierung der Zeit, die Industrieorganisationen und kritische Infrastrukturorganisationen benötigen, um Cyberbedrohungen zu erkennen, zu untersuchen und darauf zu reagieren

- Echtzeitinformationen zu OT-Risiken

- Korrelieren von Defender für IoT-Warnungen mit Splunk Enterprise Security Threat Intelligence-Repositorys

- Überwachung und Reaktion über eine zentrale Benutzeroberfläche

[:::image type="content" source="media/integration-splunk/splunk-mainpage-v2.png" alt-text="Hauptseite des Splunk-Tools":::](media/integration-splunk/splunk-mainpage-v2.png#lightbox)

:::image type="content" source="media/integration-splunk/alerts.png" alt-text="Seite für Warnungen in Splunk":::

Mit der Anwendung können Splunk-Administratoren die von Defender für IoT gesendeten Warnungen analysieren und die gesamte OT-Sicherheitsbereitstellung überwachen, einschließlich folgender Details:

- Mit welcher der fünf Analyse-Engines die Warnung erkannt wurde

- Über welches Protokoll die Warnung generiert wurde

- Von welchem Defender für IoT-Sensor die Warnung generiert wurde

- Schweregrad der Warnung

- Quelle und Ziel der Kommunikation

## <a name="requirements"></a>Anforderungen

### <a name="version-requirements"></a>Versionsanforderungen

Die folgenden Versionen sind erforderlich.

- Defender für IoT ab Version 2.4

- Splunkbase ab Version 11

- Splunk Enterprise ab Version 7.2
  
## <a name="download-the-application"></a>Herunterladen der Anwendung

Laden Sie *CyberX ICS Threat Monitoring for Splunk* über [Splunkbase](https://splunkbase.splunk.com/app/4313/) herunter.

## <a name="splunk-permission-requirements"></a>Erforderliche Splunk-Berechtigungen

Die folgenden Splunk-Berechtigungen sind erforderlich:

- Benutzer mit Berechtigungen der Benutzerrolle *admin*.

## <a name="send-defender-for-iot-alerts-to-splunk"></a>Senden von Defender für IoT-Warnungen an Splunk

Defender für IoT-Warnungen enthalten Informationen zu einem umfangreichen Spektrum an Sicherheitsereignissen, beispielsweise:

- Abweichungen von der erlernten Baseline für Netzwerkaktivitäten

- Schadsoftwareerkennungen

- Erkennungen auf der Grundlage verdächtiger Ablaufänderungen

- Netzwerkanomalien

- Abweichungen von Protokollspezifikationen

:::image type="content" source="media/integration-splunk/address-scan.png" alt-text="Fenster für Erkennungen":::

Sie können Defender für IoT so konfigurieren, dass Warnungen an den Splunk-Server gesendet werden. Dort werden die Warnungsinformationen auf dem Splunk Enterprise-Dashboard angezeigt.

:::image type="content" source="media/integration-splunk/alerts-and-details.png" alt-text="Anzeigen aller Warnungen und zugehörigen Details":::

Die folgenden Warnungsinformationen werden an den Splunk-Server gesendet.

- Datum und Uhrzeit der Warnung

- Die Defender für IoT-Engine, die das Ereignis erkannt hat: Protokollverletzung, Richtlinienverletzung, Schadsoftware, Anomalie oder Betrieb

- Der Titel der Warnung

- Die Warnmeldung

- Der Schweregrad der Warnung: Warnung, Gering, Wichtig oder Kritisch

- Der Name des Quellgeräts

- Die IP-Adresse des Quellgeräts.

- Der Name des Zielgeräts

- Die IP-Adresse des Zielgeräts

- Die IP-Adresse der Defender für IoT-Plattform (Host)

- Der Name der Appliance der Defender für IoT-Plattform (Quelltyp)

Eine Beispielausgabe:

| Time | Ereignis |
|--|--|
| 7/23/15<br />9:28:31.000 PM | **Warnung der Defender für IoT-Plattform:** A device was stopped by a PLC Command<br /><br />**Typ:** Operational Violation <br /><br />**Schweregrad**: Hauptversion <br /><br />**Quellenname:** my_device1 <br /><br />**Quell-IP-Adresse:** 192.168.110.131 <br /><br />**Zielname:** my_device2<br /><br /> **Ziel-IP-Adresse:** 10.140.33.238 <br /><br />**Meldung:** A network device was stopped using a Stop PLC command. This device will not operate until a Start command is sent. 192.168.110.131 was stopped by 10.140.33.238 (a Siemens S7 device), using a PLC Stop command.<br /><br />**Host**: 192.168.90.43 <br /><br />**Quelltyp:** Sensor_Agent |

## <a name="define-alert-forwarding-rules"></a>Definieren von Warnungsweiterleitungsregeln

Verwenden Sie *Weiterleitungsregeln* in Defender für IoT, um Warnungsinformationen an Splunk-Server zu senden.

Es sind verschiedene Optionen verfügbar, um die Warnungsregeln basierend auf folgenden Elementen anzupassen:

- Spezifische erkannte Protokolle

- Schweregrad des Ereignisses

- Defender für IoT-Engine, die Ereignisse erkennt

So erstellen Sie eine Weiterleitungsregel

1. Wählen Sie im linken Bereich des Sensors oder der lokalen Verwaltungskonsole die Option **Weiterleitung** aus.

    :::image type="content" source="media/integration-splunk/forwarding.png" alt-text="Auswählen der blauen Schaltfläche „Create Forwarding Rule“ (Weiterleitungsregel erstellen)":::

1. Wählen Sie **Create Forwarding Rule** (Weiterleitungsregel erstellen) aus. Definieren Sie im Fenster **Create Forwarding Rule** (Weiterleitungsregel erstellen) die Parameter für die Regel.

    :::image type="content" source="media/integration-splunk/forwarding-rule.png" alt-text="Erstellen der Parameter für die Weiterleitungsregel":::

    | Parameter | Beschreibung |
    |--|--|
    | **Name** | Der Name der Weiterleitungsregel |
    | **Schweregrad** | Der Mindestschweregrad für die Weiterleitung von Vorfällen. Wenn z. B. „Gering“ ausgewählt ist, werden Warnungen mit einem geringen Schweregrad und alle Warnungen mit höheren Schweregraden weitergeleitet. |
    | **Protokolle** | Standardmäßig sind alle Protokolle ausgewählt. Wenn Sie ein bestimmtes Protokoll angeben möchten, wählen Sie **Spezifisch** und dann das Protokoll aus, für das die Regel angewandt wird. |
    | **Engines** | Standardmäßig werden alle Sicherheits-Engines verwendet. Wenn Sie eine bestimmte Sicherheits-Engine auswählen möchten, für die die Regel angewandt wird, wählen Sie **Spezifisch** und dann die Engine aus. |
    | **Systembenachrichtigungen** | Weiterleiten des Online- oder Offlinestatus des Sensors. Diese Option ist nur verfügbar, wenn Sie bei Central Manager angemeldet sind. |                                            |

1. Wenn Sie in Defender für IoT festlegen möchten, dass Ressourceninformationen an Splunk gesendet werden, wählen Sie **Aktion** und dann **Send to Splunk Server** (An Splunk Server senden) aus.

1. Geben Sie die folgenden Splunk-Parameter ein.

    :::image type="content" source="media/integration-splunk/parameters.png" alt-text="In diesem Fenster einzugebende Splunk-Parameter":::

    | Parameter | BESCHREIBUNG |
    |--|--|
    | **Host** | Adresse des Splunk-Servers |
    | **Port** | 8089 |
    | **Benutzername** | Benutzername für den Splunk-Server |
    | **Kennwort** | Kennwort für den Splunk-Server |

1. Wählen Sie **Senden** aus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Weiterleiten von Warnungsinformationen](how-to-forward-alert-information-to-partners.md).

---
title: Grundlegendes zur Cisco ISE pxGrid-Integration
description: Die Überbrückung der Funktionen von Defender für IoT mit Cisco ISE pxGrid ermöglicht Sicherheitsteams bisher unerreichte Einblicke in die Geräte im Ökosystem des Unternehmens.
ms.date: 12/28/2020
ms.topic: how-to
ms.openlocfilehash: 00151f2e407c65d024f3bd59bdaa85a08ae677f4
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104784083"
---
# <a name="about-the-cisco-ise-pxgrid-integration"></a>Grundlegendes zur Cisco ISE pxGrid-Integration

Defender für IoT ist die einzige von Blue Team-Experten entwickelte ICS- und IoT-Plattform für Cybersicherheit mit einer Erfolgsbilanz für den Schutz kritischer nationaler Infrastrukturen und die einzige Plattform mit patentierter ICS-fähiger Bedrohungsanalyse und maschinellem Lernen. Defender für IoT ermöglicht Folgendes:

- Unmittelbare Einblicke in ICS-Geräte und bekannte und Zero-Day-Bedrohungen.

- ICS-bezogene Deep Embedded-Kenntnisse zu OT-Protokollen, Geräten, Anwendungen und deren Verhalten.

- Automatisierte ICS-Technologie für die Bedrohungsmodellierung, mit der die wahrscheinlichsten Pfade von gezielten ICS-Angriffen über proprietäre Analysen vorhergesagt werden können.

## <a name="powerful-device-visibility-and-control"></a>Wertvolle Transparenz- und Kontrollmechanismen für Geräte

Die Integration von Defender für IoT in Cisco ISE pxGrid stellt eine neue Ebene in Bezug auf die zentrale Transparenz, Überwachung und Kontrolle für OT-Szenarien dar.

Diese Plattformüberbrückung ermöglicht automatisierte Gerätetransparenz und Schutzmaßnahmen für bisher nicht erreichbare ICS- und IIoT-Geräte.

### <a name="ics-and-iiot-device-visibility-comprehensive-and-deep"></a>Transparenz für ICS- und IIoT-Geräte: Umfassend und eingehend

Die patentierte Defender für IoT-Technologie ermöglicht eine umfassende und eingehende Ermittlung von ICS- und IIoT-Geräten und eine Bestandsverwaltung für Unternehmensdaten.

Gerätetypen, Hersteller, offene Ports, Seriennummern, Firmwarerevisionen, IP-Adressen, MAC-Adressdaten usw. werden in Echtzeit aktualisiert. Auf dieser Basis können mit Defender für IoT die Bereiche Transparenz, Ermittlung und Analyse weiter verbessert werden, indem die Integration mit kritischen Unternehmensdatenquellen durchgeführt wird. Beispiele hierfür sind CMDBs, DNS, Firewalls und Web-APIs.

Darüber hinaus werden auf der Defender für IoT-Plattform Verfahren zur passiven Überwachung und für optionale selektive Tests kombiniert, um in Organisationen aus dem Bereich der Industrie oder der kritischen Infrastruktur die Bereitstellung von präzisen und ausführlichen Bestandsdaten zu ermöglichen.

### <a name="bridged-capabilities"></a>Funktionsüberbrückung

Die Überbrückung dieser Funktionen mit Cisco ISE pxGrid schafft für Sicherheitsteams eine bisher unerreichte Transparenz in Bezug auf die Geräte im Ökosystem eines Unternehmens.

Durch die nahtlose, stabile Integration mit Cisco ISE pxGrid wird sichergestellt, dass kein OT-Gerät bei der Ermittlung vergessen wird und keine Geräteinformationen übersehen werden.

:::image type="content" source="media/integration-cisco-isepxgrid-integration/endpoint-categories.png" alt-text="Beispiel: OUI der Endpunktkategorien":::

:::image type="content" source="media/integration-cisco-isepxgrid-integration/endpoints.png" alt-text="Beispielendpunkte im System":::  

:::image type="content" source="media/integration-cisco-isepxgrid-integration/attributes.png" alt-text="Screenshot: Attribute im System":::  

### <a name="use-case-coverage-ise-policies-based-on-defender-for-iot-attributes"></a>Abdeckung von Anwendungsfällen: Auf Defender für IoT-Attributen basierende ISE-Richtlinien

Nutzen Sie leistungsstarke ISE-Richtlinien, die auf Deep Learning-Funktionen von Defender für IoT basieren, um die Anforderungen für ICS- und IoT-Anwendungsfälle zu erfüllen.

Bei Verwendung von Richtlinien können Sie den Sicherheitszyklus abschließen und für Ihr Netzwerk in Echtzeit die Konformität herstellen.

Beispielsweise können Kunden die ICS- und IoT-Attribute von Defender für IoT nutzen, um Richtlinien für die folgenden Anwendungsfälle zu erstellen:

- Erstellen einer Autorisierungsrichtlinie, um bekannte und autorisierte Geräte in einer vertraulichen Zone anhand von zulässigen Attributen zuzulassen (z. B. spezifische Firmwareversion für speicherprogrammierbare Steuerungen von Rockwell Automation)

- Benachrichtigen von Sicherheitsteams, wenn in einer anderen Zone als einer OT-Zone ein ICS-Gerät erkannt wird

- Durchführen von Korrekturen für Computer mit veralteten oder nicht konformen Anbietern

- Stellen unter Quarantäne und Blockieren von Geräten nach Bedarf

- Erstellen von Berichten zu speicherprogrammierbaren Steuerungen oder Remote-Telemetrieeinheiten, für die Firmware mit bekannten Sicherheitsrisiken (CVEs) ausgeführt wird

### <a name="about-this-article"></a>Informationen zum Artikel

In diesem Artikel wird beschrieben, wie Sie pxGrid und die Defender für IoT-Plattform einrichten, um sicherzustellen, dass von Defender für IoT OT-Attribute für Cisco ISE eingefügt werden.

### <a name="getting-more-information"></a>Weitere Informationen

Weitere Informationen zu den Anforderungen an die Cisco ISE pxGrid-Integration finden Sie unter <https://www.cisco.com/c/en/us/products/security/pxgrid.html>.

## <a name="integration-system-requirements"></a>Systemanforderungen für die Integration

In diesem Artikel werden die Systemanforderungen für die Integration beschrieben:

Defender für IoT-Anforderungen

- Defender für IoT-Version 2.5

Cisco-Anforderungen

- pxGrid-Version 2.0

- Cisco ISE-Version 2.4

Netzwerkanforderungen

- Vergewissern Sie sich, dass die Defender für IoT-Appliance Zugriff auf die Cisco ISE-Verwaltungsschnittstelle hat.

- Stellen Sie sicher, dass Sie über CLI-Zugriff auf alle Defender für IoT-Appliances Ihres Unternehmens verfügen.

> [!NOTE]
> Nur Geräte mit MAC-Adressen werden mit Cisco ISE pxGrid synchronisiert.

## <a name="cisco-ise-pxgrid-setup"></a>Cisco ISE pxGrid-Einrichtung

Dieser Artikel beschreibt, wie Sie:

- Einrichten der Kommunikation mit pxGrid

- Abonnieren des Endpunktgerät-Themas

- Generieren von Zertifikaten

- Festlegen von pxGrid-Einstellungen

## <a name="set-up-communication-with-pxgrid"></a>Einrichten der Kommunikation mit pxGrid

In diesem Artikel wird beschrieben, wie Sie die Kommunikation mit pxGrid einrichten.

Richten Sie die Kommunikation wie folgt ein:

1. Melden Sie sich bei Cisco ISE an.

1. Wählen Sie **Verwaltung**>**System** und **Bereitstellung** aus.

1. Wählen Sie den erforderlichen Knoten aus. Aktivieren Sie auf der Registerkarte „Allgemeine Einstellungen“ das Kontrollkästchen **pxGrid**.

    :::image type="content" source="media/integration-cisco-isepxgrid-integration/pxgrid.png" alt-text="Sicherstellen der Auswahl des Kontrollkästchens „pxGrid“":::

1. Wählen Sie die Registerkarte für die **Profiling Configuration** (Profilerstellungskonfiguration) aus.

1. Aktivieren Sie das Kontrollkästchen **pxGrid**. Fügen Sie eine Beschreibung hinzu.

    :::image type="content" source="media/integration-cisco-isepxgrid-integration/profile-configuration.png" alt-text="Screenshot: Hinzufügen einer Beschreibung":::

## <a name="subscribe-to-the-endpoint-device-topic"></a>Abonnieren des Endpunktgerät-Themas

Vergewissern Sie sich, dass für den ISE pxGrid-Knoten das Endpunktgerät-Thema abonniert wurde. Navigieren Sie zu **Administration**>**pxGrid Services**>**Web Clients** (Verwaltung > pxGrid-Dienste > Webclients). Hier können Sie sich vergewissern, dass das Endpunktgerät-Thema von ISE abonniert wurde.

## <a name="generate-certificates"></a>Generieren von Zertifikaten

In diesem Artikel wird beschrieben, wie Sie Zertifikate generieren.

Führen Sie das Generieren wie folgt durch:

1. Wählen Sie **Administration** > **pxGrid Services** (Verwaltung > pxGrid-Dienste) und dann **Zertifikate** aus.

    :::image type="content" source="media/integration-cisco-isepxgrid-integration/certificates.png" alt-text="Auswählen der Registerkarte „Zertifikate“ zum Generieren eines Zertifikats":::

1. Wählen Sie im Feld **I Want To** (Ziel) die Option **Generate a single certificate (without a certificate signing request)** (Einzelnes Zertifikat generieren (ohne Zertifikatsignieranforderung)) aus.

1. Füllen Sie die restlichen Felder aus, und wählen Sie anschließend **Erstellen** aus.

1. Erstellen Sie den Clientzertifikatschlüssel und das Serverzertifikat, und konvertieren Sie diese Elemente in das Java-Keystore-Format. Sie müssen diese Elemente auf jeden Defender für IoT-Sensor Ihres Netzwerks kopieren.

## <a name="define-pxgrid-settings"></a>Festlegen von pxGrid-Einstellungen

Definieren Sie die Einstellungen wie folgt:

1. Wählen Sie **Administration** > **pxGrid Services** (Verwaltung > pxGrid-Dienste) und dann **Einstellungen** aus.

1. Aktivieren Sie die Optionen **Automatically approve new certificate-based accounts** (Neue zertifikatbasierte Konten automatisch genehmigen) und **Allow password-based account creation** (Kennwortbasierte Kontoerstellung zulassen).

  :::image type="content" source="media/integration-cisco-isepxgrid-integration/allow-these.png" alt-text="Sicherstellen, dass beide Kontrollkästchen aktiviert sind":::

## <a name="set-up-the-defender-for-iot-appliances"></a>Einrichten der Defender für IoT-Appliances

In diesem Artikel wird beschrieben, wie Sie Defender für IoT-Appliances für die Kommunikation mit pxGrid einrichten. Die Konfiguration sollte auf allen Defender für IoT-Appliances durchgeführt werden, die Daten in Cisco ISE einfügen.

Richten Sie die Appliances wie folgt ein:

1. Melden Sie sich bei der Sensor-CLI an.

1. Kopieren Sie das Element `trust.jks`, das zuvor auf dem Sensor erstellt wurde. Kopieren Sie das Element an den folgenden Speicherort: `/var/cyberx/properties/`.

1. Bearbeiten Sie `/var/cyberx/properties/pxgrid.properties`:

    1. Fügen Sie einen Schlüssel und eine Vertrauensstellung hinzu, und speichern Sie anschließend die Dateinamen und Kennwörter.

    2. Fügen Sie den Hostnamen der pxGrid-Instanz hinzu.

    3. `Enabled=true`

1. Starten Sie die Appliance neu.

1. Wiederholen Sie diese Schritte für alle Appliances Ihres Unternehmens, von denen Daten eingefügt werden.

## <a name="view-and-manage-detections-in-cisco-ise"></a>Anzeigen und Verwalten von Erkennungen in Cisco ISE

1. Endpunkterkennungen werden auf der Registerkarte **Sichtbarkeit** > **Endpunkte** für den ISE-Kontext angezeigt.

1. Wählen Sie **Richtlinien** > **Profilerstellung** > **Hinzufügen** > **Regeln** >  **+ Bedingung** > **Neue Bedingung erstellen** aus.

1. Wählen Sie **Attribut** aus, und verwenden Sie die IoT-Gerätewörterbücher, um basierend auf den eingefügten Attributen eine Regel für die Profilerstellung zu erstellen. Die folgenden Attribute werden eingefügt.

    - Gerätetyp

    - Hardwarerevision

    - IP-Adresse

    - MAC-Adresse

    - Name

    - Product ID

    - Protocol

    - Seriennummer

    - Softwarerevision

    - Hersteller

Es werden nur Geräte mit MAC-Adressen synchronisiert.

## <a name="troubleshooting-and-logs"></a>Problembehandlung und Protokolle

Die Protokolle finden Sie an den folgenden Orten:

- `/var/cyberx/logs/pxgrid.log`

- `/var/cyberx/logs/core.log`

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungsinformationen weiterleiten](how-to-forward-alert-information-to-partners.md).

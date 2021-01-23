---
title: Informationen zur Forescout-Integration
titleSuffix: Azure Defender for IoT
description: Die Azure Defender für IoT-Integration mit der Forescout-Plattform bietet eine neue Ebene zentralisierter Sichtbarkeit, Überwachung und Kontrolle für IoT- und OT-Szenarien.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/17/2021
ms.topic: article
ms.service: azure
ms.openlocfilehash: faa53c770d0d6caac471e770c80b4dfd5c5ff603
ms.sourcegitcommit: 6628bce68a5a99f451417a115be4b21d49878bb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2021
ms.locfileid: "98556797"
---
# <a name="about-the-forescout-integration"></a>Informationen zur Forescout-Integration

Azure Defender für IoT bietet eine ICS- und IoT-Cybersicherheitsplattform, die von Blue Team-Experten mit umfangreichen Kenntnissen der Verteidigung kritischer nationaler Infrastrukturen erstellt wurde. Defender für IoT ist die einzige Plattform mit patentierter ICS-fähiger Bedrohungsanalyse und Machine Learning. Defender für IoT bietet Folgendes:

- Unmittelbare Einblicke in ICS in der Gerätelandschaft und eine umfangreiche Palette von Details zu Attributen.
- ICS-fähige Deep Embedded-Kenntnisse zu OT-Protokollen, Geräten, Anwendungen und deren Verhalten.
- Unmittelbare Einblicke in Sicherheitsrisiken und bekannte und Zero-Day-Bedrohungen.
- Automatisierte ICS-Bedrohungsmodellierungstechnologie, mit der die wahrscheinlichsten Pfade von gezielten ICS-Angriffen über proprietäre Analysen vorhergesagt werden können.

Die Defender für IoT-Integration mit der Forescout-Plattform bietet eine neue Ebene zentralisierter Sichtbarkeit, Überwachung und Kontrolle für IoT- und OT-Szenarien.

Diese überbrückten Plattformen ermöglichen automatisierte Gerätesichtbarkeit und -verwaltung von zuvor nicht erreichbaren ICS-Geräten und Siloworkflows.

Die Integration bietet SOC-Analytikern mehrstufige Einblicke in OT-Protokolle, die in Industrieumgebungen bereitgestellt werden. Details sind für Elemente wie Firmware, Gerätetypen, Betriebssysteme und Risikoanalysebewertungen verfügbar, die auf proprietären Azure Defender für IoT-Technologien basieren.

> [!Note]
> Verweise auf CyberX beziehen sich auf Azure Defender für IoT.
## <a name="devices"></a>Geräte

### <a name="device-visibility-and-management"></a>Gerätesichtbarkeit und -verwaltung

Das Inventar des Geräts wird durch wichtige Attribute erweitert, die von der Defender für IoT-Plattform erkannt werden. Dadurch wird Folgendes sichergestellt:

- Umfassende und kontinuierliche Einblicke in die OT-Gerätelandschaft in einer einzelnen Sicht.
- Echtzeitinformationen zu OT-Risiken.

:::image type="content" source="media/integration-forescout/forescout-device-inventory.png" alt-text="Gerätebestand":::

:::image type="content" source="media/integration-forescout/forescout-device-details.png" alt-text="Gerätedetails":::

### <a name="device-control"></a>Gerätesteuerung

Mit der Forescout-Integration können Sie die Zeit verringern, die für Industrieorganisationen und kritische Infrastrukturorganisationen erforderlich ist, um Cyberbedrohungen zu erkennen, zu untersuchen und darauf zu reagieren.

- Verwenden Sie OT-Geräteinformationen von Azure Defender für IoT, um den Sicherheitszeitraum durch Auslösen von Forescout-Richtlinienaktionen zu schließen. Beispielsweise können Sie automatisch eine Warn-E-Mail an SOC-Administratoren senden, wenn bestimmte Protokolle erkannt werden oder wenn sich Firmwaredetails ändern.

- Korrelieren Sie Defender für IoT-Informationen mit anderen *Forescout eyeExtended*-Modulen, die Überwachung, Incident Management und Gerätesteuerung kontrollieren.

## <a name="system-requirements"></a>Systemanforderungen

- Azure Defender für IoT, Version 2.4 oder höher
- Forescout, Version 8.0 oder höher
- Eine Lizenz für das *Forescout eyeExtended*-Modul für die Azure Defender für IoT-Plattform.

### <a name="getting-more-forescout-information"></a>Weitere Informationen zu Forescout

Weitere Informationen zur Forescout-Plattform finden Sie im [Forescout Resource Center](https://www.forescout.com/company/resources/#resource_filter_group).

## <a name="system-setup"></a>Einrichtung des Systems

In diesem Artikel wird beschrieben, wie die Kommunikation zwischen der Defender für IoT-Plattform und der Forescout-Plattform eingerichtet wird.

### <a name="set-up-the-defender-for-iot-platform"></a>Einrichten der Defender für IoT-Plattform

Generieren Sie ein Zugriffstoken in Defender für IoT, um die Kommunikation zwischen Defender für IoT und Forescout sicherzustellen.

Zugriffstoken ermöglichen externen Systemen den Zugriff auf Daten, die von Defender für IoT erkannt wurden, und die Ausführung von Aktionen mit diesen Daten unter Verwendung der externen REST-API über SSL-Verbindungen. Sie können Zugriffstoken für den Zugriff auf die Rest-API von Azure Defender für IoT generieren.

So generieren Sie ein Token

1. Melden Sie sich beim Defender für IoT-Sensor an, der durch Forescout abgefragt wird.

1. Wählen Sie **Systemeinstellungen** aus, und wählen Sie dann im Abschnitt **Allgemein** die Option **Zugriffstoken** aus. Das Dialogfeld **Zugriffstoken** wird geöffnet.
   :::image type="content" source="media/integration-forescout/generate-access-tokens-screen.png" alt-text="Zugriffstoken":::
1. Wählen Sie im Dialogfeld **Zugriffstoken** die Option **Neues Token generieren** aus.
1. Geben Sie im Dialogfeld **Neues Zugriffstoken** eine Tokenbeschreibung ein.
   :::image type="content" source="media/integration-forescout/new-forescout-token.png" alt-text="Neues Zugriffstoken":::
1. Wählen Sie **Weiter** aus. Das Token wird im Dialogfeld angezeigt. :::image type="content" source="media/integration-forescout/forescout-access-token-display-screen.png" alt-text="Token anzeigen":::
   > [!NOTE]
   > *Bewahren Sie das Token an einem sicheren Ort auf. Sie benötigen es, wenn Sie die Forescout-Plattform konfigurieren*.
1. Wählen Sie **Fertig stellen** aus.

   :::image type="content" source="media/integration-forescout/forescout-access-token-added-successfully.png" alt-text="Hinzufügen des Tokens beenden":::

### <a name="set-up-the-forescout-platform"></a>Einrichten der Forescout-Plattform

Sie können die Forescout-Plattform einrichten, um mit einem Defender für IoT-Sensor zu kommunizieren.

Einrichtung:

1. Installieren Sie das *Forescout eyeExtend-Modul für CyberX* auf der Forescout-Plattform.

1. Melden Sie sich bei der CounterACT-Konsole an, und wählen Sie im Menü **Extras** die Option **Optionen** aus. Das Dialogfeld **Optionen** wird geöffnet.

1. Navigieren Sie zum Ordner **Modules**, und wählen Sie ihn aus.

1. Wählen Sie im Bereich **Modules** die Option **CyberX-Plattform** aus. Der Bereich „Defender für IoT-Plattform“ wird geöffnet.

   :::image type="content" source="media/integration-forescout/settings-for-module.png" alt-text="Einstellungen des Azure Defender für IoT-Moduls":::

   Geben Sie die folgenden Informationen ein:

   - **Serveradresse**: Geben Sie die IP-Adresse des Defender für IoT-Sensors ein, der von der Forescout-Appliance abgefragt wird.
   - **Zugriffstoken**: Geben Sie das Zugriffstoken ein, das für den Defender für IoT-Sensor generiert wird, der eine Verbindung mit der Forescout-Appliance herstellt. Informationen zum Generieren eines Tokens finden Sie unter [Einrichten der Defender für IoT-Plattform](#set-up-the-defender-for-iot-platform).

1. Wählen Sie **Übernehmen**.

Wenn Sie möchten, dass die Forescout-Plattform mit einem anderen Sensor kommuniziert:

1. Erstellen Sie ein neues Zugriffstoken im relevanten Defender für IoT-Sensor.

1. Gehen Sie im Dialogfeld **Forescout-Module** > **CyberX-Plattform** folgendermaßen vor:

   - Löschen Sie die angezeigten Informationen.
   
   - Geben Sie die neue Sensor-IP-Adresse und die neuen Zugriffstokeninformationen ein.

### <a name="verify-communication"></a>Überprüfen der Kommunikation

Nach dem Konfigurieren von Defender für IoT und Forescout öffnen Sie das Dialogfeld „Zugriffstoken“ des Sensors in Defender für IoT.

Wenn im Feld **Verwendet** für dieses Token der Wert **N/V** angezeigt wird, funktioniert die Verbindung zwischen dem Sensor und der Forescout-Appliance nicht.

**Verwendet** gibt den letzten Zeitpunkt an, an dem ein externer Aufruf mit diesem Token empfangen wurde.

:::image type="content" source="media/integration-forescout/forescout-access-token-added-successfully.png" alt-text="Überprüft, ob das Token empfangen wurde":::

## <a name="view-defender-for-iot-detections-in-forescout"></a>Anzeigen von Defender für IoT-Erkennungen in Forescout

So zeigen Sie die Attribute eines Geräts an:

1. Melden Sie sich bei der Forescout-Plattform an, und navigieren Sie dann zum **Ressourceninventar**.

1. Navigieren Sie zur **CyberX-Plattform**. Die folgenden Geräteattribute werden für von Defender für IoT erkannte OT-Geräte angezeigt.

   | Element | Beschreibung |
   |--|--|
   | Autorisiert durch Azure Defender für IoT | Ein Gerät wurde während des Netzwerklernzeitraums in Ihrem Netzwerk von Defender für IoT erkannt. |
   | Firmware | Die Firmwaredetails des Geräts. Beispielsweise Modell und Versionsdetails. |
   | Name | Der Name des Geräts. |
   | Betriebssystem | Das Betriebssystem des Geräts. |
   | type | Der Typ des Geräts. Beispielsweise eine SPS-, Historian- oder Engineering-Station. |
   | Hersteller | Der Hersteller des Geräts. Beispielsweise Rockwell Automation. |
   | Risikostufe | Die von Defender für IoT berechnete Risikostufe. |
   | Protokolle | Die Protokolle, die im vom Gerät generierten Datenverkehr erkannt wurden. |

:::image type="content" source="media/integration-forescout/device-firmware-attributes-in-forescout.png" alt-text="Anzeigen der Firmwareattribute.":::

:::image type="content" source="media/integration-forescout/vendor-attributes-in-forescout.png" alt-text="Anzeigen der Herstellerattribute.":::

### <a name="viewing-more-details"></a>Anzeigen weiterer Details

Anzeigen zusätzlicher Geräteinformationen für Geräte, die von Defender für IoT gesteuert werden. Beispielsweise Forescout-Kompatibilitäts- und -Richtlinieninformationen.

Klicken Sie hierzu im Abschnitt **Geräteinventarhosts** mit der rechten Maustaste auf ein Gerät. Das Dialogfeld „Hostdetails“ wird mit zusätzlichen Informationen geöffnet.

:::image type="content" source="media/integration-forescout/details-dialog-box-in-forescout.png" alt-text="Hostdetails":::

## <a name="create-azure-defender-for-iot-policies-in-forescout"></a>Erstellen von Azure Defender für IoT-Richtlinien in Forescout

Forescout-Richtlinien können verwendet werden, um die Steuerung und Verwaltung von Geräten zu automatisieren, die von Defender für IoT erkannt werden. Beispiel:

- Automatische E-Mail an die SOC-Administratoren, wenn bestimmte Firmwareversionen erkannt werden.

- Hinzufügen bestimmter Defender für IoT-Geräte zu einer Forescout-Gruppe zur weiteren Behandlung von Incident- und Sicherheitsworkflows, z. B. mit anderen SIEM-Integrationen.

Erstellen einer benutzerdefinierten Forescout-Richtlinie mit Defender für IoT mithilfe von Bedingungseigenschaften.

So greifen Sie auf Defender für IoT-Eigenschaften zu:

1. Navigieren Sie im Dialogfeld **Richtlinienbedingungen** zur **Eigenschaftenstruktur**.

1. Erweitern Sie den Ordner **CyberX Platform** in der **Eigenschaftenstruktur**. Die folgenden Defender für IoT-Eigenschaften sind verfügbar.

:::image type="content" source="media/integration-forescout/forescout-property-tree.png" alt-text="Eigenschaften":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Weiterleiten von Warnungsinformationen](how-to-forward-alert-information-to-partners.md).

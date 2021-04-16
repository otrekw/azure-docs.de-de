---
title: Informationen zur ServiceNow-Integration
description: Die ICS Management-Anwendung für ServiceNow in Defender für IoT bietet SOC-Analysten einen mehrdimensionalen Einblick in die speziellen OT-Protokolle und IoT-Geräte, die in Industrieumgebungen bereitgestellt werden, sowie ICS-fähige Verhaltensanalysen zur schnellen Erkennung von verdächtigem oder anomalem Verhalten.
ms.date: 1/17/2021
ms.topic: article
ms.openlocfilehash: 6e39c3d018003983f6dc5b5e16a9791de84d6005
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104786004"
---
# <a name="the-defender-for-iot-ics-management-application-for-servicenow"></a>ICS Management-Anwendung für ServiceNow in Defender für IoT

Die ICS Management-Anwendung für ServiceNow in Defender für IoT bietet SOC-Analysten einen mehrdimensionalen Einblick in die speziellen OT-Protokolle und IoT-Geräte, die in Industrieumgebungen bereitgestellt werden, sowie ICS-fähige Verhaltensanalysen zur schnellen Erkennung von verdächtigem oder anomalem Verhalten. Im Hinblick auf die fortschreitende Verschmelzung von IT und OT zur Unterstützung neuer IoT-Initiativen, z. B. intelligente Computer und Echtzeitintelligenz, stellt dies eine wichtige Weiterentwicklung dar.

Außerdem ermöglicht die Anwendung die Reaktion auf IT- und OT-Vorfälle über ein zentrales Unternehmens-SOC.

## <a name="about-defender-for-iot"></a>Informationen zu Defender für IoT

Defender für IoT ist die einzige von Blue Team-Experten entwickelte ICS- und IoT-Plattform für Cybersicherheit mit einer Erfolgsbilanz für den Schutz kritischer nationaler Infrastrukturen und die einzige Plattform mit patentierter ICS-fähiger Bedrohungsanalyse und maschinellem Lernen. Defender für IoT bietet Folgendes:

- Unmittelbare Erkenntnisse zu ICS in der Gerätelandschaft und eine umfangreiche Palette von Details zu Attributen.

- ICS-fähige Deep Embedded-Kenntnisse zu OT-Protokollen, Geräten, Anwendungen und deren Verhalten

- Unmittelbare Erkenntnisse zu Sicherheitsrisiken und bekannten Zero-Day-Bedrohungen

- Automatisierte ICS-Bedrohungsmodellierungstechnologie, mit der die wahrscheinlichsten Pfade von gezielten ICS-Angriffen über proprietäre Analysen vorhergesagt werden können

> [!Note]
> Verweise auf CyberX beziehen sich auf Azure Defender für IoT.

## <a name="about-the-integration"></a>Informationen zur Integration

Die Integration von Defender für IoT in ServiceNow bietet eine neue Ebene zentralisierter Sichtbarkeit, Überwachung und Kontrolle für IoT- und OT-Szenarien. Diese überbrückten Plattformen ermöglichen automatisierte Gerätesichtbarkeit und Bedrohungsverwaltung von bislang nicht erreichbaren ICS- und IoT-Geräten.

### <a name="threat-management"></a>Verwaltung von Bedrohungen

Die ICS Management-Anwendung in Defender für IoT ermöglicht Folgendes:

- Reduzierung der Zeit, die Industrieorganisationen und kritische Infrastrukturorganisationen benötigen, um Cyberbedrohungen zu erkennen, zu untersuchen und darauf zu reagieren

- Echtzeitinformationen zu OT-Risiken

- Korrelieren von Defender für IoT-Warnungen mit ServiceNow-Workflows für Bedrohungsüberwachung und Incident Management

- Auslösen von ServiceNow-Tickets und -Workflows mit anderen Diensten und Anwendungen auf der ServiceNow-Plattform

ICS- und SCADA-Sicherheitsbedrohungen werden von Defender für IoT-Sicherheits-Engines identifiziert, die bei Angriffen durch Schadsoftware, Abweichungen von Netzwerk- und Sicherheitsrichtlinien sowie bei Betriebs- und Protokollanomalien sofort Warnmeldungen ausgeben. Weitere Informationen zu den an ServiceNow gesendeten Warnungsdetails finden Sie unter [Warnungen](#alert-reporting).

### <a name="device-visibility-and-management"></a>Gerätesichtbarkeit und -verwaltung

Die ServiceNow Configuration Management Database (CMDB) wird durch einen umfangreichen Satz von Geräteattributen angereichert und ergänzt, die von der Defender für IoT-Plattform übertragen werden. Dies gewährleistet einen umfassenden und kontinuierlichen Einblick in die Gerätelandschaft und ermöglicht Ihnen die Überwachung und Reaktion über eine zentrale Benutzeroberfläche. Weitere Informationen zu den an ServiceNow gesendeten Geräteattributen finden Sie unter [Anzeigen von Defender für IoT-Erkennungen in ServiceNow](#view-defender-for-iot-detections-in-servicenow).

## <a name="system-requirements-and-architecture"></a>Systemanforderungen und Architektur

Dieser Artikel beschreibt Folgendes:

- **Softwareanforderungen**  
- **Architektur**

## <a name="software-requirements"></a>Softwareanforderungen

- ServiceNow Service Management Version 3.0.2

- Defender für IoT, Patch 2.8.11.1 oder höher

> [!Note]
> Wenn Sie Defender für IoT bereits mit ServiceNow integriert haben und über die lokale Verwaltungskonsole ein Upgrade durchführen, sollten frühere Daten, die von Defender für IoT-Sensoren empfangen wurden, in ServiceNow gelöscht werden.

## <a name="architecture"></a>Architektur

### <a name="on-premises-management-console-architecture"></a>Architektur der lokalen Verwaltungskonsole

Die lokale Verwaltungskonsole dient als einheitliche Quelle für alle Geräte- und Warnungsinformationen, die an ServiceNow gesendet werden.

Sie können eine lokale Verwaltungskonsole für die Kommunikation mit einer Instanz von ServiceNow einrichten. Die lokale Verwaltungskonsole überträgt Sensordaten über die REST-API an die Defender für IoT-Anwendung.

Wenn Sie Ihr System zur Verwendung einer lokalen Verwaltungskonsole einrichten, deaktivieren Sie die ServiceNow-Synchronisierung, Weiterleitungsregeln und Proxykonfigurationen in Sensoren, falls sie eingerichtet wurden.

Diese Konfigurationen sollten für die lokale Verwaltungskonsole eingerichtet werden. Die Konfigurationsanweisungen werden in diesem Artikel beschrieben.

### <a name="sensor-architecture"></a>Sensorenarchitektur

Wenn Sie Ihre Umgebung so einrichten möchten, dass sie eine direkte Kommunikation zwischen Sensoren und ServiceNow beinhaltet, definieren Sie für jeden Sensor die ServiceNow-Synchronisierung, Weiterleitungsregeln und die Proxykonfiguration (wenn ein Proxy erforderlich ist).

Es wird empfohlen, die Integration über die lokale Verwaltungskonsole für die Kommunikation mit ServiceNow einzurichten.

## <a name="create-access-tokens-in-servicenow"></a>Erstellen von Zugriffstoken in ServiceNow

In diesem Artikel wird beschrieben, wie Sie ein Zugriffstoken in ServiceNow erstellen. Das Token wird für die Kommunikation mit Defender für IoT benötigt.

Beim Erstellen von Defender für IoT-Weiterleitungsregeln, die Warnungsinformationen an ServiceNow weiterleiten, und beim Konfigurieren von Defender für IoT zur Übertragung von Geräteattributen an ServiceNow-Tabellen benötigen Sie die **Client-ID** und den **geheimen Clientschlüssel**.

## <a name="set-up-defender-for-iot-to-communicate-with-servicenow"></a>Einrichten von Defender für IoT für die Kommunikation mit ServiceNow

In diesem Artikel wird beschrieben, wie Sie Defender für IoT für die Kommunikation mit ServiceNow einrichten.

### <a name="send-defender-for-iot-alert-information"></a>Senden von Defender für IoT-Warnungsinformationen

In diesem Artikel wird beschrieben, wie Sie Defender für IoT zur Übertragung von Warnungsinformationen an ServiceNow-Tabellen konfigurieren. Informationen zu den gesendeten Warnungsdaten finden Sie unter [Warnungen](#alert-reporting).

Defender für IoT-Warnungen werden in ServiceNow als Sicherheitsvorfälle angezeigt.

Definieren Sie eine *Weiterleitungsregel* in Defender für IoT, um Warnungsinformationen an ServiceNow zu senden.

So definieren Sie die Regel

1. Wählen Sie im linken Bereich von Defender für IoT die Option **Weiterleitung** aus.  

1. Wählen Sie das Symbol :::image type="content" source="media/integration-servicenow/plus.png" alt-text="Plussymbol"::: aus . Das Dialogfeld „Create Forwarding Rule“ (Weiterleitungsregel erstellen) wird geöffnet.  

    :::image type="content" source="media/integration-servicenow/forwarding-rule.png" alt-text="„Create Forwarding Rule“ (Weiterleitungsregel erstellen)":::

1. Geben Sie einen Regelnamen ein.

1. Definieren Sie die Kriterien, nach denen die Weiterleitungsregel in Defender für IoT ausgelöst wird. Durch Verwenden von Kriterien für Weiterleitungsregeln können Sie das Volumen der von Defender für IoT an ServiceNow gesendeten Informationen genau bestimmen und verwalten. Die folgenden Optionen sind verfügbar:

    - **Schweregrade:** Dies gibt den Mindestschweregrad für die Weiterleitung von Vorfällen an. Wenn z. B. **Gering** ausgewählt ist, werden Warnungen mit einem geringen Schweregrad und alle Warnungen mit höheren Schweregraden weitergeleitet. Die Schweregrade sind in Defender für IoT vordefiniert.

    - **Protokolle:** Die Weiterleitungsregel nur auslösen, wenn der erkannte Datenverkehr über bestimmte Protokolle ausgeführt wurde. Wählen Sie die gewünschten oder alle Protokolle in der Dropdownliste aus.

    - **Engines:** Wählen Sie die erforderlichen oder alle Engines aus. Warnungen von ausgewählten Engines werden gesendet.

1. Vergewissern Sie sich, dass **Report Alert Notifications** (Warnungen melden) ausgewählt ist.

1. Wählen Sie im Bereich „Aktionen“ die Option **Hinzufügen** und dann **ServiceNow** aus.

    :::image type="content" source="media/integration-servicenow/select-servicenow.png" alt-text="Auswählen von ServiceNow in den Dropdownoptionen":::

1. Geben Sie die ServiceNow-Aktionsparameter ein:

    :::image type="content" source="media/integration-servicenow/parameters.png" alt-text="Eingeben der ServiceNow-Aktionsparameter":::

1. Legen Sie im Bereich **Aktionen** die folgenden Parameter fest:

  | Parameter | BESCHREIBUNG |
  |--|--|
  | Domain | Geben Sie die IP-Adresse des ServiceNow-Servers ein. |
  | Username | Geben Sie den Benutzernamen für den ServiceNow-Server ein. |
  | Kennwort | Geben Sie das Kennwort für den ServiceNow-Server ein. |
  | Client-ID | Geben Sie die Client-ID ein, die Sie für Defender für IoT auf der Seite **Application Registries** (Anwendungsregistrierungen) in ServiceNow erhalten haben. |
  | Geheimer Clientschlüssel | Geben Sie den geheimen Clientschlüssel ein, den Sie für Defender für IoT auf der Seite **Application Registries** (Anwendungsregistrierungen) in ServiceNow erstellt haben. |
  | Berichttyp | **Vorfälle:** Weiterleiten einer Liste von Warnungen, die in ServiceNow angezeigt werden, mit der Vorfall-ID und einer kurzen Beschreibung der einzelnen Warnungen.<br /><br />**Defender für IoT-Anwendung:** Weiterleiten der vollständigen Warnungsinformationen, einschließlich Sensordetails, Engine, Quell- und Zieladresse. Die Informationen werden an Defender für IoT in der ServiceNow-Anwendung weitergeleitet. |

1. Wählen Sie **SAVE** (SPEICHERN) aus. Defender für IoT-Warnungen werden in ServiceNow als Vorfälle angezeigt.

### <a name="send-defender-for-iot-device-attributes"></a>Senden von Defender für IoT-Geräteattributen

In diesem Artikel wird beschrieben, wie Sie Defender für IoT zur Übertragung von umfangreichen Geräteattributen an ServiceNow-Tabellen konfigurieren. Ausführliche Informationen zur Art der an ServiceNow übertragenen Informationen finden Sie unter ***Bestandsinformationen***.

Zum Senden von Attributen an ServiceNow müssen Sie die lokale Verwaltungskonsole einer ServiceNow-Instanz zuordnen. Dadurch wird die Kommunikation und Authentifizierung der Defender für IoT-Plattform mit dieser Instanz sichergestellt.

So fügen Sie eine ServiceNow-Instanz hinzu

1. Melden Sie sich bei der lokalen Verwaltungskonsole von Defender für IoT an.

1. Wählen Sie im Bereich „Integration“ der lokalen Verwaltungskonsole **Systemeinstellungen** und dann **ServiceNow** aus.

      :::image type="content" source="media/integration-servicenow/servicenow.png" alt-text="Auswählen der Schaltfläche „ServiceNow“":::

1. Geben Sie im Dialogfeld „ServiceNow Sync“ (ServiceNow-Synchronisierung) die folgenden Synchronisierungsparameter ein.

    :::image type="content" source="media/integration-servicenow/sync.png" alt-text="Dialogfeld „ServiceNow Sync“ (ServiceNow-Synchronisierung)":::

     Parameter | Beschreibung |
    |--|--|
    | Enable Sync (Synchronisierung aktivieren) | Aktivieren und deaktivieren Sie die Synchronisierung nach dem Definieren von Parametern. |
    | Synchronisierungshäufigkeit (Minuten) | Standardmäßig werden die Informationen alle 60 Minuten an ServiceNow übermittelt. Der Mindestwert ist 5 Minuten. |
    | ServiceNow-Instanz | Geben Sie die URL der ServiceNow-Instanz ein. |
    | Client-ID | Geben Sie die Client-ID ein, die Sie für Defender für IoT auf der Seite **Application Registries** (Anwendungsregistrierungen) in ServiceNow erhalten haben. |
    | Geheimer Clientschlüssel | Geben Sie den geheimen Clientschlüssel ein, den Sie für Defender für IoT auf der Seite **Application Registries** (Anwendungsregistrierungen) in ServiceNow erstellt haben. |
    | Username | Geben Sie den Benutzernamen für die Instanz ein. |
    | Kennwort | Geben Sie das Kennwort für die Instanz ein. |

1. Wählen Sie **SAVE** (SPEICHERN) aus.

## <a name="verify-communication"></a>Überprüfen der Kommunikation

Vergewissern Sie sich, dass die lokale Verwaltungskonsole mit der ServiceNow-Instanz verbunden ist, indem Sie das Datum für *Letzte Synchronisierung* überprüfen.

:::image type="content" source="media/integration-servicenow/sync-confirmation.png" alt-text="Überprüfen der Kommunikation anhand der letzten Synchronisierung":::

## <a name="set-up-the-integrations-using-the-https-proxy"></a>Einrichten der Integration mithilfe des HTTPS-Proxys

Beim Einrichten der Integration von Defender für IoT und ServiceNow erfolgt die Kommunikation zwischen der lokalen Verwaltungskonsole und dem ServiceNow-Server über Port 443. Wenn sich der ServiceNow-Server hinter dem Proxy befindet, kann der Standardport nicht verwendet werden.

Defender für IoT unterstützt einen HTTPS-Proxy in der ServiceNow-Integration durch die Änderung des für die Integration verwendeten Standardports.

So konfigurieren Sie den Proxy

1. Bearbeiten Sie die globalen Eigenschaften in der lokalen Verwaltungskonsole:  
    `sudo vim /var/cyberx/properties/global.properties`

2. Fügen Sie die folgenden Parameter hinzu:

    - `servicenow.http_proxy.enabled=1`

    - `servicenow.http_proxy.ip=1.179.148.9`

    - `servicenow.http_proxy.port=59125`

3. Speichern Sie Ihre Änderungen, und schließen Sie die Anwendung.

4. Führen Sie den folgenden Befehl aus: `sudo monit restart all`

Nach der Konfiguration werden alle ServiceNow-Daten mithilfe des konfigurierten Proxys weitergeleitet.

## <a name="download-the-defender-for-iot-application"></a>Herunterladen der Defender für IoT-Anwendung

In diesem Artikel wird beschrieben, wie Sie die Anwendung herunterladen.

So greifen Sie auf die Defender für IoT-Anwendung zu

1. Navigieren Sie zu <https://store.servicenow.com/>.

2. Suchen Sie nach `Defender for IoT` oder `CyberX IoT/ICS Management`.

   :::image type="content" source="media/integration-servicenow/search-results.png" alt-text="Suchen nach CyberX auf der Suchleiste":::

3. Wählen Sie die Anwendung aus.

   :::image type="content" source="media/integration-servicenow/cyberx-app.png" alt-text="Wählen Sie die Anwendung in der Liste aus.":::

4. Wählen Sie **Request App** (App anfordern) aus.

   :::image type="content" source="media/integration-servicenow/sign-in.png" alt-text="Anmelden bei der Anwendung mit Ihren Anmeldeinformationen":::

5. Melden Sie sich an, und laden Sie die Anwendung herunter.

## <a name="view-defender-for-iot-detections-in-servicenow"></a>Anzeigen von Defender für IoT-Erkennungen in ServiceNow

In diesem Artikel werden die in ServiceNow angezeigten Geräteattribute und Warnungsinformationen beschrieben.

So zeigen Sie die Geräteattribute an

1. Melden Sie sich bei ServiceNow an.

2. Navigieren Sie zu **CyberX Platform** (CyberX-Plattform).

3. Navigieren Sie zu **Inventory** (Bestand) oder **Alert** (Warnung).

   [:::image type="content" source="media/integration-servicenow/alert-list.png" alt-text="„Inventory“ (Bestand) oder „Alert“ (Warnung)":::](media/integration-servicenow/alert-list.png#lightbox)

## <a name="inventory-information"></a>Bestandsinformationen

Die Configuration Management Database (CMDB) wird durch die von Defender für IoT an ServiceNow gesendeten Daten angereichert und ergänzt. Durch Hinzufügen oder Aktualisieren von Geräteattributen in den ServiceNow CMDB-Tabellen mit Konfigurationselementen können in Defender für IoT die Workflows und Geschäftsregeln von ServiceNow ausgelöst werden.

Folgende Informationen stehen zur Verfügung:

- Geräteattribute, z. B. MAC-Adresse des Geräts, Betriebssystem, Hersteller oder erkanntes Protokoll

- Firmwareinformationen, z. B. Firmwareversion und Seriennummer

- Informationen zu verbundenen Geräten, z. B. Richtung des Datenverkehrs zwischen Quelle und Ziel

### <a name="devices-attributes"></a>Geräteattribute

In diesem Artikel werden die an ServiceNow übermittelten Geräteattribute beschrieben.

| Element | Beschreibung |
|--|--|
| Appliance | Der Name des Sensors, der den Datenverkehr erkannt hat |
| id | Die von Defender für IoT zugewiesene Geräte-ID |
| Name | Den Gerätenamen. |
| IP-Adresse | Die IP-Adresse(n) des Geräts |
| Typ | Der Gerätetyp, z. B. Switch, SPS, Historian oder Domänencontroller |
| MAC-Adresse | Die MAC-Adresse(n) des Geräts |
| Betriebssystem | Das Betriebssystem des Geräts |
| Hersteller | Der Gerätehersteller. |
| Protokolle | Die Protokolle, die im vom Gerät generierten Datenverkehr erkannt wurden |
| Besitzer | Geben Sie den Namen des Gerätebesitzers ein. |
| Standort | Geben Sie den physischen Standort des Geräts ein. |

In der folgenden Ansicht können Sie die mit einem Gerät verbundenen Geräte anzeigen.

So zeigen Sie verbundene Geräte an

1. Wählen Sie ein Gerät und dann die für das Gerät aufgelistete **Appliance** aus.

    :::image type="content" source="media/integration-servicenow/appliance.png" alt-text="Auswählen der gewünschten Appliance in der Liste":::

1. Wählen Sie im Dialogfeld **Device Details** (Gerätedetails) die Option **Connected Devices** (Verbundene Geräte) aus.

### <a name="firmware-details"></a>Firmwaredetails

In diesem Artikel werden die an ServiceNow übermittelten Gerätefirmwareinformationen beschrieben.

| Element | Beschreibung |
|--|--|
| Appliance | Der Name des Sensors, der den Datenverkehr erkannt hat |
| Sicherungsmedium | Den Gerätenamen. |
| Adresse | Die IP-Adresse des Geräts. |
| Moduladresse | Gerätemodell und Einschubfachnummer oder -ID. |
| Seriell | Die Seriennummer des Geräts. |
| Modell | Die Gerätemodellnummer. |
| Version | Die Firmwareversionsnummer. |
| Zusätzliche Daten | Weitere Firmwaredaten, wie vom Hersteller definiert, z. B. der Gerätetyp. |

### <a name="connection-details"></a>Verbindungsdetails

In diesem Artikel werden die an ServiceNow übermittelten Geräteverbindungsinformationen beschrieben.

:::image type="content" source="media/integration-servicenow/connections.png" alt-text="Verbindungsinformationen des Geräts":::

| Element | Beschreibung |
|--|--|
| Appliance | Der Name des Sensors, der den Datenverkehr erkannt hat |
| Direction | Die Richtung des Datenverkehrs. <br /> <br /> - **Unidirektional** gibt an, dass der Server das Ziel und der Client die Quelle ist. <br /> <br /> - **Bidirektional** gibt an, dass sowohl die Quelle als auch das Ziel Server sind oder dass der Client unbekannt ist. |
| Source device ID (Quellgeräte-ID) | Die IP-Adresse des Geräts, das mit dem verbundenen Gerät kommuniziert hat |
| Source device name (Quellgerätename) | Der Name des Geräts, das mit dem verbundenen Gerät kommuniziert hat |
| Destination device ID (Zielgeräte-ID) | Die IP-Adresse des verbundenen Geräts |
| Destination device name (Zielgerätename) | Der Name des verbundenen Geräts |

## <a name="alert-reporting"></a>Warnungen

Warnungen werden ausgelöst, wenn Defender für IoT-Engines Änderungen im Netzwerkdatenverkehr und Verhalten erkennen, die Ihre Aufmerksamkeit erfordern. Weitere Informationen zu den durch die einzelnen Engines generierten Warnungen finden Sie unter[Informationen zu Warnungs-Engines](#about-alert-engines).

In diesem Artikel werden die an ServiceNow übermittelten Gerätewarnungsinformationen beschrieben.

| Element | BESCHREIBUNG |
|--|--|
| Erstellt | Datum und Uhrzeit der Generierung der Warnung |
| Engine | Die Engine, die das Ereignis erkannt hat |
| Titel | Der Titel der Warnung |
| Beschreibung | Die Beschreibung der Warnung |
| Protocol | Das im Datenverkehr erkannte Protokoll |
| severity | Der von Defender für IoT definierte Warnungsschweregrad |
| Appliance | Der Name des Sensors, der den Datenverkehr erkannt hat |
| Quellname | Der Quellenname. |
| Quell-IP-Adresse| Die Quell-IP-Adresse |
| Zielname | Der Zielname |
| IP-Zieladresse | Die Ziel-IP-Adresse |
| Zugewiesene Person | Geben Sie den Namen der Person ein, der das Ticket zugewiesen ist. |

### <a name="updating-alert-information"></a>Aktualisieren von Warnungsinformationen

Wählen Sie den Eintrag in der erstellten Spalte aus, um die Warnungsinformationen in einem Formular anzuzeigen. Sie können Warnungsdetails aktualisieren und die Warnung einer Person zur Überprüfung und Bearbeitung zuweisen.

[:::image type="content" source="media/integration-servicenow/alert.png" alt-text="Anzeigen der Warnungsinformationen":::](media/integration-servicenow/alert.png#lightbox)

### <a name="about-alert-engines"></a>Informationen zu Warnungs-Engines

In diesem Artikel werden die Warnungstypen beschrieben, die von den einzelnen Engines ausgelöst werden.

| Warnungstyp | BESCHREIBUNG |
|--|--|
| Warnungen zu Richtlinienverstößen | Diese Warnungen werden ausgelöst, wenn das Richtlinienverstoßmodul eine Abweichung vom zuvor erlernten Datenverkehr erkennt. Beispiel: <br /><br />- Ein neues Gerät wird erkannt. <br /><br />- Auf einem Gerät wird eine neue Konfiguration erkannt. <br /><br />- Ein nicht als Programmiergerät definiertes Gerät führt eine Programmieränderung aus. <br /><br />- Eine Firmwareversion wurde geändert. |
| Warnungen zu Protokollverletzungen | Diese Warnungen werden ausgelöst, wenn die Protokollverletzungs-Engine Paketstrukturen oder Feldwerte erkennt, die nicht mit der Protokollspezifikation übereinstimmen. |
| Vorgangswarnungen | Diese Warnungen werden ausgelöst, wenn die Vorgangs-Engine Netzwerkbetriebsvorfälle oder Gerätestörungen erkennt. Beispiele: Ein Netzwerkgerät wurde über einen Befehl zum Beenden der SPS beendet, oder eine Schnittstelle eines Sensors hat die Überwachung des Datenverkehrs beendet. |
| Malwarewarnungen | Diese Warnungen werden ausgelöst, wenn die Schadsoftware-Engine schädliche Netzwerkaktivitäten erkennt, beispielsweise bekannte Angriffe wie Conficker. |
| Anomaliewarnungen | Diese Warnungen werden ausgelöst, wenn das Anomaliemodul eine Abweichung erkennt. Beispiel: Ein nicht als Scangerät definiertes Gerät führt Netzwerkscans aus. |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungsinformationen weiterleiten](how-to-forward-alert-information-to-partners.md).

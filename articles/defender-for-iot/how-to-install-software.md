---
title: Installation von Defender für IoT
description: Hier erfahren Sie, wie Sie einen Sensor und die lokale Verwaltungskonsole für Azure Defender für IoT installieren.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/2/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 5e33a7adc7b529df8c7c821cbfdcb0ad5709803b
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97836095"
---
# <a name="defender-for-iot-installation"></a>Installation von Defender für IoT

In diesem Artikel wird beschrieben, wie die folgenden Elemente von Azure Defender für IoT installiert werden:

- **Sensor**: Defender für IoT-Sensoren erfassen den ICS-Netzwerkdatenverkehr mithilfe von passiver Überwachung (ohne Agents). Da sie passiv und nicht intrusiv arbeiten, haben die Sensoren keinerlei Auswirkungen auf OT- und IoT-Netzwerke und -Geräte. Der Sensor wird mit einem SPAN-Port oder Netzwerk-TAP verbunden und beginnt sofort mit der Überwachung Ihres Netzwerks. Erkennungen werden in der Sensorkonsole angezeigt. Dort können Sie sie in einer Netzwerkübersicht, einem Geräteinventar sowie einer umfangreichen Palette von Berichten anzeigen, untersuchen und analysieren. Zu den Beispielen zählen Risikobewertungsberichte, Data Mining-Abfragen und Angriffsvektoren. Weitere Informationen zu Sensorfunktionen finden Sie im [Defender für IoT Sensor-Benutzerhandbuch (direkter Download)](https://aka.ms/AzureDefenderforIoTUserGuide).

- **Lokale Verwaltungskonsole**: Über die lokale Verwaltungskonsole können Sie Geräteverwaltung, Risikomanagement und Verwaltung von Sicherheitsrisiken durchführen. Außerdem können Sie damit die Bedrohungsüberwachung und Reaktion auf Vorfälle im gesamten Unternehmen durchführen. So erhalten Sie eine einheitliche Übersicht über alle Netzwerkgeräte, Ihnen werden wichtige IoT- und OT-Risikoindikatoren sowie Warnungen angezeigt, die in Einrichtungen erkannt wurden, in denen Sensoren bereitgestellt werden. Über die lokale Verwaltungskonsole können Sie Sensoren in Air-Gap-Netzwerken anzeigen und verwalten.

In diesem Artikel werden die folgenden Installationsinformationen behandelt:

  - **Hardware:** Details zur physischen Dell- und HPE-Appliance.

  - **Software:** Installation der Software für Sensor und lokale Verwaltungskonsole

  - **Virtuelle Geräte:** Details zum virtuellen Computer und zur Softwareinstallation.

Stellen Sie nach der Installation eine Verbindung zwischen Ihrem Sensor und Ihrem Netzwerk her.

## <a name="about-defender-for-iot-appliances"></a>Informationen zu Defender für IoT-Appliances 

In den folgenden Abschnitten finden Sie Informationen zu Defender für IoT-Sensorappliances und zur Appliance für die lokale Defender für IoT-Verwaltungskonsole.

### <a name="physical-appliances"></a>Physische Appliances

Der Defender für IoT-Appliancesensor stellt eine Verbindung mit einem SPAN-Port oder einem Netzwerk-TAP her und beginnt mithilfe von passiver Überwachung (ohne Agents) sofort mit der Erfassung von ICS-Netzwerkdatenverkehr. Dieser Prozess hat keinerlei Auswirkungen auf OT-Netzwerke und -Geräte, weil er sich nicht im Datenpfad befindet und OT-Geräte nicht aktiv überprüft.

Die folgenden Appliances zum Einbau in ein Rack stehen zur Verfügung:

| **Bereitstellungstyp** | **Unternehmen** | **Enterprise** | **SMB** |  |
|--|--|--|--|--|
| **Modell** | HPE ProLiant DL360 | Dell PowerEdge R340 XL | HPE ProLiant DL20 | HPE ProLiant DL20 |
| **Überwachungsports** | bis zu 15 RJ45 oder 8 OPT | bis zu 9 RJ45 oder 6 OPT | bis zu 8 RJ45 oder 6 OPT | 4 RJ45 |
| **Maximale Bandbreite\** _ | 3 GB/s | 1 GB/s | 1 GB/s | 100 MB/s |
| _ *Maximale Anzahl geschützter Geräte** | 30.000 | 10.000 | 15.000 | 1\.000 |

\* Die maximale Bandbreitenkapazität könnte je nach Protokollverteilung variieren.

### <a name="virtual-appliances"></a>Virtuelle Geräte

Die folgenden virtuellen Geräte stehen zur Verfügung:

| **Bereitstellungstyp** | **Enterprise** | **SMB** | **Line** |
|--|--|--|--|
| **Beschreibung** | Virtuelles Gerät für Unternehmensbereitstellungen | Virtuelles Gerät für SMB-Bereitstellungen | Virtuelles Gerät für Linienbereitstellungen |
| **Maximale Bandbreite\** _ | 150 MB/s | 15 MB/s | 3 MB/s |
| _ *Maximale Anzahl geschützter Geräte** | 3,000 | 300 | 100 |
| **Bereitstellungstyp** | Enterprise | SMB | Linie |
| **Beschreibung** | Virtuelles Gerät für Unternehmensbereitstellungen | Virtuelles Gerät für SMB-Bereitstellungen | Virtuelles Gerät für Linienbereitstellungen |

\* Die maximale Bandbreitenkapazität könnte je nach Protokollverteilung variieren.

### <a name="hardware-specifications-for-the-on-premises-management-console"></a>Hardwarespezifikationen für die lokale Verwaltungskonsole

 | Element | BESCHREIBUNG |
 |----|--|
 **Beschreibung** | In einer Architektur mit mehreren Ebenen bietet die lokale Verwaltungskonsole Transparenz und Kontrolle über geografisch verteilte Standorte. Sie ist in SOC-Sicherheitsstapel integriert, darunter SIEMs, Ticketsysteme, Firewalls der nächsten Generation, Plattformen für sicheren Remotezugriff und Defender für IoT ICS Malware-Sandbox. |
 **Bereitstellungstyp** | Enterprise |
 **Appliancetyp**  | Dell R340, VM |
 **Anzahl der verwalteten Sensoren** | Unbegrenzt |

## <a name="prepare-for-the-installation"></a>Vorbereiten der Installation

### <a name="access-the-iso-installation-image"></a>Zugreifen auf das ISO-Installationsimage

Auf das Installationsimage kann über das Defender für IoT-Portal zugegriffen werden.

So greifen Sie auf die Datei zu:

1. Melden Sie sich bei Ihrem Defender für IoT-Konto an.

2. Wechseln Sie zur Seite **Netzwerksensor** oder **Lokale Verwaltungskonsole**, und wählen Sie die Version aus, die heruntergeladen werden soll.

### <a name="install-from-dvd"></a>Installieren von einer DVD

Stellen Sie vor der Installation sicher, dass Sie Folgendes zur Verfügung haben:

- Ein tragbares DVD-Laufwerk mit dem USB-Connector.

- Ein ISO-Installationsimage.

So führen Sie die Installation durch:

1. Kopieren Sie das Image auf eine DVD, oder bereiten Sie einen Datenträger auf einem Schlüssel vor. Verbinden Sie ein tragbares DVD-Laufwerk mit Ihrem Computer, klicken Sie mit der rechten Maustaste auf das ISO-Image, und wählen Sie **Burn to disk** (Auf Datenträger kopieren) aus.

1. Verbinden Sie die DVD oder den Datenträger auf einem Schlüssel, und konfigurieren Sie die Appliance für den Start von einer DVD oder einem Datenträger auf einem Schlüssel.

### <a name="install-from-disk-on-a-key"></a>Installieren von einem Datenträger auf einem Schlüssel

Stellen Sie vor der Installation sicher, dass Sie Folgendes zur Verfügung haben:

  - Rufus wurde installiert.
  
  - Ein Datenträger auf einem Schlüssel mit USB-Version 3.0 und höher. Die Mindestgröße beträgt 4 GB.

  - Eine ISO-Imagedatei des Installationsprogramms.

Der Datenträger auf einem Schlüssel wird in diesem Vorgang gelöscht.

So bereiten Sie einen Datenträger auf einem Schlüssel vor:

1. Führen Sie „Rufus“ aus, und wählen Sie **SENSOR-ISO** aus.

2. Verbinden Sie den Datenträger auf einem Schlüssel mit der Vorderseite.

3. Legen Sie fest, dass das BIOS des Servers über den USB-Stick gestartet werden soll.

## <a name="dell-poweredger340xl-installation"></a>Installation von Dell PowerEdge R340XL

Vor der Installation der Software auf der Dell-Appliance müssen Sie deren BIOS-Konfiguration anpassen:

  - [Dell PowerEdge R340 – Vorderseite](#dell-poweredge-r340-front-panel) und [Dell PowerEdge R340 – Rückseite](#dell-poweredge-r340-back-panel) enthält die Beschreibung von Vorder- und Rückseite sowie Informationen, die für die Installation erforderlich sind (z. B. Treiber und Ports).

  - [Dell BIOS-Konfiguration](#dell-bios-configuration) bietet Informationen zum Herstellen einer Verbindung mit der Dell-Appliance-Verwaltungsschnittstelle und zum Konfigurieren des BIOS.

  - In [Softwareinstallation (Dell R340)](#software-installation-dell-r340) wird das Verfahren beschrieben, das zum Installieren der Defender für IoT-Sensorsoftware erforderlich ist.

### <a name="dell-poweredge-r340xl-requirements"></a>Anforderungen für Dell PowerEdge R340XL 

Zum Installieren der Dell PowerEdge R340XL-Appliance benötigen Sie Folgendes:

- Unternehmenslizenz für Dell Remote Access Controller (iDrac)

- XML für BIOS-Konfiguration

- Serverfirmware-Versionen:

  - BIOS-Version 2.1.6

  - iDrac-Version 3.23.23.23

### <a name="dell-poweredge-r340-front-panel"></a>Dell PowerEdge R340 – Vorderseite

:::image type="content" source="media/tutorial-install-components/view-of-dell-poweredge-r340-front-panel.jpg" alt-text="Dell PowerEdge R340 – Vorderseite":::

 1. Linkes Kontrollfeld 
 2. Optisches Laufwerk (optional) 
 3. Rechtes Bedienfeld 
 4. Informationstag 
 5. Laufwerke  

### <a name="dell-poweredge-r340-back-panel"></a>Dell PowerEdge R340 – Rückseite

:::image type="content" source="media/tutorial-install-components/view-of-dell-poweredge-r340-back-panel.jpg" alt-text="Dell PowerEdge R340 – Rückseite":::

1. Serieller Anschluss 
2. NIC-Port (GB 1) 
3. NIC-Port (GB 1) 
4. PCIe – halbe Höhe 
5. PCIe-Erweiterungskartenslot – volle Höhe 
6. Stromversorgungseinheit 1 
7. Stromversorgungseinheit 2 
8. Systemidentifikation 
9. Kabelanschluss für die Systemstatusanzeige (CMA) – Taste 
10. USB 3.0-Anschluss (2) 
11. Dedizierter iDRAC9-Netzwerkport 
12. VGA-Anschluss 

### <a name="dell-bios-configuration"></a>Dell-BIOS-Konfiguration

Die Dell-BIOS-Konfiguration ist erforderlich, um die Dell-Appliance für die Zusammenarbeit mit der Software anzupassen.

Die BIOS-Konfiguration wird über eine vordefinierte Konfiguration durchgeführt. Auf die Datei kann über das [Hilfecenter](https://help.cyberx-labs.com/) zugegriffen werden.

Importieren Sie die Konfigurationsdatei auf die Dell-Appliance. Vor Verwendung der Konfigurationsdatei müssen Sie die Kommunikation zwischen der Dell-Appliance und dem Verwaltungscomputer einrichten.

Die Dell-Appliance wird durch einen integrierten iDRAC mit Lifecycle Controller (LC) verwaltet. Der LC ist in jedem Dell PowerEdge-Server eingebettet und bietet Funktionen, mit deren Hilfe Sie Ihre Dell PowerEdge-Appliances bereitstellen, aktualisieren, überwachen und warten können.

Zum Einrichten der Kommunikation zwischen der Dell-Appliance und dem Verwaltungscomputer müssen Sie die iDRAC-IP-Adresse und die IP-Adresse des Verwaltungscomputers in demselben Subnetz definieren.

Wenn die Verbindung hergestellt wird, kann das BIOS konfiguriert werden.

So konfigurieren Sie das Dell BIOS:

1. [Konfigurieren Sie die iDRAC-IP-Adresse.](#configure-idrac-ip-address)

2. [Importieren Sie die BIOS-Konfigurationsdatei.](#import-the-bios-configuration-file)

#### <a name="configure-idrac-ip-address"></a>Konfigurieren der iDRAC-IP-Adresse

1. Schalten Sie den Sensor ein.

2. Wenn das Betriebssystem bereits installiert wurde, drücken Sie die Taste F2, um die BIOS-Konfiguration einzugeben.

3. Wählen Sie **iDRAC-Einstellungen** aus.

4. Wählen Sie **Netzwerk** aus.

   > [!NOTE]
   > Während der Installation müssen Sie die standardmäßige iDRAC-IP-Adresse und das Kennwort konfigurieren, die in den folgenden Schritten angegeben werden. Nach der Installation müssen Sie diese Definitionen ändern.

5. Ändern Sie die statische IPv4-Adresse in **10.100.100.250**.

6. Ändern Sie die statische Subnetzmaske in **255.255.255.0**.

   :::image type="content" source="media/tutorial-install-components/idrac-network-settings-screen-v2.png" alt-text="Screenshot der statischen Subnetzmaske":::

7. Wählen Sie **Zurück** > **Fertigstellen** aus.

#### <a name="import-the-bios-configuration-file"></a>Importieren der BIOS-Konfigurationsdatei

In diesem Artikel wird beschrieben, wie das BIOS mithilfe der Konfigurationsdatei konfiguriert wird.

1. Schließen Sie einen PC mit der statischen vorkonfigurierten IP-Adresse **10.100.100.200** an den **iDRAC**-Port an.

   :::image type="content" source="media/tutorial-install-components/idrac-port.png" alt-text="Screenshot des Ports mit der vorkonfigurierten IP-Adresse":::

2. Öffnen Sie einen Browser, und geben Sie **10.100.100.250** ein, um eine Verbindung mit der iDRAC-Webschnittstelle herzustellen.

3. Melden Sie sich mit Dell-Standardadministratorrechten an:

   - Benutzername: **root**

   - Kennwort: **calvin**

4. Die Anmeldeinformationen für die Appliance lauten so:

   - Benutzername: **cyberx**

   - Kennwort: **xhxvhttju,@4338**

     Der Vorgang „Serverprofil importieren“ wird eingeleitet.

     > [!NOTE]
     > Stellen Sie vor dem Importieren der Datei Folgendes sicher:
     > - Sie sind der einzige Benutzer, der zurzeit mit iDRAC verbunden ist.
     > - Das System befindet sich nicht im BIOS-Menü.

5. Wechseln Sie zu **Konfiguration** > **Serverkonfigurationsprofil**. Legen Sie die folgenden Parameter fest:

   :::image type="content" source="media/tutorial-install-components/configuration-screen.png" alt-text="Screenshot der Konfiguration für Ihr Serverprofil":::

   | Parameter | Konfiguration |
   |--|--|
   | Speicherorttyp | Wählen Sie **Lokal** aus. |
   | Dateipfad | Wählen Sie **Datei auswählen** aus, und fügen Sie die XML-Konfigurationsdatei hinzu. |
   | Importieren von Komponenten | Wählen Sie **BIOS, NIC, RAID** aus. |
   | Maximale Wartezeit | Wählen Sie **20 Minuten** aus. |

6. Wählen Sie **Importieren** aus.

7. Wechseln Sie zur Überwachung des Prozesses zu **Wartung** > **Auftragswarteschlange**.

   :::image type="content" source="media/tutorial-install-components/view-the-job-queue.png" alt-text="Screenshot der Auftragswarteschlange":::

#### <a name="manually-configuring-bios"></a>Manuelles Konfigurieren von BIOS 

Sie müssen das Appliance-BIOS in folgenden Fällen manuell konfigurieren:

- Sie haben Ihre Appliance nicht von Arrow erworben.

- Sie haben zwar eine Appliance, aber keinen Zugriff auf die XML-Konfigurationsdatei.

Wechseln Sie nach dem Zugriff auf das BIOS zu **Geräteeinstellungen**.

So konfigurieren Sie manuell:

1. Greifen Sie direkt über eine Tastatur und einen Bildschirm auf das Appliance-BIOS zu, oder verwenden Sie iDRAC.

   - Wenn die Appliance keine Defender für IoT-Appliance ist, öffnen Sie einen Browser, und navigieren Sie zu der zuvor konfigurierten IP-Adresse. Melden Sie sich mit den Dell-Standardadministratorrechten an. Verwenden Sie **root** als Benutzername und **calvin** als Kennwort.

   - Wenn die Appliance eine Defender für IoT-Appliance ist, melden Sie sich mit **cyberx** als Benutzername und **xhxvhttju,@4338** als Kennwort an.

2. Wechseln Sie nach dem Zugriff auf das BIOS zu **Geräteeinstellungen**.

3. Wählen Sie die RAID-gesteuerte Konfiguration aus, indem Sie **Integrierter RAID-Controller 1: Dell PERC\<PERC H330 Adapter\> Konfigurationshilfsprogramm** auswählen.

4. Wählen Sie **Konfigurationsverwaltung** aus.

5. Wählen Sie **Virtuellen Datenträger erstellen** aus.

6. Wählen Sie im Feld **RAID-Stufe auswählen** die Option **RAID5** aus. Geben Sie im Feld **Name des virtuellen Datenträgers** den Namen **root** ein, und wählen Sie **Physische Datenträger** aus.

7. Wählen Sie **Alle überprüfen** und dann **Änderungen übernehmen** aus.

8. Klicken Sie auf **OK**.

9. Scrollen Sie nach unten, und wählen Sie  **Virtuellen Datenträger erstellen** aus.

10. Aktivieren Sie das Kontrollkästchen **Bestätigen**, und wählen Sie **Ja** aus.

11. Klicken Sie auf **OK**.

12. Kehren Sie zum Hauptbildschirm zurück, und wählen Sie **System-BIOS** aus.

13. Wählen Sie **Starteinstellungen** aus.

14. Wählen Sie für **Startmodus** die Option **BIOS** aus.

15. Wählen Sie **Zurück** und dann **Fertigstellen** aus, um die BIOS-Einstellungen zu beenden.

### <a name="software-installation-dell-r340"></a>Softwareinstallation (Dell R340)

Der Installationsvorgang dauert ungefähr 20 Minuten. Nach der Installation wird das System mehrmals neu gestartet.

So führen Sie die Installation durch:

1. Überprüfen Sie auf eine der folgenden Arten, ob die Versionsmedien in die Appliance eingebunden werden:

   - Verbinden Sie die externe CD oder den Datenträger auf einem Schlüssel mit dem Release.

   - Binden Sie das ISO-Image mithilfe von iDRAC ein. Wählen Sie nach der Anmeldung bei iDRAC die virtuelle Konsole und dann **Virtuelle Medien** aus.

2. Wählen Sie im Abschnitt **Map CD/DVD** (CD/DVD zuordnen) **Datei auswählen** aus.

3. Wählen Sie im daraufhin angezeigten Dialogfeld die ISO-Imagedatei der Version für diese Version aus.

4. Wählen Sie die Schaltfläche **Map Device** (Gerät zuordnen) aus.

   :::image type="content" source="media/tutorial-install-components/mapped-device-on-virtual-media-screen-v2.png" alt-text="Screenshot eines zugeordneten Geräts":::

5. Die Medien werden eingebunden. Klicken Sie auf **Schließen**.

6. Starten Sie die Appliance. Wenn Sie iDRAC verwenden, können Sie die Server neu starten, indem Sie die Schaltfläche **Consul Control** (Consul-Steuerung) auswählen. Wählen Sie dann in den **Tastaturmakros** die Schaltfläche **Anwenden** aus, wodurch die Tastenfolge STRG+ALT+ENTF gestartet wird.

7. Wählen Sie **Englisch** (?Deutsch?) aus.

8. Wählen Sie **SENSOR-RELEASE-\<version\> Enterprise** aus.

   :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Screenshot der Versionsauswahl":::   

9. Definieren Sie das Applianceprofil und die Netzwerkeigenschaften:

   :::image type="content" source="media/tutorial-install-components/appliance-profile-screen-v2.png" alt-text="Screenshot des Applianceprofils":::   

   | Parameter | Konfiguration |
   |--|--|
   | **Hardwareprofil** | **enterprise** |
   | **Verwaltungsschnittstelle** | **eno1** |
   | **Netzwerkparameter (vom Kunden bereitgestellt)** | - |
   |**IP-Adresse des Verwaltungsnetzwerks:** | - |
   | **Subnetzmaske:** | - |
   | **Appliance-Hostname:** | - |
   | **DNS:** | - |
   | **IP-Adresse des Standardgateways:** | - |
   | **Eingabeschnittstellen:** |  Das System generiert automatisch die Liste der Eingabeschnittstellen. Kopieren Sie zum Spiegeln der Eingabeschnittstellen alle in der Liste mit einem Komma als Trennzeichen dargestellten Elemente. Beachten Sie, dass die Bridge-Schnittstelle nicht konfiguriert werden muss. Diese Option wird nur bei besonderen Anwendungsfällen verwendet. |

10. Nach ungefähr 10 Minuten werden die beiden Anmeldeinformationssätze angezeigt. Einer ist für einen **CyberX**-Benutzer und einer für einen **Support**-Benutzer bestimmt.  

11. Speichern Sie die Appliance-ID und die Kennwörter. Sie benötigen diese Anmeldeinformationen für den Zugriff auf die Plattform, wenn Sie sie zum ersten Mal verwenden.

12. Wählen Sie **Eingeben** aus, um den Vorgang fortzusetzen.

## <a name="hpe-proliant-dl20-installation"></a>HPE ProLiant DL20-Installation

In diesem Artikel wird der Vorgang zur Installation von HPE ProLiant DL20 beschrieben, der die folgenden Schritte umfasst:

  - Aktivieren Sie den Remotezugriff, und aktualisieren Sie das Standardadministratorkennwort.
  - Konfigurieren Sie die BIOS- und RAID-Einstellungen.
  - Installieren Sie die Software.

### <a name="about-the-installation"></a>Informationen zur Installation

  - Enterprise- und SMB-Appliances können installiert werden. Der Installationsvorgang ist bei beiden Appliancetypen identisch – mit Ausnahme der Arraykonfiguration.
  - Ein Standardadministrator wird bereitgestellt. Wir empfehlen, dass Sie das Kennwort während des Netzwerkkonfigurationsvorgangs ändern.
  - Während des Netzwerkkonfigurationsvorgangs werden Sie den iLO-Port am Netzwerkport 1 konfigurieren.
  - Der Installationsvorgang dauert ungefähr 20 Minuten. Nach der Installation wird das System mehrmals neu gestartet.

### <a name="hpe-proliant-dl20-front-panel"></a>HPE ProLiant DL20 – Vorderseite

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl20-front-panel-v2.png" alt-text="HPE ProLiant DL20 – Vorderseite":::

### <a name="hpe-proliant-dl20-back-panel"></a>HPE ProLiant DL20 – Rückseite

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl20-back-panel-v2.png" alt-text="Die Rückseite des HPE ProLiant DL20":::

### <a name="enable-remote-access-and-update-the-password"></a>Aktivieren des Remotezugriffs und Aktualisieren des Kennworts

Mit dem folgenden Verfahren können Sie Netzwerkoptionen einrichten und das Standardkennwort aktualisieren.

So aktivieren und aktualisieren Sie das Kennwort:

1. Schließen Sie einen Bildschirm und eine Tastatur an die HP-Appliance an, schalten Sie die Appliance ein, und drücken Sie **F9**.

    :::image type="content" source="media/tutorial-install-components/hpe-proliant-screen-v2.png" alt-text="Screenshot des HPE ProLiant-Fensters":::

2. Wechseln Sie zu **Systemdienstprogramme** > **Systemkonfiguration** > **iLO 5-Konfigurationshilfsprogramm** > **Netzwerkoptionen**.

    :::image type="content" source="media/tutorial-install-components/system-configuration-window-v2.png" alt-text="Screenshot des Fensters „Systemkonfiguration“":::

    1.  Wählen Sie im Feld **Netzwerkschnittstellenadapter** die Option **Freigegebene Netzwerkport-LOM** aus.
    
    1.  Deaktivieren Sie DHCP.
    
    1.  Geben Sie die IP-Adresse, die Subnetzmaske und die IP-Adresse des Gateways ein.

3. Wählen Sie **F10: Speichern** aus.

4. Drücken Sie **ESC**, um zum **iLO 5-Konfigurationsdienstprogramm** zurückzukehren, und wählen Sie **Benutzerverwaltung** aus.

5. Wählen Sie **Benutzer bearbeiten/entfernen** aus. Der Administrator ist der einzige definierte Standardbenutzer. 

6. Ändern Sie das Standardkennwort, und wählen Sie **F10: Speichern** aus.

### <a name="configure-the-hpe-bios"></a>Konfigurieren des HPE-BIOS

Im folgenden Verfahren wird beschrieben, wie Sie das HPE-BIOS für die Enterprise- und SMB-Appliances konfigurieren.

So konfigurieren Sie das HPE-BIOS:

1. Wählen Sie **Systemdienstprogramme** > **Systemkonfiguration** > **BIOS/Plattformkonfiguration (RBSU)** aus.

2. Wählen Sie im Formular **BIOS/Plattformkonfiguration (RBSU)** die Option **Startoptionen** aus.

3. Ändern Sie **Startmodus** in **Legacy-BIOS-Modus**, und wählen Sie **F10: Speichern** aus.

4. Drücken Sie zweimal **ESC**, um das Formular **Systemkonfiguration** zu schließen.

#### <a name="for-the-enterprise-appliance"></a>Für die Enterprise-Appliance

1. Wählen Sie **Eingebettetes RAID 1: HPE Smart Array P408i-a SR Gen 10** > **Arraykonfiguration** > **Array erstellen** aus.

2. Wählen Sie im Formular **Create Array** (Array erstellen) alle Optionen aus. Für die **Enterprise**-Appliance gibt es drei Optionen.

#### <a name="for-the-smb-appliance"></a>Für die SMB-Appliance

1. Wählen Sie **Eingebettetes RAID 1: HPE Smart Array P208i-a SR Gen 10** > **Arraykonfiguration** > **Array erstellen** aus.

2. Wählen Sie **Proceed to Next Form** (Mit nächstem Formular fortfahren) aus.

3. Legen Sie im Formular **Set RAID Level** (RAID-Ebene festlegen) die Ebene auf **RAID 5** für Unternehmensbereitstellungen und **RAID 1** für SMB-Bereitstellungen fest.

4. Wählen Sie **Proceed to Next Form** (Mit nächstem Formular fortfahren) aus.

5. Geben Sie im Formular **Logical Drive Label** (Bezeichnung des logischen Laufwerks) **Logisches Laufwerk 1** ein.

6. Wählen Sie **Änderungen senden** aus.

7. Wählen Sie im Formular **Senden** die Option **Zurück zum Hauptmenü** aus.

8. Wählen Sie **F10: Speichern** aus, und drücken Sie zweimal **ESC**.

9. Wählen Sie im Fenster **Systemdienstprogramme** die Option **Menü für einmaliges Anmelden** aus.

10. Wählen Sie im Formular **Menü für einmaliges Anmelden** die Option **Legacy-BIOS – Menü für einmaliges Anmelden** aus.

11. Daraufhin werden die Fenster **Starten in Legacy** und **Start außer Kraft setzen** angezeigt. Wählen Sie eine Option für „Start außer Kraft setzen“ aus, beispielsweise eine CD-ROM, ein USB-Stick, ein Festplattenlaufwerk oder eine UEFI-Shell.

    :::image type="content" source="media/tutorial-install-components/boot-override-window-one-v2.png" alt-text="Screenshot des ersten Fensters für „Start außer Kraft setzen“":::

    :::image type="content" source="media/tutorial-install-components/boot-override-window-two-v2.png" alt-text="Screenshot des zweiten Fensters für „Start außer Kraft setzen“":::
### <a name="software-installation-hpe-proliant-dl20-appliance"></a>Softwareinstallation (HPE ProLiant DL20-Appliance)

Der Installationsvorgang dauert ungefähr 20 Minuten. Nach der Installation wird das System mehrmals neu gestartet.

So installieren Sie die Software:

1. Schließen Sie den Bildschirm und die Tastatur an die Appliance an, und stellen Sie eine Verbindung mit der CLI her.

2. Verbinden Sie eine externe CD oder einen Datenträger auf dem Schlüssel mit dem ISO-Image, das Sie im Defender für IoT-Portal über die Seite **Updates** heruntergeladen haben.

3. Starten Sie die Appliance.

4. Wählen Sie **Englisch** (?Deutsch?) aus.

    :::image type="content" source="media/tutorial-install-components/select-english-screen.png" alt-text="Auswahl von „Englisch“ (?Deutsch?) im CLI-Fenster":::

5. Wählen Sie **SENSOR-RELEASE-<version> Enterprise** aus.

    :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Screenshot des Bildschirms zum Auswählen einer Version":::

6. Definieren Sie im Installations-Assistenten das Applianceprofil und die Netzwerkeigenschaften:

    :::image type="content" source="media/tutorial-install-components/installation-wizard-screen-v2.png" alt-text="Screenshot des Installations-Assistenten":::

    | Parameter | Konfiguration |
    | ----------| ------------- |
    | **Hardwareprofil** | Wählen Sie **Enterprise** oder **Office** für SMB-Bereitstellungen aus. |
    | **Verwaltungsschnittstelle** | **eno2** |
    | **Standardnetzwerkparameter (normalerweise werden die Parameter vom Kunden bereitgestellt)** | **IP-Adresse des Verwaltungsnetzwerks:** <br/> <br/>**Appliance-Hostname:** <br/>**DNS:** <br/>**die IP-Adresse des Standardgateways:**|
    | **Eingabeschnittstellen:** | Das System generiert automatisch die Liste der Eingabeschnittstellen.<br/><br/>Kopieren Sie zum Spiegeln der Eingabeschnittstellen alle in der Liste mit einem Komma als Trennzeichen dargestellten Elemente: **eno5, eno3, eno1, eno6, eno4**.<br/><br/>**Für HPE DL20: Listen Sie „eno1“ und „enp1s0f4u4“ (iLo-Schnittstellen) nicht auf**.<br/><br/>**BRIDGE**: Die Bridge-Schnittstelle muss nicht konfiguriert werden. Diese Option wird nur bei besonderen Anwendungsfällen verwendet. Drücken Sie die **EINGABETASTE**, um fortzufahren. |

7. Nach ungefähr 10 Minuten werden die beiden Anmeldeinformationssätze angezeigt. Einer ist für einen **CyberX**-Benutzer und einer für einen **Support**-Benutzer bestimmt.

8. Speichern Sie die Appliance-ID und die Kennwörter. Sie benötigen die Anmeldeinformationen für den ersten Zugriff auf die Plattform.

9. Wählen Sie **Eingeben** aus, um den Vorgang fortzusetzen.

## <a name="hpe-proliant-dl360-installation"></a>HPE ProLiant DL360-Installation

  - Ein Standardadministrator wird bereitgestellt. Wir empfehlen, dass Sie das Kennwort während der Netzwerkkonfiguration ändern.

  - Während der Netzwerkkonfiguration werden Sie den iLO-Port konfigurieren.

  - Der Installationsvorgang dauert ungefähr 20 Minuten. Nach der Installation wird das System mehrmals neu gestartet.

### <a name="hpe-proliant-dl360-front-panel"></a>HPE ProLiant DL360 – Vorderseite

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl360-front-panel.png" alt-text="HPE ProLiant DL360 – Vorderseite":::

### <a name="hpe-proliant-dl360-back-panel"></a>HPE ProLiant DL360 – Rückseite

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl360-back-panel.png" alt-text="HPE ProLiant DL360 – Rückseite":::

### <a name="enable-remote-access-and-update-the-password"></a>Aktivieren des Remotezugriffs und Aktualisieren des Kennworts

Weitere Informationen finden Sie in den vorhergehenden Abschnitten zur HPE ProLiant DL20-Installation:

  - „Aktivieren des Remotezugriffs und Aktualisieren des Kennworts“

  - „Konfigurieren des HPE-BIOS“

Die Unternehmenskonfiguration ist identisch.

> [!Note]
> Im Arrayformular müssen Sie alle Optionen auswählen.

### <a name="ilo-remote-installation-from-a-virtual-drive"></a>iLO-Remoteinstallation (von einem virtuellen Laufwerk)

In diesem Verfahren wird die iLO-Installation von einem virtuellen Laufwerk beschrieben.

So führen Sie die Installation durch:

1. Melden Sie sich bei der iLO-Konsole an, und klicken Sie mit der rechten Maustaste auf den Bildschirm des Servers.

2. Wählen Sie **HTML5-Konsole** aus.

3. Wählen Sie in der Konsole das CD-Symbol und dann die Option „CD/DVD“ aus.

4. Wählen Sie **Lokale ISO-Datei** aus.

5. Wählen Sie im Dialogfeld die relevante ISO-Datei aus.

6. Wechseln Sie zum linken Symbol, wählen Sie **Power** und dann **Zurücksetzen** aus.

7. Die Appliance wird neu gestartet und führt den Sensorinstallationsvorgang aus.

### <a name="software-installation-hpe-dl360"></a>Softwareinstallation (HPE DL360)

Der Installationsvorgang dauert ungefähr 20 Minuten. Nach der Installation wird das System mehrmals neu gestartet.

So führen Sie die Installation durch:

1. Schließen Sie den Bildschirm und die Tastatur an die Appliance an, und stellen Sie eine Verbindung mit der CLI her.

2. Verbinden Sie eine externe CD oder einen Datenträger auf einem Schlüssel mit dem ISO-Image, das Sie im Defender für IoT-Portal über die Seite **Updates** heruntergeladen haben.

3. Starten Sie die Appliance.

4. Wählen Sie **Englisch** (?Deutsch?) aus.

5. Wählen Sie **SENSOR-RELEASE-<version> Enterprise** aus.

    :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Screenshot, in dem die Auswahl der Version gezeigt wird.":::

6. Definieren Sie im Installations-Assistenten das Applianceprofil und die Netzwerkeigenschaften.

    :::image type="content" source="media/tutorial-install-components/installation-wizard-screen-v2.png" alt-text="Screenshot des Installations-Assistenten":::

    | Parameter | Konfiguration |
    | ----------| ------------- |
    | **Hardwareprofil** | Wählen Sie **Unternehmen** aus. |
    | **Verwaltungsschnittstelle** | **eno2** |
    | **Standardnetzwerkparameter (vom Kunden bereitgestellt)** | **IP-Adresse des Verwaltungsnetzwerks:** <br/>**Subnetzmaske:** <br/>**Appliance-Hostname:** <br/>**DNS:** <br/>**die IP-Adresse des Standardgateways:**|
    | **Eingabeschnittstellen:**  | Das System generiert automatisch eine Liste der Eingabeschnittstellen.<br/><br/>Kopieren Sie zum Spiegeln der Eingabeschnittstellen alle in der Liste mit einem Komma als Trennzeichen dargestellten Elemente.<br/><br/>Beachten Sie, dass die Bridge-Schnittstelle nicht konfiguriert werden muss. Diese Option wird nur bei besonderen Anwendungsfällen verwendet. |

7. Nach ungefähr 10 Minuten werden die beiden Anmeldeinformationssätze angezeigt. Einer ist für einen **CyberX**-Benutzer und einer für einen **Support**-Benutzer bestimmt.

8. Speichern Sie die Appliance-ID und die Kennwörter. Sie benötigen diese Anmeldeinformationen für den ersten Zugriff auf die Plattform.

9. Wählen Sie **Eingeben** aus, um den Vorgang fortzusetzen.

## <a name="sensor-installation-for-the-virtual-appliance"></a>Sensorinstallation für das virtuelle Gerät

Sie können den virtuellen Computer für den Defender für IoT-Sensor in den folgenden Architekturen bereitstellen:


| Aufbau | Spezifikationen | Verwendung | Kommentare |
|---|---|---|---|
| **Enterprise** | CPU: 8<br/>Memory: 32G RAM<br/>HDD: 1.800 GB | Produktionsumgebung | Standard und am häufigsten |
| **Kleinunternehmen** | CPU: 4 <br/>Memory: 8G RAM<br/>HDD: 500 GB | Test- oder kleine Produktionsumgebungen | -  |
| **Office** | CPU: 4<br/>Memory: 8G RAM<br/>HDD: 100 GB | Kleine Testumgebungen | -  |

### <a name="prerequisites"></a>Voraussetzungen

Die lokale Verwaltungskonsole unterstützt sowohl VMware- als auch Hyper-V-Bereitstellungsoptionen. Bevor Sie mit der Installation beginnen, müssen die folgenden Elemente vorhanden sein:

  - VMware (ESXi 5.5 oder höher) oder Hyper-V-Hypervisor (Windows 10 Pro oder Enterprise) installiert und betriebsbereit

  - Verfügbare Hardwareressourcen für den virtuellen Computer

  - ISO-Installationsdatei für den Azure Defender für IoT-Sensor

Sorgen Sie dafür, dass der Hypervisor ausgeführt wird.

### <a name="create-the-virtual-machine-esxi"></a>Erstellen des virtuellen Computers (ESXi)

1. Melden Sie sich beim ESXi an, wählen Sie den relevanten **Datenspeicher** und dann **Datenspeicherbrowser** aus.

2. **Laden Sie das Image hoch**, und wählen Sie **Schließen** aus.

3. Wechseln Sie zu **Virtuelle Computer**, und wählen Sie **VM erstellen/registrieren** aus.

4. Wählen Sie **Neuen virtuellen Computer erstellen** und dann **Weiter** aus.

5. Fügen Sie einen Sensornamen hinzu, und wählen Sie:

   - Kompatibilität: **&lt;Neueste ESXi-Version&gt;** aus.

   - Gastbetriebssystemfamilie: **Linux**

   - Gastbetriebssystemversion: **Ubuntu Linux (64 Bit)**

6. Wählen Sie **Weiter** aus.

7. Wählen Sie den relevanten Datenspeicher und dann **Weiter** aus.

8. Ändern Sie die virtuellen Hardwareparameter entsprechend der erforderlichen Architektur.

9. Wählen Sie für **CD/DVD-Laufwerk 1** die Option **Datenspeicher-ISO-Datei** und dann die zuvor hochgeladene ISO-Datei aus.

10. Klicken Sie auf **Weiter** > **Fertig stellen**.

### <a name="create-the-virtual-machine-hyper-v"></a>Erstellen des virtuellen Computers (Hyper-V)

In diesem Verfahren wird beschrieben, wie Sie mithilfe von Hyper-V einen virtuellen Computer erstellen.

So erstellen Sie einen virtuellen Computer:

1. Erstellen Sie im Hyper-V-Manager einen virtuellen Datenträger.

2. Wählen Sie **format = VHDX** aus.

3. Wählen Sie **type = Dynamic Expanding** (Dynamisch erweiterbar) aus.

4. Geben Sie den Namen und Speicherort für die virtuelle Festplatte ein.

5. Geben Sie die erforderliche Größe (entsprechend der Architektur) ein.   

6. Überprüfen Sie die Zusammenfassung, und wählen Sie **Fertig stellen** aus.

7. Erstellen Sie im Menü **Aktionen** einen neuen virtuellen Computer.

8. Geben Sie einen Namen für den virtuellen Computer ein.

9. Wählen Sie **Generation angeben** > **Generation 1** aus.

10. Geben Sie die Speicherbelegung (entsprechend der Architektur) an, und aktivieren Sie das Kontrollkästchen für den dynamischen Arbeitsspeicher.

11. Konfigurieren Sie den Netzwerkadapter entsprechend Ihrer Server-Netzwerktopologie.

12. Verbinden Sie die zuvor erstellte VHDX-Datei mit dem virtuellen Computer.

13. Überprüfen Sie die Zusammenfassung, und wählen Sie **Fertig stellen** aus.

14. Klicken Sie mit der rechten Maustaste auf den neuen virtuellen Computer, und wählen Sie **Einstellungen** aus.

15. Wählen Sie **Hardware hinzufügen** aus, und fügen Sie einen neuen Netzwerkadapter hinzu.

16. Wählen Sie den virtuellen Switch aus, der eine Verbindung mit dem Sensorverwaltungsnetzwerk herstellen wird.

17. Ordnen Sie CPU-Ressourcen (entsprechend der Architektur) zu.

18. Verbinden Sie das ISO-Image der Verwaltungskonsole mit einem virtuellen DVD-Laufwerk.

19. Starten Sie den virtuellen Computer.

20. Wählen Sie im Menü **Aktionen** die Option **Verbinden** aus, um die Softwareinstallation fortzusetzen.

### <a name="software-installation-esxi-and-hyper-v"></a>Softwareinstallation (ESXi und Hyper-V)

In diesem Abschnitt wird die ESXi- und Hyper-V-Softwareinstallation beschrieben.

So führen Sie die Installation durch:

1. Öffnen Sie die Konsole für virtuelle Computer.

2. Der virtuelle Computer wird aus dem ISO-Image gestartet, und der Bildschirm für die Sprachauswahl wird angezeigt. Wählen Sie **Englisch** (?Deutsch?) aus.

3. Wählen Sie die erforderliche Architektur aus.

4. Definieren Sie das Applianceprofil und die Netzwerkeigenschaften:

    | Parameter | Konfiguration |
    | ----------| ------------- |
    | **Hardwareprofil** | &lt;erforderliche Architektur&gt; |
    | **Verwaltungsschnittstelle** | **ens192** |
    | **Netzwerkparameter (vom Kunden bereitgestellt)** | **IP-Adresse des Verwaltungsnetzwerks:** <br/>**Subnetzmaske:** <br/>**Appliance-Hostname:** <br/>**DNS:** <br/>**Standardgateway:** <br/>**Eingabeschnittstellen:**|
    | **Bridge-Schnittstellen:** | Die Bridge-Schnittstelle muss nicht konfiguriert werden. Diese Option wird nur bei besonderen Anwendungsfällen verwendet. |

5. Geben Sie **Y** ein, um die Einstellungen zu akzeptieren.

6. Anmeldeinformationen werden automatisch generiert und angezeigt. Kopieren Sie den Benutzernamen und das Kennwort an einen sicheren Ort, weil sie für die Anmeldung und Verwaltung erforderlich sind.

   - **Support**: Der Administrator für die Benutzerverwaltung.

   - **CyberX**: Die Entsprechung von „root“ für den Zugriff auf die Appliance.

7. Die Appliance wird neu gestartet.

8. Greifen Sie über die zuvor konfigurierte IP-Adresse auf die-Verwaltungskonsole zu: `https://ip_address`.

    :::image type="content" source="media/tutorial-install-components/defender-for-iot-sign-in-screen.png" alt-text="Screenshot des Zugriffs auf die Verwaltungskonsole":::

## <a name="virtual-appliance-on-premises-management-console-installation"></a>Virtuelles Gerät: Installation der lokalen Verwaltungskonsole

Der virtuelle Computer der lokalen Verwaltungskonsole unterstützt die folgenden Architekturen:

| Aufbau | Spezifikationen | Verwendung | 
|--|--|--|
| Enterprise <br/>(Standard und am häufigsten) | CPU: 8 <br/>Memory: 32G RAM<br/> HDD: 1,8 TB | Große Produktionsumgebungen | 
| Enterprise | CPU: 4 <br/> Memory: 8G RAM<br/> HDD: 500 GB | Große Produktionsumgebungen |
| Enterprise | CPU: 4 <br/>Memory: 8G RAM <br/> HDD: 100 GB | Kleine Testumgebungen | 
   
### <a name="prerequisites"></a>Voraussetzungen

Die lokale Verwaltungskonsole unterstützt sowohl VMware- als auch Hyper-V-Bereitstellungsoptionen. Überprüfen Sie vor Installationsbeginn die folgenden Punkte:

- VMware (ESXi 5.5 oder höher) oder Hyper-V-Hypervisor (Windows 10 Pro oder Enterprise) wurde installiert und ist betriebsbereit.

- Die Hardwareressourcen stehen für den virtuellen Computer zur Verfügung.

- Sie haben die ISO-Installationsdatei für die lokale Verwaltungskonsole.
    
- Der Hypervisor wird ausgeführt.

### <a name="create-the-virtual-machine-esxi"></a>Erstellen des virtuellen Computers (ESXi)

So erstellen Sie den virtuellen Computer (ESXi):

1. Melden Sie sich beim ESXi an, wählen Sie den relevanten **Datenspeicher** und dann **Datenspeicherbrowser** aus.

2. Laden Sie das Image hoch, und wählen Sie **Schließen** aus.

3. Wechseln Sie zu **Virtuelle Computer**.

4. Wählen Sie **VM erstellen/registrieren** aus.

5. Wählen Sie **Neuen virtuellen Computer erstellen** und dann **Weiter** aus.

6. Fügen Sie einen Sensornamen hinzu, und wählen Sie:

   - Kompatibilität: \<latest ESXi version> aus.

   - Gastbetriebssystemfamilie: Linux

   - Gastbetriebssystemversion: Ubuntu Linux (64 Bit)

7. Wählen Sie **Weiter** aus.

8. Wählen Sie den relevanten Datenspeicher und dann **Weiter** aus.

9. Ändern Sie die virtuellen Hardwareparameter entsprechend der erforderlichen Architektur.

10. Wählen Sie für **CD/DVD-Laufwerk 1** die Option **Datenspeicher-ISO-Datei** und dann die zuvor hochgeladene ISO-Datei aus.

11. Klicken Sie auf **Weiter** > **Fertig stellen**.

### <a name="create-the-virtual-machine-hyper-v"></a>Erstellen des virtuellen Computers (Hyper-V)

So erstellen Sie einen virtuellen Computer mithilfe von Hyper-V:

1. Erstellen Sie im Hyper-V-Manager einen virtuellen Datenträger.

2. Wählen Sie das Format **VHDX** aus.

3. Wählen Sie **Weiter** aus.

4. Wählen Sie den Typ **Dynamic expanding** (Dynamisch erweiterbar) aus.

5. Wählen Sie **Weiter** aus.

6. Geben Sie den Namen und Speicherort für die virtuelle Festplatte ein.

7. Wählen Sie **Weiter** aus.

8. Geben Sie die erforderliche Größe (entsprechend der Architektur) ein.

9. Wählen Sie **Weiter** aus.

10. Überprüfen Sie die Zusammenfassung, und wählen Sie **Fertig stellen** aus.

11. Erstellen Sie im Menü **Aktionen** einen neuen virtuellen Computer.

12. Wählen Sie **Weiter** aus.

13. Geben Sie einen Namen für den virtuellen Computer ein.

14. Wählen Sie **Weiter** aus.

15. Wählen Sie **Generation** aus, und legen Sie den Wert auf **Generation 1** fest.

16. Wählen Sie **Weiter** aus.

17. Geben Sie die Speicherbelegung (entsprechend der Architektur) an, und aktivieren Sie das Kontrollkästchen für den dynamischen Arbeitsspeicher.

18. Wählen Sie **Weiter** aus.

19. Konfigurieren Sie den Netzwerkadapter entsprechend Ihrer Server-Netzwerktopologie.

20. Wählen Sie **Weiter** aus.

21. Verbinden Sie die zuvor erstellte VHDX-Datei mit dem virtuellen Computer.

22. Wählen Sie **Weiter** aus.

23. Überprüfen Sie die Zusammenfassung, und wählen Sie **Fertig stellen** aus.

24. Klicken Sie mit der rechten Maustaste auf den neuen virtuellen Computer, und wählen Sie **Einstellungen** aus.

25. Wählen Sie **Hardware hinzufügen** aus, und fügen Sie für **Netzwerkadapter** einen neuen Adapter hinzu.

26. Wählen Sie für **Virtueller Switch** den Switch aus, der eine Verbindung mit dem Sensorverwaltungsnetzwerk herstellen wird.

27. Ordnen Sie CPU-Ressourcen (entsprechend der Architektur) zu.

28. Verbinden Sie das ISO-Image der Verwaltungskonsole mit einem virtuellen DVD-Laufwerk.

29. Starten Sie den virtuellen Computer.

30. Wählen Sie im Menü **Aktionen** die Option **Verbinden** aus, um die Softwareinstallation fortzusetzen.

### <a name="software-installation-esxi-and-hyper-v"></a>Softwareinstallation (ESXi und Hyper-V)

Mit dem Start des virtuellen Computers wird der Installationsvorgang aus dem ISO-Image gestartet.

So installieren Sie die Software:

1. Wählen Sie **Englisch** (?Deutsch?) aus.

2. Wählen Sie die erforderliche Architektur für Ihre Bereitstellung aus.

3. Definieren Sie die Netzwerkschnittstelle für das Sensorverwaltungsnetzwerk: Schnittstelle, IP, Subnetz, DNS-Server und Standardgateway.

4. Anmeldeinformationen werden automatisch generiert und angezeigt. Bewahren Sie diese Anmeldeinformationen an einem sicheren Ort auf, weil sie für die Anmeldung und Verwaltung erforderlich sind.

  - **Support**: Der Administrator für die Benutzerverwaltung.

  - **CyberX**: Die Entsprechung von „root“ für den Zugriff auf die Appliance.

5. Die Appliance wird neu gestartet.

6. Greifen Sie über die zuvor konfigurierte IP-Adresse auf die Verwaltungskonsole zu: `<https://ip_address>`.

    :::image type="content" source="media/tutorial-install-components/defender-for-iot-management-console-sign-in-screen.png" alt-text="Screenshot des Anmeldebildschirms auf der Verwaltungskonsole":::

## <a name="post-installation-validation"></a>Überprüfung nach der Installation

Zum Überprüfen der Installation einer physischen Appliance müssen Sie eine Reihe von Tests durchführen. Derselbe Überprüfungsvorgang gilt für alle Appliancetypen.

Führen Sie die Überprüfung über die GUI oder die CLI durch. Die Überprüfung steht für den Benutzer **Support** und den Benutzer **CyberX** zur Verfügung.

Die Überprüfung nach der Installation muss die folgenden Tests enthalten:

  - **Integritätstest**: Überprüfen Sie, ob das System ausgeführt wird.

  - **Version**: Überprüfen Sie, ob die Version richtig ist.

  - **ifconfig**: Überprüfen Sie, ob alle während des Installationsvorgangs konfigurierten Eingabeschnittstellen ausgeführt werden.

### <a name="checking-system-health-by-using-the-gui"></a>Überprüfen der Systemintegrität mithilfe der GUI

:::image type="content" source="media/tutorial-install-components/system-health-check-screen.png" alt-text="Screenshot der Systemintegritätsprüfung":::

#### <a name="sanity"></a>Integrität

- **Appliance**: Führt die Integritätsprüfung der Appliance durch. Sie können dieselbe Überprüfung mit dem CLI-Befehl `system-sanity` durchführen.

- **Version**: Zeigt die Applianceversion an.

- **Netzwerkeigenschaften**: Zeigt die Netzwerkparameter des Sensors an.

#### <a name="redis"></a>Redis

- **Arbeitsspeicher**: Liefert das Gesamtbild der Speicherauslastung, z. B. wie viel Arbeitsspeicher belegt wurde und wie viel noch übrig ist.

- **Längster Schlüssel**: Zeigt die längsten Schlüssel an, die zu einer übermäßigen Speicherauslastung führen können.

#### <a name="system"></a>System

- **Kernprotokoll**: Stellt die letzten 500 Zeilen des Kernprotokolls bereit, sodass Sie die neuesten Protokollzeilen anzeigen können, ohne das gesamte Systemprotokoll exportieren zu müssen.

- **Task-Manager**: Übersetzt die in der Tabelle der Prozesse angezeigten Aufgaben in die folgenden Ebenen: 
  
  - Persistente Ebene (Redis) 
  - Cacheebene (SQL)

- **Netzwerkstatistik**: Zeigt Ihre Netzwerkstatistik an.

- **TOP**: Zeigt die Tabelle der Prozesse (Table Of Processes) an. Dies ist ein Linux-Befehl, der eine dynamische Echtzeitansicht des ausgeführten Systems bereitstellt.

- **Überprüfung des Sicherungsspeichers**: Gibt den Status des Sicherungsspeichers an und überprüft Folgendes:
  - Den Speicherort des Sicherungsordners 
  - Die Größe des Sicherungsordners
  - Die Einschränkungen des Sicherungsordners
  - Den Zeitpunkt der letzten Sicherung
  - Wie viel Speicherplatz für die zusätzlichen Sicherungsdateien vorhanden ist

- **ifconfig**: Zeigt die Parameter für die physischen Schnittstellen der Appliance an.

- **CyberX nload**: Zeigt den Netzwerkdatenverkehr und die Bandbreite mithilfe der 6-Sekunden-Tests an.

- **Fehler aus Core.log**: Zeigt Fehler aus der Kernprotokolldatei an.

So greifen Sie auf das Tool zu:

1. Melden Sie sich beim Sensor mit den Benutzeranmeldeinformationen für **Support** an.

2. Wählen Sie im Fenster **Systemeinstellungen** die Option **Systemstatistik** aus.

    :::image type="icon" source="media/tutorial-install-components/system-statistics-icon.png" border="false":::

### <a name="checking-system-health-by-using-the-cli"></a>Überprüfen der Systemintegrität mithilfe der CLI

**Test 1: Integrität**

Vergewissern Sie sich, dass das System aktiv ist und ausgeführt wird:

1. Stellen Sie eine Verbindung mit der CLI mit dem Linux-Terminal (z. b. PuTTY) und dem Benutzer **Support** her.

2. Geben Sie `system sanity` ein.

3. Überprüfen Sie, ob alle Dienste grün sind (d. h., sie werden ausgeführt).

    :::image type="content" source="media/tutorial-install-components/support-screen.png" alt-text="Der Screenshot zeigt, dass Dienste ausgeführt werden.":::

4. Überprüfen Sie, ob unten **System is UP! (prod)** (System ist AKTIV (Prod)) angezeigt wird.

**Test 2: Versionsüberprüfung**

Überprüfen Sie, ob die richtige Version verwendet wird:

1. Stellen Sie eine Verbindung mit der CLI mit dem Linux-Terminal (z. b. PuTTY) und dem Benutzer **Support** her.

2. Geben Sie `system version` ein.

3. Überprüfen Sie, ob die richtige Version angezeigt wird.

**Test 3: Netzwerküberprüfung**

Überprüfen Sie, ob alle während des Installationsvorgangs konfigurierten Eingabeschnittstellen ausgeführt werden:

1. Stellen Sie eine Verbindung mit der CLI mit dem Linux-Terminal (z. b. PuTTY) und dem Benutzer **Support** her.

2. Geben Sie `network list` ein (entspricht dem Linux-Befehl `ifconfig`).

3. Überprüfen Sie, ob die erforderlichen Eingabeschnittstellen angezeigt werden. Wenn beispielsweise zwei Quad-Kupfer-NICs installiert wurden, sollte die Liste 10 Schnittstellen enthalten.

    :::image type="content" source="media/tutorial-install-components/interface-list-screen.png" alt-text="Screenshot der Liste von Schnittstellen":::

**Test 4: Verwaltungszugriff auf die Benutzeroberfläche**

Überprüfen Sie, ob Sie auf die Web-GUI der Konsole zugreifen können:

1. Schließen Sie einen Laptop mit einem Ethernet-Kabel an den Verwaltungsport (**Gb1**) an.

2. Definieren Sie für die Laptop-NIC-Adresse, dass sie sich in demselben Bereich wie die Appliance befinden soll.

    :::image type="content" source="media/tutorial-install-components/access-to-ui.png" alt-text="Screenshot des Verwaltungszugriffs auf die Benutzeroberfläche":::

3. Pingen Sie die IP-Adresse der Appliance vom Laptop aus, um die Konnektivität zu überprüfen (Standard: 10.100.10.1).

4. Öffnen Sie den Chrome-Browser auf dem Laptop, und geben Sie die IP-Adresse der Appliance ein.

5. Wählen Sie im Fenster **Ihre Verbindung ist nicht privat** die Option **Erweitert** aus, und setzen Sie den Vorgang fort.

6. Der Test ist erfolgreich, wenn der Defender für IoT-Anmeldebildschirm angezeigt wird.

   :::image type="content" source="media/tutorial-install-components/defender-for-iot-sign-in-screen.png" alt-text="Screenshot des Zugriffs auf die Verwaltungskonsole":::

## <a name="troubleshooting"></a>Problembehandlung

### <a name="you-cant-connect-by-using-a-web-interface"></a>Sie können keine Verbindung über eine Weboberfläche herstellen

1. Überprüfen Sie, ob sich der Computer, mit dem Sie die Verbindung herstellen möchten, in demselben Netzwerk wie die Appliance befindet.

2. Überprüfen Sie, ob das GUI-Netzwerk mit dem Verwaltungsport verbunden ist.

3. Pingen Sie die IP-Adresse der Appliance. Wenn Pingen nicht möglich ist:

   1. Schließen Sie einen Monitor und eine Tastatur an die Appliance an.

   1. Verwenden Sie den Benutzer **Support** und dessen Kennwort für die Anmeldung.

   1. Verwenden Sie den Befehl `network list`, um die aktuelle IP-Adresse anzuzeigen.

      :::image type="content" source="media/tutorial-install-components/network-list.png" alt-text="Screenshot der Netzwerkliste":::

4. Wenn die Netzwerkparameter falsch konfiguriert wurden, ändern Sie sie mit dem folgenden Verfahren:

   1. Verwenden Sie den Befehl `network edit-settings`.

   1. Wählen Sie zum Ändern der IP-Adresse des Verwaltungsnetzwerks **Y** aus.

   1. Wählen Sie zum Ändern der Subnetzmaske **Y** aus.

   1. Wählen Sie zum Ändern des DNS **Y** aus.

   1. Wählen Sie zum Ändern der IP-Adresse des Standardgateways **Y** aus.

   1. Wählen Sie für eine Änderung der Eingabeschnittstelle (nur Sensor) **N** aus.

   1. Wählen Sie zum Übernehmen der Einstellungen **Y** aus.

5. Stellen Sie nach dem Neustart eine Verbindung mit den Anmeldeinformationen des Supports her, und überprüfen Sie mit dem Befehl `network list`, ob die Parameter geändert wurden.

6. Versuchen Sie von der GUI aus erneut, zu pingen und eine Verbindung herzustellen.

### <a name="the-appliance-isnt-responding"></a>Die Appliance reagiert nicht

1. Schließen Sie einen Monitor und eine Tastatur an die Appliance an, oder verwenden Sie PuTTY, um eine Remoteverbindung mit der CLI herzustellen.

2. Verwenden Sie die Anmeldeinformationen für den Benutzer **Support**, um sich anzumelden.

3. Verwenden Sie den Befehl `system sanity`, um zu überprüfen, ob alle Prozesse ausgeführt werden.

    :::image type="content" source="media/tutorial-install-components/system-sanity-screen.png" alt-text="Screenshot des Befehls „system sanity“ für Systemintegrität":::

Wenden Sie sich bei allen anderen Problemen an den [Microsoft-Support](https://support.microsoft.com/en-us/supportforbusiness/productselection?sapId=82c88f35-1b8e-f274-ec11-c6efdd6dd099).

## <a name="appendix-a-mirroring-port-on-vswitch-esxi"></a>Anhang A: Spiegelung des Ports auf vSwitch (ESXi)

### <a name="configure-a-span-port-on-an-existing-vswitch"></a>Konfigurieren eines SPAN-Ports auf einem vorhandenen vSwitch

Ein vSwitch enthält keine Spiegelungsfunktionen, aber Sie können einen SPAN-Port mithilfe einer einfachen Problemumgehung implementieren.

So konfigurieren Sie einen SPAN-Port:

1. Öffnen Sie „vSwitch“-Eigenschaften.

2. Wählen Sie **Hinzufügen**.

3. Wählen Sie **Virtueller Computer** > **Weiter** aus.

4. Fügen Sie die Netzwerkbezeichnung **SPAN-Netzwerk** ein, wählen Sie **VLAN-ID** > **Alle** und dann **Weiter** aus.

5. Wählen Sie **Fertig stellen** aus.

6. Wählen Sie **SPAN-Netzwerk** > **Bearbeiten* aus.

7. Wählen Sie **Sicherheit** aus, und überprüfen Sie, ob die Richtlinie für **Promisker Modus** auf den Modus **Akzeptieren** festgelegt wurde.

8. Wählen Sie **OK** und dann **Schließen** aus, um die vSwitch-Eigenschaften zu schließen.

9. Öffnen Sie die **XSense VM**-Eigenschaften.

10. Wählen Sie für **Netzwerkadapter 2** das **SPAN**-Netzwerk aus.

11. Klicken Sie auf **OK**.

12. Stellen Sie eine Verbindung mit dem Sensor her, und überprüfen Sie, ob die Spiegelung funktioniert.

## <a name="appendix-b-access-sensors-from-the-on-premises-management-console"></a>Anhang B: Zugreifen auf Sensoren über die lokale Verwaltungskonsole

Sie können die Systemsicherheit verbessern, indem Sie den direkten Benutzerzugriff auf den Sensor verhindern. Verwenden Sie stattdessen Proxy-Tunnelung, damit Benutzer mit einer einzigen Firewallregel über die lokale Verwaltungskonsole auf den Sensor zugreifen können. Diese Vorgehensweise schränkt die Möglichkeit eines nicht autorisierten Zugriffs auf die Netzwerkumgebung außerhalb des Sensors ein. Die Benutzeroberfläche bleibt bei der Anmeldung beim Sensor unverändert.

:::image type="content" source="media/tutorial-install-components/sensor-system-graph.png" alt-text="Screenshot des Zugriffs auf den Sensor":::

So aktivieren Sie Tunneln:

1. Melden Sie sich bei der CLI der lokalen Verwaltungskonsole mit den Benutzeranmeldeinformationen für **CyberX** oder **Support** an.

2. Geben Sie `sudo cyberx-management-tunnel-enable` ein.

3. Drücken Sie die **EINGABETASTE**.

4. Geben Sie `--port 10000` ein.

### <a name="next-steps"></a>Nächste Schritte

[Einrichten Ihres Netzwerks](how-to-set-up-your-network.md)

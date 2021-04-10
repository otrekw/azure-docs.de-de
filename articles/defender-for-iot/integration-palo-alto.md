---
title: Palo Alto-Integration
description: Die Defender für IoT-Plattform für die kontinuierliche ICS-Bedrohungsüberwachung wurde in die Palo Alto-Firewalls der nächsten Generation integriert, um eine schnellere und effizientere Blockierung von kritischen Bedrohungen zu ermöglichen.
ms.date: 1/17/2021
ms.topic: article
ms.openlocfilehash: 49c00f9ad7cc1980a8690e35ed19351e20417c6a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104783930"
---
# <a name="about-the-palo-alto-integration"></a>Informationen zur Palo Alto-Integration

Die Defender für IoT-Plattform für die kontinuierliche ICS-Bedrohungsüberwachung wurde in die Palo Alto-Firewalls der nächsten Generation integriert, um eine schnellere und effizientere Blockierung von kritischen Bedrohungen zu ermöglichen.

Die folgenden Integrationstypen sind verfügbar:

- Option für die automatische Blockierung: Direkte Integration von Defender für IoT und Palo Alto

- Senden von Empfehlungen zur Blockierung an das zentrale Verwaltungssystem: Integration von Defender für IoT und Panorama

## <a name="configure-immediate-blocking-by-specified-palo-alto-firewall"></a>Konfigurieren der sofortigen Blockierung durch die angegebene Palo Alto-Firewall

In kritischen Fällen, wie beispielsweise Warnungen zu Schadsoftware, können Sie eine automatische Blockierung aktivieren. Hierfür konfigurieren Sie eine Weiterleitungsregel in Defender für IoT, die den Blockierungsbefehl direkt an eine bestimmte Palo Alto-Firewall sendet.

Wenn Defender für IoT eine kritische Bedrohung identifiziert, wird eine Warnung gesendet, die eine Option zum Blockieren der infizierten Quelle enthält. Durch Auswählen von **Quelle blockieren** in den Warnungsdetails wird die Weiterleitungsregel aktiviert, die den Blockierungsbefehl an die angegebene Palo Alto-Firewall sendet.

So konfigurieren Sie die Regel:

1. Wählen Sie im linken Bereich die Option **Weiterleitung** aus.

   :::image type="content" source="media/integration-paloalto/forwarding.png" alt-text="Der Bildschirm zum Weiterleiten von Warnungen":::

1. Wählen Sie **Weiterleitungsregel erstellen** aus.

   :::image type="content" source="media/integration-paloalto/forward-rule.png" alt-text="Weiterleitungsregel erstellen":::

1. So konfigurieren Sie die Palo Alto NGFW-Weiterleitungsregel:  
 
   - Definieren Sie die Standardregelparameter, und wählen Sie im Dropdownfeld **Aktionen** die Aktion **An Palo Alto NGFW senden** aus.
   
   :::image type="content" source="media/integration-paloalto/edit.png" alt-text="Bearbeiten der Weiterleitungsregel":::

1. Legen Sie im Bereich „Aktionen“ die folgenden Parameter fest:

   - **Host**: Geben Sie die IP-Adresse des NGFW-Servers ein.
   - **Port:** Geben Sie den Port des NGFW-Servers ein.
   - **Benutzername**: Geben Sie den Benutzernamen des NGFW-Servers ein.
   - **Kennwort**: Geben Sie das Kennwort des NGFW-Servers ein.
   - **Konfigurieren**: Richten Sie die folgenden Optionen ein, um die Blockierung der verdächtigen Quellen durch die Palo Alto-Firewall zuzulassen:
     - **Unzulässige Funktionscodes blockieren**: Protokollverstöße: ungültiger Feldwert, der gegen die ICS-Protokollspezifikation verstößt (potenzieller Exploit).
     - **Nicht autorisierte SPS-Programmierung/Firmwareupdates blockieren**: Nicht autorisierte SPS-Änderungen.
     - **Nicht autorisierte SPS-Beendigung blockieren**: SPS-Beendigung (Downtime).
     - **Auf Schadsoftware bezogene Warnungen blockieren**: Blockierung von Angriffsversuchen durch industrielle Schadsoftware (TRITON, NotPetya usw.). Sie können die Option **Automatische Blockierung** auswählen. In diesem Fall wird die Blockierung automatisch und sofort ausgeführt.
     - **Nicht autorisierte Überprüfung blockieren**: Nicht autorisierte Überprüfung (potenzielle Aufklärung).
     
1. Klicken Sie auf **Submit** (Senden).

So blockieren Sie die verdächtige Quelle: 

1. Wählen Sie im Bereich **Warnungen** die auf die Palo Alto-Integration bezogene Warnung aus. Das Dialogfeld **Warnungsdetails** wird angezeigt.
   
   :::image type="content" source="media/integration-paloalto/unauthorized.png" alt-text="Warnungsdetails":::

1. Zum automatischen Blockieren der verdächtigen Quelle wählen Sie **Quelle blockieren** aus. Das Dialogfeld **Bestätigen** wird angezeigt.

   :::image type="content" source="media/integration-paloalto/please.png" alt-text="Bestätigen der Blockierung auf dem Bildschirm „Bestätigen“":::

1. Klicken Sie im Dialogfeld **Bestätigen** auf **OK**. Die verdächtige Quelle wird durch die Palo Alto-Firewall blockiert.

## <a name="sending-blocking-policies-to-palo-alto-panorama"></a>Senden von Blockierungsrichtlinien an Palo Alto Panorama

Defender für IoT und Palo Alto Networks bieten eine sofort einsatzbereite Integration, die automatisch neue Richtlinien in Panorama erstellt, der Netzwerkverwaltungslösung von Palo Alto Networks. Diese Integration erfordert eine Bestätigung durch den Panorama-Administrator und erlaubt keine automatische Blockierung.

Die Integration ist für folgende Incidents gedacht:

- **Nicht autorisierte SPS-Änderungen**: Ein Update der Leiterlogik oder Firmware eines Geräts. Dieser Incident kann eine legitime Aktivität oder eine versuchte Kompromittierung des Geräts repräsentieren. Die Kompromittierung kann durch Einschleusen von schädlichem Code wie einem Remotezugriffstrojaner (Remote Access Trojan, RAT) oder durch Parameter erfolgen, die dazu führen, dass ein physischer Prozess – beispielsweise eine drehende Turbine – auf unsichere Weise ausgeführt wird.

- **Protokollverstoß**: Eine Paketstruktur oder ein Feldwert, die bzw. der gegen die Protokollspezifikation verstößt. Dieser Incident kann eine falsch konfigurierte Anwendung oder einen böswilligen Versuch der Kompromittierung eines Geräts repräsentieren. Ein Beispiel hierfür ist das Verursachen einer Pufferüberlaufbedingung im Zielgerät.

- **SPS-Beendigung**: Ein Befehl, der bewirkt, dass ein Gerät nicht mehr funktioniert. Dies stellt ein Risiko für den physischen Prozess dar, der von der SPS gesteuert wird.

- **Industrielle Schadsoftware im ICS-Netzwerk gefunden**: Schadsoftware, die ICS-Geräte über ihre nativen Protokolle manipuliert, beispielsweise TRITON und Industroyer. Defender für IoT erkennt auch IT-Schadsoftware, die per Lateral Movement in die ICS- und SCADA-Umgebung verschoben wurde, z. B. Conficker, WannaCry und NotPetya.

- **Schadsoftware zum Scannen**: Aufklärungstools, die in einer Phase vor einem Angriff Daten zur Systemkonfiguration sammeln. Beispielsweise durchsucht der Havex-Trojaner Industrienetzwerke nach Geräten, die OPC verwenden. OPC ist ein Standardprotokoll, das von Windows-basierten SCADA-Systemen für die Kommunikation mit ICS-Geräten verwendet wird.

## <a name="the-process"></a>Der Prozess

Wenn Defender für IoT einen vorkonfigurierten Anwendungsfall erkennt, wird der Warnung die Schaltfläche **Quelle blockieren** hinzugefügt. Wenn dann der **CyberX**-Benutzer die Schaltfläche **Quelle blockieren** auswählt, erstellt Defender für IoT durch Senden der vordefinierten Weiterleitungsregel Richtlinien in Panorama.

Die Richtlinie wird nur angewendet, wenn der Panorama-Administrator sie per Push an die entsprechende NGFW im Netzwerk sendet.

In IT-Netzwerken können dynamische IP-Adressen vorhanden sein. Daher muss die Richtlinie für diese Subnetze auf dem vollqualifizierten Domänennamen (DNS-Namen) basieren, nicht auf der IP-Adresse. Defender für IoT führt ein Reverse-Lookup durch und gleicht Geräte mit dynamischer IP-Adresse im konfigurierten Stundenintervall mit dem zugehörigen vollqualifizierten Domänennamen (DNS-Namen) ab.

Darüber hinaus sendet Defender für IoT eine E-Mail an den entsprechenden Panorama-Benutzer, dass eine neue von Defender für IoT erstellte Richtlinie auf Genehmigung wartet. Die folgende Abbildung zeigt die Architektur der Integration von Defender für IoT und Panorama.

:::image type="content" source="media/integration-paloalto/structure.png" alt-text="Architektur der Integration von CyberX und Panorama":::

## <a name="create-panorama-blocking-policies-in-defender-for-iot-configuration"></a>Erstellen von Panorama-Blockierungsrichtlinien in der Defender für IoT-Konfiguration

### <a name="to-configure-dns-lookup"></a>So konfigurieren Sie das DNS-Lookup

1. Wählen Sie im linken Bereich **Systemeinstellungen** aus.

1. Wählen Sie im Bereich **Systemeinstellungen** die Option **DNS-Einstellungen** :::image type="icon" source="media/integration-paloalto/settings.png"::: aus.

   :::image type="content" source="media/integration-paloalto/configuration.png" alt-text="Konfigurieren der DNS-Einstellungen":::

1. Legen Sie Dialogfeld **DNS-Einstellungen bearbeiten** die folgenden Parameter fest:

   - **Status:** Der Status des DNS-Resolvers.

   - **DNS-Serveradresse**: Geben Sie die IP-Adresse oder den vollqualifizierten Domänenname des DNS-Netzwerkservers ein.
   - **DNS-Serverport**: Geben Sie den zur Abfrage des DNS-Servers verwendeten Port ein.
   - **Subnetze**: Legen Sie den Subnetzbereich für dynamische IP-Adressen fest. Dies ist der Bereich, den Defender für IoT zum Reverse-Lookup von IP-Adressen im DNS-Server verwendet, um aktuelle vollqualifizierte Domänennamen abzugleichen.
   - **Reverse-Lookup planen**: Definieren Sie die Planungsoptionen wie folgt:
     - Nach bestimmter Uhrzeit: Geben Sie an, wann das Reverse-Lookup jeden Tag erfolgen soll.
     - Nach festen Intervallen (in Stunden): Legen Sie fest, wie häufig das Reverse-Lookup ausgeführt werden soll.
   - **Anzahl von Bezeichnungen**: Weisen Sie Defender für IoT an, IP-Netzwerkadressen automatisch in die vollqualifizierten Domänennamen der Geräte aufzulösen. <br />Um die DNS-Auflösung zu konfigurieren, fügen Sie die Anzahl der anzuzeigenden Domänenbezeichnungen hinzu. Es werden bis zu 30 Bezeichnungen von links nach rechts angezeigt.
1. Wählen Sie **SAVE** (SPEICHERN) aus.
1. Um sicherzustellen, dass die DNS-Einstellungen korrekt sind, wählen Sie **Lookuptest** aus. Der Test stellt sicher, dass die IP-Adresse und der Port des DNS-Servers korrekt festgelegt sind.

### <a name="to-configure-a-forwarding-rule-to-blocks-suspected-traffic-with-the-palo-alto-firewall"></a>So konfigurieren Sie eine Weiterleitungsregel zum Blockieren von verdächtigem Datenverkehr mit der Palo Alto-Firewall

1. Wählen Sie im linken Bereich die Option **Weiterleitung** aus. Der Bereich „Weiterleitung“ wird angezeigt.

   :::image type="content" source="media/integration-paloalto/forward.png" alt-text="Der Weiterleitungsbildschirm":::

1. Wählen Sie im Bereich **Weiterleitung** die Option **Weiterleitungsregel erstellen** aus.

   :::image type="content" source="media/integration-paloalto/forward-rule.png" alt-text="Weiterleitungsregel erstellen":::

1. So konfigurieren Sie die Palo Alto Panorama-Weiterleitungsregel:

   Definieren Sie die Standardregelparameter, und wählen Sie im Dropdownfeld **Aktionen** die Aktion **An Palo Alto Panorama senden** aus. Der Bereich mit Aktionsdetails wird angezeigt.

   :::image type="content" source="media/integration-paloalto/details.png" alt-text="Aktion select":::

1. Legen Sie im Bereich „Aktionen“ die folgenden Parameter fest:

   - **Host**: Geben Sie die IP-Adresse des Panorama-Servers ein.

   - **Port:** Geben Sie den Port des Panorama-Servers ein.
   - **Benutzername**: Geben Sie den Benutzernamen des Panorama-Servers ein.
   - **Kennwort**: Geben Sie das Kennwort des Panorama-Servers ein.
   - **Berichtsadresse**: Definieren Sie wie folgt, wie die Blockierung ausgeführt wird:
   
     - **Nach IP-Adresse**: Blockierungsrichtlinien werden in Panorama immer basierend auf der IP-Adresse erstellt.
     
     - **Nach FQDN oder IP-Adresse**: Blockierungsrichtlinien werden in Panorama basierend auf dem vollqualifizierten Domänennamen erstellt, falls vorhanden, andernfalls basierend auf der IP-Adresse.
     
   - **E-Mail**: Legen Sie die E-Mail-Adresse für die E-Mail-Benachrichtigung zur Richtlinie fest.
     > [!NOTE]
     > Stellen Sie sicher, dass Sie einen Mailserver in Defender für IoT konfiguriert haben. Wenn keine E-Mail-Adresse angegeben wurde, sendet Defender für IoT keine Benachrichtigungs-E-Mail.
   - **Bei Warnungserkennung DNS-Lookup ausführen** (Kontrollkästchen): Wenn unter „Berichtsadresse“ die Option „Nach FQDN oder IP-Adresse“ festgelegt ist, ist dieses Kontrollkästchen standardmäßig aktiviert. Wenn nur die IP-Adresse festgelegt ist, ist diese Option deaktiviert.
   - **Konfigurieren**: Richten Sie die folgenden Optionen ein, um die Blockierung verdächtiger Quellen durch Palo Alto Panorama zuzulassen:
   
     - **Unzulässige Funktionscodes blockieren**: Protokollverstöße: ungültiger Feldwert, der gegen die ICS-Protokollspezifikation verstößt (potenzieller Exploit).
     
     - **Nicht autorisierte SPS-Programmierung/Firmwareupdates blockieren**: Nicht autorisierte SPS-Änderungen.
     
     - **Nicht autorisierte SPS-Beendigung blockieren**: SPS-Beendigung (Downtime).
     
     - **Auf Schadsoftware bezogene Warnungen blockieren**: Blockierung von Angriffsversuchen durch industrielle Schadsoftware (TRITON, NotPetya usw.). Sie können die Option **Automatische Blockierung** auswählen. In diesem Fall wird die Blockierung automatisch und sofort ausgeführt.
     
     - **Nicht autorisierte Überprüfung blockieren**: Nicht autorisierte Überprüfung (potenzielle Aufklärung).
     
1. Klicken Sie auf **Submit** (Senden).

### <a name="to-block-the-suspicious-source"></a>So blockieren Sie die verdächtige Quelle

1. Wählen Sie im Bereich **Warnungen** die auf die Palo Alto-Integration bezogene Warnung aus. Das Dialogfeld **Warnungsdetails** wird angezeigt.

   :::image type="content" source="media/integration-paloalto/unauthorized.png" alt-text="Warnungsdetails":::        

1. Zum automatischen Blockieren der verdächtigen Quelle wählen Sie **Quelle blockieren** aus.

1. Klicken Sie im Dialogfeld **Bestätigen** auf **OK**.

   :::image type="content" source="media/integration-paloalto/please.png" alt-text="Confirm":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungsinformationen weiterleiten](how-to-forward-alert-information-to-partners.md).

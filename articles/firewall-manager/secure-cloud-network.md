---
title: 'Tutorial: Schützen Ihres virtuellen Hubs mit Azure Firewall Manager'
description: In diesem Tutorial erfahren Sie, wie Sie Ihren virtuellen Hub mit Azure Firewall Manager über das Azure-Portal schützen.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 03/19/2021
ms.author: victorh
ms.openlocfilehash: 14ec6fafbbadff2ecc37b229270c269f068a666f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104670459"
---
# <a name="tutorial-secure-your-virtual-hub-using-azure-firewall-manager"></a>Tutorial: Schützen Ihres virtuellen Hubs mit Azure Firewall Manager

Mit Azure Firewall Manager können Sie geschützte virtuelle Hubs erstellen, um für private IP-Adressen, für Azure PaaS und für das Internet bestimmten Datenverkehr Ihres Cloudnetzwerks zu schützen. Das Datenverkehrsrouting an die Firewall ist automatisiert, sodass keine benutzerdefinierten Routen (User Defined Routes, UDRs) erstellt werden müssen.

![Schützen des Cloudnetzwerks](media/secure-cloud-network/secure-cloud-network.png)

Firewall Manager unterstützt außerdem die Architektur für virtuelle Hubnetzwerke. Einen Vergleich der Architekturtypen für geschützte virtuelle Hubs und virtuelle Hubnetzwerke finden Sie unter [Welche Architekturoptionen gibt es für Azure Firewall Manager?](vhubs-and-vnets.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen des virtuellen Spoke-Netzwerks
> * Erstellen eines geschützten virtuellen Hubs
> * Herstellen einer Verbindung für die virtuellen Hub-and-Spoke-Netzwerke
> * Weiterleiten von Datenverkehr an Ihren Hub
> * Bereitstellen der Server
> * Erstellen einer Firewallrichtlinie und Schützen Ihres Hubs
> * Testen der Firewall

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-hub-and-spoke-architecture"></a>Erstellen einer Hub-Spoke-Architektur

Erstellen Sie zunächst virtuelle Spoke-Netzwerke für Ihre Server.

### <a name="create-two-spoke-virtual-networks-and-subnets"></a>Erstellen von zwei virtuellen Spoke-Netzwerken und Subnetzen

Die beiden virtuellen Netzwerke werden jeweils einen Workloadserver enthalten und durch die Firewall geschützt sein.

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Suchen Sie nach **Virtuelles Netzwerk**, und wählen Sie **Erstellen** aus.
2. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, geben Sie **fw-manager-rg** als Name ein, und wählen Sie **OK** aus.
2. Geben Sie unter **Name** den Namen **Spoke-01** ein.
3. Wählen Sie unter **Region** die Option **(USA) USA, Osten** aus.
4. Klicken Sie auf **Weiter: IP-Adressen**.
1. Geben Sie unter **Adressraum** die Zeichenfolge **10.0.0.0/16** ein.
3. Wählen Sie unter **Subnetzname** die Einstellung **Standard** aus.
4. Geben Sie unter **Subnetzname** die Zeichenfolge **Workload-01-SN** ein.
5. Geben Sie unter **Subnetzadressbereich** den Bereich **10.0.1.0/24** ein.
6. Klicken Sie auf **Speichern**.
1. Klicken Sie auf **Überprüfen + erstellen**.
2. Klicken Sie auf **Erstellen**.

Wiederholen Sie dieses Verfahren, um ein weiteres ähnliches virtuelles Netzwerk zu erstellen:

Name: **Spoke-02**<br>
Adressraum: **10.1.0.0/16**<br>
Subnetzname: **Workload-02-SN**<br>
Subnetzadressbereich: **10.1.1.0/24**

### <a name="create-the-secured-virtual-hub"></a>Erstellen des geschützten virtuellen Hubs

Erstellen Sie mithilfe von Firewall Manager Ihren geschützten virtuellen Hub.

1. Wählen Sie auf der Startseite des Azure-Portals **Alle Dienste** aus.
2. Geben Sie **Firewall Manager** in das Suchfeld ein, und wählen Sie **Firewall Manager** aus.
3. Wählen Sie auf der Seite **Firewall Manager** die Option **View secured virtual hubs** (Geschützte virtuelle Hubs anzeigen) aus.
4. Wählen Sie auf der Seite **Firewall Manager | Geschützte virtuelle Hubs** die Option **Neuen geschützten virtuellen Hub erstellen** aus.
5. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **fw-manager-rg** aus.
7. Wählen Sie als **Region** die Option **USA, Osten** aus.
1. Geben Sie als **Name des geschützten virtuellen Hubs** den Namen **Hub-01** ein.
2. Geben Sie unter **Adressraum des Hubs** den Adressraum **10.2.0.0/16** ein.
3. Geben Sie als Name des neuen vWAN den Namen **Vwan-01** ein.
4. Lassen Sie das Kontrollkästchen **VPN Gateway zum Aktivieren vertrauenswürdiger Sicherheitspartner einbeziehen** deaktiviert.
5. Klicken Sie auf **Weiter: Azure Firewall** aus.
6. Übernehmen Sie die Standardeinstellung **Azure Firewall** **aktiviert**, und wählen Sie **Weiter: Vertrauenswürdiger Sicherheitspartner** aus.
7. Übernehmen Sie die Standardeinstellung **Vertrauenswürdiger Sicherheitspartner** **deaktiviert**, und wählen Sie **Weiter: Überprüfen + erstellen**.
8. Klicken Sie auf **Erstellen**. 

   Die Bereitstellung dauert ungefähr 30 Minuten.

Nach Abschluss der Bereitstellung können Sie die öffentliche IP-Adresse der Firewall abrufen.

1. Öffnen Sie **Firewall Manager**.
2. Wählen Sie **Virtuelle Hubs** aus.
3. Wählen Sie **hub-01** aus.
7. Wählen Sie **Öffentliche IP-Konfiguration** aus.
8. Notieren Sie sich die öffentliche IP-Adresse zur späteren Verwendung.

### <a name="connect-the-hub-and-spoke-virtual-networks"></a>Herstellen einer Verbindung für die virtuellen Hub-and-Spoke-Netzwerke

Jetzt können Sie das Peering für die virtuellen Hub-and-Spoke-Netzwerke durchführen.

1. Wählen Sie die Ressourcengruppe **fw-manager-rg** und anschließend das virtuelle WAN **Vwan-01** aus.
2. Wählen Sie unter **Konnektivität** die Option **Virtuelle Netzwerkverbindungen** aus.
3. Wählen Sie **Verbindung hinzufügen** aus.
4. Geben Sie unter **Verbindungsname** den Namen **hub-spoke-01** ein.
5. Wählen Sie unter **Hubs** die Option **Hub-01** aus.
6. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **fw-manager-rg** aus.
7. Wählen Sie unter **Virtuelles Netzwerk** die Option **Spoke-01** aus.
8. Klicken Sie auf **Erstellen**.

Wiederholen Sie diesen Vorgang, um das virtuelle Netzwerk **Spoke-02** zu verbinden: Verbindungsname – **hub-spoke-02**

## <a name="deploy-the-servers"></a>Bereitstellen der Server

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Wählen Sie dann in der Liste **Beliebt** die Option **Windows Server 2016 Datacenter** aus.
3. Geben Sie die folgenden Werte für den virtuellen Computer ein:

   |Einstellung  |Wert  |
   |---------|---------|
   |Ressourcengruppe     |**fw-manager-rg**|
   |Name des virtuellen Computers     |**Srv-workload-01**|
   |Region     |**(USA) USA, Osten**|
   |Benutzername des Administrators     |Geben Sie einen Benutzernamen ein.|
   |Kennwort     |Geben Sie ein Kennwort ein.|

4. Wählen Sie unter **Regeln für eingehende Ports** für **Öffentliche Eingangsports** die Option **Keine** aus.
6. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie **Weiter: Datenträger**.
7. Übernehmen Sie die Standardeinstellungen für Datenträger, und wählen Sie **Weiter: Netzwerk** aus.
8. Wählen Sie **Spoke-01** für das virtuelle Netzwerk und dann **Workload-01-SN** für das Subnetz aus.
9. Wählen Sie unter **Öffentliche IP** die Option **Keine** aus.
11. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie **Weiter: Verwaltung** aus.
12. Wählen Sie **Deaktivieren** aus, um die Startdiagnose zu deaktivieren. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie dann auf **Bewerten + erstellen**.
13. Überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und wählen Sie dann **Erstellen** aus.

Konfigurieren Sie anhand der Angaben in der folgenden Tabelle eine weitere VM mit dem Namen **Srv-Workload-02**. Die restliche Konfiguration ist mit der Konfiguration des virtuellen Computers **Srv-workload-01** identisch.

|Einstellung  |Wert  |
|---------|---------|
|Virtuelles Netzwerk|**Spoke-02**|
|Subnet|**Workload-02-SN**|

Nachdem die Server bereitgestellt wurden, wählen Sie eine Serverressource aus, und notieren Sie unter **Netzwerk** die private IP-Adresse für jeden Server.

## <a name="create-a-firewall-policy-and-secure-your-hub"></a>Erstellen einer Firewallrichtlinie und Schützen Ihres Hubs

Eine Firewallrichtlinie definiert Regelsammlungen für die Weiterleitung von Datenverkehr an einen oder mehrere geschützte virtuelle Hubs. Sie erstellen zuerst Ihre Firewallrichtlinie und schützen dann Ihren Hub.

1. Wählen Sie in Firewall Manager die Option **View Azure Firewall policies** (Azure Firewall-Richtlinien anzeigen) aus.
2. Wählen Sie die Option **Azure-Firewallrichtlinie erstellen** aus.
1. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **fw-manager-rg** aus.
1. Geben Sie unter **Richtliniendetails** für **Name** den Namen **Policy-01** ein, und wählen Sie für **Region** die Option **USA, Osten** aus.
1. Wählen Sie **Weiter: DNS-Einstellungen** aus.
1. Wählen Sie **Weiter: TLS-Überprüfung (Vorschau)** aus.
1. Wählen Sie **Weiter: Regeln** aus.
1. Wählen Sie auf der Registerkarte **Regeln** die Option **Regelsammlung hinzufügen** aus.
1. Geben Sie auf der Seite **Regelsammlung hinzufügen** unter **Name** den Namen **App-RC-01** ein.
1. Wählen Sie unter **Regelsammlungstyp** die Option **Anwendung** aus.
1. Geben Sie unter **Priorität** den Wert **100** ein.
1. Vergewissern Sie sich, dass die **Regelsammlungsaktion** auf **Zulassen** festgelegt ist.
1. Geben Sie unter **Name** den Regelnamen **Allow-msft** ein.
1. Wählen Sie unter **Quellentyp** die Option **IP-Adresse** aus.
1. Geben Sie unter **Quelle** die Zeichenfolge **\*** ein.
1. Geben Sie unter **Protokoll** Folgendes ein: **http,https**.
1. Vergewissern Sie sich, dass **Zieltyp** auf **FQDN** festgelegt ist.
1. Geben Sie unter **Ziel** das Ziel **\*.microsoft.com** ein.
1. Wählen Sie **Hinzufügen**.

Fügen Sie eine DNAT-Regel hinzu, sodass Sie einen Remotedesktop mit dem virtuellen Computer **Srv-Workload-01** verbinden können.

1. Wählen Sie **Hinzufügen/Regelsammlung** aus.
1. Geben Sie für **Name** den Namen **dnat-rdp** ein.
1. Wählen Sie unter **Regelsammlungstyp** die Option **DNAT** aus.
1. Geben Sie unter **Priorität** den Wert **100** ein.
1. Geben Sie unter **Name** den Regelnamen **Allow-rdp** ein.
1. Wählen Sie unter **Quellentyp** die Option **IP-Adresse** aus.
1. Geben Sie unter **Quelle** die Zeichenfolge **\*** ein.
1. Wählen Sie für **Protokoll** die Option **TCP** aus.
1. Geben Sie unter **Zielports** den Wert **3389** ein.
1. Wählen Sie unter **Zieltyp** die Option **IP-Adresse** aus.
1. Geben Sie für **Ziel** die öffentliche IP-Adresse der Firewall ein, die Sie zuvor notiert haben.
1. Geben Sie für **Übersetzte Adresse** die private IP-Adresse für **Srv-Workload-01** ein, die Sie zuvor notiert haben.
1. Geben Sie für **Übersetzter Port** den Wert **3389** ein.
1. Wählen Sie **Hinzufügen**.

Fügen Sie eine Netzwerkregel hinzu, sodass Sie einen Remotedesktop von **Srv-Workload-01** mit **Srv-Workload-02** verbinden können.

1. Wählen Sie **Regelsammlung hinzufügen** aus.
2. Geben Sie unter **Name** den Namen **vnet-rdp** ein.
3. Wählen Sie unter **Regelsammlungstyp** die Option **Netzwerk** aus.
4. Geben Sie unter **Priorität** den Wert **100** ein.
1. Wählen Sie unter **Regelsammlungsaktion** die Option **Zulassen** aus.
1. Geben Sie unter **Name** den Regelnamen **Allow-vnet** ein.
1. Wählen Sie unter **Quellentyp** die Option **IP-Adresse** aus.
1. Geben Sie unter **Quelle** die Zeichenfolge **\*** ein.
1. Wählen Sie für **Protokoll** die Option **TCP** aus.
1. Geben Sie unter **Zielports** den Wert **3389** ein.
1. Wählen Sie unter **Zieltyp** die Option **IP-Adresse** aus.
1. Geben Sie für **Ziel** die private IP-Adresse von **Srv-Workload-02** ein, die Sie zuvor notiert haben.
1. Wählen Sie **Hinzufügen**.
1. Klicken Sie auf **Überprüfen und erstellen**.
1. Klicken Sie auf **Erstellen**.

## <a name="associate-policy"></a>Zuordnen einer Richtlinie

Ordnen Sie die Firewallrichtlinie dem Hub zu.

1. Wählen Sie in Firewall Manager die Option **Azure Firewall-Richtlinien** aus.
1. Aktivieren Sie das Kontrollkästchen für **Policy-01**.
1. Wählen Sie **Zuordnungen verwalten/Hubs zuordnen** aus.
1. Wählen Sie **hub-01** aus.
1. Wählen Sie **Hinzufügen**.

## <a name="route-traffic-to-your-hub"></a>Weiterleiten von Datenverkehr an Ihren Hub

Als nächstes müssen Sie sicherstellen, dass Netzwerkdatenverkehr durch Ihre Firewall geleitet wird.

1. Wählen Sie in Firewall Manager die Option **Virtuelle Hubs** aus.
2. Wählen Sie **Hub-01** aus.
3. Wählen Sie unter **Einstellungen** die Option **Sicherheitskonfiguration** aus.
4. Wählen Sie unter **Internetdatenverkehr** die Option **Azure Firewall** aus.
5. Wählen Sie unter **Private traffic** (Privater Datenverkehr) die Option **Send via Azure Firewall** (Über Azure Firewall senden) aus.
1. Wählen Sie **Speichern** aus.

   Die Aktualisierung der Routingtabellen dauert einige Minuten.
1. Vergewissern Sie sich, dass für die beiden Verbindungen angezeigt wird, dass Azure Firewall sowohl den Internetdatenverkehr als auch den privaten Datenverkehr schützt.

## <a name="test-the-firewall"></a>Testen der Firewall

Zum Testen der Firewallregeln verbinden Sie einen Remotedesktop über die öffentliche IP-Adresse der Firewall, die mit NAT in **Srv-Workload-01** aufgelöst wurde. Von dort aus verwenden Sie einen Browser, um die Anwendungsregel zu testen, und verbinden einen Remotedesktop mit **Srv-Workload-02**, um die Netzwerkregel zu testen.

### <a name="test-the-application-rule"></a>Testen der Anwendungsregel

Testen Sie nun die Firewallregeln, um sicherzustellen, dass sie wie erwartet funktionieren.

1. Verbinden Sie einen Remotedesktop mit der öffentlichen IP-Adresse der Firewall, und melden Sie sich an.

3. Navigieren Sie in Internet Explorer zu `https://www.microsoft.com`.
4. Klicken Sie in den Sicherheitswarnungen von Internet Explorer auf **OK** > **Schließen**.

   Daraufhin sollte die Microsoft-Homepage angezeigt werden.

5. Navigieren Sie zu `https://www.google.com`.

   Sie sollten durch die Firewall blockiert werden.

Somit haben Sie sich jetzt vergewissert, dass die Anwendungsregel der Firewall funktioniert:

* Sie können zum einzigen zulässigen FQDN navigieren, aber nicht zu anderen.

### <a name="test-the-network-rule"></a>Testen der Netzwerkregel

Testen Sie jetzt die Netzwerkregel.

- Öffnen Sie über Srv-Workload-01 einen Remotedesktop mit der privaten IP-Adresse von Srv-Workload-02.

   Ein Remotedesktop sollte eine Verbindung mit Srv-Workload-02 herstellen.

Somit haben Sie sich jetzt vergewissert, dass die Netzwerkregel der Firewall funktioniert:
* Sie können einen Remotedesktop mit einem Server verbinden, der sich in einem anderen virtuellen Netzwerk befindet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Firewallressourcen getestet haben, löschen Sie die Ressourcengruppe **fw-manager-rg**, um alle firewallbezogenen Ressourcen zu löschen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Was sind vertrauenswürdige Sicherheitspartner (Vorschau)?](trusted-security-partners.md)

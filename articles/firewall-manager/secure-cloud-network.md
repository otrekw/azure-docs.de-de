---
title: 'Tutorial: Schützen Ihres virtuellen WAN mithilfe von Azure Firewall Manager (Vorschauversion)'
description: In diesem Tutorial erfahren Sie, wie Sie Ihr virtuelles WAN mithilfe von Azure Firewall Manager unter Verwendung des Azure-Portals schützen.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 05/01/2020
ms.author: victorh
ms.openlocfilehash: b13f3b4eeb57c34f51152bb6d1914f6c80f31be1
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691033"
---
# <a name="tutorial-secure-your-virtual-wan-using-azure-firewall-manager-preview"></a>Tutorial: Schützen Ihres virtuellen WAN mithilfe von Azure Firewall Manager (Vorschauversion) 

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Mithilfe von Azure Firewall Manager (Vorschauversion) können Sie geschützte virtuelle Hubs erstellen, um für private IP-Adressen, für Azure-PaaS und für das Internet bestimmten Datenverkehr Ihres Cloudnetzwerks zu schützen. Das Datenverkehrsrouting an die Firewall ist automatisiert, sodass keine benutzerdefinierten Routen (User Defined Routes, UDRs) erstellt werden müssen.

![Schützen des Cloudnetzwerks](media/secure-cloud-network/secure-cloud-network.png)

Firewall Manager unterstützt außerdem die Architektur für virtuelle Hubnetzwerke. Einen Vergleich der Architekturtypen für geschützte virtuelle Hubs und virtuelle Hubnetzwerke finden Sie unter [Welche Architekturoptionen gibt es für Azure Firewall Manager?](vhubs-and-vnets.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen des virtuellen Spoke-Netzwerks
> * Erstellen eines geschützten virtuellen Hubs
> * Herstellen einer Verbindung für die Hub-and-Spoke-VNETs
> * Erstellen einer Firewallrichtlinie und Schützen Ihres Hubs
> * Weiterleiten von Datenverkehr an Ihren Hub
> * Testen der Firewall

## <a name="create-a-hub-and-spoke-architecture"></a>Erstellen einer Hub-Spoke-Architektur

Erstellen Sie zunächst ein virtuelles Spoke-Netzwerk für Ihre Server.

### <a name="create-a-spoke-virtual-network-and-subnets"></a>Erstellen eines virtuellen Spoke-Netzwerks und der Subnetze

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Wählen Sie unter **Netzwerk** die Option **Virtuelles Netzwerk** aus.
2. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, geben Sie **FW-Manager** als Name ein, und wählen Sie **OK** aus.
2. Geben Sie unter **Name** den Namen **Spoke-01** ein.
3. Wählen Sie unter **Region** die Option **(USA) USA, Osten** aus.
4. Klicken Sie auf **Weiter: IP-Adressen**.
1. Übernehmen Sie für **Adressraum** den Standardwert **10.0.0.0/16**.
3. Wählen Sie unter **Subnetzname** die Einstellung **Standard** aus.
4. Ändern Sie den Subnetznamen in **Workload-SN**.
5. Geben Sie unter **Subnetzadressbereich** den Bereich **10.0.1.0/24** ein.
6. Wählen Sie **Speichern** aus.

Erstellen Sie als nächstes ein Subnetz für einen Jumpserver.

1. Wählen Sie **Subnetz hinzufügen** aus.
4. Geben Sie unter **Subnetzname** den Namen **Jump-SN** ein.
5. Geben Sie unter **Subnetzadressbereich** den Bereich **10.0.2.0/24** ein.
6. Wählen Sie **Hinzufügen**.

Erstellen Sie jetzt das virtuelle Netzwerk.

1. Klicken Sie auf **Überprüfen + erstellen**.
2. Klicken Sie auf **Erstellen**.

### <a name="create-the-secured-virtual-hub"></a>Erstellen des geschützten virtuellen Hubs

Erstellen Sie mithilfe von Firewall Manager Ihren geschützten virtuellen Hub.

1. Wählen Sie auf der Startseite des Azure-Portals **Alle Dienste** aus.
2. Geben Sie **Firewall Manager** in das Suchfeld ein, und wählen Sie **Firewall Manager** aus.
3. Wählen Sie auf der Seite **Firewall Manager** die Option **View secured virtual hubs** (Geschützte virtuelle Hubs anzeigen) aus.
4. Wählen Sie auf der Seite **Firewall Manager | Geschützte virtuelle Hubs** die Option **Neuen geschützten virtuellen Hub erstellen** aus.
5. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **FW-Manager** aus.
7. Wählen Sie als **Region** die Option **USA, Osten** aus.
1. Geben Sie als **Name des geschützten virtuellen Hubs** den Namen **Hub-01** ein.
2. Geben Sie unter **Adressraum des Hubs** den Adressraum **10.1.0.0/16** ein.
3. Geben Sie als Name des neuen vWAN den Namen **Vwan-01** ein.
4. Lassen Sie das Kontrollkästchen **VPN Gateway zum Aktivieren vertrauenswürdiger Sicherheitspartner einbeziehen** deaktiviert.
5. Wählen Sie **Weiter: Azure Firewall** aus.
6. Übernehmen Sie die Standardeinstellung **Azure Firewall** **aktiviert**, und wählen Sie **Weiter: Vertrauenswürdiger Sicherheitspartner** aus.
7. Übernehmen Sie die Standardeinstellung **Vertrauenswürdiger Sicherheitspartner** **deaktiviert**, und wählen Sie **Weiter: Überprüfen + erstellen**.
8. Klicken Sie auf **Erstellen**. Die Bereitstellung dauert ca. 30 Minuten.

### <a name="connect-the-hub-and-spoke-vnets"></a>Herstellen einer Verbindung für die Hub-and-Spoke-VNETs

Nun können Sie das Peering für die Hub-and-Spoke-VNETs durchführen.

1. Wählen Sie die Ressourcengruppe **FW-Manager** und anschließend das virtuelle WAN **Vwan-01** aus.
2. Wählen Sie unter **Konnektivität** die Option **Virtuelle Netzwerkverbindungen** aus.
3. Wählen Sie **Verbindung hinzufügen** aus.
4. Geben Sie unter **Verbindungsname** den Namen **hub-spoke** ein.
5. Wählen Sie unter **Hubs** die Option **Hub-01** aus.
6. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **FW-Manager** aus.
7. Wählen Sie unter **Virtuelles Netzwerk** die Option **Spoke-01** aus.
8. Klicken Sie auf **OK**.

## <a name="create-a-firewall-policy-and-secure-your-hub"></a>Erstellen einer Firewallrichtlinie und Schützen Ihres Hubs

Eine Firewallrichtlinie definiert Regelsammlungen für die Weiterleitung von Datenverkehr an einen oder mehrere geschützte virtuelle Hubs. Sie erstellen zuerst Ihre Firewallrichtlinie und schützen dann Ihren Hub.

1. Wählen Sie in Firewall Manager die Option **View Azure Firewall policies** (Azure Firewall-Richtlinien anzeigen) aus.
2. Wählen Sie die Option **Azure-Firewallrichtlinie erstellen** aus.
3. Geben Sie unter **Richtliniendetails** für **Name** den Namen **Policy-01** ein, und wählen Sie für **Region** die Option **USA, Osten** aus.
4. Wählen Sie **Weiter: Regeln** aus.
5. Wählen Sie auf der Registerkarte **Regeln** die Option **Regelsammlung hinzufügen** aus.
6. Geben Sie auf der Seite **Regelsammlung hinzufügen** unter **Name** den Namen **RC-01** ein.
7. Wählen Sie unter **Regelsammlungstyp** die Option **Anwendung** aus.
8. Geben Sie unter **Priorität** den Wert **100** ein.
9. Vergewissern Sie sich, dass die **Regelsammlungsaktion** auf **Zulassen** festgelegt ist.
10. Geben Sie unter **Name** den Regelnamen **Allow-msft** ein.
11. Geben Sie unter **Quelladresse** die Adresse **\*** ein.
12. Geben Sie unter **Protokoll** Folgendes ein: **http,https**.
13. Vergewissern Sie sich, dass der Zieltyp **FQDN** lautet.
14. Geben Sie unter **Ziel** das Ziel **\*.microsoft.com** ein.
15. Wählen Sie **Hinzufügen**.
16. Klicken Sie auf **Weiter: Hubs**.
17. Wählen Sie auf der Registerkarte **Hubs** die Option **Virtuelle Hubs zuordnen** aus.
18. Wählen Sie **Hub-01** und dann **Hinzufügen** aus.
1. Klicken Sie auf **Überprüfen + erstellen**.
2. Klicken Sie auf **Erstellen**.

Dieser Vorgang dauert etwa fünf Minuten (oder länger).

## <a name="route-traffic-to-your-hub"></a>Weiterleiten von Datenverkehr an Ihren Hub

Als Nächstes müssen Sie sicherstellen, dass Netzwerkdatenverkehr durch Ihre Firewall geleitet wird.

1. Wählen Sie in Firewall Manager die Option **Geschützte virtuelle Hubs** aus.
2. Wählen Sie **Hub-01** aus.
3. Wählen Sie unter **Einstellungen** die Option **Weiterleitungseinstellungen** aus.
4. Wählen Sie unter **Internetdatenverkehr** > **Datenverkehr von virtuellen Netzwerken** die Option **Send via Azure Firewall** (Über Azure Firewall senden) aus.
5. Wählen Sie unter **Privater Azure-Datenverkehr** > **Datenverkehr von virtuellen Netzwerken** die Option **Send via Azure Firewall** (Über Azure Firewall senden) aus.
6. Wählen Sie **IP-Adresspräfix(e) bearbeiten** aus.
8. Geben Sie **10.0.1.0/24** als Adresse des Workloadsubnetzes ein, und wählen Sie **Speichern** aus.
9. Wählen Sie unter **Einstellungen** die Option **Verbindungen** aus.
10. Vergewissern Sie sich, dass für die **Hub-Spoke-Verbindung** unter **Internetdatenverkehr** die Option **Geschützt** angezeigt wird.


## <a name="test-your-firewall"></a>Testen Ihrer Firewall

Um Ihre Firewallregeln testen zu können, müssen Sie einige Server bereitstellen. In diesem Beispiel stellen Sie „Workload-Srv“ im Subnetz „Workload-SN“ bereit, um die Firewallregeln zu testen. Außerdem stellen Sie „Jump-Srv“ bereit, um über das Internet eine Remotedesktopverbindung mit „Workload-Srv“ herstellen zu können.

### <a name="deploy-the-servers"></a>Bereitstellen der Server

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Wählen Sie dann in der Liste **Beliebt** die Option **Windows Server 2016 Datacenter** aus.
3. Geben Sie die folgenden Werte für den virtuellen Computer ein:

   |Einstellung  |Wert  |
   |---------|---------|
   |Resource group     |**FW-Manager**|
   |Name des virtuellen Computers     |**Jump-Srv**|
   |Region     |**(USA) USA, Osten**|
   |Benutzername des Administrators     |Geben Sie einen Benutzernamen ein.|
   |Kennwort     |Geben Sie ein Kennwort ein.|

4. Wählen Sie unter **Regeln für eingehende Ports** für **Öffentliche Eingangsports** die Option **Ausgewählte Ports zulassen** aus.
5. Wählen Sie unter **Eingangsports auswählen** die Option **RDP (3389)** aus.
6. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie **Weiter: Datenträger**.
7. Übernehmen Sie die Standardeinstellungen für Datenträger, und wählen Sie **Weiter: Netzwerk** aus.
8. Vergewissern Sie sich, dass als virtuelles Netzwerk **Spoke-01** und als Subnetz **Jump-SN** ausgewählt ist.
9. Übernehmen Sie unter **Öffentliche IP** den Standardnamen der neuen öffentlichen IP-Adresse (Jump-Srv-ip).
11. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie **Weiter: Verwaltung** aus.
12. Wählen Sie **Aus** aus, um die Startdiagnose zu deaktivieren. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie dann auf **Bewerten + erstellen**.
13. Überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und wählen Sie dann **Erstellen** aus.

Konfigurieren Sie anhand der Angaben in der folgenden Tabelle einen weiteren virtuellen Computer mit dem Namen **Workload-Srv**. Die restliche Konfiguration ist mit der Konfiguration des virtuellen Computers „Srv-Jump“ identisch.

|Einstellung  |Wert  |
|---------|---------|
|Subnet|**Workload-SN**|
|Öffentliche IP-Adresse|**None**|
|Öffentliche Eingangsports|**None**|

### <a name="add-a-route-table-and-default-route"></a>Hinzufügen einer Routingtabelle und einer Standardroute

Um eine Internetverbindung mit „Jump-Srv“ zuzulassen, müssen Sie eine Routingtabelle und eine Standardgatewayroute aus dem Subnetz **Jump-SN** zum Internet erstellen.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Geben Sie **Routingtabelle** in das Suchfeld ein, und wählen Sie **Routingtabelle** aus.
3. Klicken Sie auf **Erstellen**.
4. Geben Sie unter **Name** den Namen **RT-01** ein.
5. Wählen Sie Ihr Abonnement, die Ressourcengruppe **FW-Manager** und die Region **(USA) USA, Osten** aus.
6. Klicken Sie auf **Erstellen**.
7. Wählen Sie nach Abschluss der Bereitstellung die Routingtabelle **RT-01** aus.
8. Wählen Sie **Routen** und dann **Hinzufügen** aus.
9. Geben Sie unter **Routenname** den Namen **jump-to-inet** ein.
10. Geben Sie unter **Adresspräfix** das Präfix **0.0.0.0/0** ein.
11. Wählen Sie unter **Typ des nächsten Hops** die Option **Internet** aus.
12. Klicken Sie auf **OK**.
13. Wählen Sie nach Abschluss der Bereitstellung **Subnetze** > **Zuordnen** aus.
14. Wählen Sie unter **Virtuelles Netzwerk** die Option **Spoke-01** aus.
15. Wählen Sie unter **Subnetz** die Option **Jump-SN** aus.
16. Klicken Sie auf **OK**.

### <a name="test-the-rules"></a>Testen der Regeln

Testen Sie nun die Firewallregeln, um sicherzustellen, dass sie wie erwartet funktionieren.

1. Überprüfen Sie im Azure-Portal die Netzwerkeinstellungen für den virtuellen Computer **Workload-Srv**, und notieren Sie sich die private IP-Adresse.
2. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer **Jump-Srv** her, und melden Sie sich an. Stellen Sie von dort aus eine Remotedesktopverbindung mit der privaten IP-Adresse von **Workload-Srv** her.

3. Navigieren Sie in Internet Explorer zu https://www.microsoft.com.
4. Klicken Sie in den Sicherheitswarnungen von Internet Explorer auf **OK** > **Schließen**.

   Daraufhin sollte die Microsoft-Homepage angezeigt werden.

5. Navigieren Sie zu https://www.google.com.

   Sie sollten durch die Firewall blockiert werden.

Damit haben Sie sich vergewissert, dass die Firewallregeln funktionieren:

* Sie können zum einzigen zulässigen FQDN navigieren, aber nicht zu anderen.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Was sind vertrauenswürdige Sicherheitspartner (Vorschau)?](trusted-security-partners.md)

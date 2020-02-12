---
title: 'Schnellstart: Erstellen einer VMware-VM in einer privaten AVS-Cloud'
description: Beschreibt das Erstellen einer VMware-VM in einer privaten AVS-Cloud
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: cbe88afc4f566bad4bacb408346d4dd25a2f6c96
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020062"
---
# <a name="create-vmware-virtual-machines-on-your-avs-private-cloud"></a>Erstellen von virtuellen VMware-Computern in Ihrer privaten AVS-Cloud

Um virtuelle Computer in Ihrer privaten AVS-Cloud zu erstellen, greifen Sie als Erstes über das Azure-Portal auf das AVS-Portal zu.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="access-the-avs-portal"></a>Zugreifen auf das AVS-Portal

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **AVS Services** (AVS-Dienste).
3. Wählen Sie den AVS-Dienst aus, über den Sie Ihre private AVS-Cloud erstellen möchten.
4. Klicken Sie auf der Seite **Übersicht** auf **Go to the AVS portal** (Zum AVS-Portal wechseln), um eine neue Browserregisterkarte für das AVS-Portal zu öffnen. Melden Sie sich nach Aufforderung mit Ihren Azure-Anmeldeinformationen an. 

    ![Starten des AVS-Portals](media/launch-cloudsimple-portal.png)

## <a name="launch-vcenter-web-ui"></a>Starten der vCenter-Webbenutzeroberfläche

Jetzt können Sie vCenter starten, um virtuelle Computer und Richtlinien einzurichten.

Um auf vCenter zuzugreifen, beginnen Sie im AVS-Portal. Klicken Sie auf der Startseite unter **Common Tasks** auf **Launch vSphere Client**. Wählen Sie die private AVS-Cloud aus, und klicken Sie dann auf **Launch vSphere Client** (vSphere-Client starten) für die private AVS-Cloud.

   ![VSphere-Client starten](media/launch-vcenter-from-cloudsimple-portal.png)

## <a name="upload-an-iso-or-vsphere-template"></a>Hochladen einer ISO- oder vSphere-Vorlage

  > [!WARNING]
  > Verwenden Sie für ISO-Uploads den vSphere HTML5-Client. Die Verwendung des Flash-Clients kann zu einem Fehler führen.

1. Rufen Sie die ISO- oder vSphere-Vorlage ab, die Sie auf vCenter hochladen möchten, um einen virtuellen Computer zu erstellen, damit er in Ihrem lokalen System zur Verfügung steht.
2. Klicken Sie in vCenter, auf das Symbol **Datenträger**, und wählen Sie **vsanDatastore** aus. Klicken Sie auf **Dateien** und dann auf **Neuer Ordner**.
    ![vCenter-ISO](media/vciso00.png)

3. Erstellen Sie einen Ordner mit dem Namen „ISOs and Templates“.

4. Navigieren Sie in „ISOs and Templates“ zum Ordner „ISOs“, und klicken Sie auf **Upload Files** (Dateien hochladen). Führen Sie die Anweisungen auf dem Bildschirm aus, um die ISO-Datei hochladen.

## <a name="create-a-virtual-machine-in-vcenter"></a>Erstellen eines virtuellen Computers in vCenter

1. Klicken Sie in vCenter auf das Symbol **Hosts und Cluster**.

2. Klicken Sie mit der rechten Maustaste auf **Workload**, und wählen Sie **Neuer virtueller Computer** aus.
    ![Neuer virtueller Computer](media/vcvm01.png)

3. Wählen Sie **Neuen virtuellen Computer erstellen** aus, und klicken Sie auf **Weiter**.
    ![Neuer virtueller Computer](media/vcvm02.png)

4. Benennen Sie den Computer, wählen Sie den Ordner **Workload-VMs** aus, und klicken Sie auf **Weiter**.
    ![Neuer virtueller Computer](media/vcvm03.png)

5. Wählen Sie die Computeressource **Workload** aus, und klicken Sie auf **Weiter**.
    ![Neuer virtueller Computer](media/vcvm04.png)

6. Wählen Sie **vsanDatastore** aus, und klicken Sie auf **Weiter**.
    ![Neuer virtueller Computer](media/vcvm05.png)

7. Behalten Sie die Standardauswahl für ESXi 6.5-Kompatibilität bei, und klicken Sie auf **Weiter**.
    ![Neuer virtueller Computer](media/vcvm06.png)

8. Wählen Sie das Gastbetriebssystem der ISO-Datei für den virtuellen Computer aus, den Sie erstellen, und klicken Sie auf **Weiter**.
    ![Neuer virtueller Computer](media/vcvm07.png)

9. Wählen Sie Optionen für Festplatte und Netzwerk aus. Wählen Sie für „Neues CD-/DVD-Laufwerk“ **Datastore-ISO-Datei** aus. Wenn Sie Datenverkehr aus der öffentlichen IP-Adresse an diesen virtuellen Computer zulassen möchten, wählen Sie das Netzwerk als **vm-1** aus.
    ![Neuer virtueller Computer](media/vcvm08.png)

10. Ein Auswahlfenster wird geöffnet. Wählen Sie die Datei aus, die Sie zuvor in den Ordner „ISO-Dateien und Vorlagen“ hochgeladen haben, und klicken Sie auf **OK**.
    ![Neuer virtueller Computer](media/vcvm10.png)

11. Überprüfen Sie die Einstellungen, und klicken Sie auf **OK**, um den virtuellen Computer zu erstellen.
    ![Neuer virtueller Computer](media/vcvm11.png)

Der virtuelle Computer wird jetzt den Workload-Computeressourcen hinzugefügt und ist einsatzbereit. 
![Neuer virtueller Computer](media/vcvm12.png)

Die grundlegende Einrichtung ist jetzt abgeschlossen. Sie können Ihre private AVS-Cloud ähnlich wie Ihre lokale Infrastruktur für virtuelle Computer verwenden.

Die folgenden Abschnitte enthalten optionale Informationen zum Einrichten von DNS- und DHCP-Servern für Workloads in der privaten AVS-Cloud und zum Ändern der Standardnetzwerkkonfiguration.

## <a name="add-users-and-identity-sources-to-vcenter-optional"></a>Hinzufügen von Benutzern und Identitätsquellen zu vCenter (optional)

AVS weist ein vCenter-Standardbenutzerkonto mit dem Benutzernamen `cloudowner@AVS.local` zu. Für Ihren Einstieg ist keine zusätzliche Kontoeinrichtung erforderlich. Normalerweise weist AVS Administratoren die Berechtigungen zu, die sie zum Ausführen normaler Vorgänge benötigen. Richten Sie Ihr lokales Active Directory oder Azure AD als [zusätzliche Identitätsquelle](set-vcenter-identity.md) in der privaten AVS-Cloud ein.

## <a name="create-a-dns-and-dhcp-server-optional"></a>Erstellen eines DNS- und DHCP-Servers (optional)

Anwendungen und Workloads, die in einer privaten AVS-Cloudumgebung ausgeführt werden, erfordern Namensauflösung und DHCP-Dienste für das Nachschlagen und die IP-Adresszuweisung. Zur Bereitstellung dieser Dienste ist eine ordnungsgemäße DHCP- und DNS-Infrastruktur erforderlich. Sie können einen virtuellen Computer in vCenter konfigurieren, um diese Dienste in Ihrer privaten AVS-Cloudumgebung bereitzustellen.

Voraussetzungen

* Eine verteilte Portgruppe mit konfiguriertem VLAN

* Routeneinrichtung zu lokalen oder internetbasierten DNS-Servern

* Vorlage für virtuelle Maschinen oder ISO zum Erstellen eines virtuellen Computers

Über die folgenden Links erhalten Sie Anleitungen zum Einrichten von DHCP- und DNS-Servern unter Linux und Windows.

#### <a name="linux-based-dns-server-setup"></a>Setup für Linux-basiertes DNS-Server

Linux bietet verschiedene Pakete für das Einrichten von DNS-Servern. Hier ist ein Link zu Anleitungen, wie Sie einen Open-Source-BIND DNS-Server einrichten.

[Beispiel-Setup](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-centos-7)

#### <a name="windows-based-setup"></a>Windows-basiertes Setup

In diesen Microsoft-Themen wird beschrieben, wie Sie einen Windows-Server als DNS-Server und als DHCP-Server einrichten.

[Windows-Server als DNS-Server](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

[Windows-Server als DHCP-Server](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)

## <a name="customize-networking-configuration-optional"></a>Anpassen der Netzwerkkonfiguration (optional)

Über die Netzwerkseiten im AVS-Portal können Sie die Konfiguration für Firewalltabellen und öffentliche IP-Adressen für virtuelle Computer angeben.

### <a name="allocate-public-ips"></a>Zuordnen von öffentlichen IP-Adressen

1. Navigieren Sie im AVS-Portal zu **Network > Public IP** (Netzwerk > Öffentliche IP-Adresse).
2. Klicken Sie auf **Öffentliche IP-Adresse zuordnen**.
3. Geben Sie einen Namen ein, um den IP-Adresseintrag zu identifizieren.
4. Behalten Sie den Standardort bei.
5. Verwenden Sie den Schieberegler, um die Leerlaufzeitüberschreitung bei Bedarf zu ändern.
6. Geben Sie die lokale IP-Adresse ein, für die Sie eine öffentliche IP-Adresse zuweisen möchten.
7. Geben Sie bei Bedarf einen zugeordneten DNS-Namen ein.
8. Klicken Sie auf **Fertig**.

    ![Öffentliche IP-Adresse](media/quick-create-pc-public-ip.png)

Jetzt beginnt die Aufgabe des Zuordnens der öffentlichen IP-Adresse. Sie können den Status der Aufgabe auf der Seite **Aktivität > Aufgaben** überprüfen. Nach Abschluss der Zuordnung wird der neue Eintrag auf der Seite „Öffentliche IP-Adressen“ angezeigt.

Der virtuelle Computer, dem diese IP-Adresse zugeordnet werden muss, muss mit der oben angegebenen lokalen Adresse konfiguriert werden. Die Vorgehensweise zum Konfigurieren einer IP-Adresse ist für das Betriebssystem des virtuellen Computers spezifisch. Lesen Sie die Dokumentation zum Betriebssystem Ihres virtuellen Computers, um sich über die richtige Vorgehensweise zu informieren.

#### <a name="example"></a>Beispiel

Hier sind beispielsweise die Details für Ubuntu 16.04.

Fügen Sie die statische Methode der Konfiguration für die inet-Adressfamilie in der Datei „/etc/network/interfaces“ hinzu. Ändern Sie die Werte für Adresse, Netzmaske und Gateway. In diesem Beispiel verwenden wir die Schnittstelle "eth0", die interne IP-Adresse „192.168.24.10“, die Gateway-Adresse „192.168.24.1“ und die Netzmaske „255.255.255.0“. Für Ihre Umgebung werden die verfügbaren Subnetzinformationen in der Willkommens-E-Mail bereitgestellt.

```
sudo vi /etc/network/interfaces
```

```
auto eth0
Iface eth0 inet static
iface eth0 inet static
address 192.168.24.10
netmask 255.255.255.0
gateway 192.168.24.1
dns-nameservers 8.8.8.8
dns-domain acme.com
dns-search acme.com
```
Deaktivieren Sie die Schnittstelle manuell.

```
sudo ifdown eth0
```
Aktivieren Sie die Schnittstelle manuell erneut.

```
sudo ifup eth0
```

Der gesamte eingehende Datenverkehr aus dem Internet wird standardmäßig **verweigert**. Wenn Sie einen anderen Port öffnen möchten, erstellen Sie eine [Firewalltabelle](firewall.md).

Nachdem Sie eine interne IP-Adresse als statische IP-Adresse konfiguriert haben, überprüfen Sie, ob Sie das Internet über den virtuellen Computer erreichen können.

```
ping 8.8.8.8
```
Überprüfen Sie außerdem, ob Sie den virtuellen Computer aus dem Internet über die öffentliche IP-Adresse erreichen können.

Stellen Sie sicher, dass alle Regeln (iptable) auf dem virtuellen Computer Port 80 für eingehenden Datenverkehr nicht blockieren.
        
```
netstat -an | grep 80
```

Starten Sie einen http-Server, der an Port 80 lauscht.
       
```
python2.7 -m SimpleHTTPServer 80
```

oder

```
python3 -m http.server 80
```
Starten Sie einen Browser auf Ihrem Desktop, und verweisen Sie ihn an Port 80 für die öffentliche IP-Adresse, um die Dateien auf Ihrem virtuellen Computer zu durchsuchen.

### <a name="default-avs-firewall-rules-for-public-ip"></a>AVS-Standardfirewallregeln für öffentliche IP-Adressen

* VPN-Datenverkehr: Der gesamte Datenverkehr zwischen (von/in) dem VPN und allen Workloadnetzwerken und dem Verwaltungsnetzwerk wird zugelassen.
* Interner Datenverkehr in der privaten AVS-Cloud: Der gesamte Ost-West-Datenverkehr zwischen (von/in) Workloadnetzwerken und dem Verwaltungsnetzwerk (siehe oben) wird zugelassen.
* Internet-Datenverkehr:
  * Der gesamte eingehende Datenverkehr aus dem Internet in Workloadnetzwerke und das Verwaltungsnetzwerk wird verweigert.
  * Der gesamte ausgehende Datenverkehr in das Internet aus Workloadnetzwerken oder dem Verwaltungsnetzwerk wird zugelassen.

Mithilfe des Features „Firewallregeln“ können Sie auch die Art und Weise ändern, wie Ihr Datenverkehr geschützt wird. Weitere Informationen finden Sie unter [Einrichten von Firewalltabellen und -regeln](firewall.md).

## <a name="install-solutions-optional"></a>Installieren von Lösungen (optional)

Sie können Lösungen in Ihrer privaten AVS-Cloud installieren, um Ihre vCenter-Umgebung für die private AVS-Cloud voll nutzen zu können. Sie können Sicherung, Notfallwiederherstellung, Replizierung und andere Funktionen einrichten, um Ihre virtuellen Computer zu schützen. Beispiele hierfür sind VMware Site Recovery Manager (VMware SRM) und Veeam Backup & Replication.

Um eine Lösung zu installieren, müssen Sie zusätzliche Berechtigungen für einen begrenzten Zeitraum anfordern. Lesen Sie [Eskalieren von Berechtigungen](escalate-private-cloud-privileges.md).

## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* [Connect from on-premises to CloudSimple using ExpressRoute](on-premises-connection.md)
* [Einrichten von VPN-Gateways im AVS-Netzwerk](vpn-gateway.md)

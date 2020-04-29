---
title: Verwenden von Azure Virtual WAN zum Erstellen einer Point-to-Site-Verbindung mit Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Azure Virtual WAN verwenden, um eine Point-to-Site-VPN-Verbindung mit Azure zu erstellen.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 04/16/2020
ms.author: alzam
ms.openlocfilehash: 11007bc39cb1112799c89afaf0ca670aa6760de6
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81482130"
---
# <a name="tutorial-create-a-user-vpn-connection-using-azure-virtual-wan"></a>Tutorial: Erstellen einer Benutzer-VPN-Verbindung per Azure Virtual WAN

In diesem Tutorial wird beschrieben, wie Sie Virtual WAN zum Verbinden Ihrer Ressourcen in Azure über eine IPsec/IKE (IKEv2)- oder OpenVPN-VPN-Verbindung nutzen. Für diese Art von Verbindung muss auf dem Clientcomputer ein Client konfiguriert sein. Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines WAN
> * Erstellen eines Hubs
> * Erstellen einer P2S-Konfiguration
> * Herunterladen eines VPN-Clientprofils
> * Anwenden einer P2S-Konfiguration auf einen Hub
> * Verbinden eines VNET mit einem Hub
> * Herunterladen und Anwenden der VPN-Clientkonfiguration
> * Anzeigen Ihrer Virtual WAN-Instanz

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Sie verfügen über ein virtuelles Netzwerk, mit dem Sie eine Verbindung herstellen möchten. Stellen Sie sicher, dass sich kein Subnetz Ihres lokalen Netzwerks mit den virtuellen Netzwerken für die Verbindungsherstellung überschneidet. Informationen zum Erstellen eines virtuellen Netzwerks im Azure-Portal finden Sie in der [Schnellstartanleitung](../virtual-network/quick-create-portal.md).

* Ihr virtuelles Netzwerk verfügt nicht über Gateways für virtuelle Netzwerke. Falls Ihr virtuelles Netzwerk über ein Gateway verfügt (entweder VPN oder ExpressRoute), müssen Sie alle Gateways entfernen. Für diese Konfiguration ist es erforderlich, dass virtuelle Netzwerke stattdessen mit dem Gateway des Virtual WAN-Hubs verbunden werden.

* Beschaffen Sie sich einen IP-Adressbereich für Ihre Hubregion. Der Hub ist ein virtuelles Netzwerk, das von Virtual WAN erstellt und verwendet wird. Der von Ihnen für den Hub angegebene Adressbereich darf sich nicht mit einem Ihrer vorhandenen virtuellen Netzwerke überlappen, mit denen Sie eine Verbindung herstellen. Außerdem ist keine Überlappung mit Ihren Adressbereichen möglich, mit denen Sie lokal eine Verbindung herstellen. Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, sollten Sie sich an eine Person wenden, die Ihnen diese Informationen zur Verfügung stellen kann.

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Erstellen eines Virtual WAN

Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.

1. Navigieren Sie zur Seite „Virtual WAN“. Klicken Sie im Portal auf **Ressource erstellen**. Geben Sie **Virtual WAN** in das Suchfeld ein, und drücken Sie die EINGABETASTE.
2. Wählen Sie in den Ergebnissen **Virtual WAN** aus. Klicken Sie auf der Seite „Virtual WAN“ auf **Erstellen**, um die Seite „WAN erstellen“ zu öffnen.
3. Füllen Sie auf der Seite **WAN erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

   ![Virtuelles WAN](./media/virtual-wan-point-to-site-portal/vwan.png)

   * **Abonnement**: Wählen Sie das Abonnement aus, das Sie verwenden möchten.
   * **Ressourcengruppe**: Erstellen Sie eine neue Ressourcengruppe, oder verwenden Sie eine vorhandene.
   * **Ressourcengruppenstandort**: Wählen Sie in der Dropdownliste einen Ressourcengruppenstandort aus. Ein WAN ist eine globale Ressource, die nicht in einer bestimmten Region angeordnet ist. Sie müssen aber eine Region auswählen, damit Sie die von Ihnen erstellte WAN-Ressource leichter verwalten und finden können.
   * **Name**: Geben Sie den Namen ein, den Sie Ihrem WAN geben möchten.
   * **Typ:** Standard. Bei der Erstellung eines WAN vom Typ „Basic“ können Sie auch nur einen Hub vom Typ „Basic“ erstellen. Für „Basic“-Hubs sind nur Site-to-Site-VPN-Verbindungen möglich.
4. Klicken Sie nach dem Ausfüllen der Felder auf **Überprüfen + erstellen**.
5. Klicken Sie nach bestandenen Überprüfung auf **Erstellen**, um das virtuelle WAN zu erstellen.

## <a name="create-an-empty-virtual-hub"></a><a name="hub"></a>Erstellen eines leeren virtuellen Hubs

1. Wählen Sie unter Ihrem virtuellen WAN „Hubs“ aus, und klicken Sie auf **+Neuer Hub**.

   ![Neue Site](media/virtual-wan-point-to-site-portal/hub1.jpg)
2. Füllen Sie auf der Seite zum Erstellen virtueller Hubs die folgenden Felder aus.

   **Region**: Wählen Sie die Region aus, in der Sie den virtuellen Hub bereitstellen möchten.

   **Name**: Geben Sie den Namen ein, den Sie Ihrem virtuellen Hub zuweisen möchten.

   **Privater Adressraum des Hubs**: Der Adressbereich des Hubs in CIDR-Notation.

   ![Neue Site](media/virtual-wan-point-to-site-portal/hub2.jpg)  
3. Klicken Sie auf **Überprüfen und erstellen**.
4. Klicken Sie auf der Seite **Überprüfung erfolgreich** auf **Erstellen**.

## <a name="create-a-p2s-configuration"></a><a name="p2sconfig"></a>Erstellen einer P2S-Konfiguration

Eine P2S-Konfiguration definiert die Parameter für das Herstellen der Verbindung mit Remoteclients.

1. Navigieren Sie zu **Alle Ressourcen**.
2. Klicken Sie auf das virtuelle WAN, das Sie erstellt haben.
3. Klicken Sie oben auf der Seite auf **+Benutzer-VPN-Konfiguration erstellen**, um die Seite **Neue Benutzer-VPN-Konfiguration erstellen** zu öffnen.

   ![Neue Site](media/virtual-wan-point-to-site-portal/p2s1.jpg)
4. Füllen Sie auf der Seite **Neue Benutzer-VPN-Konfiguration erstellen** die folgenden Felder aus:

   **Konfigurationsname**: Dies ist der Name, unter dem Sie auf Ihre Konfiguration verweisen möchten.

   **Tunneltyp**: Das Protokoll, das für den Tunnel verwendet wird.

   **Name des Stammzertifikats**: Ein beschreibender Name für das Zertifikat.

   **Daten des öffentlichen Zertifikats**: X.509-Zertifikatdaten mit Base64-Codierung.
  
5. Klicken Sie auf **Erstellen**, um die Konfiguration zu erstellen.

## <a name="edit-hub-assignment"></a><a name="edit"></a>Bearbeiten der Hub-Zuweisung

1. Navigieren Sie unter dem virtuellen WAN zum Blatt **Hubs**.
2. Wählen Sie den Hub aus, dem Sie die VPN-Serverkonfiguration zuordnen möchten, und klicken Sie auf **...** .

   ![Neue Site](media/virtual-wan-point-to-site-portal/p2s4.jpg)
3. Klicken Sie auf **Virtuellen Hub bearbeiten**.
4. Aktivieren Sie das Kontrollkästchen **Point-to-Site-Gateway einschließen**, und wählen Sie die gewünschte **Gatewayskalierungseinheit** aus.

   ![Neue Site](media/virtual-wan-point-to-site-portal/p2s2.jpg)

Die folgende Tabelle enthält Details zu den verfügbaren **Skalierungseinheiten**:

| **Skalierungseinheit** | **Durchsatz** | **P2S-Verbindungen** |
| --- | --- | --- |
| 1| 500 MBit/s | 500 |
| 2| 1 GBit/s | 500 |
| 3| 1,5 GBit/s | 500 |
| 4| 2 GBit/s | 1000 |
| 5| 2,5 GBit/s | 1000 |
| 6| 3 GBit/s | 1000 |
| 7| 3,5 GBit/s | 5\.000 |
| 8| 4 GBit/s | 5\.000 |
| 9| 4,5 GBit/s | 5\.000 |
| 10| 5 GBit/s | 5\.000 |
| 11| 5,5 GBit/s | 5\.000 |
| 12| 6 GBit/s | 5\.000 |
| 13| 6,5 GBit/s | 10000 |
| 14| 7 GBit/s | 10000 |
| 15| 7,5 GBit/s | 10000 |
| 16| 8 GBit/s | 10000 |
| 17| 8,5 GBit/s | 10000 |
| 18| 9 GBit/s | 10000 |
| 19| 9,5 GBit/s | 10000 |
| 20| 10 GBit/s | 10000 |

5. Geben Sie den **Adresspool** ein, aus dem den VPN-Clients IP-Adressen zugewiesen werden.
6. Klicken Sie auf **Bestätigen**.
7. Der Vorgang kann bis zu 30 Minuten dauern.

## <a name="download-vpn-profile"></a><a name="download"></a>Herunterladen des VPN-Profils

Verwenden Sie das VPN-Profil, um Ihre Clients zu konfigurieren.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Benutzer-VPN-Konfigurationen**.
2. Klicken Sie oben auf der Seite auf **Benutzer-VPN-Konfiguration herunterladen**.
3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.
4. Verwenden Sie die Profildatei, um die VPN-Clients zu konfigurieren.

### <a name="configure-user-vpn-clients"></a>Konfigurieren von Benutzer-VPN-Clients
Verwenden Sie das heruntergeladene Profil, um die Clients für den Remotezugriff zu konfigurieren. Das Verfahren ist für jedes Betriebssystem anders. Befolgen Sie jeweils die unten angegebene Anleitung:

#### <a name="microsoft-windows"></a>Microsoft Windows
##### <a name="openvpn"></a>OpenVPN

1. Laden Sie den OpenVPN-Client von der offiziellen Website herunter, und installieren Sie ihn.
2. Laden Sie das VPN-Profil für das Gateway herunter. Dies kann auf der Registerkarte „Benutzer-VPN-Konfigurationen“ im Azure-Portal oder über „New-AzureRmVpnClientConfiguration“ in PowerShell erfolgen.
3. Entzippen Sie das Profil. Öffnen Sie dann im Editor im Ordner „OpenVPN“ die Konfigurationsdatei „vpnconfig.ovpn“.
4. Füllen Sie den Abschnitt „P2S client certificate“ mit dem öffentlichen Schlüssel für das P2S-Clientzertifikat in Base64 aus. In einem Zertifikat im PEM-Format können Sie einfach die CER-Datei öffnen und den Base64-Schlüssel zwischen den Zertifikatskopfzeilen herauskopieren. Die Schritte sind unter [Generieren und Exportieren von Zertifikaten für VPN-Verbindungen von Virtual WAN-Benutzern](certificates-point-to-site.md) beschrieben.
5. Füllen Sie den Abschnitt „Private key“ mit dem privaten Schlüssel für das P2S-Clientzertifikat in Base64 aus. Weitere Informationen finden Sie unter [Extrahieren des privaten Schlüssels](howto-openvpn-clients.md#windows).
6. Ändern Sie keine anderen Felder. Verwenden Sie die ausgefüllte Konfiguration in der Clienteingabe, um eine Verbindung mit dem VPN herzustellen.
7. Kopieren Sie die Datei „vpnconfig.ovpn“ in den Ordner „C:\Programme\OpenVPN\config“.
8. Klicken Sie mit rechten Maustaste auf das Symbol „OpenVPN“, und wählen Sie „Verbinden“ aus.

##### <a name="ikev2"></a>IKEv2

1. Wählen Sie die VPN-Clientkonfigurationsdateien, die der Architektur des Windows-Computers entsprechen. Wählen Sie für eine 64-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupAmd64“ aus. Wählen Sie für eine 32-Bit-Prozessorarchitektur das Installer-Paket „VpnClientSetupX86“ aus.
2. Doppelklicken Sie auf das Paket, um es zu installieren. Sollte ein SmartScreen-Popup erscheinen, klicken Sie auf „Weitere Informationen“ und anschließend auf „Trotzdem ausführen“.
3. Navigieren Sie auf dem Clientcomputer zu „Netzwerkeinstellungen“, und klicken Sie auf „VPN“. Die VPN-Verbindung zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird.
4. Bevor Sie versuchen, eine Verbindung herzustellen, überprüfen Sie, ob Sie auf dem Clientcomputer ein Clientzertifikat installiert haben. Ein Clientzertifikat ist für die Authentifizierung erforderlich, wenn Sie den Typ „native Azure-Zertifikatauthentifizierung“ verwenden. Weitere Informationen zum Generieren von Zertifikaten finden Sie unter [Generieren von Zertifikaten](certificates-point-to-site.md). Weitere Informationen zum Installieren eines Clientzertifikats finden Sie unter [Installieren eines Clientzertifikats](../vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.
2. Auf der Seite „Übersicht“ steht jeder Punkt auf der Karte für einen Hub.
3. Im Abschnitt mit den Hubs und Verbindungen können Sie den Hubstatus, die Site, die Region, den VPN-Verbindungsstatus und die Anzahl von ein- und ausgehenden Byte anzeigen.


## <a name="clean-up-resources"></a><a name="cleanup"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

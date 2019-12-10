---
title: Konfigurieren der Azure AD-Authentifizierung für eine Point-to-Site-Verbindung mit Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie die Azure Active Directory-Authentifizierung für ein Benutzer-VPN konfigurieren.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 12/02/2019
ms.author: alzam
ms.openlocfilehash: 19aa029311584b5a9762691d24ed10c1666a032c
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781727"
---
# <a name="tutorial-create-a-user-vpn-connection-by-using-azure-virtual-wan"></a>Tutorial: Erstellen einer Benutzer-VPN-Verbindung per Azure Virtual WAN

In diesem Tutorial erfahren Sie, wie Sie die Azure AD-Authentifizierung für ein Benutzer-VPN in Virtual WAN konfigurieren, um eine OpenVPN-Verbindung mit Ihren Ressourcen in Azure herzustellen. Die Azure Active Directory-Authentifizierung ist nur für Gateways mit OpenVPN-Protokoll und Clients unter Windows verfügbar.

Für diese Art von Verbindung muss auf dem Clientcomputer ein Client konfiguriert sein. Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

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
> * Anzeigen der Ressourcenintegrität

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Sie verfügen über ein virtuelles Netzwerk, mit dem Sie eine Verbindung herstellen möchten. Stellen Sie sicher, dass sich kein Subnetz Ihres lokalen Netzwerks mit den virtuellen Netzwerken überlappt, mit denen Sie eine Verbindung herstellen möchten. Informationen zum Erstellen eines virtuellen Netzwerks im Azure-Portal finden Sie im [Schnellstart](../virtual-network/quick-create-portal.md).

* Ihr virtuelles Netzwerk verfügt nicht über Gateways für virtuelle Netzwerke. Falls Ihr virtuelles Netzwerk über ein Gateway verfügt (entweder VPN oder ExpressRoute), müssen Sie alle Gateways entfernen. Für diese Konfiguration ist es erforderlich, dass virtuelle Netzwerke stattdessen mit dem Gateway des Virtual WAN-Hubs verbunden werden.

* Beschaffen Sie sich einen IP-Adressbereich für Ihre Hubregion. Der Hub ist ein virtuelles Netzwerk, das von Virtual WAN erstellt und verwendet wird. Der von Ihnen für den Hub angegebene Adressbereich darf sich nicht mit einem Ihrer vorhandenen virtuellen Netzwerke überlappen, mit denen Sie eine Verbindung herstellen. Außerdem ist keine Überlappung mit Ihren Adressbereichen möglich, mit denen Sie lokal eine Verbindung herstellen. Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, sollten Sie sich an eine Person wenden, die Ihnen diese Informationen zur Verfügung stellen kann.

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="wan"></a>Erstellen eines Virtual WAN

Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.

1. Navigieren Sie zur Seite „Virtual WAN“. Klicken Sie im Portal auf **Ressource erstellen**. Geben Sie **Virtual WAN** in das Suchfeld ein, und drücken Sie die EINGABETASTE.
2. Wählen Sie in den Ergebnissen **Virtual WAN** aus. Klicken Sie auf der Seite „Virtual WAN“ auf **Erstellen**, um die Seite „WAN erstellen“ zu öffnen.
3. Füllen Sie auf der Seite **WAN erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

   ![Virtuelles WAN](./media/virtual-wan-point-to-site-azure-ad/vwan.png)

   * **Abonnement**: Wählen Sie das Abonnement aus, das Sie verwenden möchten.
   * **Ressourcengruppe**: Erstellen Sie eine neue Ressourcengruppe, oder verwenden Sie eine vorhandene.
   * **Ressourcengruppenstandort**: Wählen Sie in der Dropdownliste einen Ressourcengruppenstandort aus. Ein WAN ist eine globale Ressource, die nicht in einer bestimmten Region angeordnet ist. Sie müssen aber eine Region auswählen, damit Sie die von Ihnen erstellte WAN-Ressource leichter verwalten und finden können.
   * **Name**: Geben Sie den Namen ein, den Sie Ihrem WAN geben möchten.
   * **Typ:** Standard. Bei der Erstellung eines WAN vom Typ „Basic“ können Sie auch nur einen Hub vom Typ „Basic“ erstellen. Für „Basic“-Hubs sind nur Site-to-Site-VPN-Verbindungen möglich.
4. Klicken Sie nach dem Ausfüllen der Felder auf **Überprüfen + erstellen**.
5. Klicken Sie nach erfolgreicher Überprüfung auf **Erstellen**, um das virtuelle WAN zu erstellen.

## <a name="site"></a>Erstellen eines leeren virtuellen Hubs

1. Wählen Sie unter Ihrem virtuellen WAN „Hubs“ aus, und klicken Sie auf **+Neuer Hub**.

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/hub1.jpg)
2. Füllen Sie auf der Seite zum Erstellen virtueller Hubs die folgenden Felder aus.

   **Region**: Wählen Sie die Region aus, in der Sie den virtuellen Hub bereitstellen möchten.

   **Name**: Geben Sie den Namen ein, den Sie Ihrem virtuellen Hub zuweisen möchten.

   **Privater Adressraum des Hubs**: Der Adressbereich des Hubs in CIDR-Notation.

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/hub2.jpg)  
3. Klicken Sie auf **Überprüfen + erstellen**.
4. Klicken Sie auf der Seite **Überprüfung erfolgreich** auf **Erstellen**.

## <a name="site"></a>Erstellen einer neuen P2S-Konfiguration

Eine P2S-Konfiguration definiert die Parameter für das Herstellen der Verbindung mit Remoteclients.

1. Legen Sie die folgenden Variablen fest, und ersetzen Sie dabei die Werte nach Bedarf für Ihre Umgebung.

   ```powershell
   $aadAudience = "00000000-abcd-abcd-abcd-999999999999"
   $aadIssuer = "https://sts.windows.net/00000000-abcd-abcd-abcd-999999999999/"
   $aadTenant = "https://login.microsoftonline.com/00000000-abcd-abcd-abcd-999999999999"    
   ```

2. Führen Sie die folgenden Befehle aus, um die Konfiguration zu erstellen:

   ```powershell
   $aadConfig = New-AzVpnServerConfiguration -ResourceGroupName <ResourceGroup> -Name newAADConfig -VpnProtocol OpenVPN -VpnAuthenticationType AAD -AadTenant $aadTenant -AadIssuer $aadIssuer -AadAudience $aadAudience -Location westcentralus
   ```

## <a name="hub"></a>Bearbeiten der Hub-Zuweisung

1. Navigieren Sie unter dem virtuellen WAN zum Blatt **Hubs**.
2. Wählen Sie den Hub aus, dem Sie die VPN-Serverkonfiguration zuordnen möchten, und klicken Sie auf die Auslassungspunkte (...).

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. Klicken Sie auf **Virtuellen Hub bearbeiten**.
4. Aktivieren Sie das Kontrollkästchen **Point-to-Site-Gateway einschließen**, und wählen Sie die gewünschte **Gatewayskalierungseinheit** aus.

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. Geben Sie den **Adresspool** ein, aus dem den VPN-Clients IP-Adressen zugewiesen werden.
6. Klicken Sie auf **Confirm** (Bestätigen).
7. Der Vorgang kann bis zu 30 Minuten dauern.

## <a name="device"></a>Herunterladen des VPN-Profils

Verwenden Sie das VPN-Profil, um Ihre Clients zu konfigurieren.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Benutzer-VPN-Konfigurationen**.
2. Klicken Sie oben auf der Seite auf **Benutzer-VPN-Konfiguration herunterladen**.
3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.
4. Verwenden Sie die Profildatei, um die VPN-Clients zu konfigurieren.

## <a name="configure-user-vpn-clients"></a>Konfigurieren von Benutzer-VPN-Clients

Für die Verbindungsherstellung muss auf jedem Computer, von dem eine Verbindung mit dem VNET hergestellt werden soll, Azure VPN Client (Vorschauversion) heruntergeladen und das in den vorherigen Schritten heruntergeladene VPN-Clientprofil importiert werden.

> [!NOTE]
> Die Azure AD-Authentifizierung wird nur für Verbindungen mit dem OpenVPN®-Protokoll unterstützt.
>

#### <a name="to-download-the-azure-vpn-client"></a>So laden Sie Azure VPN Client herunter

Verwenden Sie [diesen Link](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab), um Azure VPN Client (Vorschauversion) herunterzuladen.

#### <a name="import"></a>So importieren Sie ein Clientprofil

1. Wählen Sie auf der Seite **Importieren**aus.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Navigieren Sie zur XML-Datei für das Profil und wählen Sie sie aus. Wählen Sie bei ausgewählter Datei **Öffnen** aus.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Geben Sie den Namen des Profils an und wählen Sie **Speichern** aus.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. Wählen Sie **Verbinden** aus, um die Verbindung zum VPN herzustellen.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Nachdem die Verbindung hergestellt wurde, wird das Symbol grün und **Verbunden** angezeigt.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="delete"></a>So löschen Sie ein Clientprofil

1. Wählen Sie neben dem Clientprofil, das Sie löschen möchten, die Auslassungspunkte (...) aus. Wählen Sie dann **Entfernen** aus.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Wählen Sie **Entfernen** aus, um den Löschvorgang durchzuführen.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose"></a>Diagnose von Verbindungsproblemen

1. Zum Diagnostizieren von Verbindungsproblemen können Sie das **Diagnose**-Tool verwenden. Wählen Sie neben der VPN-Verbindung, die Sie diagnostizieren möchten, die Auslassungspunkte (...) aus, um das Menü einzublenden. Wählen Sie dann **Diagnose** aus.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. Wählen Sie auf der Seite **Verbindungseigenschaften** die Option **Diagnose ausführen** aus.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Melden Sie sich mit Ihren Anmeldeinformationen an.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. Zeigen Sie die Diagnoseergebnisse an.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="viewwan"></a>Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.
2. Auf der Seite „Übersicht“ steht jeder Punkt auf der Karte für einen Hub. Bewegen Sie den Mauszeiger jeweils auf einen dieser Punkte, um die Zusammenfassung zur Integrität des Hubs anzuzeigen.
3. Im Abschnitt mit den Hubs und Verbindungen können Sie den Hubstatus, die Site, die Region, den VPN-Verbindungsstatus und die Anzahl von ein- und ausgehenden Byte anzeigen.

## <a name="viewhealth"></a>Anzeigen der Ressourcenintegrität

1. Navigieren Sie zu Ihrem WAN.
2. Klicken Sie auf der Seite für Ihr WAN im Abschnitt **Support + Problembehandlung** auf **Integrität**, und zeigen Sie Ihre Ressource an.


## <a name="cleanup"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

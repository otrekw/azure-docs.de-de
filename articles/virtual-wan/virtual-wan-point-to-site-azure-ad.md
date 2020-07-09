---
title: 'Konfigurieren der Azure AD-Authentifizierung für die Benutzer-VPN-Verbindung: Virtuelles WAN'
description: Erfahren Sie, wie Sie die Azure Active Directory-Authentifizierung für ein Benutzer-VPN konfigurieren.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/17/2020
ms.author: alzam
ms.openlocfilehash: dd80724d62c71fdec81965fb4aa6a07a6233a288
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753965"
---
# <a name="configure-azure-active-directory-authentication-for-user-vpn"></a>Konfigurieren der Azure Active Directory-Authentifizierung für ein Benutzer-VPN

In diesem Artikel erfahren Sie, wie Sie die Azure AD-Authentifizierung für ein Benutzer-VPN in Virtual WAN konfigurieren, um eine OpenVPN-Verbindung mit Ihren Ressourcen in Azure herzustellen. Die Azure Active Directory-Authentifizierung ist nur für Gateways mit OpenVPN-Protokoll und Clients unter Windows verfügbar.

Für diese Art von Verbindung muss auf dem Clientcomputer ein Client konfiguriert sein. Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

In diesem Artikel werden folgende Vorgehensweisen behandelt:

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

   ![Virtuelles WAN](./media/virtual-wan-point-to-site-azure-ad/vwan.png)

   * **Abonnement**: Wählen Sie das Abonnement aus, das Sie verwenden möchten.
   * **Ressourcengruppe**: Erstellen Sie eine neue Ressourcengruppe, oder verwenden Sie eine vorhandene.
   * **Ressourcengruppenstandort**: Wählen Sie in der Dropdownliste einen Ressourcengruppenstandort aus. Ein WAN ist eine globale Ressource, die nicht in einer bestimmten Region angeordnet ist. Sie müssen aber eine Region auswählen, damit Sie die von Ihnen erstellte WAN-Ressource leichter verwalten und finden können.
   * **Name**: Geben Sie den Namen ein, den Sie Ihrem WAN geben möchten.
   * **Typ:** Standard. Bei der Erstellung eines WAN vom Typ „Basic“ können Sie auch nur einen Hub vom Typ „Basic“ erstellen. Für „Basic“-Hubs sind nur Site-to-Site-VPN-Verbindungen möglich.
4. Klicken Sie nach dem Ausfüllen der Felder auf **Überprüfen + erstellen**.
5. Klicken Sie nach erfolgreicher Überprüfung auf **Erstellen**, um das virtuelle WAN zu erstellen.

## <a name="create-an-empty-virtual-hub"></a><a name="site"></a>Erstellen eines leeren virtuellen Hubs

1. Wählen Sie unter Ihrem virtuellen WAN „Hubs“ aus, und klicken Sie auf **+Neuer Hub**.

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/hub1.jpg)
2. Füllen Sie auf der Seite zum Erstellen virtueller Hubs die folgenden Felder aus.

   **Region**: Wählen Sie die Region aus, in der Sie den virtuellen Hub bereitstellen möchten.

   **Name**: Geben Sie den Namen ein, den Sie Ihrem virtuellen Hub zuweisen möchten.

   **Privater Adressraum des Hubs**: Der Adressbereich des Hubs in CIDR-Notation.

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/hub2.jpg)  
3. Klicken Sie auf **Überprüfen + erstellen**.
4. Klicken Sie auf der Seite **Überprüfung erfolgreich** auf **Erstellen**.

## <a name="create-a-new-p2s-configuration"></a><a name="site"></a>Erstellen einer neuen P2S-Konfiguration

Eine P2S-Konfiguration definiert die Parameter für das Herstellen der Verbindung mit Remoteclients.

1. Wählen Sie für Ihr virtuelles WAN die Option **Benutzer-VPN-Konfigurationen** aus.

   ![Neue Konfiguration](media/virtual-wan-point-to-site-azure-ad/aadportal1.jpg)

2. Klicken Sie auf **+ Benutzer-VPN-Konfiguration erstellen**.

   ![Neue Konfiguration](media/virtual-wan-point-to-site-azure-ad/aadportal2.jpg)

3. Geben Sie die entsprechenden Informationen ein, und klicken Sie auf **Erstellen**.

   ![Neue Konfiguration](media/virtual-wan-point-to-site-azure-ad/aadportal3.jpg)

## <a name="edit-hub-assignment"></a><a name="hub"></a>Bearbeiten der Hub-Zuweisung

1. Navigieren Sie unter dem virtuellen WAN zum Blatt **Hubs**.
2. Wählen Sie den Hub aus, dem Sie die VPN-Serverkonfiguration zuordnen möchten, und klicken Sie auf die Auslassungspunkte (...).

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. Klicken Sie auf **Virtuellen Hub bearbeiten**.
4. Aktivieren Sie das Kontrollkästchen **Point-to-Site-Gateway einschließen**, und wählen Sie die gewünschte **Gatewayskalierungseinheit** aus.

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. Geben Sie den **Adresspool** ein, aus dem den VPN-Clients IP-Adressen zugewiesen werden.
6. Klicken Sie auf **Confirm** (Bestätigen).
7. Der Vorgang kann bis zu 30 Minuten dauern.

## <a name="download-vpn-profile"></a><a name="device"></a>Herunterladen des VPN-Profils

Verwenden Sie das VPN-Profil, um Ihre Clients zu konfigurieren.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Benutzer-VPN-Konfigurationen**.
2. Klicken Sie oben auf der Seite auf **Benutzer-VPN-Konfiguration herunterladen**.
3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.
4. Verwenden Sie die Profildatei, um die VPN-Clients zu konfigurieren.

## <a name="configure-user-vpn-clients"></a>Konfigurieren von Benutzer-VPN-Clients

Für die Verbindungsherstellung muss auf jedem Computer, von dem eine Verbindung mit dem VNET hergestellt werden soll, Azure VPN Client heruntergeladen und das in den vorherigen Schritten heruntergeladene VPN-Clientprofil importiert werden.

> [!NOTE]
> Die Azure AD-Authentifizierung wird nur für Verbindungen mit dem OpenVPN&reg;-Protokoll unterstützt.
>

#### <a name="to-download-the-azure-vpn-client"></a>So laden Sie Azure VPN Client herunter

Verwenden Sie diesen [Link](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab), um Azure VPN Client herunterzuladen.

#### <a name="to-import-a-client-profile"></a><a name="import"></a>So importieren Sie ein Clientprofil

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

#### <a name="to-delete-a-client-profile"></a><a name="delete"></a>So löschen Sie ein Clientprofil

1. Wählen Sie neben dem Clientprofil, das Sie löschen möchten, die Auslassungspunkte (...) aus. Wählen Sie dann **Entfernen** aus.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Wählen Sie **Entfernen** aus, um den Löschvorgang durchzuführen.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose-connection-issues"></a><a name="diagnose"></a>Diagnose von Verbindungsproblemen

1. Zum Diagnostizieren von Verbindungsproblemen können Sie das **Diagnose**-Tool verwenden. Wählen Sie neben der VPN-Verbindung, die Sie diagnostizieren möchten, die Auslassungspunkte (...) aus, um das Menü einzublenden. Wählen Sie dann **Diagnose** aus.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. Wählen Sie auf der Seite **Verbindungseigenschaften** die Option **Diagnose ausführen** aus.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Melden Sie sich mit Ihren Anmeldeinformationen an.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. Zeigen Sie die Diagnoseergebnisse an.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.
2. Auf der Seite „Übersicht“ steht jeder Punkt auf der Karte für einen Hub.
3. Im Abschnitt mit den Hubs und Verbindungen können Sie den Hubstatus, die Site, die Region, den VPN-Verbindungsstatus und die Anzahl von ein- und ausgehenden Byte anzeigen.


## <a name="clean-up-resources"></a><a name="cleanup"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

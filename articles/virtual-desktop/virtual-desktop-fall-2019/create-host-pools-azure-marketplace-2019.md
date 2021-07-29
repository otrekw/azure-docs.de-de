---
title: 'Azure Virtual Desktop-Hostpool (klassisch) in Azure Marketplace: Azure'
description: Erfahren Sie, wie Sie einen Azure Virtual Desktop-Hostpool (klassisch) mit dem Azure Marketplace erstellen.
author: Heidilohr
ms.topic: how-to
ms.date: 03/31/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 6d9836872f96d6fda0006128a39277d3e9f85997
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111749901"
---
# <a name="tutorial-create-a-host-pool-in-azure-virtual-desktop-classic"></a>Tutorial: Erstellen eines Hostpools in Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../create-host-pools-azure-marketplace.md) weiter.

In diesem Tutorial erfahren Sie, wie Sie innerhalb eines Azure Virtual Desktop-Mandanten unter Verwendung eines Microsoft Azure Marketplace-Angebots einen Hostpool erstellen.

Hostpools sind eine Sammlung identischer virtueller Computer innerhalb von Azure Virtual Desktop-Mandantenumgebungen. Jeder Hostpool kann eine App-Gruppe enthalten, mit der Benutzer genau wie auf einem physischen Desktop interagieren können.

Dieses Tutorial umfasst die folgenden Aufgaben:

> [!div class="checklist"]
>
> * Erstellen eines Hostpools in Azure Virtual Desktop
> * Erstellen einer Ressourcengruppe mit VMs in Ihrem Azure-Abonnement.
> * Verknüpfen der VMs mit der Active Directory-Domäne.
> * Registrieren der VMs bei Azure Virtual Desktop

## <a name="prerequisites"></a>Voraussetzungen

* Ein Mandant in Virtual Desktop. In einem vorangehenden [Tutorial](tenant-setup-azure-active-directory.md) wird ein Mandant erstellt.
* [PowerShell-Modul für Azure Virtual Desktop](/powershell/windows-virtual-desktop/overview/)

Nachdem Sie das Modul heruntergeladen haben, führen Sie das folgende Cmdlet aus, um sich bei Ihrem Konto anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="run-the-azure-marketplace-offering-to-provision-a-new-host-pool"></a>Ausführen des Azure Marketplace-Angebots zum Bereitstellen eines neuen Hostpools

So führen Sie das Azure Marketplace-Angebot aus, um einen neuen Hostpool bereitzustellen:

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.
1. Geben Sie **Azure Virtual Desktop** in das Marketplace-Suchfenster ein.
1. Wählen Sie **Azure Virtual Desktop – Hostpool bereitstellen** und anschließend **Erstellen** aus.

Befolgen Sie anschließend die Anweisungen im nächsten Abschnitt, um die Informationen auf den entsprechenden Registerkarten einzugeben.

### <a name="basics"></a>Grundlagen

Gehen Sie auf der Registerkarte **Grundlagen** wie folgt vor:

1. Wählen Sie ein **Abonnement** aus.
1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die neue Ressourcengruppe an.
1. Wählen Sie eine **Region** aus.
1. Geben Sie einen Namen für den Hostpool ein, der innerhalb des Azure Virtual Desktop-Mandanten eindeutig sein muss.
1. Wählen Sie einen **Desktoptyp** aus. Wenn Sie **Persönlich** auswählen, wird jeder Benutzer, der eine Verbindung mit diesem Hostpool herstellt, dauerhaft einer VM zugewiesen.
1. Geben Sie Benutzer ein, die sich bei den Azure Virtual Desktop-Clients anmelden und auf einen Desktop zugreifen können. Verwenden Sie dazu eine durch Trennzeichen getrennte Liste. Geben Sie also beispielsweise *`user1@contoso.com,user2@contoso.com`* ein, um `user1@contoso.com` und `user2@contoso.com` Zugriff zu gewähren.
1. Wählen Sie als **Speicherort des Metadatendiensts** den Standort des virtuellen Netzwerks aus, das mit dem Active Directory-Server verbunden ist.

   >[!IMPORTANT]
   >Wenn Sie eine reine Azure Active Directory Domain Services (Azure AD DS)- und Azure Active Directory (Azure AD)-Lösung verwenden, stellen Sie Ihren Hostpool in derselben Region wie Azure AD DS bereit, um Fehler beim Domänenbeitritt und mit Anmeldeinformationen zu vermeiden.

1. Klicken Sie auf **Weiter: Configure virtual machines** (Virtuelle Computer konfigurieren).

### <a name="configure-virtual-machines"></a>Konfigurieren virtueller Computer

Gehen Sie auf der Registerkarte zum **Konfigurieren virtueller Computer** wie folgt vor:

1. Übernehmen Sie die Standardeinstellungen, oder passen Sie die Anzahl und Größe der VMs an.

    >[!NOTE]
    >Wird die gewünschte VM-Größe nicht in der VM-Größenauswahl angezeigt, wurde sie noch nicht ins Azure Marketplace-Tool aufgenommen.

2. Geben Sie ein Namenspräfix für die virtuellen Computer ein. Wenn Sie also beispielsweise *prefix* eingeben, heißen die VMs **prefix-0**, **prefix-1** usw.
3. Klicken Sie auf **Weiter: Virtual machine settings** (Weiter: VM-Einstellungen) aus.

### <a name="virtual-machine-settings"></a>Einstellungen des virtuellen Computers

Gehen Sie auf der Registerkarte mit den **Einstellungen des virtuellen Computers** wie folgt vor:

1. Wählen Sie die Quelle für die **Imagequelle** aus, und geben Sie entsprechende Informationen zum Speicherort und zur Art der Speicherung ein. Die Optionen für Blob-Speicher, das verwaltete Image und den Katalog unterscheiden sich.

   Falls Sie keine verwalteten Datenträger verwenden möchten, wählen Sie das Speicherkonto mit der *VHD*-Datei aus.
1. Geben Sie den Benutzerprinzipalnamen und das Kennwort ein. Bei diesem Konto muss es sich um das Domänenkonto handeln, mit dem die VMs der Active Directory-Domäne beitreten. Auf den virtuellen Computern wird ein lokales Konto mit dem gleichen Benutzernamen und Kennwort erstellt. Diese lokalen Konten können später zurückgesetzt werden.

   >[!NOTE]
   > Wenn Sie Ihre VMs einer Azure AD DS-Umgebung beitreten lassen, stellen Sie sicher, dass der Benutzer für den Domänenbeitritt Mitglied der [Gruppe „AAD DC-Administratoren“](../../active-directory-domain-services/tutorial-create-instance-advanced.md#configure-an-administrative-group) ist.
   >
   > Das Konto muss auch Mitglied der verwalteten Azure AD DS-Domäne oder des Azure AD-Mandanten sein. Konten aus externen Verzeichnissen, die Ihrem Azure AD-Mandanten zugeordnet sind, können während der Einbindung in die Domäne nicht richtig authentifiziert werden.

1. Wählen Sie das **virtuelle Netzwerk**, das mit dem Active Directory-Server verbunden ist, und anschließend ein Subnetz als Host für die VMs aus.
1. Wählen Sie **Weiter: Azure Virtual Desktop-Informationen** aus.

### <a name="azure-virtual-desktop-tenant-information"></a>Informationen zum Azure Virtual Desktop-Mandanten

Gehen Sie auf der Registerkarte mit den **Informationen zum Azure Virtual Desktop-Mandanten** wie folgt vor:

1. Geben Sie in **Name der Azure Virtual Desktop-Mandantengruppe** den Namen für die Mandantengruppe ein, die Ihren Mandanten enthält. Lassen Sie den Standardnamen, es sei denn, Ihnen wurde ein spezieller Mandantengruppenname bereitgestellt.
1. Geben Sie für **Name des Azure Virtual Desktop-Mandanten** den Namen des Mandanten ein, in dem Sie diesen Hostpool erstellen möchten.
1. Geben Sie die Art der Anmeldeinformationen an, die Sie für die Authentifizierung als RDS-Besitzer des Azure Virtual Desktop-Mandanten verwenden möchten. Geben Sie den Benutzerprinzipalnamen (User Principal Name, UPN) oder Dienstprinzipal und ein Kennwort ein.

   Wenn Sie das Tutorial [Erstellen von Dienstprinzipalen und Rollenzuweisungen mit PowerShell](create-service-principal-role-powershell.md) abgeschlossen haben, wählen Sie **Dienstprinzipal** aus.

1. Geben Sie unter **Dienstprinzipal** für die **Azure AD-Mandanten-ID** das Mandantenadministratorkonto für die Azure AD-Instanz ein, die den Dienstprinzipal enthält. Es werden nur Dienstprinzipale mit Kennwort unterstützt.
1. Klicken Sie auf **Weiter: Überprüfen + erstellen**.

## <a name="complete-setup-and-create-the-virtual-machine"></a>Abschließen der Einrichtung und Erstellen des virtuellen Computers

Überprüfen Sie unter **Überprüfen und erstellen** die Setupinformationen. Falls Sie etwas ändern müssen, können Sie zurückgehen und Änderungen vornehmen. Wenn alle Angaben korrekt sind, wählen Sie **Erstellen** aus, um Ihren Hostpool bereitzustellen.

Dieser Vorgang kann je nach Anzahl zu erstellender VMs 30 Minuten oder länger dauern.

>[!IMPORTANT]
> Zum Schutz Ihrer Azure Virtual Desktop-Umgebung in Azure empfiehlt es sich, den eingehenden Port 3389 auf Ihren VMs nicht zu öffnen. Für Azure Virtual Desktop muss der eingehende Port 3389 nicht geöffnet sein, damit Benutzer auf die VMs des Hostpools zugreifen können.
>
> Wenn Sie den Port 3389 zur Problembehandlung öffnen müssen, verwenden Sie am besten den Just-In-Time-Zugriff. Weitere Informationen finden Sie unter [Sichern Ihrer Verwaltungsports mit Just-in-Time-Zugriff (JIT)](../../security-center/security-center-just-in-time.md).

## <a name="optional-assign-additional-users-to-the-desktop-application-group"></a>Optional: Zuweisen zusätzlicher Benutzer zur Desktopanwendungsgruppe

Nachdem Azure Marketplace die Erstellung des Pools abgeschlossen hat, können Sie der Desktopanwendungsgruppe weitere Benutzer zuweisen. Überspringen Sie diesen Abschnitt, falls Sie keine weiteren Benutzer hinzufügen möchten.

So weisen Sie der Desktopanwendungsgruppe Benutzer zu:

1. Öffnen Sie ein PowerShell-Fenster.

1. Führen Sie den folgenden Befehl aus, um sich bei der Azure Virtual Desktop-Umgebung anzumelden:

   ```powershell
   Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
   ```

1. Fügen Sie der Desktopanwendungsgruppe mit diesem Befehl Benutzer hinzu:

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
   ```

   Der UPN des Benutzers muss der Identität des Benutzers in Azure AD entsprechen, beispielweise *user1@contoso.com* . Wenn Sie mehrere Benutzer hinzufügen möchten, müssen Sie den Befehl für jeden Benutzer separat ausführen.

Benutzer, die Sie der Desktopanwendungsgruppe hinzufügen, können sich mit unterstützten Remotedesktopclients bei Azure Virtual Desktop anmelden und eine Ressource für einen Sitzungsdesktop anzeigen.

Derzeit werden folgende Clients unterstützt:

* [Remotedesktopclient für Windows 7 und Windows 10](connect-windows-7-10-2019.md)
* [Azure Virtual Desktop-Webclient](connect-web-2019.md)

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun einen Hostpool erstellt und Benutzer zugewiesen, die Zugriff auf den zugehörigen Desktop haben. Als Nächstes können Sie Ihren Hostpool mit RemoteApp-Programmen füllen. Weitere Informationen zum Verwalten von Apps in Azure Virtual Desktop finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Manage app groups for Windows Virtual Desktop Preview](manage-app-groups-2019.md) (Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion))

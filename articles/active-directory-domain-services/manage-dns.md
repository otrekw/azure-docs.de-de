---
title: Verwalten von DNS für Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die DNS-Servertools zur Verwaltung von DNS und zum Erstellen bedingter Weiterleitungen für eine von Azure Active Directory Domain Services verwaltete Domäne installieren.
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 04/16/2020
ms.author: iainfou
ms.openlocfilehash: f4bd3f75c3246cb11e88dbaae817eba8ac76b394
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2020
ms.locfileid: "81603472"
---
# <a name="administer-dns-and-create-conditional-forwarders-in-an-azure-ad-domain-services-managed-domain"></a>Verwalten von DNS und Erstellen bedingter Weiterleitungen in einer von Azure AD Domain Services verwalteten Domäne

In Azure Active Directory Domain Services (Azure AD DS) ist DNS eine Schlüsselkomponente. Azure AD DS umfasst einen DNS-Server, der die Namensauflösung für die verwaltete Domäne bereitstellt. Dieser DNS-Server enthält integrierte DNS-Einträge und Updates für die wichtigsten Komponenten, die die Ausführung des Dienstes ermöglichen.

Wenn Sie Ihre eigenen Anwendungen und Dienste ausführen, müssen Sie unter Umständen DNS-Einträge für nicht in die Domäne eingebundene Computer erstellen, virtuelle IP-Adressen für Load Balancer konfigurieren oder externe DNS-Weiterleitungen einrichten. Benutzern, die zur Gruppe *AAD DC-Administratoren* gehören, werden DNS-Administratorrechte in der von Azure AD DS verwalteten Domäne gewährt und sie können benutzerdefinierte DNS-Einträge erstellen und bearbeiten.

In einer Hybridumgebung werden in anderen DNS-Namespaces, z. B. einer lokalen AD DS-Umgebung, konfigurierte DNS-Zonen und DNS-Einträge nicht mit Azure AD DS synchronisiert. Um benannte Ressourcen in anderen DNS-Namespaces aufzulösen, erstellen und verwenden Sie bedingte Weiterleitungen, die auf vorhandene DNS-Server in Ihrer Umgebung verweisen.

In diesem Artikel erfahren Sie, wie Sie die DNS-Servertools installieren und dann die DNS-Konsole zur Verwaltung von Einträgen und zum Erstellen bedingter Weiterleitungen in Azure AD DS verwenden.

## <a name="before-you-begin"></a>Voraussetzungen

Für diesen Artikel benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die in Ihrem Azure AD-Mandanten aktiviert und konfiguriert ist.
    * Führen Sie bei Bedarf das Tutorial zum [Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz][create-azure-ad-ds-instance] aus.
* Konnektivität zwischen Ihrem virtuellen Azure AD DS-Netzwerk und dem Ort, an dem die anderen DNS-Namespaces gehostet werden.
    * Diese Konnektivität kann über eine [Azure ExpressRoute][expressroute]- oder [Azure VPN Gateway][vpn-gateway]-Verbindung bereitgestellt werden.
* Eine Windows Server-Verwaltungs-VM, die in die verwaltete Azure AD DS-Domäne eingebunden ist.
    * Führen Sie bei Bedarf die [Schritte im Tutorial zum Erstellen einer Windows Server-VM und Einbinden der VM in eine verwaltete Domäne][create-join-windows-vm] aus.
* Ein Benutzerkonto, das Mitglied der *Administratorengruppe für Azure AD-Domänencontroller* (AAD-DC-Administratoren) in Ihrem Azure AD-Mandanten ist.

## <a name="install-dns-server-tools"></a>Installieren von DNS-Servertools

Zum Erstellen und Ändern von DNS-Einträgen in Azure AD DS müssen Sie die DNS-Servertools installieren. Diese Tools können als Feature in Windows Server installiert werden. Weitere Informationen zum Installieren der Verwaltungstools auf einem Windows-Client finden Sie unter [Installieren der Remoteserver-Verwaltungstools (RSAT)][install-rsat].

1. Melden Sie sich bei Ihrer Verwaltungs-VM an. Weitere Informationen zu den Schritten zum Herstellen einer Verbindung mithilfe des Azure-Portals finden Sie unter [Herstellen einer Verbindung mit einer Windows Server-VM][connect-windows-server-vm].
1. Wenn **Server-Manager** bei der Anmeldung beim virtuellen Computer nicht standardmäßig geöffnet wird, wählen Sie das **Startmenü** und dann **Server-Manager** aus.
1. Wählen Sie im Bereich *Dashboard* des Fensters **Server-Manager** die Option **Rollen und Features hinzufügen** aus.
1. Klicken Sie auf der Seite **Vorbereitung** des *Assistenten zum Hinzufügen von Rollen und Features* auf **Weiter**.
1. Lassen Sie für *Installationstyp* die Option **Rollenbasierte oder featurebasierte Installation** aktiviert, und wählen Sie **Weiter** aus.
1. Wählen Sie auf der Seite **Serverauswahl** den aktuellen virtuellen Computer aus dem Serverpool aus (z. B. *myvm.aaddscontoso.com*), und wählen Sie dann **Weiter** aus.
1. Klicken Sie auf der Seite **Serverrollen** auf **Weiter**.
1. Erweitern Sie auf der Seite **Features** den Knoten **Remote Server-Verwaltungstools** und anschließend den Knoten **Rollenverwaltungstools**. Wählen Sie das Feature **DNS-Servertools** in der Liste der Rollenverwaltungstools aus.

    ![Wählen Sie aus der Liste der verfügbaren Tools zur Rollenverwaltung die Installation der DNS-Servertools aus.](./media/manage-dns/install-dns-tools.png)

1. Wählen Sie auf der Seite **Bestätigung** die Option **Installieren** aus. Die Installation der Tools für die Gruppenrichtlinienverwaltung kann ein bis zwei Minuten dauern.
1. Wenn die Installation des Features abgeschlossen ist, wählen Sie **Schließen** aus, um den **Assistenten zum Hinzufügen von Rollen und Features** zu beenden.

## <a name="open-the-dns-management-console-to-administer-dns"></a>Öffnen der DNS-Verwaltungskonsole zum Verwalten von DNS

Wenn die DNS-Servertools installiert sind, können Sie DNS-Einträge auf der von Azure AD DS verwalteten Domäne verwalten.

> [!NOTE]
> Zum Verwalten von DNS in einer verwalteten Azure AD DS-Domäne müssen Sie bei einem Benutzerkonto angemeldet sein, das Mitglied der Gruppe *AAD DC-Administrators* ist.

1. Klicken Sie auf dem Startbildschirm auf **Verwaltung**. Es wird eine Liste der verfügbaren Verwaltungstools angezeigt, einschließlich **DNS**, das im vorherigen Abschnitt installiert wurde. Wählen Sie **DNS** aus, um die DNS-Verwaltungskonsole zu starten.
1. Wählen Sie im Dialogfeld **Verbindung mit DNS-Server herstellen** die Option **Folgender Computer** aus, und geben Sie dann den DNS-Domänennamen der verwalteten Domäne (z. B. *aaddscontoso.com*) ein:

    ![Verbinden mit der von Azure AD DS verwalteten Domäne in der DNS-Konsole](./media/manage-dns/connect-dns-server.png)

1. Die DNS-Konsole stellt eine Verbindung mit der von Azure AD DS verwalteten Domäne her. Erweitern Sie die **Forward-Lookup-Zonen** oder **Reverse-Lookup-Zonen**, um Ihre gewünschten DNS-Einträge zu erstellen oder bestehende Einträge nach Bedarf zu bearbeiten.

    ![DNS-Konsole – Domäne verwalten](./media/manage-dns/dns-manager.png)

> [!WARNING]
> Wenn Sie Einträge mit den DNS-Servertools verwalten, stellen Sie sicher, dass Sie die integrierten DNS-Einträge, die von Azure AD DS verwendet werden, nicht löschen oder ändern. Zu den integrierten DNS-Einträgen zählen Domänen-DNS-Einträge, Namenservereinträge sowie weitere für den DC-Standort verwendete Einträge. Wenn Sie diese Einträge ändern, werden die Domänendienste im virtuellen Netzwerk unterbrochen.

## <a name="create-conditional-forwarders"></a>Erstellen bedingter Weiterleitungen

Eine Azure AD DS-DNS-Zone sollte nur die Zone und die Einträge für die verwaltete Domäne selbst enthalten. Erstellen Sie keine zusätzlichen Zonen in Azure AD DS, um benannte Ressourcen in anderen DNS-Namespaces aufzulösen. Verwenden Sie stattdessen bedingte Weiterleitungen in der von Azure AD DS verwalteten Domäne, um dem DNS-Server mitzuteilen, wo er Adressen für diese Ressourcen auflösen kann.

Eine bedingte Weiterleitung ist eine Konfigurationsoption auf einem DNS-Server, mit der Sie eine DNS-Domäne wie *contoso.com* zum Weiterleiten von Abfragen definieren können. Es wird also nicht versucht, mithilfe des lokalen DNS-Servers Abfragen für Einträge in dieser Domäne aufzulösen, sondern DNS-Abfragen werden an das konfigurierte DNS für diese Domäne weitergeleitet. Mit dieser Konfiguration wird sichergestellt, dass die richtigen DNS-Einträge zurückgegeben werden, da Sie keine lokale DNS-Zone mit doppelten Einträgen in der von Azure AD DS verwalteten Domäne erstellen, um diese Ressourcen widerzuspiegeln.

Führen Sie die folgenden Schritte aus, um eine bedingte Weiterleitung in Ihrer von Azure AD DS verwalteten Domäne zu erstellen:

1. Wählen Sie Ihre Azure AD DS-DNS-Zone (z. B. *aaddscontoso.com*) aus.
1. Wählen Sie **Bedingte Weiterleitungen** aus, klicken Sie mit der rechten Maustaste, und wählen Sie dann **Neue bedingte Weiterleitung** aus.
1. Geben Sie Ihre andere **DNS-Domäne** (z. B. *contoso.com*) und dann die IP-Adressen der DNS-Server für diesen Namespace wie im folgenden Beispiel gezeigt ein:

    ![Hinzufügen und Konfigurieren einer bedingten Weiterleitung für den DNS-Server](./media/manage-dns/create-conditional-forwarder.png)

1. Aktivieren Sie das Kontrollkästchen **Diese bedingte Weiterleitung in Active Directory speichern und wie folgt replizieren**, und wählen Sie dann die Option *Alle DNS-Server in dieser Domänen* aus, wie im folgenden Beispiel gezeigt:

    ![DNS-Konsole – Domäne verwalten](./media/manage-dns/store-in-domain.png)

    > [!IMPORTANT]
    > Wenn die bedingte Weiterleitung in der *Gesamtstruktur* anstelle der *Domäne* gespeichert wird, führt sie zu einem Fehler.

1. Wählen Sie zum Erstellen der bedingten Weiterleitung **OK** aus.

Die Namensauflösung der Ressourcen in anderen Namespaces von virtuellen Computern, die mit der von Azure AD DS verwalteten Domäne verbunden sind, sollte jetzt ordnungsgemäß erfolgen. Abfragen für die in der bedingten Weiterleitung konfigurierte DNS-Domäne werden an die entsprechenden DNS-Server übergeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur DNS-Verwaltung finden Sie im [Technet-Artikel zu DNS-Tools](https://technet.microsoft.com/library/cc753579.aspx).

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[expressroute]: ../expressroute/expressroute-introduction.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[create-join-windows-vm]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm

<!-- EXTERNAL LINKS -->
[install-rsat]: /windows-server/remote/remote-server-administration-tools#BKMK_Thresh

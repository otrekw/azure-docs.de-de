---
title: 'Tutorial: Erstellen einer Gesamtstruktur-Vertrauensstellung in Azure AD Domain Services | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie für Azure AD Domain Services im Azure-Portal eine unidirektionale ausgehende Gesamtstruktur-Vertrauensstellung zu einer lokalen AD DS-Domäne erstellen.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: iainfou
ms.openlocfilehash: 5620d1cdc7dc71bdac17057b9a13a74150b12d5c
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77612519"
---
# <a name="tutorial-create-an-outbound-forest-trust-to-an-on-premises-domain-in-azure-active-directory-domain-services-preview"></a>Tutorial: Erstellen einer ausgehenden Gesamtstruktur-Vertrauensstellung zu einer lokalen Domäne in Azure Active Directory Domain Services (Vorschauversion)

In Umgebungen, in denen Sie keine Kennworthashes synchronisieren können, oder wenn Sie Benutzer haben, die sich ausschließlich mit Smartcards anmelden, sodass sie ihre Kennwörter nicht wissen, können Sie eine Ressourcengesamtstruktur in Azure Active Directory Domain Services (AD DS) verwenden. Eine Ressourcengesamtstruktur verwendet eine unidirektionale ausgehende Vertrauensstellung von Azure AD DS zu mindestens einer lokalen AD DS-Umgebung. Diese Vertrauensstellung ermöglicht es Benutzern, Anwendungen und Computern, sich aus der verwalteten Azure AD DS-Domäne bei einer lokalen Domäne zu authentifizieren. Azure AD DS-Ressourcengesamtstrukturen befinden sich derzeit in der Vorschauphase.

![Darstellung einer Gesamtstruktur-Vertrauensstellung von Azure AD DS zu lokalem AD DS](./media/concepts-resource-forest/resource-forest-trust-relationship.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren von DNS in einer lokalen AD DS-Umgebung, um Azure AD DS-Konnektivität zu unterstützen
> * Erstellen einer unidirektionalen eingehenden Gesamtstruktur-Vertrauensstellung in einer lokalen AD DS-Umgebung
> * Erstellen einer unidirektionalen ausgehenden Gesamtstruktur-Vertrauensstellung in Azure AD DS
> * Testen und Überprüfen der Vertrauensstellung hinsichtlich Authentifizierung und Ressourcenzugriff

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die mit einer Ressourcengesamtstruktur erstellt wurde und in Ihrem Azure AD-Mandanten konfiguriert ist.
    * Bei Bedarf [erstellen und konfigurieren Sie eine Azure Active Directory Domain Services-Instanz][create-azure-ad-ds-instance-advanced].
    
    > [!IMPORTANT]
    > Erstellen Sie unbedingt eine verwaltete Azure AD DS-Domäne mithilfe einer *Ressourcengesamtstruktur*. Mit der Standardoption wird eine *Benutzergesamtstruktur* erstellt. Nur Ressourcengesamtstrukturen können Vertrauensstellungen für lokale AD DS-Umgebungen erstellen. Sie müssen außerdem mindestens ein *Enterprise*-SKU für Ihre verwaltete Domäne verwenden. [Ändern Sie die SKU in eine verwaltete Azure AD DS-Domäne][howto-change-sku], wenn Bedarf besteht.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

In diesem Tutorial erstellen und konfigurieren Sie die ausgehende Gesamtstruktur-Vertrauensstellung von Azure AD DS im Azure-Portal. Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com) an.

## <a name="networking-considerations"></a>Überlegungen zum Netzwerkbetrieb

Das virtuelle Netzwerk, in dem die Azure AD DS-Ressourcengesamtstruktur gehostet wird, benötigt Netzwerkkonnektivität mit Ihrer lokalen Active Directory-Instanz. Außerdem benötigen Anwendungen und Dienste Netzwerkkonnektivität mit dem virtuellen Netzwerk, in dem die Azure AD DS-Ressourcengesamtstruktur gehostet wird. Die Netzwerkkonnektivität mit der Azure AD DS-Ressourcengesamtstruktur muss immer aktiv und stabil sein, denn andernfalls kann es passieren, dass Benutzer sich nicht authentifizieren oder nicht auf Ressourcen zugreifen können.

Bevor Sie eine Gesamtstruktur-Vertrauensstellung in Azure AD DS konfigurieren, sollten Sie sich vergewissern, dass die Netzwerkkonnektivität zwischen Azure und der lokalen Umgebung die folgenden Anforderungen erfüllt:

* Es werden private IP-Adressen verwendet. Sie sollten sich nicht auf DHCP mit dynamischer Zuweisung von IP-Adressen verlassen.
* Überlappende IP-Adressräume sollten vermieden werden, damit Peering und Routing von virtuellen Netzwerken ermöglicht wird, um eine erfolgreiche Kommunikation zwischen Azure und lokalen Ressourcen zu erreichen.
* Ein virtuelles Azure-Netzwerk erfordert ein Gatewaysubnetz, um eine Site-to-Site-VPN- oder ExpressRoute-Verbindung zu konfigurieren.
* Es wurden Subnetze mit genügend IP-Adressen erstellt, um Ihr Szenario zu unterstützen.
* Vergewissern Sie sich, dass Azure AD DS ein eigenes Subnetz hat. Dieses Subnetz des virtuellen Netzwerks sollte nicht gemeinsam mit Anwendungs-VMs und Diensten verwendet werden.
* Über Peering verbundene virtuelle Netzwerke sind nicht transitiv.
    * Peerings für virtuelle Azure-Netzwerke müssen zwischen allen virtuellen Netzwerken erstellt werden, die Sie zwischen der Gesamtstruktur-Vertrauensstellung der Azure AD DS-Ressourcen und der lokalen AD DS-Umgebung verwenden möchten.
* Es muss durchgängige Netzwerkkonnektivität mit Ihrer lokalen Active Directory-Gesamtstruktur bereitgestellt werden. Es dürfen keine bedarfsgesteuerten Verbindungen verwendet werden.
* Stellen Sie sicher, dass kontinuierliche Namensauflösung (DNS) zwischen dem Namen Ihrer Azure AD DS-Ressourcengesamtstruktur und dem Namen Ihrer lokalen Active Directory-Gesamtstruktur vorhanden ist.

## <a name="configure-dns-in-the-on-premises-domain"></a>Konfigurieren von DNS in der lokalen Domäne

Um die von Azure AD DS verwaltete Domäne ordnungsgemäß aus der lokalen Umgebung aufzulösen, müssen Sie möglicherweise Weiterleitungen zu den vorhandenen DNS-Servern hinzufügen. Wenn Sie die lokale Umgebung nicht für die Kommunikation mit der verwalteten Azure AD DS-Domäne konfiguriert haben, führen Sie auf einer Verwaltungsarbeitsstation die folgenden Schritte für die lokale AD DS-Domäne aus:

1. Wählen Sie **Start | Verwaltung | DNS** aus.
1. Klicken Sie mit der rechten Maustaste auf den DNS-Server, z. B. *meinAD01*, und wählen Sie **Eigenschaften** aus.
1. Wählen Sie **Weiterleitungen** und dann **Bearbeiten** aus, um weitere Weiterleitungen hinzuzufügen.
1. Fügen Sie die IP-Adressen der verwalteten Azure AD DS-Domäne hinzu, z. B. *10.0.1.4* und *10.0.1.5*.

## <a name="create-inbound-forest-trust-in-the-on-premises-domain"></a>Erstellen der eingehenden Gesamtstruktur-Vertrauensstellung in der lokalen Domäne

Für die lokale AD DS-Domäne ist eine eingehende Gesamtstruktur-Vertrauensstellung für die verwaltete Azure AD DS-Domäne erforderlich. Diese Vertrauensstellung muss manuell in der lokalen AD DS-Domäne erstellt werden. Sie kann nicht im Azure-Portal erstellt werden.

Um die eingehende Vertrauensstellung in der lokalen AD DS-Domäne zu konfigurieren, führen Sie für die lokale AD DS-Domäne die folgenden Schritte auf einer Verwaltungsarbeitsstation aus:

1. Wählen Sie **Start | Verwaltung | Active Directory-Domänen und -Vertrauensstellungen** aus.
1. Wählen Sie mit der rechten Maustaste eine Domäne aus, z. B. *onprem.contoso.com*, und wählen Sie **Eigenschaften** aus.
1. Wählen Sie die Registerkarte **Vertrauensstellungen** aus, und wählen Sie dann **Neue Vertrauensstellung** aus.

   > [!NOTE]
   > Wenn die Menüoption **Vertrauensstellungen** nicht angezeigt wird, suchen Sie unter **Eigenschaften** nach dem *Gesamtstrukturtyp*. Nur *Ressourcengesamtstrukturen* können Vertrauensstellungen erstellen. Wenn der Gesamtstrukturtyp *Benutzer* lautet, können Sie keine Vertrauensstellungen erstellen. Es gibt derzeit keine Möglichkeit, den Gesamtstrukturtyp einer verwalteten Azure AD DS-Domäne zu ändern. Sie müssen die verwaltete Domäne löschen und als eine Ressourcengesamtstruktur neu erstellen.

1. Geben Sie einen Namen für die Azure AD DS-Domäne ein, z. B. *aaddscontoso.com*, und wählen Sie dann **Weiter** aus.
1. Wählen Sie die Option zum Erstellen einer **Gesamtstruktur-Vertrauenswürdigkeit** und dann die Option zum Erstellen einer **Unidirektional: eingehend**-Vertrauensstellung aus.
1. Wählen Sie die Option aus, mit der die Vertrauensstellung **Nur für diese Domäne** erstellt wird. Im nächsten Schritt erstellen Sie die Vertrauensstellung im Azure-Portal für die verwaltete Azure AD DS-Domäne.
1. Wählen Sie die Option zum Verwenden von **Gesamtstrukturweite Authentifizierung** aus, und geben Sie dann ein Vertrauensstellungskennwort ein. Dasselbe Kennwort wird auch im Azure-Portal im nächsten Abschnitt eingegeben.
1. Durchlaufen Sie die nächsten Fenster mit Standardoptionen, und aktivieren Sie dann die Option **Nein, ausgehende Vertrauensstellung nicht bestätigen**.
1. Wählen Sie **Fertig stellen**.

## <a name="create-outbound-forest-trust-in-azure-ad-ds"></a>Erstellen einer ausgehenden Gesamtstruktur-Vertrauensstellung in Azure AD DS

Nachdem Sie die lokale AD DS-Domäne konfiguriert haben, um die verwaltete Azure AD DS-Domäne aufzulösen, und eine eingehende Gesamtstruktur-Vertrauensstellung erstellt haben, erstellen Sie nun die ausgehende Gesamtstruktur-Vertrauensstellung. Diese ausgehende Gesamtstruktur-Vertrauensstellung schließt die Vertrauensstellung zwischen der lokalen AD DS-Domäne und der verwalteten Azure AD DS-Domäne ab.

Führen Sie die folgenden Schritte aus, um die ausgehende Vertrauensstellung für die verwaltete Azure AD DS-Domäne im Azure-Portal zu erstellen:

1. Suchen Sie im Azure-Portal nach **Azure AD Domain Services**, und wählen Sie dann Ihre verwaltete Domäne aus, z. B. *aaddscontoso.com*.
1. Wählen Sie im Menü auf der linken Seite der verwalteten Azure AD DS-Domäne die Option **Vertrauensstellungen** aus, und wählen Sie dann **+ Hinzufügen** aus.
1. Geben Sie einen Anzeigenamen ein, der Ihre Vertrauensstellung kennzeichnet, und geben Sie dann den lokalen DNS-Namen der vertrauenswürdigen Gesamtstruktur ein, z. B. *onprem.contoso.com*.
1. Geben Sie dasselbe Vertrauensstellungskennwort an, das beim Konfigurieren der eingehenden Gesamtstruktur-Vertrauensstellung für die lokale AD DS-Domäne im vorherigen Abschnitt verwendet wurde.
1. Geben Sie mindestens zwei DNS-Server für die lokale AD DS-Domäne an, z. B. *10.0.2.4* und *10.0.2.5*
1. Wählen Sie dann **Speichern** aus, um die ausgehende Gesamtstruktur-Vertrauensstellung zu speichern.

    [Erstellen einer ausgehenden Gesamtstruktur-Vertrauensstellung im Azure-Portal](./media/create-forest-trust/portal-create-outbound-trust.png)

## <a name="validate-resource-authentication"></a>Überprüfen der Ressourcenauthentifizierung

Mit den folgenden gängigen Szenarien können Sie überprüfen, ob die Gesamtstruktur-Vertrauensstellung Benutzer und Zugriff auf Ressourcen ordnungsgemäß authentifiziert:

* [Lokale Benutzerauthentifizierung aus der Azure AD DS-Ressourcengesamtstruktur](#on-premises-user-authentication-from-the-azure-ad-ds-resource-forest)
* [Zugreifen auf Ressourcen in der Azure AD DS-Ressourcengesamtstruktur über einen lokalen Benutzer](#access-resources-in-the-azure-ad-ds-resource-forest-using-on-premises-user)
    * [Aktivieren von Datei- und Druckerfreigabe](#enable-file-and-printer-sharing)
    * [Erstellen einer Sicherheitsgruppe und Hinzufügen von Mitgliedern](#create-a-security-group-and-add-members)
    * [Erstellen einer Dateifreigabe für gesamtstrukturübergreifenden Zugriff](#create-a-file-share-for-cross-forest-access)
    * [Überprüfen der gesamtstrukturübergreifenden Authentifizierung für eine Ressource](#validate-cross-forest-authentication-to-a-resource)

### <a name="on-premises-user-authentication-from-the-azure-ad-ds-resource-forest"></a>Lokale Benutzerauthentifizierung aus der Azure AD DS-Ressourcengesamtstruktur

Sie sollten den virtuellen Windows Server-Computer mit der Azure AD DS-Ressourcendomäne verknüpft haben. Verwenden Sie diesen virtuellen Computer, um zu testen, ob der lokale Benutzer sich bei einem virtuellen Computer authentifizieren kann.

1. Stellen Sie über „Remotedesktop“ und Ihre Azure AD DS-Administratoranmeldeinformationen eine Verbindung mit dem virtuellen Windows Server-Computer her, der mit Azure AD DS-Ressourcengesamtstruktur verknüpft ist. Wird ein „Authentifizierung auf Netzwerkebene“-Fehler gemeldet, überprüfen Sie, ob das von Ihnen verwendete Benutzerkonto tatsächlich ein Domänenbenutzerkonto ist.

    > [!NOTE]
    > Um sicher eine Verbindung mit Ihren virtuellen Computern herzustellen, die mit Azure AD Domain Services verknüpft sind, können Sie den [Azure Bastion-Hostdienst](https://docs.microsoft.com/azure/bastion/bastion-overview) in unterstützten Azure-Regionen verwenden.

1. Öffnen Sie eine Eingabeaufforderung, und verwenden Sie den `whoami`-Befehl, um den kennzeichnenden Namen (distinguished name) des derzeit authentifizierten Benutzers anzuzeigen:

    ```console
    whoami /fqdn
    ```

1. Verwenden Sie den `runas`-Befehl, um sich als Benutzer aus der lokalen Domäne zu authentifizieren. Ersetzen Sie `userUpn@trusteddomain.com` im folgenden Befehl durch den Benutzerprinzipalnamen (User Principal Name, UPN) eines Benutzers aus der vertrauenswürdigen lokalen Domäne. Der Befehl fordert Sie auf, das Kennwort des Benutzers einzugeben:

    ```console
    Runas /u:userUpn@trusteddomain.com cmd.exe
    ```

1. Wenn die Authentifizierung erfolgreich ist, wird eine neue Eingabeaufforderung geöffnet. Der Titel der neuen Eingabeaufforderung enthält `running as userUpn@trusteddomain.com`.
1. Verwenden Sie `whoami /fqdn` in der neuen Eingabeaufforderung, um den kennzeichnenden Namen des authentifizierten Benutzers aus dem lokalen Active Directory anzuzeigen.

### <a name="access-resources-in-the-azure-ad-ds-resource-forest-using-on-premises-user"></a>Zugreifen auf Ressourcen in der Azure AD DS-Ressourcengesamtstruktur über einen lokalen Benutzer

Mit dem virtuellen Windows Server-Computer, der mit der Azure AD DS-Ressourcengesamtstruktur verknüpft ist, können Sie das Szenario testen, in dem Benutzer auf Ressourcen, die in der Ressourcengesamtstruktur gehostet werden, zugreifen können, wenn sie sich von Computern in der lokalen Domäne als Benutzer der lokalen Domäne authentifizieren. In den folgenden Beispielen wird gezeigt, wie verschiedene gängige Szenarien erstellt und getestet werden.

#### <a name="enable-file-and-printer-sharing"></a>Aktivieren von Datei- und Druckerfreigabe

1. Stellen Sie über „Remotedesktop“ und Ihre Azure AD DS-Administratoranmeldeinformationen eine Verbindung mit dem virtuellen Windows Server-Computer her, der mit Azure AD DS-Ressourcengesamtstruktur verknüpft ist. Wird ein „Authentifizierung auf Netzwerkebene“-Fehler gemeldet, überprüfen Sie, ob das von Ihnen verwendete Benutzerkonto tatsächlich ein Domänenbenutzerkonto ist.

    > [!NOTE]
    > Um sicher eine Verbindung mit Ihren virtuellen Computern herzustellen, die mit Azure AD Domain Services verknüpft sind, können Sie den [Azure Bastion-Hostdienst](https://docs.microsoft.com/azure/bastion/bastion-overview) in unterstützten Azure-Regionen verwenden.

1. Öffnen Sie **Windows-Einstellungen**, suchen Sie nach **Netzwerk- und Freigabecenter**, und wählen Sie diese Option aus.
1. Wählen Sie die Option **Erweiterte Freigabeeinstellungen ändern** aus.
1. Wählen Sie unter **Domänenprofil** die Option **Datei- und Druckerfreigabe aktivieren** aus, und wählen Sie dann **Änderungen speichern** aus.
1. Schließen Sie **Netzwerk- und Freigabecenter**.

#### <a name="create-a-security-group-and-add-members"></a>Erstellen einer Sicherheitsgruppe und Hinzufügen von Mitgliedern

1. Öffnen Sie **Active Directory-Benutzer und -Computer**.
1. Wählen Sie mit der rechten Maustaste den Domänennamen aus, wählen Sie **Neu** aus, und wählen Sie dann **Organisationseinheit** aus.
1. Geben Sie in das Feld „Name“ den Namen *LokaleObjekte* ein, und wählen Sie dann **OK** aus.
1. Klicken Sie im Navigationsbereich mit der rechten Maustaste auf **LokaleObjekte**. Wählen Sie **Neu** und dann **Gruppe** aus.
1. Geben Sie *Dateiserverzugriff* in das Feld **Gruppenname** ein. Wählen Sie für **Gruppenbereich** die Option **Lokal (in Domäne)** aus, und wählen Sie dann **OK** aus.
1. Doppelklicken Sie im Inhaltsbereich auf **Dateiserverzugriff**. Wählen Sie **Mitglieder** aus, wählen Sie **Hinzufügen** aus, und wählen Sie dann **Standorte** aus.
1. Wählen Sie Ihr lokales Active Directory in der **Standort**-Ansicht aus, und wählen Sie dann **OK** aus.
1. Geben Sie *Domänenbenutzer* in das Feld **Geben Sie die zu verwendenden Objektnamen ein** ein. Wählen Sie **Namen überprüfen** aus, geben Sie die Anmeldeinformationen für das lokale Active Directory an, und wählen Sie dann **OK** aus.

    > [!NOTE]
    > Sie müssen Anmeldeinformationen angeben, weil die Vertrauensstellung nur unidirektional ist. Das heißt, dass Benutzer aus Azure AD DS weder auf Ressourcen in der vertrauenswürdigen (lokalen) Domäne zugreifen noch dort nach Benutzern oder Gruppen suchen können.

1. Die Gruppe **Domänenbenutzer** aus Ihrem lokalen Active Directory muss Mitglied der Gruppe **Dateiserverzugriff** sein. Wählen Sie **OK** aus, um die Gruppe zu speichern und das Fenster zu schließen.

#### <a name="create-a-file-share-for-cross-forest-access"></a>Erstellen einer Dateifreigabe für gesamtstrukturübergreifenden Zugriff

1. Erstellen Sie auf dem virtuellen Windows Server-Computer, der mit der Azure AD DS-Ressourcengesamtstruktur verknüpft ist, einen Ordner, und geben Sie diesem einen Namen wie *GesamtstrukturFreigabe* (CrossForestShare).
1. Wählen Sie den Ordner mit der rechten Maustaste aus, und wählen Sie **Eigenschaften** aus.
1. Wählen Sie die Registerkarte **Sicherheit** aus, und wählen Sie dann **Bearbeiten** aus.
1. Wählen Sie im Dialogfeld *Berechtigungen für GesamtstrukturFreigabe* die Option **Hinzufügen** aus.
1. Geben Sie *Dateiserverzugriff* in das Feld **Geben Sie die zu verwendenden Objektnamen ein** ein, und wählen Sie dann **OK** aus.
1. Wählen Sie *Dateiserverzugriff* in der Liste **Gruppen- oder Benutzernamen** aus. Wählen Sie in der Liste **Berechtigungen für GesamtstrukturFreigabe** die Option *Zulassen* für die Berechtigungen **Ändern** und **Schreiben** aus, und wählen Sie dann **OK** aus.
1. Wählen Sie die Registerkarte **Freigabe** aus, und wählen Sie dann **Erweiterte Freigabe...** aus.
1. Wählen Sie **Diesen Ordner freigeben** aus, und geben Sie dann einen Namen für die Dateifreigabe in **Freigabename** ein, z. B. *GesamtstrukturFreigabe*.
1. Wählen Sie **Berechtigungen** aus. Wählen Sie in der Liste **Berechtigungen für Jeder** die Option **Zulassen** für die Berechtigung **Ändern** aus.
1. Wählen Sie zweimal **OK** und dann **Schließen** aus.

#### <a name="validate-cross-forest-authentication-to-a-resource"></a>Überprüfen der gesamtstrukturübergreifenden Authentifizierung für eine Ressource

1. Melden Sie sich mit einem Benutzerkonto aus Ihrem lokalen Active Directory bei einem Windows-Computer an, der mit Ihrem lokalen Active Directory verknüpft ist.
1. Stellen Sie über **Windows-Explorer-** eine Verbindung mit der von Ihnen erstellten Freigabe her. Verwenden Sie dazu den vollqualifizierten Hostnamen und den Namen der Freigabe, z. B. `\\fs1.aaddscontoso.com\CrossforestShare`.
1. Um die Schreibberechtigung zu überprüfen, wählen Sie den Ordner mit der rechten Maustaste aus, wählen Sie **Neu** aus, und wählen Sie dann **Textdokument** aus. Verwenden Sie den Standardnamen **Neues Textdokument**.

    Wenn die Schreibberechtigungen ordnungsgemäß festgelegt sind, wird ein neues Textdokument erstellt. In den folgenden Schritten öffnen, bearbeiten und löschen Sie die Datei nach Bedarf.
1. Um die Leseberechtigung zu überprüfen, öffnen Sie **Neues Textdokument**.
1. Um die Änderungsberechtigung zu überprüfen, fügen Sie Text zur Datei hinzu, und schließen Sie **Editor**. Wenn Sie aufgefordert werden, Änderungen zu speichern, wählen Sie **Speichern** aus.
1. Um die Löschberechtigung zu überprüfen, wählen Sie mit der rechten Maustaste den Namen **Neues Textdokument** aus, und wählen Sie **Löschen** aus. Wählen Sie **Ja** aus, um das Löschen der Datei Löschung zu bestätigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren von DNS in einer lokalen AD DS-Umgebung, um Azure AD DS-Konnektivität zu unterstützen
> * Erstellen einer unidirektionalen eingehenden Gesamtstruktur-Vertrauensstellung in einer lokalen AD DS-Umgebung
> * Erstellen einer unidirektionalen ausgehenden Gesamtstruktur-Vertrauensstellung in Azure AD DS
> * Testen und Überprüfen der Vertrauensstellung hinsichtlich Authentifizierung und Ressourcenzugriff

Weitere konzeptbezogene Informationen zu Gesamtstrukturtypen in Azure AD DS finden Sie unter [Was sind Ressourcengesamtstrukturen?][concepts-forest] sowie unter [Wie funktionieren Gesamtstruktur-Vertrauensstellungen in Azure AD DS?][concepts-trust].

<!-- INTERNAL LINKS -->
[concepts-forest]: concepts-resource-forest.md
[concepts-trust]: concepts-forest-trust.md
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance-advanced]: tutorial-create-instance-advanced.md
[howto-change-sku]: change-sku.md

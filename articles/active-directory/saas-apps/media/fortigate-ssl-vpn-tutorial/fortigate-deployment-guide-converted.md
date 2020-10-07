---
title: FortiGate-Bereitstellungshandbuch | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die FortiGate-Firewall der nächsten Generation von Fortinet einrichten und verwenden.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 18a3d9d5-d81c-478c-be7e-ef38b574cb88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 357eb0a60e6246996de9ab75337ecc213d845ae7
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91273329"
---
# <a name="fortigate-deployment-guide"></a>FortiGate-Bereitstellungshandbuch

In diesem Bereitstellungshandbuch ist beschrieben, wie Sie die FortiGate-Firewall der nächsten Generation von Fortinet einrichten und verwenden.

## <a name="redeem-the-fortigate-license"></a>Einlösen der FortiGate-Lizenz

Das Fortinet-Produkt „FortiGate Next-Generation Firewall“ (FortiGate: Firewall der nächsten Generation) ist als virtueller Computer in Azure IaaS (Infrastructure-as-a-Service) verfügbar. Für diesen virtuellen Computer gibt es zwei Lizenzierungsmodi: „Nutzungsbasierte Bezahlung“ und „Bring Your Own License“ (BYOL).

Unter Umständen werden von Fortinet für Mitglieder des Azure AD-Get-to-Production-SHA-Teams Lizenzen zur Verfügung gestellt. Wurde keine Lizenz bereitgestellt, kann auch die Bereitstellung mit nutzungsbasierter Bezahlung verwendet werden.

Wenn eine Lizenz ausgestellt wurde, stellt Fortinet einen Registrierungscode zur Verfügung, der online eingelöst werden muss.

![Screenshot: Registrierungscode für FortiGate SSL VPN](registration-code.png)

1. Registrieren Sie sich unter https://support.fortinet.com/.
2. Melden Sie sich nach der Registrierung unter https://support.fortinet.com/ an.
3. Navigieren Sie zu **Asset** > **Register/Activate** (Ressource > Registrieren/Aktivieren).
4. Geben Sie den von Fortinet bereitgestellten Registrierungscode ein.
5. Geben Sie den Registrierungscode an, und wählen Sie **The product will be used by a non-government user** (Das Produkt wird von einem Benutzer verwendet, der nicht im Staatsdienst tätig ist.) und dann **Next** (Weiter) aus.
6. Geben Sie eine Produktbeschreibung (z. B. FortiGate) ein, legen Sie als Fortinet-Partner **Other** > **Microsoft** (Andere > Microsoft) fest, und wählen Sie **Next** (Weiter) aus.
7. Akzeptieren Sie die **Fortinet Product Registration Agreement** (Fortinet-Produktregistrierungsvereinbarung), und wählen Sie **Next** (Weiter) aus.
8. Akzeptieren Sie die **Terms** (Bedingungen), und wählen Sie **Confirm** (Bestätigen) aus.
9. Wählen Sie die Option **License File Download** (Download der Lizenzdatei) aus, und speichern Sie die Lizenz zur späteren Verwendung.


## <a name="download-firmware"></a>Herunterladen der Firmware

Die Fortinet FortiGate-Azure-VM wird derzeit nicht mit der für die SAML-Authentifizierung erforderlichen Firmwareversion ausgeliefert. Die aktuelle Version muss von Fortinet abgerufen werden.

1. Melden Sie sich bei https://support.fortinet.com/ an.
2. Navigieren Sie zu **Download** > **Firmware Images** (Herunterladen > Firmwareimages).
3. Wählen Sie rechts neben **Release Notes** (Versionshinweise) die Option **Download** (Herunterladen) aus.
4. Wählen Sie **v6.** > **6.** > **6.4.** aus.
5. Laden Sie **FGT_VM64_AZURE-v6-build1637-FORTINET.out** herunter, indem Sie in derselben Zeile den **HTTPS**-Link auswählen.
6. Speichern Sie die Datei zur späteren Verwendung.


## <a name="deploy-the-fortigate-vm"></a>Bereitstellen des virtuellen FortiGate-Computers

1. Navigieren Sie zu https://portal.azure.com, und melden Sie sich bei dem Abonnement an, unter dem Sie den virtuellen FortiGate-Computer bereitstellen möchten.
2. Erstellen Sie eine neue Ressourcengruppe, oder öffnen Sie die Ressourcengruppe, in der Sie den virtuellen FortiGate-Computer bereitstellen möchten.
3. Wählen Sie **Hinzufügen**.
4. Geben Sie unter **Marketplace durchsuchen** den Suchbegriff *Forti* ein. Wählen Sie **Fortinet FortiGate Next-Generation Firewall** aus.
5. Wählen Sie den Softwareplan aus („Bring Your Own License“, wenn Sie über eine Lizenz verfügen, andernfalls „Nutzungsbasierte Bezahlung“). Klicken Sie auf **Erstellen**.
6. Fügen Sie die VM-Konfiguration ein.

    ![Screenshot: Virtuellen Computer erstellen](virtual-machine.png)

7. Legen Sie den **Authentifizierungstyp** auf **Kennwort** fest, und geben Sie Administratoranmeldeinformationen für den virtuellen Computer ein.
8. Klicken Sie auf **Überprüfen** > **Erstellen**.
9. Warten Sie, bis die Bereitstellung des virtuellen Computers abgeschlossen ist.


### <a name="set-a-static-public-ip-address-and-assign-a-fully-qualified-domain-name"></a>Festlegen einer statischen öffentlichen IP-Adresse und Zuweisen eines vollqualifizierten Domänennamens

Um eine einheitliche Benutzerumgebung zu erhalten, sollten Sie die der FortiGate-VM zugewiesene öffentliche IP-Adresse so festlegen, dass ihre Zuweisung statisch erfolgt. Ordnen Sie sie zusätzlich einem vollqualifizierten Domänennamen (FQDN) zu.

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Wählen Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse aus.

    ![Screenshot: FortiGate SSL VPN](public-ip-address.png)

3. Wählen Sie **Statisch** > **Speichern** aus.

Erstellen Sie einen Hosteintrag (A) für den virtuellen Computer, wenn Sie über einen öffentlich routingfähigen Domänennamen für die Umgebung verfügen, in der der virtuelle FortiGate-Computer bereitgestellt wird. Dieser Eintrag wird der vorherigen öffentlichen IP-Adresse zugeordnet, die statisch zugewiesen wurde.

### <a name="create-a-new-inbound-network-security-group-rule-for-tcp-port"></a>Erstellen einer neuen Netzwerksicherheitsgruppen-Eingangsregel für den TCP-Port

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Klicken Sie im Menü auf der linken Seite auf **Netzwerk**. Die Netzwerkschnittstelle wird aufgeführt, und die Regeln für eingehende Ports werden angezeigt.
3. Wählen Sie **Regel für eingehenden Port hinzufügen** aus.
4. Erstellen Sie eine neue Regel für den eingehenden Port für TCP 8443.

    ![Screenshot: Eingangssicherheitsregel hinzufügen](port-rule.png)

5. Wählen Sie **Hinzufügen**.


## <a name="create-a-custom-azure-app-for-fortigate"></a>Erstellen einer benutzerdefinierten Azure-App für FortiGate

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie den Azure AD-Bereich für den Mandanten, der die Identität für FortiGate-Anmeldungen bereitstellt.
2. Wählen Sie im Menü auf der linken Seite die Option **Unternehmensanwendungen** aus.
3. Wählen Sie **Neue Anwendung** > **Nicht-Kataloganwendung** aus.
4. Geben Sie einen Namen ein (z. B. FortiGate), und wählen Sie **Hinzufügen** aus.
5. Wählen Sie im linken Menü **Benutzer und Gruppen** aus.
6. Fügen Sie Benutzer hinzu, denen Sie die Anmeldung ermöglichen möchten, und wählen Sie **Zuweisen** aus.
7. Wählen Sie im Menü auf der linken Seite die Option **Einmaliges Anmelden** aus.
8. Wählen Sie **SAML** aus.
9. Wählen Sie unter **Grundlegende SAML-Konfiguration** das Stiftsymbol aus, um die Konfiguration zu bearbeiten.
10. Konfigurieren Sie Folgendes:
    - **Bezeichner (Entitäts-ID)** : `https://<address>/remote/saml/metadata`.
    - **Antwort-URL (Assertionsverbraucherdienst-URL)** : `https://<address>/remote/saml/login`.
    - **Abmelde-URL**: `https://<address>/remote/saml/logout`.

    `<address>` ist der vollqualifizierte Domänenname oder die öffentliche IP-Adresse des virtuellen FortiGate-Computers.

11. Notieren Sie sich die folgenden URLs zur späteren Verwendung: Entitäts-ID, Antwort-URL und Abmelde-URL.
12. Wählen Sie **Speichern** aus, und schließen Sie **Grundlegende SAML-Konfiguration**.
13. Führen Sie unter **3. SAML-Signaturzertifikat** den Download für **Zertifikat (Base64)** durch, und speichern Sie das Zertifikat zur späteren Verwendung.
14. Kopieren Sie unter **4. Einrichtung (App-Name)** die Azure-Anmelde-URL, den Azure AD-Bezeichner und die Azure-Abmelde-URL, und speichern Sie die Werte zur späteren Verwendung.
15. Wählen Sie unter **2. Benutzerattribute und Ansprüche** das Stiftsymbol aus, um die Konfiguration zu bearbeiten.
16. Wählen Sie **Neuen Anspruch hinzufügen** aus, und legen Sie den Namen auf **username** fest.
17. Legen Sie das Quellattribut auf **user.userprincipalname** fest.
18. Wählen Sie **Speichern** > **Gruppenanspruch hinzufügen** > **Alle Gruppen** aus.
19. Wählen Sie **Name des Gruppenanspruchs anpassen** aus, und legen Sie den Namen auf **group** fest.
20. Wählen Sie **Speichern** aus.


## <a name="prepare-for-group-matching"></a>Vorbereiten auf Gruppenabstimmung

Mit FortiGate können nach der Anmeldung im Benutzerportal basierend auf der Gruppenmitgliedschaft verschiedene Benutzeroberflächen angezeigt werden. Beispielsweise kann eine Benutzeroberfläche für die Gruppe „Marketing“ und eine andere für die Gruppe „Finance“ angezeigt werden. Erstellen Sie Gruppen für Benutzer wie folgt:

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie den Azure AD-Bereich für den Mandanten, der die Identität für FortiGate-Anmeldungen bereitstellt.
2. Wählen Sie **Gruppen** > **Neue Gruppe** aus.
3. Erstellen Sie eine Gruppe mit den folgenden Details:
    - Gruppentyp: Sicherheit
    - Gruppenname: `a meaningful name`
    - Gruppenbeschreibung: `a meaningful description for the group`
    - Mitgliedschaftstyp: Zugewiesen
    - Mitglieder: `users for the user experience that will map to this group`
4. Wiederholen Sie die Schritte 3 und 4 für alle weiteren Benutzeroberflächen.
5. Wählen Sie nach der Erstellung der Gruppen nacheinander jeweils eine Gruppe aus, und notieren Sie sich ihre **Objekt-ID**.
6. Speichern Sie diese Objekt-IDs und Gruppennamen zur späteren Verwendung.


## <a name="configure-the-fortigate-vm"></a>Konfigurieren des virtuellen FortiGate-Computers

In den folgenden Abschnitten wird Schritt für Schritt beschrieben, wie Sie die FortiGate-VM einrichten.

### <a name="install-the-license"></a>Installieren der Lizenz

1. Gehe zu `https://<address>`. Hierbei steht `<address>` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Setzen Sie den Vorgang trotz etwaiger Zertifikatfehler fort.
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Wenn für die Bereitstellung das BYOL-Modell (Bring Your Own License) verwendet wird, wird eine Aufforderung zum Hochladen einer Lizenz angezeigt. Wählen Sie die weiter oben erstellte Lizenzdatei aus, und laden Sie sie hoch. Wählen Sie **OK** aus, und starten Sie den virtuellen FortiGate-Computer neu.

    ![Screenshot: FortiGate-VM-Lizenz](license.png)

5. Melden Sie sich nach dem Neustart erneut mit den Administratoranmeldeinformationen an, um die Lizenz zu überprüfen.

### <a name="update-firmware"></a>Aktualisieren der Firmware

1. Gehe zu `https://<address>`. Hierbei steht `<address>` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Setzen Sie den Vorgang trotz etwaiger Zertifikatfehler fort.
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Wählen Sie im Menü auf der linken Seite **System** > **Firmware** aus.
5. Wählen Sie unter **Firmwareverwaltung** die Option **Durchsuchen** und dann die zuvor heruntergeladene Firmwaredatei aus.
6. Ignorieren Sie die Warnung, und wählen Sie **Backup config and upgrade** (Konfiguration sichern und aktualisieren) aus.

    ![Screenshot: Firmwareverwaltung](backup-configure-upgrade.png)

7. Wählen Sie **Weiter**.
8. Wählen Sie **Speichern** aus, wenn Sie zum Speichern der FortiGate-Konfiguration (als CONF-Datei) aufgefordert werden.
9. Warten Sie, bis die Firmware hochgeladen und angewendet wurde. Warten Sie, bis der virtuelle FortiGate-Computer neu gestartet wurde.
10. Melden Sie sich nach dem Neustart des virtuellen FortiGate-Computers erneut mit den Administratoranmeldeinformationen an.
11. Wählen Sie **Später** aus, wenn Sie zum Einrichten des Dashboards aufgefordert werden.
12. Wählen Sie **OK** aus, wenn das Tutorialvideo beginnt.

### <a name="change-the-management-port-to-tcp"></a>Ändern des Verwaltungsports in „TCP“

1. Gehe zu `https://<address>`. Hierbei steht `<address>` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Setzen Sie den Vorgang trotz etwaiger Zertifikatfehler fort.
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Wählen Sie im Menü auf der linken Seite die Option **System** aus.
5. Ändern Sie unter **Administration Settings** (Verwaltungseinstellungen) den HTTPS-Port in **8443**, und wählen Sie **Übernehmen** aus.
6. Nachdem die Änderung übernommen wurde, wird vom Browser versucht, die Verwaltungsseite erneut zu laden. Hierbei tritt aber ein Fehler auf. Ab jetzt lautet die Adresse für die Verwaltungsseite wie folgt: `https://<address>`.

    ![Screenshot: Remotezertifikat hochladen](certificate.png)

### <a name="upload-the-azure-ad-saml-signing-certificate"></a>Hochladen des Azure AD-SAML-Signaturzertifikats

1. Gehe zu `https://<address>`. Hierbei steht `<address>` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Setzen Sie den Vorgang trotz etwaiger Zertifikatfehler fort.
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Wählen Sie im Menü auf der linken Seite **System** > **Zertifikate** aus.
5. Wählen Sie **Import** > **Remote Certificate** (Importieren > Remotezertifikat) aus.
6. Navigieren Sie zu dem Zertifikat, das bei der benutzerdefinierten FortiGate-App-Bereitstellung im Azure-Mandanten heruntergeladen wurde. Wählen Sie es aus, und wählen Sie anschließend **OK** aus.

### <a name="upload-and-configure-a-custom-ssl-certificate"></a>Hochladen und Konfigurieren eines benutzerdefinierten SSL-Zertifikats

Es kann ratsam sein, den virtuellen FortiGate-Computer mit Ihrem eigenen SSL-Zertifikat zu konfigurieren, das den von Ihnen verwendeten vollqualifizierten Domänennamen unterstützt. Wenn Sie Zugriff auf ein SSL-Zertifikat haben, das mit dem privaten Schlüssel im PFX-Format verpackt ist, kann es für diesen Zweck verwendet werden.

1. Gehe zu `https://<address>`. Hierbei steht `<address>` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Setzen Sie den Vorgang trotz etwaiger Zertifikatfehler fort.
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Wählen Sie im Menü auf der linken Seite **System** > **Zertifikate** aus.
5. Wählen Sie **Import** > **Local Certificate** > **PKCS #12 Certificate** (Importieren > Lokales Zertifikat > PKCS #12-Zertifikat) aus.
6. Navigieren Sie zur PFX-Datei, die das SSL-Zertifikat und den privaten Schlüssel enthält.
7. Geben Sie das PFX-Kennwort und einen aussagekräftigen Namen für das Zertifikat an. Klicken Sie anschließend auf **OK**.
8. Wählen Sie im Menü auf der linken Seite **System** > **Einstellungen** aus.
9. Erweitern Sie unter **Administration Settings** (Verwaltungseinstellungen) die Liste neben dem **HTTPS-Serverzertifikat**, und wählen Sie das zuvor importierte SSL-Zertifikat aus.
10. Wählen Sie **Übernehmen**.
11. Schließen Sie das Browserfenster, und navigieren Sie zu `https://<address>`.
12. Melden Sie sich mit den FortiGate-Administratoranmeldeinformationen an. Nun sollte die Verwendung des richtigen SSL-Zertifikats angezeigt werden.


### <a name="perform-command-line-configuration"></a>Durchführen der Befehlszeilenkonfiguration

In den folgenden Abschnitten sind die Schritte beschrieben, mit denen Sie verschiedene Konfigurationen über die Befehlszeile durchführen können.

#### <a name="for-saml-authentication"></a>SAML-Authentifizierung

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Wählen Sie im Menü auf der linken Seite **Serielle Konsole** aus.
3. Melden Sie sich mit den Administratoranmeldeinformationen des virtuellen FortiGate-Computers bei der seriellen Konsole an. Im nächsten Schritt werden die weiter oben notierten URLs benötigt:
    - Entitäts-ID
    - Antwort-URL
    - Abmelde-URL
    - Azure-Anmelde-URL
    - Azure AD-Bezeichner
    - Azure-Abmelde-URL
4. Führen Sie in der seriellen Konsole die folgenden Befehle aus:

    ```
    config user saml
    edit azure
    set entity-id <Entity ID>
    set single-sign-on-url <Reply URL>
    set single-logout-url <Logout URL>
    set idp-single-sign-on-url <Azure Login URL>
    set idp-entity-id <Azure AD Identifier>
    set idp-single-logout-url <Azure Logout URL>
    set idp-cert REMOTE_Cert_
    set user-name username
    set group-name group
    end
    ```
    > [!NOTE]
    > Die Azure-Abmelde-URL enthält das Zeichen `?`. Hierfür ist eine spezielle Tastenkombination erforderlich, damit die URL ordnungsgemäß für die serielle FortiGate-Konsole bereitgestellt werden kann. Normalerweise lautet die URL `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1`. Geben Sie Folgendes ein, um dies in der seriellen Konsole anzugeben:
        ```
        set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation
        ```.
    Drücken Sie dann STRG+V, um den restlichen Teil der URL einzufügen und die Zeile zu vervollständigen: 
        ```
        set idp-single-logout-url
        https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.
        ```

5. Führen Sie Folgendes aus, um die Konfiguration zu bestätigen:

    ```
    show user saml
    ```

#### <a name="for-group-matching"></a>Gruppenabgleich

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Wählen Sie im Menü auf der linken Seite **Serielle Konsole** aus.
3. Melden Sie sich mit den Administratoranmeldeinformationen des virtuellen FortiGate-Computers bei der seriellen Konsole an.
4. Führen Sie in der seriellen Konsole die folgenden Befehle aus:

    ```
    config user group
    edit <group 1 name>
    set member azure
    config match
    edit 1
    set server-name azure
    set group-name <group 1 Object Id>
    next
    end
    next
    ```

    Wiederholen Sie diese Befehle (ab der zweiten Codezeile) für jede weitere Gruppe, für die in FortiGate eine andere Portalumgebung verwendet wird.

#### <a name="for-authentication-timeout"></a>Authentifizierungszeitlimit

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Wählen Sie im Menü auf der linken Seite **Serielle Konsole** aus.
3. Melden Sie sich mit den Administratoranmeldeinformationen des virtuellen FortiGate-Computers bei der seriellen Konsole an.
4. Führen Sie in der seriellen Konsole die folgenden Befehle aus:

    ```
    config system global
    set remoteauthtimeout 60
    end
    ```
### <a name="create-vpn-portals-and-firewall-policy"></a>Erstellen von VPN-Portalen und einer Firewallrichtlinie

1. Gehe zu `https://<address>`. Hierbei steht `<address>` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
3. Wählen Sie im Menü auf der linken Seite **VPN** > **SSL-VPN Portals** > **Create New** (VPN > SSL-VPN-Portale > Neu erstellen) aus.
6. Geben Sie einen Namen an. (In der Regel wird dieser Name auf die Azure-Gruppe abgestimmt, die zum Bereitstellen der benutzerdefinierten Portaloberfläche verwendet wird.)
7. Wählen Sie das Pluszeichen ( **+** ) neben **Source IP Pools** (Quell-IP-Pools), den Standardpool und dann die Option **Close** (Schließen) aus.
8. Passen Sie die Benutzeroberfläche für diese Gruppe an. Zu Testzwecken können die Portalmeldung und das Design angepasst werden. Hier können Sie auch benutzerdefinierte Lesezeichen erstellen, die Benutzer an interne Ressourcen weiterleiten.
9. Klicken Sie auf **OK**.
10. Wiederholen Sie die Schritte 5 bis 9 für jede Azure-Gruppe, die über ein benutzerdefiniertes Portal verfügt.
11. Wählen Sie unter „VPN“ die Option **SSL-VPN Settings** (SSL-VPN-Einstellungen) aus.
12. Wählen Sie das Pluszeichen ( **+** ) neben **Listen on Interfaces** (An Schnittstellen lauschen), **Port1** und dann **Close** (Schließen) aus.
14. Falls Sie vorher ein benutzerdefiniertes SSL-Zertifikat installiert haben, müssen Sie im Dropdownmenü das **Serverzertifikat** ändern, um es verwenden zu können.
15. Wählen Sie unter **Authentication/Portal Mapping** (Zuordnung von Authentifizierung/Portal) die Option **Create New** (Neu erstellen) aus. Wählen Sie die erste Azure-Gruppe aus, und verknüpfen Sie sie mit dem gleichnamigen Portal. Klicken Sie anschließend auf **OK**.
18. Wiederholen Sie die Schritte 15 bis 17 für jedes Paar (Azure-Gruppe und Portal).
19. Bearbeiten Sie unter **Authentication/Portal Mapping** (Zuordnung von Authentifizierung/Portal) die Option **All Other Users/Groups** (Alle anderen Benutzer/Gruppen).
20. Legen Sie für das Portal die Option **Vollzugriff** fest, und wählen Sie **OK** > **Übernehmen** aus.
23. Scrollen Sie zum oberen Rand der Seite **SSL-VPN Settings** (SSL-VPN-Einstellungen), und wählen Sie die Option **No SSL-VPN policies exist. Click here to create a new SSL-VPN policy using these settings** (Keine SSL-VPN-Richtlinien vorhanden. Klicken Sie hier, um eine neue SSL-VPN-Richtlinie mit diesen Einstellungen zu erstellen.) aus.
24. Geben Sie einen Namen an, z. B. **VPN Grp**. Legen Sie anschließend **Outgoing Interface** (Ausgangsschnittstelle) auf **Port** fest, und wählen Sie **Source** (Quelle) aus.
27. Wählen Sie unter **Address** (Adresse) die Option **all** (alle) aus.
28. Wählen Sie unter **User** (Benutzer) die erste Azure-Gruppe aus.
29. Wählen Sie **Close** > **Destination** (Schließen > Ziel) aus. Für **Address** (Adresse) ist dies normalerweise das interne Netzwerk. Wählen Sie zu Testzwecken **login.microsoft.com** aus.
32. Wählen Sie **Close** > **Service** > **All** (Schließen > Dienst > Alle) aus. Wählen Sie anschließend **Close** > **OK** (Schließen > OK) aus.
37. Wählen Sie im Menü auf der linken Seite die Option **Policy & Objects** > **Firewall Policy** (Richtlinie und Objekte > Firewallrichtlinie) aus.
39. Erweitern Sie **SSL-VPN tunnel interface (ssl.root)**  > **port** (SSL-VPN-Tunnelschnittstelle (ssl.root) > Port).
40. Klicken Sie mit der rechten Maustaste auf die zuvor erstellte VPN-Richtlinie (**VPN Grp 1**), und wählen Sie **Copy** (Kopieren) aus.
41. Klicken Sie mit der rechten Maustaste unterhalb der VPN-Richtlinie, und wählen Sie **Paste** > **Below** (Einfügen > Unter) aus.
42. Bearbeiten Sie die neue Richtlinie, indem Sie einen anderen Namen dafür angeben (z. B. **VPN Grp2**). Ändern Sie auch die Gruppe, für die der Name gilt (in eine andere Azure-Gruppe).
43. Klicken Sie mit der rechten Maustaste auf die neue Richtlinie, und legen Sie den Status auf **Enabled** (Aktiviert) fest.


## <a name="test-sign-in-by-using-azure"></a>Testen der Anmeldung mit Azure

1. Navigieren Sie in einer InPrivate-Browsersitzung zu `https://<address>`.  
2. Sie sollten an die Anmeldung für Azure AD umgeleitet werden.
3. Nach dem Angeben der Anmeldeinformationen für einen Benutzer, der der FortiGate-App im Azure-Mandanten zugewiesen wurde, sollte das entsprechende Benutzerportal angezeigt werden.

    ![Screenshot: FortiGate SSL VPN](test-sign-in.png)

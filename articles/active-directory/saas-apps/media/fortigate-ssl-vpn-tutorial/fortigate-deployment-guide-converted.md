---
title: FortiGate-Bereitstellungshandbuch | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und FortiGate SSL VPN konfigurieren.
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
ms.openlocfilehash: 999e19ffad1d18e163881c844cbf30f8b7fef574
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88657103"
---
# <a name="fortigate-deployment-guide"></a>FortiGate-Bereitstellungshandbuch

## <a name="contents"></a>Contents

- Einlösen der FortiGate-Lizenz
- Herunterladen der Firmware
- Bereitstellen des virtuellen FortiGate-Computers
   - Festlegen einer statischen öffentlichen IP-Adresse und Zuweisen eines voll qualifizierten Domänennamens
   - Erstellen einer neuen Netzwerksicherheitsgruppen-Eingangsregel für den TCP-Port
- Erstellen einer benutzerdefinierten Azure-App für FortiGate
- Vorbereiten auf Gruppenabstimmung
   - Erstellen von Gruppen für Benutzer
- Konfigurieren des virtuellen FortiGate-Computers
   - Installieren der Lizenz
   - Aktualisieren der Firmware
   - Ändern des Verwaltungsports in TCP
   - Hochladen des Azure Active Directory-SAML-Signaturzertifikats
   - Hochladen und Konfigurieren eines benutzerdefinierten SSL-Zertifikats
   - Ausführen der Befehlszeilenkonfiguration
   - Erstellen von VPN-Portalen und einer Firewallrichtlinie
- Testen der Anmeldung mithilfe von Azure

## <a name="redeeming-the-fortigate-license"></a>Einlösen der FortiGate-Lizenz

Das Fortinet-Produkt „FortiGate Next-Generation Firewall“ ist als virtueller Computer in Azure IaaS verfügbar. Es gibt zwei Lizenzierungsmodi für diesen virtuellen Computer:

- Nutzungsbasierte Bezahlung (Pay-as-you-go, PAYG)
- Bring Your Own License (BYOL)

Im Rahmen der Zusammenarbeit mit Fortinet, um Informationen zu SHA (Secure Hybrid Access, sicherer Hybridzugriff) bereitzustellen, stellt Fortinet dem Azure AD-Get-to-Production-SHA-Team unter Umständen Lizenzen zur Verfügung. Wurden keine Lizenz bereitgestellt, kann auch die Bereitstellung mit nutzungsbasierter Zahlung verwendet werden.

Wurde eine Lizenz ausgestellt, stellt Fortinet einen Registrierungscode zur Verfügung, der online eingelöst werden muss:

![FortiGate SSL VPN](registration-code.png)

1. Registrieren Sie sich unter https://support.fortinet.com/.
2. Melden Sie sich nach der Registrierung unter https://support.fortinet.com/ an.
3. Navigieren Sie zu **Asset** (Ressource) > **Register/Activate** (Registrieren/Aktivieren).
4. Geben Sie den von Fortinet bereitgestellten Registrierungscode ein.
5. Geben Sie den Registrierungscode an, wählen Sie **The product will be used by a non-government user** (Das Produkt wird von einem Benutzer verwendet, der nicht im Staatsdienst tätig ist.) aus, und klicken Sie auf **Next** (Weiter).
6. Geben Sie eine Produktbeschreibung (z. B. FortiGate) ein, legen Sie den Fortinet-Partner auf **Other** (Andere) > **Microsoft** fest, und klicken Sie auf **Next** (Weiter).
7. Akzeptieren Sie die **Fortinet-Produktregistrierungsvereinbarung**, und klicken Sie auf **Next** (Weiter).
8. Akzeptieren Sie die **Bedingungen**, und klicken Sie auf **Confirm** (Bestätigen).
9. Klicken Sie auf die Option für den **Download der Lizenzdatei**, und speichern Sie die Lizenz zur späteren Verwendung.


## <a name="download-firmware"></a>Herunterladen der Firmware

Zum Zeitpunkt der Artikelerstellung wird die Fortinet FortiGate-Azure-VM nicht mit der für die SAML-Authentifizierung erforderlichen Firmwareversion ausgeliefert. Die aktuelle Version muss von Fortinet abgerufen werden.

1. Melden Sie sich unter https://support.fortinet.com/ an.
2. Navigieren Sie zu **Download** (Herunterladen) > **Firmware Images** (Firmwareimages).
3. Klicken Sie rechts neben **Release Notes** (Versionshinweise) auf **Download** (Herunterladen).
4. Klicken Sie auf **v6**.
5. Klicken Sie auf **6**.
6. Klicken Sie auf **6.4**.
7. Laden Sie **FGT_VM64_AZURE-v6-build1637-FORTINET.out** herunter, indem Sie in der gleichen Zeile auf den **HTTPS**-Link klicken.
8. Speichern Sie die Datei für die spätere Verwendung.


## <a name="deploy-the-fortigate-vm"></a>Bereitstellen des virtuellen FortiGate-Computers

1. Navigieren Sie zu https://portal.azure.com, und melden Sie sich bei dem Abonnement an, in dem Sie den virtuellen FortiGate-Computer bereitstellen möchten.
2. Erstellen Sie eine neue Ressourcengruppe, oder öffnen Sie die Ressourcengruppe, in der Sie den virtuellen FortiGate-Computer bereitstellen möchten.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie „Forti“ in das Dialogfeld **Marketplace durchsuchen** ein, und wählen sie **Fortinet FortiGate Next-** **Generation Firewall** aus.
5. Wählen Sie den Softwareplan aus („BYOL“, wenn Sie eine Lizenz besitzen, bzw. „Nutzungsbasierte Zahlung“, wenn Sie keine besitzen), und klicken Sie auf **Erstellen**.
6. Auffüllen der VM-Konfiguration

    ![FortiGate SSL VPN](virtual-machine.png)

7. Legen Sie den Authentifizierungstyp auf **Kennwort** fest, und geben Sie Administratoranmeldeinformationen für den virtuellen Computer ein.
8. Klicken Sie auf **Überprüfen + erstellen**.
9. Klicken Sie auf **Erstellen**
10. Warten Sie, bis die Bereitstellung des virtuellen Computers abgeschlossen ist.


### <a name="set-a-statuc-public-ip-address-and-assign-a-fully-qualified-domain-name"></a>Festlegen einer statischen öffentlichen IP-Adresse und Zuweisen eines voll qualifizierten Domänennamens

Um eine konsistente Benutzerumgebung zu erhalten, wird empfohlen, die der FortiGate-VM zugewiesene öffentliche IP-Adresse so festzulegen, dass ihre Zuweisung statisch erfolgt. Darüber hinaus ist die Zuordnung zu einem vollqualifizierten Domänennamen ebenfalls aus den gleichen Gründen nützlich.

_Festlegen einer statischen öffentlichen IP-Adresse_

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Klicken Sie auf dem Bildschirm **Übersicht** auf die öffentliche IP-Adresse.

    ![FortiGate SSL VPN](public-ip-address.png)

3. Klicken Sie auf **Statisch** und dann auf **Speichern**.

_Zuweisen eines vollqualifizierten Domänennamens_

Wenn Sie für die Umgebung, in der der virtuelle FortiGate-Computer bereitgestellt wird, einen öffentlich routingfähigen Domänennamen besitzen, erstellen Sie einen Hosteintrag (A) für den virtuellen Computer, der der oben statisch zugewiesenen öffentlichen IP-Adresse zugeordnet ist.

### <a name="create-a-new-inbound-network-security-group-rule-for-tcp-port"></a>Erstellen einer neuen Netzwerksicherheitsgruppen-Eingangsregel für den TCP-Port

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Klicken Sie im linken Menü auf **Netzwerk**. Die Netzwerkschnittstelle wird aufgeführt, und die Regeln für eingehende Ports werden angezeigt.
3. Klicken Sie auf **Regel für eingehenden Port hinzufügen**.
4. Erstellen einer neuen Regel für den eingehenden Port für TCP 8443

    ![FortiGate SSL VPN](port-rule.png)

5. Klicken Sie auf **Hinzufügen**.


## <a name="create-a-custom-azure-app-for-fortigate"></a>Erstellen einer benutzerdefinierten Azure-App für FortiGate

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie das Blatt „Azure Active Directory“ für den Mandanten, der die Identität für FortiGate-Anmeldungen bereitstellt.
2. Klicken Sie im Menü auf der linken Seite auf **Unternehmensanwendungen**.
3. Klicken Sie auf **Neue Anwendung**.
4. Klicken Sie auf **Nicht-Kataloganwendung**.
5. Geben Sie einen Namen (z. B FortiGate) ein, und klicken Sie auf **Hinzufügen**.
6. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.
7. Fügen Sie Benutzer hinzu, denen Sie die Anmeldung ermöglichen möchten, und klicken Sie auf **Zuweisen**.
8. Klicken Sie im Menü auf der linken Seite auf **Einmaliges Anmelden**.
9. Klicken Sie auf **SAML**.
10. Klicken Sie unter **Grundlegende SAML-Konfiguration** auf den Stift, um die Konfiguration zu bearbeiten
11. Konfigurieren
    - Bezeichner (Entitäts-ID): `https://<address>/remote/saml/metadata`
    - Antwort-URL (Assertionsverbraucherdienst-URL): `https://<address>/remote/saml/login`
    - Abmelde-URL: `https://<address>/remote/saml/logout`

    Dabei steht `address` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

    Notieren Sie sich die folgenden Werte zur späteren Verwendung:

    - Entitäts-ID
    - Antwort-URL
    - Abmelde-URL
12. Klicken Sie unten auf der Seite auf **Speichern**.
13. Schließen Sie die grundlegende SAML-Konfiguration.
14. Laden Sie unter **3. SAML-Signaturzertifikat** das **Zertifikat (Base64)** herunter, und speichern Sie es zur späteren Verwendung.
15. Kopieren Sie unter **4. Einrichtung (App-Name)** die Azure-Anmelde-URL, den Azure AD-Bezeichner und die Azure-Abmelde-URL, und speichern Sie die Werte zur späteren Verwendung.
    - Azure-Anmelde-URL
    - Azure AD-Bezeichner
    - Azure-Abmelde-URL
16. Klicken Sie unter **2. Benutzerattribute und Ansprüche** auf den Stift, um die Konfiguration zu bearbeiten.
17. Klicken Sie auf **Neuen Anspruch hinzufügen**.
18. Legen Sie den Namen auf **username** fest.
19. Legen Sie das Quellattribut auf **user.userprincipalname** fest.
20. Klicken Sie unten auf der Seite auf **Speichern**.
21. Klicken Sie auf **Gruppenanspruch hinzufügen**.
22. Wählen Sie **Alle Gruppen** aus.
23. Aktivieren Sie **Name des Gruppenanspruchs anpassen**.
24. Legen Sie den Namen auf **group** fest.
25. Klicken Sie unten auf der Seite auf **Speichern**.


## <a name="prepare-for-group-matching"></a>Vorbereiten auf Gruppenabstimmung

Mit FortiGate können nach der Anmeldung basierend auf der Gruppenmitgliedschaft verschiedene Benutzeroberflächen im Benutzerportal angezeigt werden. Beispielsweise kann eine Benutzeroberfläche für die Gruppe „Marketing“ und eine andere für die Gruppe „Finance“ angezeigt werden.

Nehmen Sie die Konfiguration wie folgt vor:

### <a name="create-groups-for-users"></a>Erstellen von Gruppen für Benutzer

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie das Blatt „Azure Active Directory“ für den Mandanten, der die Identität für FortiGate-Anmeldungen bereitstellt.
2. Klicken Sie auf **Gruppen**.
3. Klicken Sie auf **Neue Gruppe**.
4. Erstellen Sie eine Gruppe mit den folgenden Werten:
    - Gruppentyp: Sicherheit
    - Gruppenname: `a meaningful name`
    - Gruppenbeschreibung: `a meaningful description for the group`
    - Mitgliedschaftstyp: Zugewiesen
    - Mitglieder: `users for the user experience that will map to this group`
5. Wiederholen Sie die Schritte 3 und 4 für alle weiteren Benutzeroberflächen.
6. Wählen Sie nach der Erstellung der Gruppen jede Gruppe aus, und notieren Sie sich die einzelnen Objekt-IDs.
7. Speichern Sie diese Objekt-IDs und Gruppennamen zur späteren Verwendung.


## <a name="configure-the-fortigate-vm"></a>Konfigurieren des virtuellen FortiGate-Computers

### <a name="install-the-license"></a>Installieren der Lizenz

1. Navigieren Sie zu `https://<address>`.

    Dabei steht `address` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Fortsetzen nach Zertifikatfehlern
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Verwendet die Bereitstellung das BYOL-Modell, wird eine Aufforderung zum Hochladen einer Lizenz angezeigt. Wählen Sie die zuvor erstellte Lizenzdatei aus, und laden Sie sie hoch. Klicken Sie dann auf **OK**, und starten Sie den virtuellen FortiGate-Computer neu.

    ![FortiGate SSL VPN](license.png)

5. Melden Sie sich nach dem Neustart erneut mit den Administratoranmeldeinformationen an, um die Lizenz zu überprüfen.

### <a name="update-firmware"></a>Aktualisieren der Firmware

1. Navigieren Sie zu `https://<address>`.

    Dabei steht `address` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Fortsetzen nach Zertifikatfehlern
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Klicken Sie im linken Menü auf **System**.
5. Klicken Sie im Menü auf der linken Seite unter „System“ auf **Firmware**.
6. Klicken Sie auf der Seite für die Firmwareverwaltung auf **Browse** (Durchsuchen), und wählen Sie die zuvor heruntergeladene Firmwaredatei aus.
7. Ignorieren Sie die Warnung, und klicken Sie auf **Backup config and upgrade** (Konfiguration sichern und aktualisieren).

    ![FortiGate SSL VPN](backup-configure-upgrade.png)

8. Klicken Sie auf **Weiter**.
9. Wenn Sie zur Speicherung der FortiGate-Konfiguration (als CONF-Datei) aufgefordert werden, klicken Sie auf **Speichern**.
10. Warten Sie, bis die Firmware hochgeladen und angewendet wurde und der virtuelle FortiGate-Computer neu gestartet wird.
11. Melden Sie sich nach dem Neustart des virtuellen FortiGate-Computers erneut mit den Administratoranmeldeinformationen an.
12. Klicken Sie bei der Aufforderung zur Dashboardeinrichtung auf **Later** (Später).
13. Wenn das Tutorialvideo beginnt, klicken Sie auf **OK**.

### <a name="change-the-management-port-to-tcp"></a>Ändern des Verwaltungsports in TCP

1. Navigieren Sie zu `https://<address>`.

    Dabei steht `address` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Fortsetzen nach Zertifikatfehlern
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Klicken Sie im linken Menü auf **System**.
5. Ändern Sie unter „Administration Settings“ (Verwaltungseinstellungen) den HTTPS-Port in **8443**.
6. Klicken Sie auf **Anwenden**.
7. Nachdem die Änderung übernommen wurde, wird vom Browser versucht, die Seite „Administration“ (Verwaltung) erneut zu laden. Dabei tritt jedoch ein Fehler auf. Ab jetzt lautet die Adresse für die Seite „Administration“ (Verwaltung) `https://<address>`.

    ![FortiGate SSL VPN](certificate.png)

### <a name="upload-the-azure-active-directory-saml-signing-certificate"></a>Hochladen des Azure Active Directory-SAML-Signaturzertifikats

1. Navigieren Sie zu `https://<address>`.

    Dabei steht `address` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Fortsetzen nach Zertifikatfehlern
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Klicken Sie im linken Menü auf **System**.
5. Klicken Sie unter „System“ auf **Certificates** (Zertifikate).
6. Klicken Sie auf **Import** (Importieren) > **Remote Certificate** (Remotezertifikat).
7. Navigieren Sie zu dem Zertifikat, das bei der benutzerdefinierten FortiGate-App-Bereitstellung im Azure-Mandanten heruntergeladen wurde, wählen Sie es aus, und klicken Sie auf **OK**.

### <a name="upload-and-configure-a-custom-ssl-certificate"></a>Hochladen und Konfigurieren eines benutzerdefinierten SSL-Zertifikats

Ggf. möchten Sie den virtuellen FortiGate-Computer mit Ihrem eigenen SSL-Zertifikat konfigurieren, das den von Ihnen verwendeten vollqualifizierten Domänennamen unterstützt. Wenn Sie Zugriff auf ein SSL-Zertifikat haben, das mit dem privaten Schlüssel im PFX-Format verpackt ist, kann es für diesen Zweck verwendet werden.

1. Navigieren Sie zu `https://<address>`.

    Dabei steht `address` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Fortsetzen nach Zertifikatfehlern
3. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
4. Klicken Sie im linken Menü auf **System**.
5. Klicken Sie unter „System“ auf **Certificates** (Zertifikate).
6. Klicken Sie auf **Import** (Importieren) > **Local Certificate** (Lokales Zertifikat).
7. Klicken Sie auf**PKCS #12 Certificate** (PKCS #12-Zertifikat).
8. Navigieren Sie zur PFX-Datei mit dem SSL-Zertifikat und dem privaten Schlüssel.
9. Geben Sie das PFX-Kennwort an.
10. Geben Sie einen aussagekräftigen Namen für das Zertifikat ein.
11. Klicken Sie auf **OK**
12. Klicken Sie im linken Menü auf **System**.
13. Klicken Sie unter „System“ auf **Settings** (Einstellungen).
14. Erweitern Sie unter „Administration Settings“ (Verwaltungseinstellungen) das Dropdownmenü neben dem HTTPS-Serverzertifikat, und wählen Sie das oben importierte SSL-Zertifikat aus.
15. Klicken Sie auf **Anwenden**.
16. Schließen Sie das Browserfenster, und navigieren Sie dann erneut zu `https://<address>`.
17. Melden Sie sich mit den FortiGate-Administratoranmeldeinformationen an, und überprüfen Sie, ob das richtige SSL-Zertifikat verwendet wird.


### <a name="perform-command-line-configuration"></a>Ausführen der Befehlszeilenkonfiguration

_Ausführen der Befehlszeilenkonfiguration für die SAML-Authentifizierung_

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Klicken Sie im linken Menü auf **Serielle Konsole**.
3. Melden Sie sich mit den Administratoranmeldeinformationen des virtuellen FortiGate-Computers bei der seriellen Konsole an.

    Im nächsten Schritt werden die zuvor notierten URLs benötigt, d. h.:

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
    > Die Azure-Abmelde-URL enthält das Zeichen „?“. Hierfür ist eine spezielle Tastenkombination erforderlich, damit die URL ordnungsgemäß für die serielle FortiGate-Konsole bereitgestellt werden kann. Die URL lautet in der Regel wie folgt:

    `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1`

    Geben Sie für die Angabe in der seriellen Konsole Folgendes ein:

    ```
    set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation
    ```
    Drücken Sie anschließend STRG+V.

    Fügen Sie dann die restliche URL ein, um die Zeile zu vervollständigen.

    ```
    set idp-single-logout-url
    https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.
    ```

5. Führen Sie zum Bestätigen der Konfiguration Folgendes aus:

    ```
    show user saml
    ```

_Ausführen der Befehlszeilenkonfiguration für die Gruppenabstimmung_

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Klicken Sie im linken Menü auf **Serielle Konsole**.
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

    Wiederholen Sie diesen Befehl ab „edit `group 1 name`“ für jede weitere Gruppe, die über eine andere Portaloberfläche in FortiGate verfügt.

_Ausführen der Befehlszeilenkonfiguration für das Authentifizierungstimeout_

1. Navigieren Sie zu https://portal.azure.com, und öffnen Sie die Einstellungen für den virtuellen FortiGate-Computer.
2. Klicken Sie im linken Menü auf **Serielle Konsole**.
3. Melden Sie sich mit den Administratoranmeldeinformationen des virtuellen FortiGate-Computers bei der seriellen Konsole an.
4. Führen Sie in der seriellen Konsole die folgenden Befehle aus:

    ```
    config system global
    set remoteauthtimeout 60
    end
    ```
### <a name="create-vpn-portals-and-firewall-policy"></a>Erstellen von VPN-Portalen und einer Firewallrichtlinie

1. Navigieren Sie zu `https://<address>`.

    Dabei steht `address` für den vollqualifizierten Domänennamen oder die öffentliche IP-Adresse, die dem virtuellen FortiGate-Computer zugewiesen ist.

2. Melden Sie sich mit den Administratoranmeldeinformationen an, die während der Bereitstellung des virtuellen FortiGate-Computers bereitgestellt wurden.
3. Klicken Sie im linken Menü auf **VPN**.
4. Klicken Sie unter „VPN“ auf **SSL-VPN Portals** (SSL-VPN-Portale).
5. Klicken Sie auf **Create New** (Neu erstellen).
6. Geben Sie einen Namen an. (In der Regel wird dieser auf die Azure-Gruppe abgestimmt, die zum Bereitstellen der benutzerdefinierten Portaloberfläche verwendet wird.)
7. Klicken Sie auf das Pluszeichen ( **+** ) neben „Source IP Pools“ (Quell-IP-Pools), wählen Sie den Standardpool aus, und klicken Sie auf **Close** (Schließen).
8. Passen Sie die Benutzeroberfläche für diese Gruppe an. Zu Testzwecken können die Portalmeldung und das Design angepasst werden. Hier können Sie auch benutzerdefinierte Lesezeichen erstellen, die Benutzer an interne Ressourcen weiterleiten.
9. Klicken Sie auf **OK**
10. Wiederholen Sie die Schritte 5 bis 9 für jede Azure-Gruppe, die über ein benutzerdefiniertes Portal verfügt.
11. Klicken Sie unter „VPN“ auf **SSL-VPN Settings** (SSL-VPN-Einstellungen).
12. Klicken Sie neben „Listen on Interfaces“ (Lauschen an Schnittstellen) auf das Pluszeichen ( **+** ).
13. Wählen Sie **Port1** aus, und klicken Sie auf **Close** (Schließen).


14. Wurde zuvor ein benutzerdefiniertes SSL-Zertifikat installiert, ändern Sie im Dropdownmenü das Serverzertifikat, um das benutzerdefinierte SSL-Zertifikat zu verwenden.
15. Klicken Sie unter „Authentication/Portal Mapping“ (Authentifizierung/Portalzuordnung) auf **Create New** (Neu erstellen).
16. Wählen Sie die erste Azure-Gruppe aus, und stimmen Sie sie auf das Portal mit dem gleichen Namen ab.
17. Klicken Sie auf **OK**
18. Wiederholen Sie die Schritte 15 bis 17 für jedes Paar aus Azure-Gruppe und Portal.
19. Bearbeiten Sie unter „Authentication/Portal Mapping“ (Authentifizierung/Portalzuordnung) die Option **All Other Users/Groups** (Alle anderen Benutzer/Gruppen).
20. Legen Sie **Vollzugriff** für das Portal fest.
21. Klicken Sie auf **OK**
22. Klicken Sie auf **Anwenden**.
23. Scrollen Sie auf der Seite „SSL-VPN Settings“ (SSL-VPN-Einstellungen) ganz nach oben, und klicken Sie auf die Warnung **No SSL-VPN policies**
    **exist. Click here to create a new SSL-VPN policy using these settings** (Keine SSL-VPN-Richtlinien vorhanden. Klicken Sie hier, um eine neue SSL-VPN-Richtlinie mit diesen Einstellungen zu erstellen.).
24. Geben Sie einen Namen an, z. B. **VPN Grp**.
25. Legen Sie „Outgoing Interface“ (Ausgangsschnittstelle) auf **Port** fest.
26. Klicken Sie auf **Source** (Quelle).
27. Wählen Sie unter „Address“ (Adresse) die Option **all** (alle) aus.
28. Wählen Sie unter „User“ (Benutzer) die erste Azure-Gruppe aus.
29. Klicken Sie auf **Schließen**.
30. Klicken Sie auf **Destination** (Ziel).
31. Unter „Address“ (Adresse) ist dies in der Regel das interne Netzwerk. Wählen Sie zum Testen „login.microsoft.com“ aus.
32. Klicken Sie auf **Schließen**.
33. Klicken Sie auf **Service** (Dienst).
34. Klicken Sie auf **All** (Alle).
35. Klicken Sie auf **Schließen**.
36. Klicken Sie auf **OK**
37. Klicken Sie im linken Menü auf **Policy & Objects** (Richtlinie und Objekte).
38. Klicken Sie unter „Policy & Objects“ (Richtlinie und Objekte) auf **Firewall Policy** (Firewallrichtlinie).
39. Erweitern Sie **SSL-VPN tunnel interface (ssl.root) > port** (SSL-VPN-Tunnelschnittstelle (ssl.root) > Port).
40. Klicken Sie mit der rechten Maustaste auf die zuvor erstellte VPN-Richtlinie (**VPN Grp 1**), und wählen Sie **Copy** (Kopieren) aus.
41. Klicken Sie mit der rechten Maustaste unterhalb der VPN-Richtlinie, und wählen Sie **Paste** (Einfügen) > **Below** (Unter) aus.
42. Bearbeiten Sie die neue Richtlinie. Geben Sie Ihr einen anderen Namen (z. B. **VPN Grp2**), und ändern Sie die Gruppe, für die sie gilt (eine andere Azure-Gruppe).
43. Klicken Sie mit der rechten Maustaste auf die neue Richtlinie, und legen Sie den Status auf **Enabled** (Aktiviert) fest.


## <a name="test-sign-in-using-azure"></a>Testen der Anmeldung mithilfe von Azure

1. Navigieren Sie in einer InPrivate-Browsersitzung zu `https://<address>`. 
2. Bei der Anmeldung sollten Sie für die Anmeldung zu Azure Active Directory umgeleitet werden.
3. Nach der Angabe von Anmeldeinformationen für einen Benutzer, der der FortiGate-App im Azure-Mandanten zugewiesen wurde, sollte das entsprechende Benutzerportal angezeigt werden.

    ![FortiGate SSL VPN](test-sign-in.png)

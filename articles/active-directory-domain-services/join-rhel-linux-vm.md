---
title: Einbinden eines virtuellen RHEL-Computers in Azure AD Domain Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen virtuellen Red Hat Enterprise Linux-Computer in eine durch Azure AD Domain Services verwaltete Domäne einbinden.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 16100caa-f209-4cb0-86d3-9e218aeb51c6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: iainfou
ms.openlocfilehash: d12dd0c79f2e9c1d2b0cc17956a0bb8d8fa35865
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299141"
---
# <a name="join-a-red-hat-enterprise-linux-virtual-machine-to-an-azure-ad-domain-services-managed-domain"></a>Einbinden eines virtuellen Red Hat Enterprise Linux-Computers in eine durch Azure AD Domain Services verwaltete Domäne

Damit Benutzer sich mit einem einzigen Satz von Anmeldeinformationen bei virtuellen Computern (VMs) in Azure anmelden können, binden Sie VMs in eine durch Azure Active Directory Domain Services (AD DS) verwaltete Domäne ein. Wenn Sie eine VM in eine durch Azure AD DS verwaltete Domäne einbinden, können Benutzerkonten und Anmeldeinformationen aus der Domäne zum Anmelden und Verwalten von Servern verwendet werden. Gruppenmitgliedschaften aus der durch Azure AD DS verwalteten Domäne werden ebenfalls angewendet, damit Sie den Zugriff auf Dateien oder Dienste auf der VM steuern können.

In diesem Artikel wird gezeigt, wie Sie eine RHEL-VM (Red Hat Enterprise Linux) in eine durch Azure AD DS verwaltete Domäne einbinden.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die in Ihrem Azure AD-Mandanten aktiviert und konfiguriert ist.
    * Falls Sie keine solche Domäne haben, gehen Sie wie im ersten Tutorial beschrieben vor, um eine [Azure Active Directory Domain Services-Instanz zu erstellen und zu konfigurieren][create-azure-ad-ds-instance].
* Ein Benutzerkonto, das Teil der von Azure AD DS verwalteten Domäne ist.

## <a name="create-and-connect-to-a-rhel-linux-vm"></a>Erstellen einer RHEL-VM und Herstellen einer Verbindung

Wenn Sie über eine vorhandene RHEL-VM in Azure verfügen, stellen Sie über SSH eine Verbindung mit dieser VM her. Dann fahren Sie mit dem nächsten Schritt fort und beginnen mit der [Konfiguration der VM](#configure-the-hosts-file).

Wenn Sie eine RHEL-VM erstellen müssen oder eine Test-VM zur Verwendung mit diesem Artikel erstellen möchten, können Sie eine der folgenden Methoden verwenden:

* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure-Befehlszeilenschnittstelle](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Achten Sie beim Erstellen der VM auf die Einstellungen des virtuellen Netzwerks, um sicherzustellen, dass die VM mit der durch Azure AD DS verwalteten Domäne kommunizieren kann:

* Stellen Sie die VM in demselben oder einem mittels Peering verbundenen virtuellen Netzwerk bereit, in dem Sie Azure AD Domain Services aktiviert haben.
* Stellen Sie die VM in einem anderen Subnetz als die Azure AD Domain Services-Instanz bereit.

Nachdem die VM bereitgestellt wurde, führen Sie die Schritte zum Herstellen einer Verbindung mit der VM über SSH aus.

## <a name="configure-the-hosts-file"></a>Konfigurieren der Datei „hosts“

Um sicherzustellen, dass der VM-Hostname ordnungsgemäß für die verwaltete Domäne konfiguriert ist, bearbeiten Sie die Datei */etc/hosts*, und legen Sie den Hostnamen fest:

```console
sudo vi /etc/hosts
```

Aktualisieren Sie in der Datei *hosts* die Adresse *localhost*. Siehe folgendes Beispiel:

* *aaddscontoso.com* ist der DNS-Domänenname Ihrer verwalteten Azure AD DS-Domäne.
* *rhel* ist der Hostname Ihrer RHEL-VM, die Sie in die verwaltete Domäne einbinden.

Aktualisieren Sie diese Namen mit Ihren eigenen Werten:

```console
127.0.0.1 rhel rhel.aaddscontoso.com
```

Anschließend speichern und beenden Sie die Datei *hosts* mit dem Befehl `:wq` im Editor.

## <a name="install-required-packages"></a>Installieren erforderlicher Pakete

Die VM benötigt einige zusätzliche Pakete, damit sie in die durch Azure AD DS verwalteten Domäne eingebunden werden kann. Zum Installieren und Konfigurieren dieser Pakete aktualisieren und installieren Sie die Tools zum Einbinden in eine Domäne mit `yum`. Weil es einige Unterschiede zwischen RHEL 7.x und RHEL 6.x gibt, verwenden Sie in den restlichen Abschnitten dieses Artikels die entsprechenden Befehle für Ihre Distribution-Version.

**RHEL 7**

```console
sudo yum install realmd sssd krb5-workstation krb5-libs oddjob oddjob-mkhomedir samba-common-tools
```

**RHEL 6**

```console
sudo yum install adcli sssd authconfig krb5-workstation
```

## <a name="join-vm-to-the-managed-domain"></a>Einbinden der VM in die verwaltete Domäne

Nachdem Sie die erforderlichen Pakete auf der VM installiert haben, binden Sie die VM in die durch Azure AD DS verwaltete Domäne ein. Verwenden Sie dazu erneut die entsprechenden Schritte für Ihre RHEL-Distribution-Version.

### <a name="rhel-7"></a>RHEL 7

1. Verwenden Sie den Befehl `realm discover`, um die durch Azure AD DS verwaltete Domäne zu ermitteln. Im folgenden Beispiel wird der Bereich *AADDSCONTOSO.COM* erkannt. Geben Sie den Namen Ihrer eigenen durch Azure AD DS verwalteten Domäne in Großbuchstaben an:

    ```console
    sudo realm discover AADDSCONTOSO.COM
    ```

   Wenn mit dem Befehl `realm discover` Ihre durch Azure AD DS verwaltete Domäne nicht gefunden werden kann, führen Sie die folgenden Schritte zur Problembehandlung aus:

    * Vergewissern Sie sich, dass die VM die Domäne erreichen kann. Versuchen Sie `ping aaddscontoso.com`, um zu überprüfen, ob eine positive Antwort zurückgegeben wird.
    * Überprüfen Sie, ob die VM in demselben oder einem mittels Peering verbundenen virtuellen Netzwerk bereitgestellt wurde, in dem die durch Azure AD DS verwaltete Domäne verfügbar ist.
    * Stellen Sie sicher, dass die DNS-Servereinstellungen für das virtuelle Netzwerk so aktualisiert wurden, dass auf die Domänencontroller der durch Azure AD DS verwalteten Domäne verwiesen wird.

1. Initialisieren Sie nun Kerberos mit dem Befehl `kinit`. Geben Sie einen Benutzer an, der Teil der von Azure AD DS verwalteten Domäne ist. Bei Bedarf [fügen Sie ein Benutzerkonto zu einer Gruppe in Azure AD hinzu](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md).

    Auch hier muss der Name der durch Azure AD DS verwalteten Domäne in Großbuchstaben eingegeben werden. Im folgenden Beispiel wird das Konto mit dem Namen `contosoadmin@aaddscontoso.com` zum Initialisieren von Kerberos verwendet. Geben Sie Ihr eigenes Benutzerkonto ein, das Teil der von Azure AD DS verwalteten Domäne ist:

    ```console
    kinit contosoadmin@AADDSCONTOSO.COM
    ```

1. Zum Schluss binden Sie den Computer mit dem Befehl `realm join` in die durch Azure AD DS verwaltete Domäne ein. Verwenden Sie dasselbe Benutzerkonto, das Teil der von Azure AD DS verwalteten Domäne ist, das Sie im vorherigen Befehl `kinit` angegeben haben, z. B. `contosoadmin@AADDSCONTOSO.COM`:

    ```console
    sudo realm join --verbose AADDSCONTOSO.COM -U 'contosoadmin@AADDSCONTOSO.COM'
    ```

Das Einbinden der VM in die durch Azure AD DS verwaltete Domäne dauert einen Moment. Die folgende Beispielausgabe zeigt, dass die VM erfolgreich in die durch Azure AD DS verwaltete Domäne eingebunden wurde:

```output
Successfully enrolled machine in realm
```

### <a name="rhel-6"></a>RHEL 6

1. Verwenden Sie den Befehl `adcli info`, um die durch Azure AD DS verwaltete Domäne zu ermitteln. Im folgenden Beispiel wird der Bereich *AADDSCONTOSO.COM* erkannt. Geben Sie den Namen Ihrer eigenen durch Azure AD DS verwalteten Domäne in Großbuchstaben an:

    ```console
    sudo adcli info aaddscontoso.com
    ```

   Wenn mit dem Befehl `adcli info` Ihre durch Azure AD DS verwaltete Domäne nicht gefunden werden kann, führen Sie die folgenden Schritte zur Problembehandlung aus:

    * Vergewissern Sie sich, dass die VM die Domäne erreichen kann. Versuchen Sie `ping aaddscontoso.com`, um zu überprüfen, ob eine positive Antwort zurückgegeben wird.
    * Überprüfen Sie, ob die VM in demselben oder einem mittels Peering verbundenen virtuellen Netzwerk bereitgestellt wurde, in dem die durch Azure AD DS verwaltete Domäne verfügbar ist.
    * Stellen Sie sicher, dass die DNS-Servereinstellungen für das virtuelle Netzwerk so aktualisiert wurden, dass auf die Domänencontroller der durch Azure AD DS verwalteten Domäne verwiesen wird.

1. Fügen Sie zunächst die Domäne mit dem Befehl `adcli join` hinzu. Mit diesem Befehl wird auch die Schlüsseltabelle zur Authentifizierung des Computers erstellt. Verwenden Sie ein Benutzerkonto, das Teil der von Azure AD DS verwalteten Domäne ist.

    ```console
    sudo adcli join aaddscontoso.com -U contosoadmin
    ```

1. Konfigurieren Sie nun die `/ect/krb5.conf`, und erstellen Sie die `/etc/sssd/sssd.conf`-Dateien, um die `aaddscontoso.com` Active Directory-Domäne zu verwenden.
   Stellen Sie sicher, dass `AADDSCONTOSO.COM` durch Ihren eigenen Domänennamen ersetzt wird:

    Öffnen Sie die Datei `/ect/krb5.conf` in einem Editor:

    ```console
    sudo vi /etc/krb5.conf
    ```

    Aktualisieren Sie die Datei `krb5.conf` entsprechend dem folgenden Beispiel:

    ```console
    [logging]
     default = FILE:/var/log/krb5libs.log
     kdc = FILE:/var/log/krb5kdc.log
     admin_server = FILE:/var/log/kadmind.log
    
    [libdefaults]
     default_realm = AADDSCONTOSO.COM
     dns_lookup_realm = true
     dns_lookup_kdc = true
     ticket_lifetime = 24h
     renew_lifetime = 7d
     forwardable = true
    
    [realms]
     AADDSCONTOSO.COM = {
     kdc = AADDSCONTOSO.COM
     admin_server = AADDSCONTOSO.COM
     }
    
    [domain_realm]
     .AADDSCONTOSO.COM = AADDSCONTOSO.COM
     AADDSCONTOSO.COM = AADDSCONTOSO.COM
    ```
    
   Erstellen Sie die Datei `/etc/sssd/sssd.conf`:
    
    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

    Aktualisieren Sie die Datei `sssd.conf` entsprechend dem folgenden Beispiel:

    ```console
    [sssd]
     services = nss, pam, ssh, autofs
     config_file_version = 2
     domains = AADDSCONTOSO.COM
    
    [domain/AADDSCONTOSO.COM]
    
     id_provider = ad
    ```

1. Stellen Sie sicher, dass die `/etc/sssd/sssd.conf`-Berechtigungen 600 lauten und dass die Datei im Besitz des Root-Benutzers ist:

    ```console
    sudo chmod 600 /etc/sssd/sssd.conf
    sudo chown root:root /etc/sssd/sssd.conf
    ```

1. Verwenden Sie `authconfig`, um die VM über die AD Linux-Integration anzuweisen:

    ```console
    sudo authconfig --enablesssd --enablesssdauth --update
    ```

1. Starten und aktivieren Sie den SSSD-Dienst:

    ```console
    sudo service sssd start
    sudo chkconfig sssd on
    ```

Wenn für die VM der Prozess der Einbindung in die Domäne nicht erfolgreich abgeschlossen werden kann, stellen Sie sicher, dass die Netzwerksicherheitsgruppe der VM ausgehenden Kerberos-Datenverkehr über TCP und UDP-Port 464 an das Subnetz des virtuellen Netzwerks für Ihre durch Azure AD DS verwaltete Domäne zulässt.

Überprüfen Sie nun, ob Sie mithilfe von `getent` Benutzerinformationen abfragen können

```console
sudo getent passwd contosoadmin
```

## <a name="allow-password-authentication-for-ssh"></a>Zulassen der Kennwortauthentifizierung für SSH

Standardmäßig können sich Benutzer nur mithilfe der auf einen öffentlichen Schlüssel basierenden SSH-Authentifizierung bei einer VM anmelden. Die kennwortbasierte Authentifizierung schlägt fehl. Wenn Sie die VM in eine durch Azure AD DS verwalteten Domäne einbinden, müssen diese Domänenkonten die kennwortbasierte Authentifizierung verwenden. Aktualisieren Sie die SSH-Konfiguration wie folgt, um die kennwortbasierte Authentifizierung zuzulassen.

1. Öffnen Sie die Datei *sshd_conf* mit einem Editor:

    ```console
    sudo vi /etc/ssh/sshd_config
    ```

1. Aktualisieren Sie die Zeile für *PasswordAuthentication* in *yes*:

    ```console
    PasswordAuthentication yes
    ```

    Anschließend speichern und beenden Sie die Datei *sshd_conf* mit dem Befehl `:wq` im Editor.

1. Starten Sie den SSH-Dienst für Ihre RHEL-Distribution-Version neu, um die Änderungen zu übernehmen und Benutzern das Anmelden mit einem Kennwort zu ermöglichen:

   **RHEL 7**

    ```console
    sudo systemctl restart sshd
    ```

   **RHEL 6**

    ```console
    sudo service sshd restart
    ```

## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>Erteilen von sudo-Berechtigungen für die Gruppe „AAD DC-Administratoren“

Um Mitgliedern der Gruppe *AAD DC-Administratoren* Administratorrechte für die RHEL-VM zu erteilen, fügen Sie */etc/sudoers* einen Eintrag hinzu. Nach dem Hinzufügen können Mitglieder der Gruppe *AAD DC-Administratoren* den Befehl `sudo` auf der RHEL-VM verwenden.

1. Öffnen Sie die Datei *sudoers* zur Bearbeitung:

    ```console
    sudo visudo
    ```

1. Fügen Sie den folgenden Eintrag am Ende der Datei */etc/sudoers* hinzu. Die Gruppe *AAD DC-Administratoren* enthält Leerzeichen im Namen. Fügen Sie deshalb den umgekehrten Schrägstrich als Escapezeichen in den Gruppennamen ein. Fügen Sie den Namen Ihrer Domäne (z. B. *aaddscontoso.com*) hinzu:

    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators@aaddscontoso.com ALL=(ALL) NOPASSWD:ALL
    ```

    Anschließend speichern und beenden Sie den Editor mit dem Befehl `:wq` im Editor.

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Anmelden bei der VM mit einem Domänenkonto

Um zu überprüfen, ob die VM erfolgreich in die durch Azure AD DS verwaltete Domäne eingebunden wurde, starten Sie eine neue SSH-Verbindung mithilfe eines Domänenbenutzerkontos. Vergewissern Sie sich, dass ein Basisverzeichnis erstellt wurde und die Gruppenmitgliedschaft aus der Domäne angewendet wird.

1. Erstellen Sie eine neue SSH-Verbindung über die Konsole. Verwenden Sie mithilfe des Befehls `ssh -l` ein Domänenkonto, das der verwalteten Domäne angehört (z. B. `contosoadmin@aaddscontoso.com`), und geben Sie dann die Adresse Ihres virtuellen Computers (z. B. *rhel.aaddscontoso.com*) ein. Bei Verwendung von Azure Cloud Shell verwenden Sie die öffentliche IP-Adresse der VM anstelle des internen DNS-Namens.

    ```console
    ssh -l contosoadmin@AADDSCONTOSO.com rhel.aaddscontoso.com
    ```

1. Wenn Sie erfolgreich eine Verbindung mit der VM hergestellt haben, vergewissern Sie sich, dass das Basisverzeichnis ordnungsgemäß initialisiert wurde:

    ```console
    pwd
    ```

    Sie sollten sich mit Ihrem eigenen Verzeichnis, das dem Benutzerkonto entspricht, im Verzeichnis */home* befinden.

1. Überprüfen Sie nun, ob die Gruppenmitgliedschaften ordnungsgemäß aufgelöst werden:

    ```console
    id
    ```

    Es sollten Ihre Gruppenmitgliedschaften aus der durch Azure AD DS verwalteten Domäne angezeigt werden.

1. Wenn Sie sich als Mitglied der Gruppe *AAD DC-Administratoren* bei der VM angemeldet haben, überprüfen Sie, ob Sie den Befehl `sudo` ordnungsgemäß verwenden können:

    ```console
    sudo yum update
    ```

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Herstellen einer Verbindung der VM mit der durch Azure AD DS verwalteten Domäne oder bei der Anmeldung mit einem Domänenkonto Probleme auftreten, finden Sie weitere Informationen unter [Behandeln von Problemen mit dem Einbinden in eine Domäne](join-windows-vm.md#troubleshoot-domain-join-issues).

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md

---
title: Konfigurieren eines NFS-Clients für Azure NetApp Files | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie NFS-Clients zur Verwendung mit Azure NetApp Files konfigurieren.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/17/2021
ms.author: b-juche
ms.openlocfilehash: effca5e663f91489bc534934d26faec8c18e7460
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110090621"
---
# <a name="configure-an-nfs-client-for-azure-netapp-files"></a>Konfigurieren eines NFS-Clients für Azure NetApp Files

Die in diesem Artikel beschriebene NFS-Clientkonfiguration ist Teil des Setups, wenn Sie die [NFSv4.1 Kerberos-Verschlüsselung konfigurieren](configure-kerberos-encryption.md) oder ein [Dual-Protokoll-Volume erstellen](create-volumes-dual-protocol.md). Für die Verwendung mit Azure NetApp Files steht eine Vielzahl von Linux-Distributionen zur Verfügung. In diesem Artikel werden die Konfigurationen für zwei der gängigsten Umgebungen beschrieben: RHEL 8 und Ubuntu 18.04. 

## <a name="requirements-and-considerations"></a>Anforderungen und Überlegungen  

Unabhängig davon, welche Linux-Variante Sie verwenden, sind die folgenden Konfigurationen erforderlich:

* Konfigurieren Sie einen NTP-Client, um Probleme mit der Zeitabweichung zu vermeiden.
* Konfigurieren Sie DNS-Einträge des Linux-Clients für die Namensauflösung.  
    Diese Konfiguration muss den A-Eintrag (vorwärts) und einen PTR-Eintrag (rückwärts) umfassen. 
* Erstellen Sie für den Domänenbeitritt ein Computerkonto für den Linux-Client in der Active Directory-Zielinstanz (die beim Befehl „realm join“ erstellt wird). 
    > [!NOTE] 
    > Die in den folgenden Befehlen verwendete Variable `$SERVICEACCOUNT` sollte ein Benutzerkonto mit Berechtigungen oder Delegierung zum Erstellen eines Computerkontos in der Zielorganisationseinheit sein.

## <a name="rhel-8-configuration"></a>RHEL 8-Konfiguration 

In diesem Abschnitt werden RHEL-Konfigurationen beschrieben, die für die NFSv4.1 Kerberos-Verschlüsselung und das duale Protokoll erforderlich sind.  

Die Beispiele in diesem Abschnitt verwenden den folgenden Domänennamen und die folgende IP-Adresse:  

* Domänenname: `contoso.com`
* Private IP-Adresse: `10.6.1.4`

### <a name="rhel-8-configuration-if-you-are-using-nfsv41-kerberos-encryption"></a><a name="rhel8_nfsv41_kerberos"></a>RHEL 8-Konfiguration, wenn Sie NFSv4.1 Kerberos-Verschlüsselung verwenden

1. Konfigurieren Sie `/etc/resolv.conf` mit dem richtigen DNS-Server.  

    Zum Beispiel:  

    `[root@reddoc cbs]# cat /etc/resolv.conf`   
    `search contoso.com`   
    `nameserver 10.6.1.4(private IP)`   

2. Fügen Sie den NFS-Clienteintrag im DNS-Server für die DNS-Forward- und DNS-Reverse-Lookupzone hinzu.

3. Verwenden Sie zum Überprüfen von DNS die folgenden Befehle über den NFS-Client:   

    `# nslookup [hostname/FQDN of NFS client(s)]`   
    `# nslookup [IP address of NFS client(s)]`

4. Installieren der Pakete:   

    `yum update`   
    `sudo yum -y install realmd sssd adcli samba-common krb5-workstation chrony nfs-utils`

5.  Konfigurieren des NTP-Clients:  

    RHEL 8 verwendet standardmäßig chrony. Befolgen Sie die Konfigurationsrichtlinien unter [Using the `Chrony` suite to configure NTP](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/using-chrony-to-configure-ntp) (Verwenden der Chrony-Suite für die NTP-Konfiguration).

6.  Verknüpfen mit der Active Directory-Domäne:  

    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou="OU=$YOUROU"`

    Zum Beispiel: 

    `sudo realm join CONTOSO.COM -U ad_admin --computer-ou="CN=Computers"`
    
    Stellen Sie sicher, dass `default_realm` auf den bereitgestellten Bereich in `/etc/krb5.conf` festgelegt ist.  Andernfalls fügen Sie den Bereich unter dem Abschnitt `[libdefaults]` in der Datei ein, wie im folgenden Beispiel gezeigt:
    
    ```
    [libdefaults]
        default_realm = CONTOSO.COM
        default_tkt_enctypes = aes256-cts-hmac-sha1-96
        default_tgs_enctypes = aes256-cts-hmac-sha1-96
        permitted_enctypes = aes256-cts-hmac-sha1-96
    [realms]
        CONTOSO.COM = {
            kdc = dc01.contoso.com
            admin_server = dc01.contoso.com
            master_kdc = dc01.contoso.com
            default_domain = contoso.com
        }
    [domain_realm]
        .contoso.com = CONTOSO.COM
        contoso.com = CONTOSO.COM
    [logging]
        kdc = SYSLOG:INFO
        admin_server = FILE=/var/kadm5.log
    ```

7. Alle NFS-Dienste neu starten:  
 
    `systemctl start nfs-*`   
    `systemctl restart rpc-gssd.service`

    Der Neustart verhindert, dass die Fehlerbedingung `“mount.nfs: an incorrect mount option was specified”` während der Kerberos-Einbindung auftritt.

8. Führen Sie den Befehl `kinit` mit dem Benutzerkonto aus, um Tickets abzurufen: 
 
    `sudo kinit $SERVICEACCOUNT@DOMAIN`  

    Zum Beispiel:   

    `sudo kinit ad_admin@CONTOSO.COM`


### <a name="rhel-8-configuration-if-you-are-using-dual-protocol"></a>RHEL 8-Konfiguration, wenn Sie das duale Protokoll verwenden.

Die folgenden Schritte sind optional. Sie müssen die Schritte nur durchführen, wenn Sie die Benutzerzuordnung beim NFS-Client verwenden: 

1. Führen Sie alle im Abschnitt [RHEL 8-Konfiguration, wenn Sie NFSv4.1 Kerberos-Verschlüsselung verwenden](#rhel8_nfsv41_kerberos) beschriebenen Schritte aus.   

2. Fügen Sie einen statischen DNS-Eintrag in Ihrer „/etc/hosts“-Datei hinzu, um den vollqualifizierten Domänennamen (FQDN) für Ihr AD zu verwenden, anstatt die IP-Adresse in der SSSD-Konfigurationsdatei zu verwenden:  

    `cat /etc/hosts`   
    `10.6.1.4 winad2016.contoso.com`

3. Fügen Sie einen zusätzlichen Abschnitt für Domänen hinzu, um Bezeichner vom AD LDAP-Server aufzulösen:    

    `[root@reddoc cbs]# cat /etc/sssd/sssd.conf`   
    `[sssd]`   
    `domains = contoso.com, contoso-ldap (new entry added for LDAP as id_provider)`   
    `config_file_version = 2`   
    `services = nss, pam, ssh, sudo (ensure nss is present in this list)`   
 
    `[domain/contoso-ldap] (Copy the following lines. Modify as per your domain name.)`   
    `auth_provider = krb5`   
    `chpass_provider = krb5`   
    `id_provider = ldap`   
    `ldap_search_base = dc=contoso,dc=com(your domain)`   
    `ldap_schema = rfc2307bis`   
    `ldap_sasl_mech = GSSAPI`   
    `ldap_user_object_class = user`   
    `ldap_group_object_class = group`   
    `ldap_user_home_directory = unixHomeDirectory`   
    `ldap_user_principal = userPrincipalName`   
    `ldap_account_expire_policy = ad`   
    `ldap_force_upper_case_realm = true`   
    `ldap_user_search_base = cn=Users,dc=contoso,dc=com (based on your domain)`   
    `ldap_group_search_base = cn=Users,dc=contoso,dc=com (based on your domain)`   
    `ldap_sasl_authid = REDDOC$ (ensure $ at the end you can get this from “klist -kte” command)`   
    `krb5_server = winad2016.contoso.com (same as AD address which is added in /etc/hosts)`   
    `krb5_realm = CONTOSO.COM (domain name in caps)`   
    `krb5_kpasswd = winad2016.contoso.com (same as AD address which is added in /etc/hosts)`   
    `use_fully_qualified_names = false`   
 
    `[domain/contoso.com]  (Do not edit or remove any of the following information. This information is automatically generated during the realm join process.)`   
    `ad_domain = contoso.com`   
    `krb5_realm = CONTOSO.COM`   
    `realmd_tags = manages-system joined-with-adcli`   
    `cache_credentials = True`   
    `id_provider = ad`   
    `krb5_store_password_if_offline = True`   
    `default_shell = /bin/bash`   
    `ldap_id_mapping = True`   
    `use_fully_qualified_names = True`   
    `fallback_homedir = /home/%u@%d`   
    `access_provider = ad`   

4. Stellen Sie sicher, dass Ihre `/etc/nsswitch.conf` den Eintrag `sss` enthält:   

    `cat /etc/nsswitch.conf`   
    `passwd: sss files systemd`   
    `group: sss files systemd`   
    `netgroup: sss files`   

5. Starten Sie den `sssd` Dienst neu, und löschen Sie den Cache:   

    `service sssd stop`   
    `rm -f /var/lib/sss/db/*`   
    `service sssd start`   
 
6. Testen Sie, um sicherzustellen, dass Ihr Client in den LDAP-Server integriert ist:   

    `[root@red81 cbs]# id ldapuser1`   
    `uid=1234(ldapuser1) gid=1111(ldapgroup1) groups=1111(ldapgroup1)`   

## <a name="ubuntu-configuration"></a>Ubuntu-Konfiguration   

In diesem Abschnitt werden Ubuntu-Konfigurationen beschrieben, die für die NFSv4.1 Kerberos-Verschlüsselung und das duale Protokoll erforderlich sind. 

Die Beispiele in diesem Abschnitt verwenden den folgenden Domänennamen und die folgende IP-Adresse:  

* Domänenname: `contoso.com`
* Private IP-Adresse: `10.6.1.4`

1. Konfigurieren Sie `/etc/resolv.conf` mit dem richtigen DNS-Server:

    `root@ubuntu-rak:/home/cbs# cat /etc/resolv.conf`   
    `search contoso.com`   
    `nameserver <private IP address of DNS server>`   

2. Fügen Sie den NFS-Clienteintrag im DNS-Server für die DNS-Forward- und DNS-Reverse-Lookupzone hinzu.
 
    Verwenden Sie zum Überprüfen von DNS die folgenden Befehle über den NFS-Client:

    `# nslookup [hostname/FQDN of NFS client(s)]`   
    `# nslookup [IP address of NFS client(s)]`   

3. Installieren der Pakete:
 
    `apt-get update`   
    `apt-get install -y realmd packagekit sssd adcli samba-common chrony krb5-user nfs-common`
    
    Wenn Sie dazu aufgefordert werden, geben Sie `$DOMAIN.NAME` (in Großbuchstaben, z. B. `CONTOSO.COM`) als standardmäßigen Kerberos-Bereich ein.

4. Starten Sie den Dienst `rpc-gssd.service` neu: 

    `sudo systemctl start rpc-gssd.service`

5. Ubuntu 18.04 verwendet standardmäßig chrony. Befolgen Sie die Konfigurationsrichtlinien unter [Ubuntu Bionic: Using Chrony to configure NTP](https://ubuntu.com/blog/ubuntu-bionic-using-chrony-to-configure-ntp) (Ubuntu Bionic: Verwenden von Chrony für die NTP-Konfiguration).

6. Verknüpfen mit der Active Directory-Domäne:   
 
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou="OU=$YOUROU"`
 
    Zum Beispiel:    
    `sudo realm join CONTOSO.COM -U ad_admin --computer-ou="CN=Computers"`

7. Führen Sie `kinit` mit dem Benutzer aus, um Tickets abzurufen: 
 
    `sudo kinit $SERVICEACCOUNT`   
 
    Zum Beispiel:    
    `sudo kinit ad_admin`  

### <a name="ubuntu-configuration-if-you-are-using-dual-protocol"></a>Ubuntu-Konfiguration, wenn Sie das duale Protokoll verwenden.  

Die folgenden Schritte sind optional.  Sie müssen die Schritte nur durchführen, wenn Sie die Benutzerzuordnung beim NFS-Client verwenden möchten:  

1. Führen Sie den folgenden Befehl aus, um das installierte Paket zu upgraden:   
    `sudo apt update && sudo apt install libnss-ldap libpam-ldap ldap-utils nscd`

    Das folgende Beispiel verwendet Beispielwerte. Wenn der Befehl Sie zur Eingabe auffordert, sollten Sie Eingaben auf der Grundlage Ihrer Umgebung machen. 

    `base dc=contoso,dc=com uri ldap://10.20.0.4:389/ ldap_version 3 rootbinddn cn=admin,cn=Users,dc=contoso,dc=com pam_password ad`   

2. Führen Sie den folgenden Befehl aus, um den Dienst neu zu starten und zu aktivieren:

    `sudo systemctl restart nscd && sudo systemctl enable nscd`   

Im folgenden Beispiel wird der AD LDAP-Server vom Ubuntu LDAP-Client nach dem LDAP-Benutzer `‘hari1’` abgefragt: 

`root@cbs-k8s-varun4-04:/home/cbs# getent passwd hari1`   
`hari1:*:1237:1237:hari1:/home/hari1:/bin/bash`   

## <a name="configure-two-vms-with-the-same-hostname-to-access-nfsv41-volumes"></a>Konfigurieren von zwei VMs mit demselben Hostnamen für den Zugriff auf NFSv4.1-Volumes 

In diesem Abschnitt wird erläutert, wie Sie zwei VMs mit demselben Hostnamen für den Zugriff auf Azure NetApp Files NFSv4.1-Volumes konfigurieren können. Dieses Verfahren kann nützlich sein, wenn Sie einen Notfallwiederherstellungstest durchführen und ein Testsystem mit demselben Hostnamen wie das primäre Notfallwiederherstellungssystem benötigen. Dieses Verfahren ist nur erforderlich, wenn Sie denselben Hostnamen auf zwei VMs verwenden, die auf dieselben Azure NetApp Files-Volumes zugreifen.  

NFSv4.x erfordert, dass sich jeder Client bei Servern mit einer *eindeutigen* Zeichenfolge identifiziert. Der Geöffnet- und Sperrzustand von Dateien, der zwischen einem Client und einem Server geteilt wird, ist dieser Identität zugeordnet. Um eine robuste NFSv4.x-Zustandswiederherstellung und eine transparente Zustandsmigration zu unterstützen, darf sich diese Identitätszeichenfolge nicht nach Clientneustarts ändern.

1. Sie können die `nfs4_unique_id`-Zeichenfolge mit dem folgenden Befehl auf den VM-Clients anzeigen:
    
    `# systool -v -m nfs | grep -i nfs4_unique`     
    `    nfs4_unique_id      = ""`

    Um dasselbe Volume auf einem zusätzlichen virtuellen Computer mit demselben Hostnamen einzubinden, z. B. dem Notfallwiederherstellungssystem, erstellen Sie eine `nfs4_unique_id`, damit es sich eindeutig beim NFS-Dienst von Azure NetApp Files identifizieren kann.  Mit diesem Schritt kann der Dienst zwischen den beiden virtuellen Computern mit demselben Hostnamen unterscheiden und das Einbinden von NFSv4.1-Volumes auf beiden VMs ermöglichen.  

    Sie müssen diesen Schritt nur auf dem Notfallwiederherstellungstest-System ausführen. Aus Gründen der Konsistenz können Sie erwägen, eine eindeutige Einstellung auf jeden beteiligten virtuellen Computer anzuwenden.

2. Fügen Sie auf dem Notfallwiederherstellungstest-System die folgende Zeile in der Datei `nfsclient.conf` hinzu, die sich in der Regel in `/etc/modprobe.d/` befindet:

    `options nfs nfs4_unique_id=uniquenfs4-1`  

    Die Zeichenfolge `uniquenfs4-1` kann eine beliebige alphanumerische Zeichenfolge sein, solange sie über die VMs hinweg eindeutig ist, die mit dem Dienst verbunden werden sollen.

    Informationen zum Konfigurieren von NFS-Clienteinstellungen finden Sie in der Dokumentation Ihrer Distribution.

    Starten Sie den virtuellen Computer neu, damit die Änderung wirksam wird.

3. Überprüfen Sie auf dem Notfallwiederherstellungstest-System, ob `nfs4_unique_id` nach dem Neustart des virtuellen Computers festgelegt wurde:       

    `# systool -v -m nfs | grep -i nfs4_unique`   
    `   nfs4_unique_id      = "uniquenfs4-1"`   

4. [Binden Sie das NFSv4.1-Volume](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) ganz normal auf beiden VMs ein.

    Beide VMs mit demselben Hostnamen können das NFSv4.1-Volume jetzt einbinden und darauf zugreifen.  

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen eines NFS-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Erstellen eines Volumes mit dualem Protokoll für Azure NetApp Files](create-volumes-dual-protocol.md)
* [Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) 
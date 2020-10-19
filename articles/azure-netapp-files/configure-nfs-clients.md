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
ms.date: 09/28/2020
ms.author: b-juche
ms.openlocfilehash: b2e597ff8fc761b66de6228063c471933a364144
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91449650"
---
# <a name="configure-an-nfs-client-for-azure-netapp-files"></a>Konfigurieren eines NFS-Clients für Azure NetApp Files

Die in diesem Artikel beschriebene NFS-Clientkonfiguration ist Teil des Setups, wenn Sie die [NFSv4.1 Kerberos-Verschlüsselung konfigurieren](configure-kerberos-encryption.md) oder ein [Dual-Protokoll-Volume erstellen](create-volumes-dual-protocol.md). Für die Verwendung mit Azure NetApp Files steht eine Vielzahl von Linux-Distributionen zur Verfügung. In diesem Artikel werden die Konfigurationen für zwei der gängigsten Umgebungen beschrieben: RHEL 8 und Ubuntu 18.04. 

Unabhängig davon, welche Linux-Variante Sie verwenden, sind die folgenden Konfigurationen erforderlich:
* Konfigurieren Sie einen NTP-Client, um Probleme mit der Zeitabweichung zu vermeiden.
* Konfigurieren Sie DNS-Einträge des Linux-Clients für die Namensauflösung.  
    Diese Konfiguration umfasst einen A-Eintrag (vorwärts) und einen PTR-Eintrag (rückwärts). 
* Erstellen Sie für den Domänenbeitritt ein Computerkonto in der Active Directory-Zielinstanz (die beim Befehl „realm join“ erstellt wird). 
    > [!NOTE] 
    > Die in den folgenden Befehlen verwendete Variable `$SERVICEACCOUNT` sollte ein Benutzerkonto mit Berechtigungen oder Delegierung zum Erstellen eines Computerkontos in der Zielorganisationseinheit sein.
* Aktivieren Sie den Client für die Einbindung von NFS-Volumes und anderen relevanten Überwachungstools.

## <a name="rhel-8-configuration"></a>RHEL 8-Konfiguration 

1. Installieren der Pakete:   
    `sudo yum -y install realmd sssd adcli samba-common krb5-workstation chrony`

2. Konfigurieren des NTP-Clients:  
    Unter RHEL 8 wird standardmäßig `chrony` verwendet.  Befolgen Sie die Konfigurationsrichtlinien unter [Using the Chrony suite to configure NTP](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/using-chrony-to-configure-ntp) (Verwenden der Chrony-Suite für die NTP-Konfiguration).

3. Verknüpfen mit der Active Directory-Domäne:  
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou= OU=$YOUROU,DC=$DOMAIN,DC=TLD`

## <a name="ubuntu-configuration"></a>Ubuntu-Konfiguration 
In diesem Abschnitt wird die Ubuntu-Konfiguration für die NFS-Clients beschrieben.  

### <a name="if-you-are-using-nfsv41-kerberos-encryption"></a>Wenn Sie NFSv4.1-Kerberos-Verschlüsselung verwenden 

1. Installieren der Pakete:  
    `sudo yum -y install realmd packagekit sssd adcli samba-common krb5-workstation chrony`

2. Konfigurieren des NTP-Clients:  
    Unter Ubuntu 18.04 wird standardmäßig `chrony` verwendet.  Befolgen Sie die Konfigurationsrichtlinien unter [Ubuntu Bionic: Using Chrony to configure NTP](https://ubuntu.com/blog/ubuntu-bionic-using-chrony-to-configure-ntp) (Ubuntu Bionic: Verwenden von Chrony für die NTP-Konfiguration).

3. Verknüpfen mit der Active Directory-Domäne:  
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou= OU=$YOUROU,DC=$DOMAIN,DC=TLD`

### <a name="if-you-are-using-dual-protocol"></a>Wenn Sie zwei Protokolle verwenden  

1. Führen Sie den folgenden Befehl aus, um das installierte Paket zu upgraden:  
    `sudo apt update && sudo apt install libnss-ldap libpam-ldap ldap-utils nscd`

    Beispiel:   

    `base dc=hariscus,dc=com` `uri ldap://10.20.0.4:389/`
    `ldap_version 3`
    `rootbinddn cn=admin,cn=Users,dc=hariscus,dc=com`
    `pam_password ad`
 
2. Führen Sie den folgenden Befehl aus, um den Dienst neu zu starten und zu aktivieren:   
    `sudo systemctl restart nscd && sudo systemctl enable nscd`

Im folgenden Beispiel wird der AD LDAP-Server vom Ubuntu LDAP-Client nach dem LDAP-Benutzer `ldapu1` abgefragt:   

`root@cbs-k8s-varun4-04:/home/cbs# getent passwd hari1`   
`hari1:*:1237:1237:hari1:/home/hari1:/bin/bash`   

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen eines NFS-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Erstellen eines Volumes mit dualem Protokoll für Azure NetApp Files](create-volumes-dual-protocol.md)


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
ms.date: 07/27/2020
ms.author: b-juche
ms.openlocfilehash: bdcffcea8d695b6a3d49272ffa135187d77aae1e
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87535242"
---
# <a name="configure-an-nfs-client-for-azure-netapp-files"></a>Konfigurieren eines NFS-Clients für Azure NetApp Files

Für die Verwendung mit Azure NetApp Files steht eine Vielzahl von Linux-Distributionen zur Verfügung. In diesem Artikel werden die Konfigurationen für zwei der gängigsten Umgebungen beschrieben: RHEL 8 und Ubuntu 18.04.

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

1. Installieren der Pakete:  
    `sudo yum -y install realmd packagekit sssd adcli samba-common krb5-workstation chrony`

2. Konfigurieren des NTP-Clients:  
    Unter Ubuntu 18.04 wird standardmäßig `chrony` verwendet.  Befolgen Sie die Konfigurationsrichtlinien unter [Ubuntu Bionic: Using Chrony to configure NTP](https://ubuntu.com/blog/ubuntu-bionic-using-chrony-to-configure-ntp) (Ubuntu Bionic: Verwenden von Chrony für die NTP-Konfiguration).

3. Verknüpfen mit der Active Directory-Domäne:  
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou= OU=$YOUROU,DC=$DOMAIN,DC=TLD`

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen eines NFS-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Erstellen eines Volumes mit dualem Protokoll für Azure NetApp Files](create-volumes-dual-protocol.md)


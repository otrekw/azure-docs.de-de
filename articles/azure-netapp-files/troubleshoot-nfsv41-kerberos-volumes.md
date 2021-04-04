---
title: Problembehandlung bei NFSv4.1-Kerberos-Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: In diesem Artikel werden Fehlermeldungen und Lösungen beschrieben, mit denen Sie Probleme bei NFSv4.1-Kerberos-Volumes für Azure NetApp Files beheben können.
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
ms.topic: troubleshooting
ms.date: 01/12/2021
ms.author: b-juche
ms.openlocfilehash: 638607da02b1db4842cdc04f86a4fed1860c243f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98134312"
---
# <a name="troubleshoot-nfsv41-kerberos-volume-issues"></a>Problembehandlung bei NFSv4.1-Kerberos-Volumes 

In diesem Artikel werden Lösungen für Fehlerbedingungen beschrieben, die beim Erstellen oder Verwalten von NFSv4.1-Kerberos-Volumes auftreten können. 

## <a name="error-conditions-and-resolutions"></a>Fehlerbedingungen und Auflösungen

|     Fehlerbedingungen    |     Lösungen    |
|-|-|
|`Error allocating volume - Export policy rules does not match kerberosEnabled flag` | Azure NetApp Files bietet keine Unterstützung für Kerberos mit NFSv3-Volumes. Kerberos wird nur mit dem NFSv4.1-Protokoll unterstützt.  |
|`This NetApp account has no configured Active Directory   connections`  |  Konfigurieren Sie Active Directory für das NetApp-Konto mit den Feldern **KDC-IP-Adresse** und **AD-Servername**. Anweisungen finden Sie unter [Konfigurieren des Azure-Portals](configure-kerberos-encryption.md#configure-the-azure-portal). |
|`Mismatch between KerberosEnabled flag value and ExportPolicyRule's access type parameter values.`  | Azure NetApp Files bietet keine Unterstützung für das Umwandeln eines einfachen NFSv4.1-Volumes in ein NFSv4.1-Kerberos-Volume (und umgekehrt). |
|`mount.nfs: access denied by server when mounting volume <SMB_SERVER_NAME-XXX.DOMAIN_NAME>/<VOLUME_NAME>` <br>  Beispiel: `smb-test-64d9.contoso.com:/nfs41-vol101` | <ol><li> Stellen Sie sicher, dass die A-/PTR-Einträge ordnungsgemäß eingerichtet wurden und in Active Directory für den Servernamen `smb-test-64d9.contoso.com` vorhanden sind. <br> Wenn `nslookup` für `smb-test-64d9.contoso.com` im NFS-Client in die IP-Adresse „IP1“ (`10.1.1.68`) aufgelöst wird, darf `nslookup` von IP1 nur in einen Eintrag (`smb-test-64d9.contoso.com`) aufgelöst werden. `nslookup` von IP1 *darf nicht* in mehrere Namen aufgelöst werden. </li>  <li>Legen Sie „AES-256“ für das NFS-Computerkonto vom Typ `NFS-<Smb NETBIOS NAME>-<few random characters>` in AD mithilfe von PowerShell oder über die Benutzeroberfläche fest. <br> Beispielbefehle: <ul><li>`Set-ADComputer <NFS_MACHINE_ACCOUNT_NAME> -KerberosEncryptionType AES256` </li><li>`Set-ADComputer NFS-SMB-TEST-64 -KerberosEncryptionType AES256` </li></ul> </li> <li>Stellen Sie sicher, dass die Zeit von NFS-Client, AD und Azure NetApp Files-Speichersoftware miteinander synchronisiert wird und höchstens eine Abweichung von fünf Minuten aufweist. </li>  <li>Verwenden Sie den Befehl `kinit <administrator>`, um das Kerberos-Ticket auf dem NFS-Client abzurufen.</li> <li>Kürzen Sie den Hostnamen des NFS-Clients auf weniger als 15 Zeichen, und führen Sie den Bereichsjoin erneut aus. </li><li>Starten Sie den NFS-Client und den Dienst `rpcgssd` wie folgt neu. Der Befehl kann abhängig vom Betriebssystem variieren.<br> RHEL 7: <br> `service nfs restart` <br> `service rpcgssd restart` <br> CentOS 8: <br> `systemctl enable nfs-client.target && systemctl start nfs-client.target` <br> Ubuntu: <br> (Starten Sie den Dienst `rpc-gssd` neu.) <br> `sudo systemctl start rpc-gssd.service` </ul>| 
|`mount.nfs: an incorrect mount option was specified`   | Das Problem kann mit einem Problem mit dem NFS-Client zusammenhängen. Starten Sie den NFS-Client neu.    |
|`Hostname lookup failed`   | Sie müssen eine Reverse-Lookupzone auf dem DNS-Server erstellen und dann einen PTR-Eintrag des AD-Hostcomputers in dieser Reverse-Lookupzone hinzufügen. <br> Nehmen Sie beispielsweise an, dass die IP-Adresse des AD-Computers `10.1.1.4`, der Hostname des AD-Computers (wie mit dem Befehl „hostname“ gefunden) `AD1` und der Domänenname `contoso.com` lautet. Der PTR-Eintrag, der der Reverse-Lookupzone hinzugefügt wurde, sollte `10.1.1.4 -> AD1.contoso.com` sein. |
|`Volume creation fails due to unreachable DNS server`  | Zwei mögliche Lösungen: <br> <ul><li> Dieser Fehler weist darauf hin, dass das DNS nicht erreichbar ist. Der Grund ist möglicherweise eine falsche DNS-IP-Adresse oder ein Netzwerkproblem. Überprüfen Sie die in der AD-Verbindung eingegebene DNS-IP-Adresse, und vergewissern Sie sich, dass sie richtig ist. </li> <li> Stellen Sie sicher, dass sich AD und das Volume in derselben Region und im gleichen VNet befinden. Wenn sie sich in verschiedenen VNets befinden, stellen Sie sicher, dass das VNet-Peering zwischen den beiden VNets eingerichtet ist. </li></ul> |
|Bei der Erstellung von NFSv4.1-Kerberos-Volumes tritt ein Fehler auf, der dem im folgenden Beispiel ähnelt: <br> `Failed to enable NFS Kerberos on LIF "svm_e719cde8d6d0413fbd6adac0636cdecb_7ad0b82e_73349613". Failed to bind service principal name on LIF "svm_e719cde8d6d0413fbd6adac0636cdecb_7ad0b82e_73349613". SecD Error: server create fail join user auth.` |Die KDC-IP-Adresse ist falsch, und das Kerberos-Volume wurde erstellt. Korrigieren Sie die KDC-IP-Adresse. <br> Nachdem Sie die KDC-IP-Adresse aktualisiert haben, tritt der Fehler nicht mehr auf. Sie müssen das Volume neu erstellen. |

## <a name="next-steps"></a>Nächste Schritte  

* [Konfigurieren der NFSv4.1-Kerberos-Verschlüsselung für Azure NetApp Files](configure-kerberos-encryption.md)

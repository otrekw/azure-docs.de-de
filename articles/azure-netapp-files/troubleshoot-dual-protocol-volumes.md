---
title: Problembehandlung bei Volumes mit SMB oder dualem Protokoll für Azure NetApp Files | Microsoft-Dokumentation
description: In diesem Artikel werden Fehlermeldungen und Lösungen beschrieben, mit denen Sie Probleme mit SMB oder dem dualem Protokoll für Azure NetApp Files beheben können.
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
ms.date: 02/02/2021
ms.author: b-juche
ms.openlocfilehash: dbc9f466437a575866c33219ff11af2d85d9a58b
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100571079"
---
# <a name="troubleshoot-smb-or-dual-protocol-volumes"></a>Problembehandlung für SMB-Volumes und Volumes mit dualem Protokoll

In diesem Artikel werden Lösungen für Fehlerbedingungen beschrieben, die beim Erstellen oder Verwalten von Volumes mit dualem Protokoll auftreten können.

## <a name="errors-for-dual-protocol-volumes"></a>Fehler bei Volumes mit dualem Protokoll

|     Fehlerbedingungen    |     Lösungen    |
|-|-|
| LDAP über TLS ist aktiviert, und beim Erstellen eines Volumes mit dualem Protokoll tritt der Fehler `This Active Directory has no Server root CA Certificate` auf.    |     Wenn dieser Fehler beim Erstellen eines Volumes mit dualem Protokoll auftritt, stellen Sie sicher, dass das Zertifikat der Stammzertifizierungsstelle in Ihr NetApp-Konto hochgeladen wird.    |
| Beim Erstellen eines Volumes mit dualem Protokoll tritt der Fehler `Failed to validate LDAP configuration, try again after correcting LDAP configuration` auf.    |  Der Zeigereintrag (PTR) des AD-Hostcomputers ist möglicherweise auf dem DNS-Server nicht vorhanden. Sie müssen eine Reverse-Lookupzone auf dem DNS-Server erstellen und dann einen PTR-Eintrag des AD-Hostcomputers in dieser Reverse-Lookupzone hinzufügen. <br> Nehmen wir beispielsweise an, dass die IP-Adresse des AD-Computers `10.X.X.X`, der Hostname des AD-Computers (wie mit dem Befehl `hostname` gefunden) `AD1` und der Domänenname `contoso.com` ist.  Der PTR-Eintrag, der der Reverse-Lookupzone hinzugefügt wurde, sollte `10.X.X.X` -> `contoso.com` sein.   |
| Beim Erstellen eines Volumes mit dualem Protokoll tritt der Fehler `Failed to create the Active Directory machine account \\\"TESTAD-C8DD\\\". Reason: Kerberos Error: Pre-authentication information was invalid Details: Error: Machine account creation procedure failed\\n [ 434] Loaded the preliminary configuration.\\n [ 537] Successfully connected to ip 10.X.X.X, port 88 using TCP\\n**[ 950] FAILURE` auf. |     Dieser Fehler weist darauf hin, dass das AD-Kennwort falsch ist, wenn Active Directory mit dem NetApp-Konto verknüpft wird. Aktualisieren Sie die AD-Verbindung mit dem richtigen Kennwort, und versuchen Sie es erneut. |
| Beim Erstellen eines Volumes mit dualem Protokoll tritt der Fehler `Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available` auf. |   Dieser Fehler weist darauf hin, dass das DNS nicht erreichbar ist. Der Grund hierfür könnte sein, dass die DNS-IP-Adresse falsch ist oder ein Netzwerkproblem vorliegt. Überprüfen Sie die in der AD-Verbindung eingegebene DNS-IP-Adresse, und vergewissern Sie sich, dass sie richtig ist. <br> Stellen Sie außerdem sicher, dass sich AD und Volume in derselben Region und im gleichen VNET befinden. Wenn sie sich in verschiedenen VNETs befinden, stellen Sie sicher, dass das VNET-Peering zwischen den beiden VNETs eingerichtet ist.|
| „Berechtigung verweigert“-Fehler beim Einbinden eines Volumes mit dualem Protokoll. | Ein Volume mit dualem Protokoll unterstützt das NFS- und das SMB-Protokoll.  Wenn Sie versuchen, auf das eingebundene Volume auf dem UNIX-System zuzugreifen, versucht das System, den verwendeten UNIX-Benutzer einem Windows-Benutzer zuzuordnen. Wenn keine Zuordnung gefunden wird, tritt der Fehler „Berechtigung verweigert“ auf. <br> Dies trifft auch zu, wenn Sie den Root-Benutzer für den Zugriff verwenden. <br> Um den Fehler „Berechtigung verweigert“ zu vermeiden, sollten Sie sicherstellen, dass Windows Active Directory `pcuser` enthält, bevor Sie auf den Bereitstellungspunkt zugreifen. Wenn Sie `pcuser` hinzufügen, nachdem der Fehler „Berechtigung verweigert“ aufgetreten ist, müssen Sie 24 Stunden warten, bis der Cacheeintrag gelöscht wurde, bevor Sie erneut versuchen, auf das Volume zuzugreifen. |

## <a name="common-errors-for-smb-and-dual-protocol-volumes"></a>Häufige Fehler bei SMB-Volumes und Volumes mit dualem Protokoll

|     Fehlerbedingungen    |     Lösungen    |
|-|-|
| Bei der Erstellung des SMB-Volumes oder des Volumes mit dualem Protokoll tritt der folgende Fehler auf: <br> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.","details":[{"code":"InternalServerError", "message":"Error when creating - Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available."}]}` | Dieser Fehler weist darauf hin, dass das DNS nicht erreichbar ist. <br> Ziehen Sie die folgenden Lösungen in Betracht: <ul><li>Überprüfen Sie, ob AD DS und das Volume in derselben Region bereitgestellt werden.</li> <li>Überprüfen Sie, ob AD DS und das Volume dasselbe VNet verwenden. Wenn Sie verschiedene VNets verwenden, stellen Sie sicher, dass die VNets miteinander gepeert sind. Lesen Sie den Artikel mit den [Richtlinien für die Azure NetApp Files-Netzwerkplanung](azure-netapp-files-network-topologies.md). </li> <li>Auf dem DNS-Server sind möglicherweise Netzwerksicherheitsgruppen (NSGs) angewendet.  In diesem Fall lässt er keinen Durchfluss des Datenverkehrs zu. Öffnen Sie in diesem Fall die NSGs für das DNS oder AD, um eine Verbindung mit verschiedenen Ports herzustellen. Informationen zu Portanforderungen finden Sie unter [Anforderungen für Active Directory-Verbindungen](create-active-directory-connections.md#requirements-for-active-directory-connections). </li></ul> <br>Dieselben Lösungen gelten für Azure AD DS. Azure AD DS sollte in derselben Region bereitgestellt werden. Das VNet sollte sich in derselben Region befinden oder mit dem vom Volume verwendeten VNet gepeert sein. | 
| Bei der Erstellung des SMB-Volumes oder des Volumes mit dualem Protokoll tritt der folgende Fehler auf: <br> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.","details":[{"code":"InternalServerError", "message":"Error when creating - Failed to create the Active Directory machine account \"SMBTESTAD-C1C8\". Reason: Kerberos Error: Invalid credentials were given Details: Error: Machine account creation procedure failed\n [ 563] Loaded the preliminary configuration.\n**[ 670] FAILURE: Could not authenticate as 'test@contoso.com':\n** Unknown user (KRB5KDC_ERR_C_PRINCIPAL_UNKNOWN)\n. "}]}`  |  <ul><li>Stellen Sie sicher, dass der Benutzername richtig eingegeben wurde. </li> <li>Stellen Sie sicher, dass der Benutzer zur Administratorengruppe gehört, die über die Berechtigung zum Erstellen von Computerkonten verfügt. </li> <li> Wenn Sie Azure AD DS verwenden, stellen Sie sicher, dass der Benutzer zur Azure AD-Gruppe `Azure AD DC Administrators` gehört. </li></ul> | 
| Bei der Erstellung des SMB-Volumes oder des Volumes mit dualem Protokoll tritt der folgende Fehler auf: <br> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.","details":[{"code":"InternalServerError", "message":"Error when creating - Failed to create the Active Directory machine account \"SMBTESTAD-A452\". Reason: Kerberos Error: Pre-authentication information was invalid Details: Error: Machine account creation procedure failed\n [ 567] Loaded the preliminary configuration.\n [ 671] Successfully connected to ip 10.X.X.X, port 88 using TCP\n**[ 1099] FAILURE: Could not authenticate as\n** 'user@contoso.com': CIFS server account password does\n** not match password stored in Active Directory\n** (KRB5KDC_ERR_PREAUTH_FAILED)\n. "}]}` | Stellen Sie sicher, dass das für den Beitritt zur AD-Verbindung eingegebene Kennwort korrekt ist. |
| Bei der Erstellung des SMB-Volumes oder des Volumes mit dualem Protokoll tritt der folgende Fehler auf: `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.","details":[{"code":"InternalServerError","message":"Error when creating - Failed to create the Active Directory machine account \"SMBTESTAD-D9A2\". Reason: SecD Error: ou not found Details: Error: Machine account creation procedure failed\n [ 561] Loaded the preliminary configuration.\n [ 665] Successfully connected to ip 10.X.X.X, port 88 using TCP\n [ 1039] Successfully connected to ip 10.x.x.x, port 389 using TCP\n**[ 1147] FAILURE: Specifed OU 'OU=AADDC Com' does not exist in\n** contoso.com\n. "}]}` | Stellen Sie sicher, dass das für den Beitritt zur AD-Verbindung angegebene OE-Pfad korrekt ist. Wenn Sie Azure AD DS verwenden, stellen Sie sicher, dass der Pfad der Organisationseinheit `OU=AADDC Computers` ist. |

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen eines SMB-Volumes](azure-netapp-files-create-volumes-smb.md)
* [Erstellen eines Dual-Protokoll-Volumes](create-volumes-dual-protocol.md)
* [Konfigurieren eines NFS-Clients für Azure NetApp Files](configure-nfs-clients.md)

---
title: Problembehandlung bei Volumes mit dualem Protokoll für Azure NetApp Files | Microsoft-Dokumentation
description: In diesem Artikel werden Fehlermeldungen und Lösungen beschrieben, mit denen Sie Probleme mit dem dualem Protokoll für Azure NetApp Files beheben können.
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
ms.date: 01/22/2021
ms.author: b-juche
ms.openlocfilehash: fb4233a87231dddb1e3cb2777ac2ef53a61f833e
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98726614"
---
# <a name="troubleshoot-dual-protocol-volumes"></a>Problembehandlung für Volumes mit dualem Protokoll

In diesem Artikel werden Lösungen für Fehlerbedingungen beschrieben, die beim Erstellen oder Verwalten von Volumes mit dualem Protokoll auftreten können.

## <a name="error-conditions-and-resolutions"></a>Fehlerbedingungen und Auflösungen

|     Fehlerbedingungen    |     Lösung    |
|-|-|
| LDAP über TLS ist aktiviert, und beim Erstellen eines Volumes mit dualem Protokoll tritt der Fehler `This Active Directory has no Server root CA Certificate` auf.    |     Wenn dieser Fehler beim Erstellen eines Volumes mit dualem Protokoll auftritt, stellen Sie sicher, dass das Zertifikat der Stammzertifizierungsstelle in Ihr NetApp-Konto hochgeladen wird.    |
| Beim Erstellen eines Volumes mit dualem Protokoll tritt der Fehler `Failed to validate LDAP configuration, try again after correcting LDAP configuration` auf.    |  Der Zeigereintrag (PTR) des AD-Hostcomputers ist möglicherweise auf dem DNS-Server nicht vorhanden. Sie müssen eine Reverse-Lookupzone auf dem DNS-Server erstellen und dann einen PTR-Eintrag des AD-Hostcomputers in dieser Reverse-Lookupzone hinzufügen. <br> Nehmen wir beispielsweise an, dass die IP-Adresse des AD-Computers `1.1.1.1`, der Hostname des AD-Computers (wie mit dem Befehl `hostname` gefunden) `AD1` und der Domänenname `contoso.com` ist.  Der PTR-Eintrag, der der Reverse-Lookupzone hinzugefügt wurde, sollte `1.1.1.1` -> `contoso.com` sein.   |
| Beim Erstellen eines Volumes mit dualem Protokoll tritt der Fehler `Failed to create the Active Directory machine account \\\"TESTAD-C8DD\\\". Reason: Kerberos Error: Pre-authentication information was invalid Details: Error: Machine account creation procedure failed\\n [ 434] Loaded the preliminary configuration.\\n [ 537] Successfully connected to ip 1.1.1.1, port 88 using TCP\\n**[ 950] FAILURE` auf. |  Dieser Fehler weist darauf hin, dass das AD-Kennwort falsch ist, wenn Active Directory mit dem NetApp-Konto verknüpft wird. Aktualisieren Sie die AD-Verbindung mit dem richtigen Kennwort, und versuchen Sie es erneut. |
| Beim Erstellen eines Volumes mit dualem Protokoll tritt der Fehler `Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available` auf. |   Dieser Fehler weist darauf hin, dass das DNS nicht erreichbar ist. Der Grund hierfür könnte sein, dass die DNS-IP-Adresse falsch ist oder ein Netzwerkproblem vorliegt. Überprüfen Sie die in der AD-Verbindung eingegebene DNS-IP-Adresse, und vergewissern Sie sich, dass sie richtig ist. <br> Stellen Sie außerdem sicher, dass sich AD und Volume in derselben Region und im gleichen VNET befinden. Wenn sie sich in verschiedenen VNETs befinden, stellen Sie sicher, dass das VNET-Peering zwischen den beiden VNETs eingerichtet ist.|
| „Berechtigung verweigert“-Fehler beim Einbinden eines Volumes mit dualem Protokoll. | Ein Volume mit dualem Protokoll unterstützt das NFS- und das SMB-Protokoll.  Wenn Sie versuchen, auf das eingebundene Volume auf dem UNIX-System zuzugreifen, versucht das System, den verwendeten UNIX-Benutzer einem Windows-Benutzer zuzuordnen. Wenn keine Zuordnung gefunden wird, tritt der Fehler „Berechtigung verweigert“ auf. <br> Dies trifft auch zu, wenn Sie den Root-Benutzer für den Zugriff verwenden. <br> Um den Fehler „Berechtigung verweigert“ zu vermeiden, sollten Sie sicherstellen, dass Windows Active Directory `pcuser` enthält, bevor Sie auf den Bereitstellungspunkt zugreifen. Wenn Sie `pcuser` hinzufügen, nachdem der Fehler „Berechtigung verweigert“ aufgetreten ist, müssen Sie 24 Stunden warten, bis der Cacheeintrag gelöscht wurde, bevor Sie erneut versuchen, auf das Volume zuzugreifen. |

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen eines Dual-Protokoll-Volumes](create-volumes-dual-protocol.md)
* [Konfigurieren eines NFS-Clients für Azure NetApp Files](configure-nfs-clients.md)

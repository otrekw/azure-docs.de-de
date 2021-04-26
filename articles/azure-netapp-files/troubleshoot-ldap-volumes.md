---
title: Behandeln von Problemen mit LDAP-Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt Auflösungen zu Fehlerzuständen, die beim Konfigurieren von LDAP-Volumes für Azure NetApp-Dateien auftreten können.
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
ms.date: 04/05/2021
ms.author: b-juche
ms.openlocfilehash: eea3f691bc6d91948dc73b4a02c89abfac12d384
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498885"
---
# <a name="troubleshoot-ldap-volume-issues"></a>Behandeln von Problemen mit LDAP-Volumes

In diesem Artikel werden Lösungen für Fehlerbedingungen beschrieben, die beim Erstellen Konfigurieren von LDAP-Volumes auftreten können.

## <a name="errors-and-resolutions-for-ldap-volumes"></a>Fehler und Lösungen für LDAP-Volumes

|     Fehlerbedingungen    |     Lösungen    |
|-|-|
| Fehler beim Erstellen eines SMB-Volumes mit ldapEnabled als „true“: <br> `Error Message: ldapEnabled option is only supported with NFS protocol volume. ` | Ein SMB-Volume mit aktiviertem LDAP kann nicht erstellt werden. <br> Erstellen Sie SMB-Volumes mit deaktiviertem LDAP. |
| Fehler beim Aktualisieren des ldapEnabled-Parameterwerts für ein vorhandenes Volume: <br> `Error Message: ldapEnabled parameter is not allowed to update` |  Nachdem Sie ein Volume erstellt haben, können Sie die Einstellung der LDAP-Option nicht mehr ändern. <br> Aktualisieren Sie die Einstellung der LDAP-Option auf einem erstellten Volume nicht. Weitere Informationen finden Sie unter [Konfigurieren von ADDS LDAP mit erweiterten Gruppen für NFS-Volume-Zugriff](configure-ldap-extended-groups.md) . |
| Fehler beim Erstellen eines LDAP-aktivierten NFS-Volumes: <br> `Could not query DNS server` <br> `Sample error message:` <br> `"log": time="2020-10-21 05:04:04.300" level=info msg=Res method=GET url=/v2/Volumes/070d0d72-d82c-c893-8ce3-17894e56cea3  x-correlation-id=9bb9e9fe-abb6-4eb5-a1e4-9e5fbb838813 x-request-id=c8032cb4-2453-05a9-6d61-31ca4a922d85 xresp="200:  {\"created\":\"2020-10-21T05:02:55.000Z\",\"lifeCycleState\":\"error\",\"lifeCycleStateDetails\":\"Error when creating - Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available.\",\"name\":\"smb1\",\"ownerId\ \":\"8c925a51-b913-11e9-b0de-9af5941b8ed0\",\"region\":\"westus2stage\",\"volumeId\":\"070d0d72-d82c-c893-8ce3-` |  Dieser Fehler tritt auf, weil DNS nicht erreichbar ist. <br> <ul><li> Überprüfen Sie, ob Sie den korrekten Standort (Standortbereich) für Azure NetApp-Dateien konfiguriert haben. </li><li> Die Gründe, warum DNS nicht erreichbar ist, können falsche DNS-IP-Adressen oder Netzwerkprobleme sein. Überprüfen Sie die in der AD-Verbindung eingegebene DNS-IP-Adresse und vergewissern Sie sich, dass sie richtig ist. </li><li> Stellen Sie sicher, dass sich AD und das Volume in derselben Region und im gleichen VNet befinden. Wenn sie sich in verschiedenen VNets befinden, stellen Sie sicher, dass das VNet-Peering zwischen den beiden VNets eingerichtet ist.</li></ul> |
| Fehler beim Erstellen eines Volumes aus einer Momentaufnahme: <br> `Aggregate does not exist` | Azure NetApp Files bietet keine Unterstützung für die Bereitstellung eines neuen LDAP-fähigen Volumes aus einer Momentaufnahme, das zu einem LDAP-deaktivierten Volume gehört. <br> Erstellen Sie ein neues LDAP-deaktiviertes Volume aus der angegebenen Momentaufnahme. |

## <a name="next-steps"></a>Nächste Schritte  

* [Konfigurieren von ADDS LDAP mit erweiterten Gruppen für Zugriff auf NFS-Volumes](configure-ldap-extended-groups.md)
* [Erstellen eines NFS-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Erstellen eines Volumes mit dualem Protokoll (NFSv3 und SMB) für Azure NetApp Files](create-volumes-dual-protocol.md)
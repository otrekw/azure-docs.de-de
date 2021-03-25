---
title: Auswirkungen von Kerberos auf die Leistung von Azure NetApp Files NFSv4.1-Volumes | Microsoft-Dokumentation
description: Beschreibt die verfügbaren Sicherheitsoptionen, die getesteten Leistungsvektoren und die erwarteten Auswirkungen von Kerberos auf die Leistung von Azure NetApp Files NFSv4.1-Volumes.
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
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: b-juche
ms.openlocfilehash: e9054f11c8f55e9fe00266840a9f6e257f14e659
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101744042"
---
# <a name="performance-impact-of-kerberos-on-azure-netapp-files-nfsv41-volumes"></a>Auswirkungen von Kerberos auf die Leistung von Azure NetApp Files NFSv4.1-Volumes

Azure NetApp Files unterstützt die [Verschlüsselung des NFS-Clients in Kerberos-Modi](configure-kerberos-encryption.md) (krb5, krb5i und krb5p) mit AES-256-Verschlüsselung. In diesem Artikel werden die Auswirkungen von Kerberos auf die Leistung von NFSv4.1-Volumes beschrieben. 

## <a name="available-security-options"></a>Verfügbare Sicherheitsoptionen 

Derzeit sind folgende Sicherheitsoptionen für NFSv4.1-Volumes verfügbar: 

* **sec=sys** verwendet lokale UNIX-UIDs und -GIDs mithilfe von AUTH_SYS zum Authentifizieren von NFS-Vorgängen.
* **sec=krb5** verwendet Kerberos V5 anstelle von lokalen UNIX-UIDs und -GIDs zum Authentifizieren von Benutzern.
* **sec=krb5i** verwendet Kerberos V5 für die Benutzerauthentifizierung und führt die Integritätsüberprüfung von NFS-Vorgängen mithilfe von sicheren Prüfsummen durch, um Datenmanipulationen zu verhindern.
* **sec=krb5p** verwendet Kerberos V5 für die Benutzerauthentifizierung und die Integritätsüberprüfung. Bei dieser Option wird der NFS-Datenverkehr verschlüsselt, um das Ausspähen des Datenverkehrs zu verhindern. Diese Option ist die sicherste Einstellung, sie stellt aber auch die größten Anforderungen an die Leistung.

## <a name="performance-vectors-tested"></a>Getestete Leistungsvektoren

In diesem Abschnitt werden die Leistungsauswirkungen der verschiedenen `sec=*`-Optionen auf einen einzelnen Client beschrieben.

* Die Leistungsauswirkungen wurden auf zwei Ebenen getestet: geringe Parallelität (niedrige Auslastung) und hohe Parallelität (Obergrenze für E/A und Durchsatz).  
* Drei Typen von Workloads wurden getestet:  
    * Kleine Vorgänge mit zufälligem Lese-/Schreibzugriff (mit FIO)
    * Umfangreiche Vorgänge mit sequenziellem Lese-/Schreibzugriff (mit FIO)
    * Hohe Arbeitsauslastung durch Metadaten, z. B. durch Anwendungen wie Git generiert

## <a name="expected-performance-impact"></a>Erwartete Leistungsauswirkungen 

Es gibt zwei Schwerpunktbereiche: geringe Auslastung und Obergrenze. In den folgenden Listen werden die Leistungsauswirkungen der einzelnen Sicherheitseinstellungen und Szenarien beschrieben. Alle Vergleiche beziehen sich auf den Sicherheitsparameter `sec=sys`. Der Test wurde für ein einzelnes Volume unter Verwendung eines einzelnen Clients durchgeführt. 

Leistungsauswirkungen von krb5:

* Geringe Parallelität (L/S):
    * Sequenzielle Latenz um 0,3 ms erhöht.
    * Zufällige E/A-Latenz um 0,2 ms erhöht.
    * E/A-Latenz bei Metadaten um 0,2 ms erhöht.
* Hohe Parallelität (L/S): 
    * Der maximale sequenzielle Durchsatz wurde durch krb5 nicht beeinträchtigt.
    * Die maximale zufällige E/A verringerte sich bei reinen Leseworkloads um 30 %, wobei die Gesamtauswirkungen bei reinen Leseworkloads bei 0 lagen. 
    * Die maximale Workload für Metadaten wurde um 30 % verringert.

Leistungsauswirkungen von krb5i: 

* Geringe Parallelität (L/S):
    * Sequenzielle Latenz um 0,5 ms erhöht.
    * Zufällige E/A-Latenz um 0,2 ms erhöht.
    * E/A-Latenz bei Metadaten um 0,2 ms erhöht.
* Hohe Parallelität (L/S): 
    * Der maximale sequenzielle Durchsatz wurde unabhängig von der Workloadmischung um insgesamt 70 % verringert.
    * Die maximale zufällige E/A verringerte sich bei reinen Leseworkloads um 50 %, wobei die Gesamtauswirkungen bei reinen Leseworkloads bei 25 % lagen. 
    * Die maximale Workload für Metadaten wurde um 30 % verringert.

Leistungsauswirkungen von krb5p:

* Geringe Parallelität (L/S):
    * Sequenzielle Latenz um 0,8 ms erhöht.
    * Zufällige E/A-Latenz um 0,2 ms erhöht.
    * E/A-Latenz bei Metadaten um 0,2 ms erhöht.
* Hohe Parallelität (L/S): 
    * Der maximale sequenzielle Durchsatz wurde unabhängig von der Workloadmischung um insgesamt 85 % verringert. 
    * Die maximale zufällige E/A verringerte sich bei reinen Leseworkloads um 65 %, wobei die Gesamtauswirkungen bei reinen Leseworkloads bei 43 % lagen. 
    * Die maximale Workload für Metadaten wurde um 30 % verringert.

## <a name="next-steps"></a>Nächste Schritte  

* [Konfigurieren der NFSv4.1-Kerberos-Verschlüsselung für Azure NetApp Files](configure-kerberos-encryption.md) 

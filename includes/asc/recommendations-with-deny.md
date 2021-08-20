---
author: memildin
ms.service: security-center
ms.topic: include
ms.date: 07/25/2021
ms.author: memildin
ms.custom: generated
ms.openlocfilehash: 8615c84a611d8f05ebb7db017bc8e7dbfa54fbcd
ms.sourcegitcommit: 98e126b0948e6971bd1d0ace1b31c3a4d6e71703
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2021
ms.locfileid: "114678251"
---
- [Bei Bedarf aktivieren] Azure Cosmos DB-Konten müssen kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden
- [Bei Bedarf aktivieren] Azure Machine Learning-Arbeitsbereiche müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden
- [Bei Bedarf aktivieren] Cognitive Services-Konten müssen eine Datenverschlüsselung mit einem kundenseitig verwalteten Schlüssel (CMK) aktivieren
- [Bei Bedarf aktivieren] Containerregistrierungen müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden
- Der Zugriff auf Speicherkonten mit Konfiguration für Firewall und virtuelle Netzwerke sollte eingeschränkt werden.
- Automation-Kontovariablen sollten verschlüsselt werden.
- Azure Cache for Redis muss sich in einem virtuellen Netzwerk befinden.
- Azure Spring Cloud muss Netzwerkinjektion verwenden
- Für Container müssen CPU- und Arbeitsspeicherlimits erzwungen werden
- Containerimages dürfen nur aus vertrauenswürdigen Registrierungen bereitgestellt werden
- Container mit Rechteausweitung müssen vermieden werden
- Container mit Freigabe sensibler Hostnamespaces vermeiden
- Container dürfen nur an zulässigen Ports lauschen
- Unveränderliches (schreibgeschütztes) Stammdateisystem für Container erzwingen
- Key Vault-Schlüssel sollten ein Ablaufdatum aufweisen.
- Key Vault-Geheimnisse sollten ein Ablaufdatum aufweisen.
- Für Schlüsseltresore sollte der Löschschutz aktiviert sein.
- Für Schlüsseltresore sollte vorläufiges Löschen aktiviert sein.
- Linux-Funktionen mit den niedrigsten Berechtigungen für Container erzwingen
- Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden
- Überschreiben oder Deaktivieren des AppArmor-Profils für Container einschränken
- Privilegierte Container müssen vermieden werden
- Das Ausführen von Containern als Root-Benutzer muss vermieden werden
- Sichere Übertragung in Speicherkonten sollte aktiviert werden.
- Service Fabric Cluster sollten die Eigenschaft ClusterProtectionLevel auf EncryptAndSign setzen
- Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.
- Dienste dürfen nur an zulässigen Ports lauschen
- Der öffentliche Zugriff auf Speicherkonten muss untersagt sein
- Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.
- Speicherkonten müssen den Netzwerkzugriff mithilfe von VNET-Regeln einschränken
- Verwendung von Hostnetzwerken und -ports einschränken
- Verwendung von HostPath-Volumeeinbindungen von Pods sollte auf eine bekannte Liste beschränkt sein, um den Knotenzugriff von kompromittierten Containern zu begrenzen
- Gültigkeitszeitraum für in Azure Key Vault gespeicherte Zertifikate sollte zwölf Monate nicht überschreiten
- VMs sollten zu neuen Azure Resource Manager-Ressourcen migriert werden
- Web Application Firewall (WAF) muss für Application Gateway aktiviert sein.
- Web Application Firewall (WAF) muss für Azure Front Door Service aktiviert sein


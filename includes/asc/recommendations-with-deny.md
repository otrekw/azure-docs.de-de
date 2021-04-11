---
author: memildin
ms.service: security-center
ms.topic: include
ms.date: 03/14/2021
ms.author: memildin
ms.custom: generated
ms.openlocfilehash: c142f6a00275b4b21b6bbc64e51a5c4d7d957258
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2021
ms.locfileid: "103468006"
---
- Der Zugriff auf Speicherkonten mit Firewall- und VNet-Konfigurationen sollte eingeschränkt werden Netzwerkzugriff auf Speicherkonten einschränken
- Automation-Kontovariablen müssen verschlüsselt werden Automation-Kontovariablen müssen verschlüsselt werden
- Azure Cache for Redis muss sich in einem virtuellen Netzwerk befinden.
- Azure Cosmos DB-Konten sollten kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden.
- Azure Machine Learning-Arbeitsbereiche müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden
- Azure Spring Cloud muss Netzwerkinjektion verwenden
- Cognitive Services-Konten müssen eine Datenverschlüsselung mit einem kundenseitig verwalteten Schlüssel (CMK) aktivieren
- Für Container müssen CPU- und Arbeitsspeicherlimits erzwungen werden
- Containerimages dürfen nur aus vertrauenswürdigen Registrierungen bereitgestellt werden
- Containerregistrierungen müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden
- Container mit Rechteausweitung müssen vermieden werden
- Container mit Freigabe sensibler Hostnamespaces vermeiden
- Container dürfen nur an zulässigen Ports lauschen
- Unveränderliches (schreibgeschütztes) Stammdateisystem für Container erzwingen
- Key Vault-Schlüssel sollten ein Ablaufdatum aufweisen.
- Key Vault-Geheimnisse sollten ein Ablaufdatum aufweisen.
- Für Schlüsseltresore sollte der Löschschutz aktiviert sein.
- Für Schlüsseltresore sollte vorläufiges Löschen aktiviert sein.
- Linux-Funktionen mit den niedrigsten Berechtigungen für Container erzwingen
- Für Ihren Redis Cache dürfen nur sichere Verbindungen aktiviert sein Für Ihren Azure Cache for Redis dürfen nur sichere Verbindungen aktiviert sein
- Überschreiben oder Deaktivieren des AppArmor-Profils für Container einschränken
- Privilegierte Container müssen vermieden werden
- Das Ausführen von Containern als Root-Benutzer muss vermieden werden
- Für Speicherkonten muss die sichere Übertragung aktiviert sein Für Speicherkonten muss die sichere Übertragung aktiviert sein
- Für Service Fabric-Cluster muss die ClusterProtectionLevel-Eigenschaft auf EncryptAndSign festgelegt sein Für Service Fabric-Cluster muss die ClusterProtectionLevel-Eigenschaft auf EncryptAndSign festgelegt sein
- Service Fabric-Cluster dürfen nur Azure Active Directory für die Clientauthentifizierung verwenden Service Fabric-Cluster dürfen nur Azure Active Directory für die Clientauthentifizierung verwenden
- Dienste dürfen nur an zulässigen Ports lauschen
- Der öffentliche Zugriff auf Speicherkonten muss untersagt sein
- Speicherkonten müssen zu neuen Azure Resource Manager-Ressourcen migriert werden Speicherkonten müssen zu neuen Azure Resource Manager-Ressourcen migriert werden
- Speicherkonten müssen den Netzwerkzugriff mithilfe von VNET-Regeln einschränken
- Verwendung von Hostnetzwerken und -ports einschränken
- Verwendung von HostPath-Volumeeinbindungen von Pods sollte auf eine bekannte Liste beschränkt sein, um den Knotenzugriff von kompromittierten Containern zu begrenzen
- Gültigkeitszeitraum für in Azure Key Vault gespeicherte Zertifikate sollte zwölf Monate nicht überschreiten
- VMs müssen zu neuen Azure Resource Manager-Ressourcen migriert werden VMs müssen zu neuen Azure Resource Manager-Ressourcen migriert werden
- Web Application Firewall (WAF) muss für Application Gateway aktiviert sein.
- Web Application Firewall (WAF) muss für Azure Front Door Service aktiviert sein


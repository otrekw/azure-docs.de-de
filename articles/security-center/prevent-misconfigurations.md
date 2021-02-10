---
title: Verhindern von Fehlkonfigurationen mit Azure Security Center
description: Es wird beschrieben, wie Sie auf Seiten mit Empfehlungsdetails die Optionen „Erzwingen“ und „Ablehnen“ von Security Center verwenden.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 02/04/2021
ms.author: memildin
ms.openlocfilehash: a3da9cdea543894aa7aec66112e28658beac84b5
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2021
ms.locfileid: "99558190"
---
# <a name="prevent-misconfigurations-with-enforcedeny-recommendations"></a>Verhindern von Fehlkonfigurationen mit den Optionen zum Erzwingen/Ablehnen für Empfehlungen

Sicherheitsbezogene Fehlkonfigurationen sind eine häufige Ursache für Sicherheitsincidents. Security Center verfügt jetzt über ein Feature zum *Verhindern* von Fehlkonfigurationen neuer Ressourcen für bestimmte Empfehlungen. 

Dieses Feature kann dazu beitragen, dass Ihre Workloads geschützt sind und Ihre Sicherheitsbewertung stabil bleibt.

Für die Erzwingung einer sicheren Konfiguration basierend auf einer bestimmten Empfehlung gibt es zwei Modi:

- Mit dem Effekt **Deny** (Ablehnen) von Azure Policy können Sie verhindern, dass fehlerhafte Ressourcen erstellt werden.
- Mit der Option **Erzwingen** können Sie den Effekt **DeployIfNotExist** von Azure Policy nutzen und nicht konforme Ressourcen bei der Erstellung automatisch korrigieren.

Sie finden diese Option oben auf der Seite mit den Ressourcendetails für ausgewählte Sicherheitsempfehlungen (siehe [Empfehlungen mit der Option „Ablehnen/Erzwingen“](#recommendations-with-denyenforce-options)).

## <a name="prevent-resource-creation"></a>Verhindern der Ressourcenerstellung

1. Öffnen Sie die Empfehlung, deren Anforderungen von Ihren neuen Ressourcen erfüllt werden müssen, und wählen Sie oben auf der Seite die Schaltfläche **Ablehnen** aus.

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-deny-button.png" alt-text="Empfehlungsseite mit hervorgehobener Schaltfläche „Ablehnen“":::

    Der Konfigurationsbereich wird geöffnet, und die Bereichsoptionen werden aufgeführt. 

1. Legen Sie den Bereich fest, indem Sie das relevante Abonnement oder die Verwaltungsgruppe auswählen.

    > [!TIP]
    > Sie können die drei Punkte am Ende der Zeile verwenden, um ein Abonnement zu ändern, oder über die Kontrollkästchen mehrere Abonnements oder Gruppen und dann die Option **In „Ablehnen“ ändern** auswählen.

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-prevent-resource-creation.png" alt-text="Festlegen des Bereichs für „Ablehnen“ von Azure Policy":::


## <a name="enforce-a-secure-configuration"></a>Erzwingen einer sicheren Konfiguration

1. Öffnen Sie die Empfehlung, für die Sie eine Vorlagenbereitstellung durchführen möchten, falls die Anforderungen der Empfehlung von neuen Ressourcen nicht erfüllt werden. Wählen Sie anschließend oben auf der Seite die Schaltfläche **Erzwingen** aus.

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-enforce-button.png" alt-text="Empfehlungsseite mit hervorgehobener Schaltfläche „Erzwingen“":::

    Der Konfigurationsbereich wird geöffnet, und alle Optionen der Richtlinienkonfiguration werden angezeigt. 

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-enforce-config.png" alt-text="Erzwingen von Konfigurationsoptionen":::

1. Legen Sie den Bereich, den Zuweisungsnamen und andere relevante Optionen fest.

1. Klicken Sie auf **Überprüfen + erstellen**.

## <a name="recommendations-with-denyenforce-options"></a>Empfehlungen mit Optionen zum Ablehnen/Erzwingen

Diese Empfehlungen können mit der Option **Ablehnen** verwendet werden:

- Der Zugriff auf Speicherkonten mit Konfiguration für Firewall und virtuelle Netzwerke sollte eingeschränkt werden.
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
- Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden
- Überschreiben oder Deaktivieren des AppArmor-Profils für Container einschränken
- Privilegierte Container müssen vermieden werden
- Das Ausführen von Containern als Root-Benutzer muss vermieden werden
- Sichere Übertragung in Speicherkonten sollte aktiviert werden.
- Service Fabric Cluster sollten die Eigenschaft ClusterProtectionLevel auf EncryptAndSign setzen
- Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.
- Dienste dürfen nur an zulässigen Ports lauschen
- Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.
- Speicherkonten müssen den Netzwerkzugriff mithilfe von VNET-Regeln einschränken
- Verwendung von Hostnetzwerken und -ports einschränken
- Verwendung von HostPath-Volumeeinbindungen von Pods sollte auf eine bekannte Liste beschränkt sein, um den Knotenzugriff von kompromittierten Containern zu begrenzen
- Gültigkeitszeitraum für in Azure Key Vault gespeicherte Zertifikate sollte zwölf Monate nicht überschreiten
- VMs sollten zu neuen Azure Resource Manager-Ressourcen migriert werden
- Web Application Firewall (WAF) muss für Application Gateway aktiviert sein.
- Web Application Firewall (WAF) muss für Azure Front Door Service aktiviert sein

Diese Empfehlungen können mit der Option **Erzwingen** verwendet werden:

- Die Überwachung in SQL Server muss aktiviert werden.
- Azure Backup sollte für virtuelle Computer aktiviert sein
- Auf Ihren SQL-Server sollte Azure Defender für SQL aktiviert sein
- Diagnoseprotokolle in Azure Stream Analytics sollten aktiviert werden.
- Diagnoseprotokolle in Batch-Konten sollten aktiviert sein.
- In Data Lake Analytics sollten Diagnoseprotokolle aktiviert sein.
- In Event Hub sollten Diagnoseprotokolle aktiviert sein
- Diagnoseprotokolle in Key Vault sollten aktiviert sein.
- In Logic Apps müssen Diagnoseprotokolle aktiviert sein
- In den Suchdiensten müssen Diagnoseprotokolle aktiviert sein
- In Service Bus sollten Diagnoseprotokolle aktiviert sein.
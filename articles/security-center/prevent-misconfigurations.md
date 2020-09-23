---
title: Verhindern von Fehlkonfigurationen mit Azure Security Center
description: Es wird beschrieben, wie Sie auf Seiten mit Empfehlungsdetails die Optionen „Erzwingen“ und „Ablehnen“ von Security Center verwenden.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 09/08/2020
ms.author: memildin
ms.openlocfilehash: 19e9a33350b6a1a67986dc35a372f737e45ab39a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906399"
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

- VMs sollten zu neuen Azure Resource Manager-Ressourcen migriert werden
- Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.
- Alle Autorisierungsregeln außer RootManageSharedAccessKey sollten aus dem Event Hub-Namespace entfernt werden.
- Alle Autorisierungsregeln außer RootManageSharedAccessKey sollten aus dem Service Bus-Namespace entfernt werden.
- Sichere Übertragung in Speicherkonten sollte aktiviert werden.
- Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden
- Automation-Kontovariablen sollten verschlüsselt werden.
- Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.
- Service Fabric Cluster sollten die Eigenschaft ClusterProtectionLevel auf EncryptAndSign setzen
- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen


Diese Empfehlungen können mit der Option **Erzwingen** verwendet werden:

- In Logic Apps müssen Diagnoseprotokolle aktiviert sein
- In Data Lake Analytics sollten Diagnoseprotokolle aktiviert sein.
- In IoT Hub sollten Diagnoseprotokolle aktiviert sein.
- Diagnoseprotokolle in Batch-Konten sollten aktiviert sein.
- Diagnoseprotokolle in Azure Stream Analytics sollten aktiviert werden.
- In Service Bus sollten Diagnoseprotokolle aktiviert sein.
- In den Suchdiensten müssen Diagnoseprotokolle aktiviert sein
- In Event Hub sollten Diagnoseprotokolle aktiviert sein
- In Virtual Machine Scale Sets sollten Diagnoseprotokolle aktiviert sein.
- Diagnoseprotokolle in Key Vault sollten aktiviert sein.
- Die Überwachung in SQL Server muss aktiviert werden.
- Advanced Data Security muss für Ihre SQL-Server aktiviert werden.




---
title: Verhindern von Fehlkonfigurationen mit Azure Security Center
description: Es wird beschrieben, wie Sie auf Seiten mit Empfehlungsdetails die Optionen „Erzwingen“ und „Ablehnen“ von Security Center verwenden.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 03/17/2021
ms.author: memildin
ms.openlocfilehash: fabbd255f64e5614ae052c9d7c8d65fc35d45856
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104600495"
---
# <a name="prevent-misconfigurations-with-enforcedeny-recommendations"></a>Verhindern von Fehlkonfigurationen mit den Optionen zum Erzwingen/Ablehnen für Empfehlungen

Sicherheitsbezogene Fehlkonfigurationen sind eine häufige Ursache für Sicherheitsincidents. In Security Center ist jetzt ein Feature verfügbar, mit dem Sie Fehlkonfigurationen neuer Ressourcen dank bestimmter Empfehlungen *verhindern* können. 

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

[!INCLUDE [azure-security-center-recommendations-deny](../../includes/asc/recommendations-with-deny.md)]

Diese Empfehlungen können mit der Option **Erzwingen** verwendet werden:

- Die Überwachung in SQL Server muss aktiviert werden.
- Azure Backup sollte für virtuelle Computer aktiviert sein
- Auf Ihren SQL-Server sollte Azure Defender für SQL aktiviert sein
- Azure Policy-Add-On für Kubernetes muss auf Ihren Clustern installiert und aktiviert sein
- Diagnoseprotokolle in Azure Stream Analytics sollten aktiviert werden.
- Diagnoseprotokolle in Batch-Konten sollten aktiviert sein.
- In Data Lake Analytics sollten Diagnoseprotokolle aktiviert sein.
- In Event Hub sollten Diagnoseprotokolle aktiviert sein
- Diagnoseprotokolle in Key Vault sollten aktiviert sein.
- In Logic Apps müssen Diagnoseprotokolle aktiviert sein
- In den Suchdiensten müssen Diagnoseprotokolle aktiviert sein
- In Service Bus sollten Diagnoseprotokolle aktiviert sein.

---
title: Aktivieren des integrierten Workloadschutzes von Azure Security Center
description: Erfahren Sie, wie Sie Azure Defender aktivieren, um die Schutzmechanismen von Azure Security Center auf Ihre Hybrid- und Multi-Cloud-Ressourcen zu erweitern
author: memildin
ms.author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: quickstart
ms.date: 06/07/2021
ms.openlocfilehash: 4f7900c4abc37df776c231ac9d70a09862f6d49f
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111748395"
---
# <a name="quickstart-enable-azure-defender"></a>Schnellstart: Aktivieren von Azure Defender

Informationen zu den Vorteilen von Azure Defender finden Sie unter [Security Center Free und Azure Defender-Aktivierung](security-center-pricing.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Security Center-Schnellstartanleitungen -Tutorials muss Azure Defender aktiviert werden. 

Sie können ein gesamtes Azure-Abonnement mit Azure Defender schützen, und der Schutz wird von allen Ressourcen innerhalb des Abonnements geerbt.

Sie können eine kostenlose 30-Tage-Testversion nutzen. Preisdetails in Ihrer bevorzugten Währung und für Ihre Region finden Sie unter [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="enable-azure-defender-from-the-azure-portal"></a>Aktivieren von Azure Defender über das Azure-Portal

Wenn Sie alle Security Center-Features einschließlich des Bedrohungsschutzes aktivieren möchten, müssen Sie Azure Defender für das Abonnement mit den entsprechenden Workloads aktivieren. Durch die Aktivierung auf der Arbeitsbereichsebene werden Just-In-Time-VM-Zugriff, adaptive Anwendungssteuerungen und Netzwerkerkennungen für Azure-Ressourcen nicht aktiviert. Darüber hinaus sind auf der Arbeitsbereichsebene nur die Azure Defender-Pläne „Azure Defender für Server“ und „Azure Defender für SQL-Server auf Computern“ verfügbar.

- Sie können **Azure Defender für Storage-Konten** entweder auf Abonnement- oder auf Ressourcenebene aktivieren
- Sie können **Azure Defender für SQL** entweder auf Abonnement- oder auf Ressourcenebene aktivieren
- Sie können **Azure Defender für relationale Open-Source-Datenbanken** nur auf Ressourcenebene aktivieren.

### <a name="to-enable-azure-defender-on-your-subscriptions-and-workspaces"></a>So aktivieren Sie Azure Defender für Ihre Abonnements und Arbeitsbereiche:

- So aktivieren Sie Azure Defender für ein Abonnement:

    1. Wählen Sie im Hauptmenü von Security Center die Option **Preise und Einstellungen** aus.
    1. Wählen Sie das Abonnement oder den Arbeitsbereich aus, das bzw. den Sie schützen möchten.
    1. Wählen Sie zum Aktualisieren **Azure Defender ein** aus.
    1. Klicken Sie auf **Speichern**.

    > [!TIP]
    > Sie werden feststellen, dass jeder Plan in Azure Defender separat abgerechnet wird und einzeln aktiviert oder deaktiviert werden kann. Es kann z. B. sinnvoll sein, Azure Defender für App Service bei Abonnements zu deaktivieren, die nicht über einen verknüpften Azure App Service-Plan verfügen. 

    :::image type="content" source="./media/security-center-pricing/pricing-tier-page.png" alt-text="Preisseite für Security Center im Portal":::

- So aktivieren Sie Azure Defender für mehrere Abonnements oder Arbeitsbereiche:

    1. Wählen Sie auf der Security Center-Randleiste die Option **Erste Schritte** aus.

        Auf der Registerkarte **Upgrade** werden für das Onboarding geeignete Abonnements und Arbeitsbereiche aufgeführt.

        :::image type="content" source="./media/enable-azure-defender/get-started-upgrade-tab.png" alt-text="Registerkarte „Upgrade“ der Seite für die ersten Schritte"::: 

    1. Wählen Sie in der Liste **Abonnements und Arbeitsbereiche für die Aktivierung von Azure Defender auswählen** die Abonnements und Arbeitsbereiche aus, die aktualisiert werden sollen, und wählen Sie dann **Upgrade** aus, um Azure Defender zu aktivieren.

       - Wenn Sie Abonnements und Arbeitsbereiche auswählen, die nicht für eine Testversion geeignet sind, wird für diese im nächsten Schritt ein Upgrade durchgeführt, und es fallen ab diesem Zeitpunkt Kosten an.
       - Wenn Sie einen Arbeitsbereich auswählen, der für eine kostenlose Testversion geeignet ist, wird im nächsten Schritt eine Testversion gestartet.

        :::image type="content" source="./media/enable-azure-defender/upgrade-selected-workspaces-and-subscriptions.png" alt-text="Aktualisieren aller ausgewählten Arbeitsbereiche und Abonnements über die Seite „Erste Schritte“":::


## <a name="disable-azure-defender"></a>Deaktivieren von Azure Defender

Wenn Sie Azure Defender für ein Abonnement deaktivieren müssen, ist das Verfahren identisch, aber Sie wählen **Azure Defender aus** aus:
 
1. Wählen Sie im Menü von Security Center **Preise und Einstellungen** aus.
1. Wählen Sie das relevante Abonnement aus.
1. Wenn für Ihr Abonnement Azure Defender aktiviert ist, öffnen Sie **Azure Defender-Pläne**, und wählen Sie **Azure Defender aus** aus.

    :::image type="content" source="./media/enable-azure-defender/disable-plans.png" alt-text="Aktivieren oder Deaktivieren von Azure Defender":::

1. Wählen Sie **Speichern** aus.

> [!NOTE]
> Nachdem Sie Azure Defender deaktiviert haben – unabhängig davon, ob Sie einen einzelnen Plan oder alle Pläne gleichzeitig deaktivieren – kann die Datensammlung für einen kurzen Zeitraum fortgesetzt werden. 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Azure Defender aktiviert haben, aktivieren Sie die automatische Datensammlung durch die erforderlichen Agents und Erweiterungen, wie unter [Automatische Bereitstellung von Agents und Erweiterungen](security-center-enable-data-collection.md) beschrieben.
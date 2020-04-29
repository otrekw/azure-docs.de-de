---
title: Herstellen einer Verbindung mit einem Azure Synapse-Arbeitsbereich mit privaten Links
description: In diesem Artikel erfahren Sie, wie Sie über private Links eine Verbindung mit Ihrem Azure Synapse-Arbeitsbereich herstellen.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: how-to
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 5a00fc44021278a8b910cf454b43b0bae2c3a1f9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427742"
---
# <a name="connect-to-your-azure-synapse-workspace-using-private-links-preview"></a>Herstellen einer Verbindung mit Ihrem Azure Synapse-Arbeitsbereich mit privaten Links (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einen private Endpunkt in Ihrem Azure Synapse-Arbeitsbereich erstellen. Weitere Informationen finden Sie unter [private Links und private Endpunkte](https://docs.microsoft.com/azure/private-link/).

## <a name="step-1-open-your-azure-synapse-workspace-in-azure-portal"></a>Schritt 1: Öffnen Ihres Azure Synapse-Arbeitsbereichs im Azure-Portal

Wählen Sie **Private Endpunktverbindungen** unter **Sicherheit** aus und dann **+ Privater Endpunkt**.
![Öffnen eines Azure Synapse-Arbeitsbereichs im Azure-Portal](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-1.png)

## <a name="step-2-select-your-subscription-and-region-details"></a>Schritt 2: Auswählen Ihres Abonnements und der Regionsdetails

Wählen Sie auf der Registerkarte **Grundlagen** im Fenster **Privaten Endpunkt erstellen** Ihr **Abonnement** und Ihre **Ressourcengruppe** aus. Geben Sie dem privaten Endpunkt, den Sie erstellen möchten, einen **Namen**. Wählen Sie die **Region** aus, in der der private Endpunkt erstellt werden soll.

Private Endpunkte werden in einem Subnetz erstellt. Durch die Auswahl des Abonnements, der Ressourcengruppe und der Region werden die privaten Endpunktsubnetze gefiltert. Klicken Sie auf **Weiter: Ressource >** , wenn Sie fertig sind.
![Auswählen von Abonnement und Regionsdetails](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-2.png)

## <a name="step-3-select-your-azure-synapse-workspace-details"></a>Schritt 3: Auswählen der Details Ihres Azure Synapse-Arbeitsbereichs

Wählen Sie auf der Registerkarte **Ressource** die Option **Mit einer Azure-Ressource in meinem Verzeichnis verbinden** aus. Wählen Sie das **Abonnement** aus, das Ihren Azure Synapse-Arbeitsbereich enthält. Der **Ressourcentyp** zum Erstellen privater Endpunkte in einem Azure Synapse-Arbeitsbereich ist *Microsoft.synapse/workspaces*.

Wählen Sie Ihren Azure Synapse-Arbeitsbereich als die **Ressource** aus Jeder Azure Synapse-Arbeitsbereich verfügt über drei **Untergeordnete Zielressourcen**, für die Sie einen privaten Endpunkt erstellen können:  „Sql“, „SqlOnDemand“ und „Dev“.

Klicken Sie auf **Weiter: Konfiguration>** , um mit dem nächsten Teil der Einrichtung fortzufahren.
![Auswählen von Abonnement und Regionsdetails](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-3.png)

Wählen Sie auf der Registerkarte **Konfiguration** das **Virtuelle Netzwerk** und das **Subnetz** aus, in dem Sie den privaten Endpunkt erstellen möchten. Außerdem müssen Sie einen DNS-Eintrag erstellen, der dem privaten Endpunkt zugeordnet ist.

Wählen Sie für **In private DNS-Zone integrieren** den Wert **Ja** aus, um Ihren privaten Endpunkt in eine private DNS-Zone zu integrieren. Wenn Sie Ihrem VNet keine private DNS-Zone zugeordnet haben, wird eine neue private DNS-Zone erstellt. Wählen Sie **Überprüfen + erstellen** aus, wenn Sie fertig sind.

![Auswählen von Abonnement und Regionsdetails](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-4.png)

Öffnen Sie nach Abschluss der Bereitstellung Ihren Azure Synapse-Arbeitsbereich im Azure-Portal, und wählen Sie **Private Endpunktverbindungen** aus. Der neue private Endpunkt und der Name der diesem zugeordneten privaten Endpunktverbindung werden angezeigt.

![Auswählen von Abonnement und Regionsdetails](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-5.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [VNets mit verwalteten Arbeitsbereichen](./synapse-workspace-managed-vnet.md).

Weitere Informationen zu [Verwalteten privaten Endpunkten](./synapse-workspace-managed-private-endpoints.md).

[Erstellen von verwalteten privaten Endpunkten für Ihre Datenquellen](./how-to-create-managed-private-endpoints.md)

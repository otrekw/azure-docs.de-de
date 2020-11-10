---
title: Konfigurieren von IP-Firewallregeln
description: In diesem Artikel lernen Sie, wie Sie IP-Firewallregeln in Azure Synapse Analytics konfigurieren.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: security
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 00b4f38f4a2a44b88718224db2b2442033816f83
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93313901"
---
# <a name="azure-synapse-analytics-ip-firewall-rules-preview"></a>IP-Firewallregeln in Azure Synapse Analytics (Vorschau)

In diesem Artikel werden IP-Firewallregeln erläutert, und Sie erfahren, wie Sie diese in Azure Synapse Analytics konfigurieren.

## <a name="ip-firewall-rules"></a>IP-Firewallregeln

IP-Firewallregeln gewähren oder verweigern den Zugriff auf Ihren Synapse-Arbeitsbereich basierend auf der IP-Quelladresse der jeweiligen Anforderung. Sie können IP-Firewallregeln für Ihren Arbeitsbereich konfigurieren. Auf Arbeitsbereichsebene konfigurierte IP-Firewallregeln gelten für alle öffentlichen Endpunkte des Arbeitsbereichs (dedizierte SQL-Pools, serverloser SQL-Pool und Entwicklung).

## <a name="create-and-manage-ip-firewall-rules"></a>Erstellen und Verwalten von IP-Firewallregeln

Es gibt zwei Möglichkeiten, um einem Synapse-Arbeitsbereich IP-Firewallregeln hinzuzufügen. Um eine IP-Firewall zu Ihrem Arbeitsbereich hinzuzufügen, wählen Sie **Sicherheit + Netzwerk** aus, und aktivieren beim Erstellen des Arbeitsbereichs die Option **Verbindungen von allen IP-Adressen zulassen**.

![Screenshot, auf dem die Schaltfläche „Sicherheit + Netzwerk“ hervorgehoben ist](./media/synpase-workspace-ip-firewall/ip-firewall-1.png)

![IP-Konfiguration für einen Synapse-Arbeitsbereich im Azure-Portal.](./media/synpase-workspace-ip-firewall/ip-firewall-2.png)

Sie können einem Synapse-Arbeitsbereich auch nach der Erstellung IP-Firewallregeln hinzufügen. Wählen Sie dazu im Azure-Portal im Bereich **Sicherheit** die Option **Firewalls** aus. Um eine neue IP-Firewallregel hinzuzufügen, geben Sie einen Namen sowie eine Start-IP-Adresse und eine End-IP-Adresse an. Wenn Sie abschließend **Speichern** aus.

![IP-Konfiguration für einen Azure Synapse-Arbeitsbereich im Azure-Portal.](./media/synpase-workspace-ip-firewall/ip-firewall-3.png)

## <a name="connect-to-synapse-from-your-own-network"></a>Herstellen einer Verbindung mit Synapse aus dem eigenen Netzwerk

Sie können über Synapse Studio eine Verbindung mit Ihrem Synapse-Arbeitsbereich herstellen. Sie können auch SQL Server Management Studio (SSMS) verwenden, um eine Verbindung mit den SQL-Ressourcen (dedizierte SQL-Pools und serverloser SQL-Pool) in Ihrem Arbeitsbereich herzustellen.

Stellen Sie sicher, dass die Firewall in Ihrem Netzwerk und auf Ihrem lokalen Computer ausgehende TCP-Kommunikation für Synapse Studio über die TCP-Ports 80, 443 und 1443 zulässt.

Sie müssen auch die ausgehende Kommunikation über den UDP-Port 53 für Synapse Studio zulassen. Um eine Verbindung mithilfe von Tools wie SSMS oder Power BI herzustellen, müssen Sie die ausgehende Kommunikation über den TCP-Port 1433 zulassen.

Wenn Sie die standardmäßige Richtlinieneinstellung für die Umleitung von Verbindungen verwenden, müssen Sie möglicherweise die ausgehende Kommunikation über weitere Ports zulassen. Mehr über Verbindungsrichtlinien erfahren Sie [hier](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy).

## <a name="next-steps"></a>Nächste Schritte

Erstellen eines [Azure Synapse-Arbeitsbereichs](../quickstart-create-workspace.md)

Erstellen eines Azure Synapse-Arbeitsbereichs mit einem [verwalteten virtuellen Arbeitsbereichsnetzwerk](./synapse-workspace-managed-vnet.md)
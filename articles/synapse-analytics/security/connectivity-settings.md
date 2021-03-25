---
title: Konnektivitätseinstellungen für Azure Synapse
description: In diesem Artikel lernen Sie, wie Sie Konnektivitätseinstellungen in Azure Synapse Analytics konfigurieren.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: security
ms.date: 03/15/2021
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: e0d8a8e3320b49b6fbe3e8ab66c0b4569fac9afd
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104587932"
---
# <a name="azure-synapse-analytics-connectivity-settings"></a>Konnektivitätseinstellungen für Azure Synapse Analytics

In diesem Artikel werden die Konnektivitätseinstellungen in Azure Synapse Analytics beschrieben und erläutert, wie sie ggf. konfiguriert werden.


## <a name="connection-policy"></a>Verbindungsrichtlinie
Die Verbindungsrichtlinie für Synapse SQL in Azure Synapse Analytics ist auf *Standard* festgelegt. Dies kann in Azure Synapse Analytics nicht geändert werden. Weitere Informationen dazu, wie sich dies auf Verbindungen mit Synapse SQL in Azure Synapse Analytics auswirkt, finden Sie [hier](../../azure-sql/database/connectivity-architecture.md#connection-policy). 

## <a name="minimal-tls-version"></a>TLS-Mindestversion
Synapse SQL in Azure Synapse Analytics ermöglicht Verbindungen unter Verwendung aller TLS-Versionen. Sie können die TLS-Mindestversion für Synapse SQL in Azure Synapse Analytics nicht festlegen.

## <a name="next-steps"></a>Nächste Schritte

Erstellen eines [Azure Synapse-Arbeitsbereichs](./synapse-workspace-ip-firewall.md)
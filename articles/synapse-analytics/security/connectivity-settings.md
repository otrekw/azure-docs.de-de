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
ms.openlocfilehash: ce1a4808833cbd897da17f9ad75af346538d23d1
ms.sourcegitcommit: 66ce33826d77416dc2e4ba5447eeb387705a6ae5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2021
ms.locfileid: "103472777"
---
# <a name="azure-synapse-analytics-connectivity-settings"></a>Konnektivitätseinstellungen für Azure Synapse Analytics

In diesem Artikel werden die Konnektivitätseinstellungen in Azure Synapse Analytics beschrieben und erläutert, wie sie ggf. konfiguriert werden.


## <a name="connection-policy"></a>Verbindungsrichtlinie
Die Verbindungsrichtlinie für Synapse SQL in Azure Synapse Analytics ist auf *Standard* festgelegt. Dies kann in Azure Synapse Analytics nicht geändert werden. Weitere Informationen dazu, wie sich dies auf Verbindungen mit Synapse SQL in Azure Synapse Analytics auswirkt, finden Sie [hier](https://docs.microsoft.com/azure/azure-sql/database/connectivity-architecture#connection-policy). 

## <a name="minimal-tls-version"></a>TLS-Mindestversion
Synapse SQL in Azure Synapse Analytics ermöglicht Verbindungen unter Verwendung aller TLS-Versionen. Sie können die TLS-Mindestversion für Synapse SQL in Azure Synapse Analytics nicht festlegen.

## <a name="next-steps"></a>Nächste Schritte

Erstellen eines [Azure Synapse-Arbeitsbereichs](./synapse-workspace-ip-firewall.md)

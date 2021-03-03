---
title: IT Service Management Connector in Azure Monitor
description: Dieser Artikel enthält Informationen dazu, wie Sie Ihre ITSM-Produkte bzw. -Dienste mit dem IT Service Management-Connector (ITSMC) in Azure Monitor verbinden, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/12/2020
ms.openlocfilehash: 7ac842df17e80b49990d89d1623330c4e31ab566
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100601792"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>Verbinden von ITSM-Produkten/-Diensten mit dem ITSM-Connector
Dieser Artikel bietet Informationen dazu, wie Sie die Verbindung zwischen Ihrem ITSM-Produkt bzw. -Dienst und dem ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um Arbeitselemente zentral zu verwalten. Weitere Informationen zu ITSMC finden Sie in der [Übersicht](../platform/itsmc-overview.md).

Die folgenden ITSM-Produkte und -Dienste werden unterstützt. Wählen Sie ein Produkt aus, um detaillierte Informationen dazu zu erhalten, wie Sie das Produkt mit ITSMC verbinden.

- [ServiceNow](../platform/itsmc-connections-servicenow.md)
- [System Center Service Manager](../platform/itsmc-connections-scsm.md)
- [Cherwell](../platform/itsmc-connections-cherwell.md)
- [Provance](../platform/itsmc-connections-provance.md)

> [!NOTE]
> Unseren Cherwell- und Provance-Kunden schlagen wir die Verwendung der [Webhookaktion](../platform/action-groups.md#webhook) für Cherwell- und Provance-Endpunkte als weitere Lösung für die Integration vor.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung in ITSM-Connector](../platform/itsmc-resync-servicenow.md)
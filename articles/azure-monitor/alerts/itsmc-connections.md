---
title: IT Service Management Connector in Azure Monitor
description: Dieser Artikel enthält Informationen dazu, wie Sie Ihre ITSM-Produkte bzw. -Dienste mit dem IT Service Management-Connector (ITSMC) in Azure Monitor verbinden, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/12/2020
ms.openlocfilehash: 046fcb9d7473de5666b3acb25cbcaa1f9549e679
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102039494"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>Verbinden von ITSM-Produkten/-Diensten mit dem ITSM-Connector
Dieser Artikel bietet Informationen dazu, wie Sie die Verbindung zwischen Ihrem ITSM-Produkt bzw. -Dienst und dem ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um Arbeitselemente zentral zu verwalten. Weitere Informationen zu ITSMC finden Sie in der [Übersicht](./itsmc-overview.md).

Die folgenden ITSM-Produkte und -Dienste werden unterstützt. Wählen Sie ein Produkt aus, um detaillierte Informationen dazu zu erhalten, wie Sie das Produkt mit ITSMC verbinden.

- [ServiceNow](./itsmc-connections-servicenow.md)
- [System Center Service Manager](./itsmc-connections-scsm.md)
- [Cherwell](./itsmc-connections-cherwell.md)
- [Provance](./itsmc-connections-provance.md)

> [!NOTE]
> Unseren Cherwell- und Provance-Kunden schlagen wir die Verwendung der [Webhookaktion](./action-groups.md#webhook) für Cherwell- und Provance-Endpunkte als weitere Lösung für die Integration vor.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)
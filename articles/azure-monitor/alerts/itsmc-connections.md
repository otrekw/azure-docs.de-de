---
title: IT Service Management Connector in Azure Monitor
description: Dieser Artikel enthält Informationen dazu, wie Sie Ihre ITSM-Produkte bzw. -Dienste mit dem IT Service Management-Connector (ITSMC) in Azure Monitor verbinden, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/12/2020
ms.openlocfilehash: 40e737a1ec5fb34cd22a08925143a100d36cdc6b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103009316"
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

## <a name="ip-ranges-for-itsm-partners-connections"></a>IP-Adressbereiche für ITSM-Partnerverbindungen
Wenn Sie die ITSM-IP-Adressen auflisten möchten, um ITSM-Verbindungen von Partner-ITSM-Tools zuzulassen, empfiehlt es sich, den gesamten öffentlichen IP-Adressbereich der Azure-Region aufzulisten, zu der der Log Analytics-Arbeitsbereich gehört. [Weitere Details finden Sie hier.](https://www.microsoft.com/en-us/download/details.aspx?id=56519) Für die Regionen „Europa, Süden“, „Europa, Westen“, „Europa, Süden 2“, „USA, Westen 2“, „USA, Süden-Mitte“ können Kunden nur das Netzwerktag „ActionGroup“ auflisten.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)

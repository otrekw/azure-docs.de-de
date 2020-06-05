---
title: Azure Event Grid-Partnerthemen
description: Mit Azure Event Grid können Sie Ereignisse von SaaS- und PaaS-Drittanbieterpartnern direkt an Azure-Dienste senden.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: babanisa
ms.openlocfilehash: 4546874b8a6cd8a7e45c3e6957a5181d66c7433f
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83690036"
---
# <a name="partner-topics-in-azure-event-grid-preview"></a>Partnerthemen in Azure Event Grid (Vorschau)
Mit dem Feature „Partnerthemen“ können Sie Ereignisquellen von Drittanbietern direkt mit Event Grid verbinden. Diese Integration ermöglicht Ihnen, Ereignisse von Partnern auf dieselbe Weise wie Ereignisse von Azure-Diensten zu abonnieren. 

## <a name="available-partners"></a>Verfügbare Partner
Der erste Partner, der über Event Grid-Partnerthemen verfügbar ist, ist Auth0. Das [Partnerthema Auth0](auth0-overview.md) ermöglicht Ihnen die Herstellung einer Verbindung Ihrer Auth0- und Azure-Konten. Die Integration ermöglicht Ihnen, in Echtzeit auf Auth0-Ereignisse zu reagieren, diese zu protokollieren und zu überwachen.

[Probieren Sie es noch heute aus](auth0-how-to.md), indem Sie sich bei Ihrem Auth0-Konto anmelden und eine Event Grid-Integration vornehmen. Nachdem Sie in Auth0 auf „Create“ (Erstellen) geklickt haben, sehen Sie in Ihrem Azure-Konto ein ausstehendes Auth0-Thema. Klicken Sie auf „Aktivieren“. Anschließend können Sie Abonnements für Event Grid erstellen, um Ihre Ereignisse wie jede andere Ereignisquelle weiterzuleiten, zu filtern und zu übermitteln.

## <a name="pricing"></a>Preise
Partnerthemen werden zum gleichen Tarif wie Systemthemen in Rechnung gestellt.

## <a name="limits"></a>Einschränkungen
Partnerthemen sind in der Public Preview-Phase. In dieser Phase gelten für Partnerthemen die [ gleichen Grenzwerte](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#event-grid-limits) wie für System- und benutzerdefinierte Themen.

## <a name="how-do-i-become-an-event-grid-partner"></a>Wie werde ich Event Grid-Partner?
Die zur Unterstützung dieser Produkteinführung geschaffene Infrastruktur ermöglicht neuen Partnern einfach und schnell, ihre Ereignisverarbeitungskapazitäten in Event Grid zu integrieren. Weitere Informationen finden Sie in der [Dokumentation zum Onboarding von Partnern](partner-onboarding-overview.md).

## <a name="next-steps"></a>Nächste Schritte

- [Auth0-Partnerthema](auth0-overview.md)
- [Verwenden des Auth0-Partnerthemas](auth0-how-to.md)
- [Registrieren als Event Grid-Partner](partner-onboarding-overview.md)
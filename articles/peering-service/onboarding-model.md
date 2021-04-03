---
title: Onboardingmodell für Azure Peering Service
description: Erfahren Sie, wie Sie Azure Peering Service integrieren.
services: peering-service
documentationcenter: na
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 7a257b4c2bea3bd3384a55c0a6b85d7fdd2ca583
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "84871400"
---
# <a name="onboarding-peering-service-model"></a>Onboardingmodell für Peering Service

Der Onboardingvorgang für Peering Service umfasst zwei Modelle, die unten aufgeführt sind:

 - Onboarding einer Peering Service-Verbindung

 - Telemetrie für das Onboarding einer Peering Service-Verbindung

Die Aktionspläne für die oben aufgeführten Modelle werden nachstehend beschrieben:

| **Schritt** | **Aktion**| **Ergebnis**|
|-----------|---------|---------|---------|
|1|Der Kunde stellt die Verbindung von einem Konnektivitätspartner bereit (keine Interaktion mit Microsoft). |Ein Internetanbieter unterhält gute Verbindungen mit Microsoft und erfüllt die technischen Anforderungen für leistungsstarke und zuverlässige Konnektivität mit Microsoft.  |
|2 (optional)|Der Kunde registriert Standorte im Azure-Portal. Ein Standort ist folgendermaßen definiert: ISP/IXP-Name, physischer Standort der Kundenzweigstelle (nach Bundesstaat), vom Dienstanbieter oder dem Unternehmen zugewiesenes IP-Präfix für den Standort.  |Telemetrie: Internetroutenüberwachung, Datenverkehrspriorisierung von Microsoft zum nächstgelegenen PoP-Standort des Benutzers. |



## <a name="onboarding-peering-service-connection"></a>Onboarding einer Peering Service-Verbindung

Gehen Sie zum Onboarding der Peering Service-Verbindung wie folgt vor:

Erhalten Sie in Zusammenarbeit mit einem Internetdienst- (ISP) oder Internet Exchange-Anbieter (IX) Peering Service, um eine Verbindung zwischen Ihrem Netzwerk und dem Microsoft-Netzwerk herzustellen.

Achten Sie darauf, dass es sich bei den [Konnektivitätsanbietern](location-partners.md) um Microsoft-Partner für Peering Service handelt. 

## <a name="onboarding-peering-service-connection-telemetry"></a>Telemetrie für das Onboarding einer Peering Service-Verbindung

Kunden können Telemetrie wie BGP-Routenanalysen abonnieren, um die Netzwerklatenz und -leistung beim Zugriff auf das Microsoft-Netzwerk zu überwachen. Dies kann durch Registrieren der Verbindung im Azure-Portal erreicht werden.

Zum Integrieren der Peering Service-Verbindungstelemetrie muss der Kunde die Peering Service-Verbindung im Azure-Portal registrieren. Weitere Informationen zur Vorgehensweise finden Sie unter [Registrieren von Peering Service – Azure-Portal](azure-portal.md).

Anschließend können Sie die Telemetrie anhand der Informationen [hier](measure-connection-telemetry.md) messen.

## <a name="next-steps"></a>Nächste Schritte

Eine Schritt-für-Schritt-Anleitung zum Registrieren der Peering Service-Verbindung finden Sie unter [Registrieren von Peering Service – Azure-Portal](azure-portal.md).

Informationen zum Messen der Verbindungstelemetrie finden Sie unter [Messen der Verbindungstelemetrie](measure-connection-telemetry.md).

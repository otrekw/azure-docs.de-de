---
title: Wartungsfenster
description: Erfahren Sie, wie Wartungsfenster für Azure SQL-Datenbank und Azure SQL Managed Instance konfiguriert werden können.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.topic: conceptual
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.custom: references_regions
ms.date: 03/02/2021
ms.openlocfilehash: 4006cedf5f24ab2fc08e41b58f8acf90c404f668
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678507"
---
# <a name="maintenance-window-preview"></a>Wartungsfenster (Vorschau)
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Die Wartungsfensterfunktion bietet die Möglichkeit, Zeitpläne für vorhersehbare Wartungsfenster für [Azure SQL-Datenbank](sql-database-paas-overview.md) und [Azure SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md) zu konfigurieren. 

Weitere Informationen zu Wartungsereignissen finden Sie unter [Planen von Azure-Wartungsereignissen für Azure SQL-Datenbank und Azure SQL Managed Instance](planned-maintenance.md).

## <a name="overview"></a>Übersicht

Azure führt regelmäßig geplante Wartungsupdates für Ressourcen in Azure SQL-Datenbank und Azure SQL Managed Instance aus, die häufig Aktualisierungen der zugrundeliegenden Hardware, der Software (einschließlich des zugrundeliegenden Betriebssystems) und der SQL-Engine beinhalten. Während eines Wartungsupdates sind die Ressourcen in vollem Umfang verfügbar und zugänglich, doch ist bei einigen Wartungsupdates ein Failover erforderlich, da Azure zum Anwenden der Wartungsupdates die Instanzen für kurze Zeit offline schaltet (im Schnitt etwa acht Sekunden).  Geplante Wartungsupdates erfolgen im Schnitt alle 35 Tage. Dies bedeutet, dass der Kunde mit etwa einem geplanten Wartungsereignis pro Monat für jede Azure SQL-Datenbank oder verwaltete SQL-Instanz rechnen kann, das nur in dem vom Kunden ausgewählten Wartungszeitfenster erfolgt.   

Das Wartungsfenster ist für geschäftliche Workloads gedacht, die empfindlich auf mögliche Verbindungsunterbrechungen reagieren, die durch geplante Wartungsereignisse während des Standardwartungsfensters entstehen können.  

Das Wartungsfenster kann im Azure-Portal, mit Azure PowerShell, der Befehlszeilenschnittstelle (CLI) oder der Azure-API konfiguriert werden. Es kann beim Erstellen von SQL-Datenbanken und verwalteten SQL-Instanzen, aber auch für vorhandene SQL-Datenbanken und verwaltete SQL-Instanzen konfiguriert werden.

### <a name="gain-more-predictability-with-maintenance-window"></a>Mehr Vorhersehbarkeit bei Wartungsfenstern

Standardmäßig werden alle Azure SQL- und Azure SQL Managed Instance-Datenbanken über Nacht zwischen 17 Uhr und 8 Uhr (Ortszeit, an allen Tagen) aktualisiert, um Unterbrechungen während der Hauptgeschäftszeiten zu vermeiden. Die Ortszeit hängt von der [Azure-Region](https://azure.microsoft.com/global-infrastructure/geographies/) ab, in der die Ressource gehostet wird. Zum weiteren Anpassen der Wartungsupdates auf eine für Ihre Datenbank geeignete Zeit stehen zwei zusätzliche Wartungszeitfenster zur Auswahl:

* Wartungsfenster **Standard**, 17 Uhr bis 8 Uhr Ortszeit (Montag-Sonntag) 
* Wartungsfenster „Wochentag“, 22 Uhr bis 6 Uhr Ortszeit (Montag-Donnerstag): **muss vom Kunden aktiviert werden** 
* Wartungsfenster „Wochenende“, 22 Uhr bis 6 Uhr Ortszeit (Freitag-Sonntag): **muss vom Kunden aktiviert werden**  

Nach Auswahl des Wartungsfensters werden alle geplanten Wartungsupdates nur im ausgewählten Wartungsfenster ausgeführt.   

> [!Note]
> Neben den geplanten Wartungsupdates können in seltenen Fällen nicht geplante Wartungsereignisse eine Nichtverfügbarkeit verursachen. 

### <a name="cost"></a>Kosten

Die Auswahl eines Wartungsfensters ist für die folgenden [Abonnement-Angebotstypen](https://azure.microsoft.com/support/legal/offer-details/) kostenlos: Nutzungsbasierte Bezahlung (Pay-As-You-Go), Cloud Solution Provider (CSP), Microsoft Enterprise oder Microsoft-Kundenvereinbarung.

> [!Note]
> Ein Azure-Angebot ist der Typ von Azure-Abonnement, das Sie besitzen. Zum Beispiel handelt es sich bei Abonnements des Typs [Nutzungsbasierte Bezahlung (Pay-As-You-Go)](https://azure.microsoft.com/offers/ms-azr-0003p/), [Azure in Open](https://azure.microsoft.com/en-us/offers/ms-azr-0111p/) und [Visual Studio Enterprise](https://azure.microsoft.com/en-us/offers/ms-azr-0063p/) insgesamt um Azure-Angebote. Jedes Angebot bzw. jeder Plan hat unterschiedliche Vorteile und unterliegt unterschiedlichen Bestimmungen. Ihr Angebot bzw. Ihr Plan wird in der Abonnementsübersicht angezeigt. Wie Sie mit Ihrem Abonnement zu einem anderen Angebot wechseln können, erfahren Sie unter [Ändern Ihres Azure-Abonnements in ein anderes Angebot](/azure/cost-management-billing/manage/switch-azure-offer).

## <a name="advance-notifications"></a>Vorabbenachrichtigungen

Wartungsbenachrichtigungen können so konfiguriert werden, dass Kunden 24 Stunden vor anstehenden geplanten Wartungsereignissen, zum Zeitpunkt der Wartung und am Ende des Wartungsfensters benachrichtigt werden. Weitere Informationen finden Sie unter [Vorabbenachrichtigungen](advance-notifications.md).

## <a name="availability"></a>Verfügbarkeit

### <a name="supported-service-level-objectives"></a>Unterstützte Servicelevelziele

Die Auswahl eines anderen Wartungsfensters anstelle des Standardwartungsfensters ist **mit folgenden Ausnahmen** für alle Servicelevelziele (Service Level Objectives, SLOs) verfügbar:
* Hyperscale 
* Legacy Gen4 vCore
* Basis, S0 und S1 
* DC, Fsv2, M-Serie

### <a name="azure-region-support"></a>Unterstützte Azure-Regionen

Die Auswahl eines anderen Wartungsfensters anstelle des Standardwartungsfensters ist derzeit in folgenden Regionen verfügbar:

- Australien (Osten)
- Australien, Südosten
- Brasilien Süd
- Kanada, Mitte
- USA (Mitte)
- East US
- USA (Ost 2)
- Japan, Osten
- USA, Norden-Mitte
- Nordeuropa
- USA, Süden-Mitte
- Asien, Südosten
- UK, Süden
- Europa, Westen
- USA (Westen)
- USA, Westen 2

## <a name="gateway-maintenance-for-azure-sql-database"></a>Gatewaywartung für Azure SQL-Datenbank

Für die optimale Nutzung von Wartungsfenstern müssen Sie sicherstellen, dass Ihre Clientanwendungen die Verbindungsrichtlinie „Umleiten“ verwenden. Empfohlen wird die Verbindungsrichtlinie „Umleiten“, da die Clients Verbindungen direkt mit dem Knoten herstellen, der die Datenbank hostet. Dies führt zu niedrigerer Latenz und verbessertem Durchsatz.  

* In Azure SQL-Datenbank sind möglicherweise alle Verbindungen, die die Verbindungsrichtlinie „Proxy“ verwenden, von dem ausgewählten Wartungsfenster und einem Wartungsfenster für den Gatewayknoten betroffen. Hingegen sind Clientverbindungen, die die empfohlene Verbindungsrichtlinie „Umleiten“ verwenden, von einem Failover bei der Wartung des Gatewayknotens nicht betroffen. 

* In einer verwalteten Azure SQL-Instanz befinden sich die Gatewayknoten [im virtuellen Cluster](../../azure-sql/managed-instance/connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture) und verfügen über das gleiche Wartungsfenster wie die verwaltete Instanz. Daher sind Verbindungen bei Verwendung der Proxy-Verbindungsrichtlinie möglicherweise nicht von einem zusätzlichen Wartungsfenster betroffen.

Weitere Informationen zur Clientverbindungsrichtlinie in Azure SQL-Datenbank finden Sie unter [Verbindungsrichtlinie von Azure SQL-Datenbank](../database/connectivity-architecture.md#connection-policy). 

Weitere Informationen zu Clientverbindungsrichtlinien in verwalteten Azure SQL-Instanzen finden Sie unter [Azure SQL Managed Instance: Verbindungstypen](../../azure-sql/managed-instance/connection-types-overview.md).


## <a name="next-steps"></a>Nächste Schritte

* [Vorabbenachrichtigungen](advance-notifications.md)
* [Konfigurieren von Wartungsfenstern](maintenance-window-configure.md)

## <a name="learn-more"></a>Weitere Informationen

* [Wartungsfenster – Häufig gestellte Fragen](maintenance-window-faq.yml)
* [Azure SQL-Datenbank](sql-database-paas-overview.md) 
* [SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md)
* [Planen von Azure-Wartungsereignissen in Azure SQL-Datenbank und Azure SQL Managed Instance](planned-maintenance.md)





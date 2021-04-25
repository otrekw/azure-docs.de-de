---
title: Azure Advisor for PostgreSQL
description: Hier lernen Sie Azure Advisor-Empfehlungen für PostgreSQL kennen.
author: alau-ms
ms.author: alau
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/08/2021
ms.openlocfilehash: a44f808daf32a1175035005067dfbb5edd664ef6
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107310052"
---
# <a name="azure-advisor-for-postgresql"></a>Azure Advisor for PostgreSQL
Hier erfahren Sie, wie Azure Advisor auf Azure Database for PostgreSQL angewendet wird, und Sie erhalten Antworten auf häufig gestellte Fragen.
## <a name="what-is-azure-advisor-for-postgresql"></a>Was ist Azure Advisor for PostgreSQL?
Das Azure Advisor-System verwendet Telemetriedaten, um Empfehlungen zur Leistung und Zuverlässigkeit für Ihre PostgreSQL-Datenbank auszugeben. Advisor-Empfehlungen sind auf unsere PostgreSQL-Datenbankangebote aufgeteilt:
* Azure Database for PostgreSQL (Einzelserver)
* Azure Database for PostgreSQL – Flexible Server
* Azure Database for PostgreSQL: Hyperscale (Citus)

Einige Empfehlungen gelten für mehrere Produktangebote, während andere Empfehlungen auf produktspezifischen Optimierungen basieren.
## <a name="where-can-i-view-my-recommendations"></a>Wo kann ich meine Empfehlungen anzeigen?
Empfehlungen finden Sie im Azure-Portal in der Navigationsleiste **Übersicht**. Eine Vorschau wird als Bannerbenachrichtigung eingeblendet, und Details können im Abschnitt **Benachrichtigungen** direkt unterhalb der Graphen zur Ressourcennutzung angezeigt werden.

:::image type="content" source="./media/concepts-azure-advisor-recommendations/advisor-example.png" alt-text="Screenshot des Azure-Portals mit einer Azure Advisor-Empfehlung":::

## <a name="recommendation-types"></a>Empfehlungstypen
In Azure Database for PostgreSQL werden die folgenden Arten von Empfehlungen priorisiert:
* **Leistung**: Hilft Ihnen, die Geschwindigkeit Ihres PostgreSQL-Servers zu verbessern. Dazu gehören CPU-Auslastung, Arbeitsspeicherauslastung, Verbindungspooling, Datenträgerauslastung und produktspezifische Serverparameter. Weitere Informationen finden Sie unter [Advisor-Empfehlungen zur Leistung](../advisor/advisor-performance-recommendations.md).
* **Zuverlässigkeit**: Hilft Ihnen, die ununterbrochene Verfügbarkeit Ihrer unternehmenskritischen Datenbanken zu gewährleisten und zu verbessern. Dazu gehören Speichergrenzwerte, Verbindungsgrenzwerte und Empfehlungen zur Hyperscale-Datenverteilung. Weitere Informationen finden Sie unter [Verbessern der Zuverlässigkeit Ihrer Anwendung mit dem Azure Advisor](../advisor/advisor-high-availability-recommendations.md).
* **Kosten**: Mit dem Ratgeber können Sie Ihre Gesamtausgaben für Azure senken und optimieren. Dazu gehören Empfehlungen für die richtige Dimensionierung des Servers. Weitere Informationen finden Sie unter [Advisor-Empfehlungen zu Kosten](../advisor/advisor-cost-recommendations.md).

## <a name="understanding-your-recommendations"></a>Grundlegendes zu Ihren Empfehlungen
* **Täglicher Zeitplan:** Bei Azure PostgreSQL-Datenbanken überprüfen wir die Servertelemetriedaten und geben Empfehlungen nach einem täglichen Zeitplan aus. Wenn Sie an Ihrer Serverkonfiguration eine Änderung vornehmen, bleiben vorhandene Empfehlungen so lange sichtbar, bis wir die Telemetriedaten am nächsten Tag erneut untersuchen. 
* **Leistungsverlauf:** Einige unserer Empfehlungen basieren auf dem Leistungsverlauf. Diese Empfehlungen werden erst angezeigt, nachdem ein Server 7 Tage lang mit derselben Konfiguration ausgeführt wurde. Dies ermöglicht es uns, Muster mit hoher Auslastung (z. B. hohe CPU-Aktivität oder hohes Verbindungsvolumen) über einen längeren Zeitraum zu erkennen. Wenn Sie einen neuen Server bereitstellen oder zu einer neuen Konfiguration der virtuellen Kerne wechseln, werden diese Empfehlungen vorübergehend angehalten. Dadurch wird verhindert, dass Legacytelemetrie auf einem neu konfigurierten Server Empfehlungen auslöst. Dies bedeutet aber auch, dass leistungsverlaufsbasierte Empfehlungen möglicherweise nicht sofort identifiziert werden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Azure Advisor – Übersicht](../advisor/advisor-overview.md).

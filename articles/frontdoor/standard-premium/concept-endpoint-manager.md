---
title: 'Azure Front Door: Endpoint Manager'
description: Dieser Artikel bietet eine Übersicht über Azure Front Door Endpoint Manager.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: c916be9a54d62e16f488c94f4fa88a2207fb8788
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101098045"
---
# <a name="what-is-azure-front-door-standardpremium-preview-endpoint-manager"></a>Was ist Endpoint Manager in Azure Front Door Standard/Premium (Vorschau)?

> [!NOTE]
> * Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Lesen Sie die [Azure Front Door-Dokumente](../front-door-overview.md).

Endpoint Manager bietet einen Überblick über Endpunkte, die Sie für Azure Front Door konfiguriert haben. Ein Endpunkt ist eine logische Gruppierung von Domänen und ihren Konfigurationen. Endpoint Manager unterstützt Sie bei der Verwaltung Ihrer Sammlung von Endpunkten für den CRUD-Vorgang (Create, Read, Update, Delete – Erstellen, Lesen, Aktualisieren, Löschen). Sie können mit Endpoint Manager die folgenden Elemente für Ihre Endpunkte verwalten:

* Domänen
* Ursprungsgruppen
* Routen
* Sicherheit

:::image type="content" source="../media/concept-endpoint-manager/endpoint-manager-1.png" alt-text="Screenshot von Endpoint Manager ohne Konfigurationen" lightbox="../media/concept-endpoint-manager/endpoint-manager-1-expanded.png":::

In Endpoint Manager wird die Anzahl der in einem Endpunkt erstellten Instanzen jedes Elements aufgeführt. Der Zuordnungsstatus für jedes Element wird ebenfalls angezeigt. Beispielsweise können Sie mehrere Domänen und Ursprungsgruppen erstellen und die Zuordnung zwischen ihnen mit unterschiedlichen Routen zuweisen.

> [!IMPORTANT]
> * Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie in den [**zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen**](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="linked-view"></a>Verknüpfte Ansicht

Mit der verknüpften Ansicht in Endpoint Manager können Sie die Zuordnung zwischen den Azure Front Door-Elementen leicht erkennen, um z. B. Folgendes zu bestimmen:

* Welche Domänen sind dem aktuellen Endpunkt zugeordnet?
* Welche Ursprungsgruppe ist welcher Domäne zugeordnet?
* Welche WAF-Richtlinie ist welcher Domäne zugeordnet?

:::image type="content" source="../media/concept-endpoint-manager/endpoint-manager-2.png" alt-text="Screenshot von Endpoint Manager mit Konfigurationen" lightbox="../media/concept-endpoint-manager/endpoint-manager-2-expanded.png":::

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Erstellen einer Instanz von Front Door Standard/Premium](create-front-door-portal.md).

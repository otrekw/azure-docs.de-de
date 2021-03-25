---
title: Cachebereinigung in Azure Front Door Standard/Premium (Vorschau)
description: In diesem Artikel erfahren Sie, wie Sie den Cache in Azure Front Door Standard/Premium bereinigen.
services: frontdoor
author: duongau
manager: KumudD
ms.service: frontdoor
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: aacbf2ceab8580727b1885bf6533cd74a7c4e60a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101097936"
---
# <a name="cache-purging-in-azure-front-door-standardpremium-preview"></a>Cachebereinigung in Azure Front Door Standard/Premium (Vorschau)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

In Azure Front Door Standard/Premium werden Ressourcen zwischengespeichert, bis ihre Gültigkeitsdauer (Time-to-Live, TTL) abläuft. Wenn ein Client eine Ressource nach Ablauf ihrer Gültigkeitsdauer anfordert, ruft die Azure Front Door-Umgebung eine neue aktualisierte Kopie der Ressource ab, um die Anforderung zu erfüllen und den aktualisierten Cache zu speichern.

Es empfiehlt sich, sicherzustellen, dass die Benutzer immer die neueste Kopie Ihrer Ressourcen erhalten. Zu diesem Zweck erstellen Sie Versionen für die einzelnen Aktualisierungen der Ressourcen und veröffentlichen sie als neue URLs. Azure Front Door Standard/Premium ruft sofort die neuen Ressourcen für die nächsten Clientanforderungen ab. Manchmal möchten Sie möglicherweise zwischengespeicherten Inhalt aus allen Edgeknoten löschen und sie zwingen, neue aktualisierte Assets abzurufen. Sie möchten zwischengespeicherten Inhalt löschen, weil Sie neue Updates Ihrer Anwendung durchgeführt haben oder Ressourcen aktualisieren möchten, die falsche Informationen enthalten.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie [Caching with Azure Front Door Standard/Premium (Preview)](concept-caching.md) (Zwischenspeicherung mit Azure Front Door Standard/Premium (Vorschau), in englischer Sprache), um sich mit der Funktionsweise der Zwischenspeicherung vertraut zu machen.

## <a name="configure-cache-purge"></a>Konfigurieren der Cachebereinigung

1. Wechseln Sie zur Übersichtsseite des Azure Front Door-Profils mit den Ressourcen, die Sie bereinigen möchten, und wählen Sie dann **Cache bereinigen** aus.

   :::image type="content" source="../media/how-to-cache-purge/front-door-cache-purge-1.png" alt-text="Screenshot der Cachebereinigung auf der Übersichtsseite":::

1. Wählen Sie den Endpunkt und die Domäne aus, die Sie von den Edgeknoten löschen möchten. *(Sie können mehrere Domänen auswählen.)*

   :::image type="content" source="../media/how-to-cache-purge/front-door-cache-purge-2.png" alt-text="Screenshot der Seite für die Cachebereinigung":::

1. Wenn Sie alle Ressourcen löschen möchten, wählen Sie **Alle Ressourcen für die ausgewählten Domänen bereinigen** aus. Andernfalls geben Sie in **Pfade** den Pfad jeder Ressource ein, die Sie löschen möchten.

Diese Formate werden in der Liste der zu löschenden Pfade unterstützt:

* **Löschen eines einzelnen Pfads**: Löschen Sie einzelne Ressourcen, indem Sie den vollständigen Pfad der Ressource, ohne Protokoll und Domäne, aber mit der Dateierweiterung angeben. Beispiel: „/pictures/strasbourg.png“.
* **Löschen der Stammdomäne**: Löschen Sie den Stamm des Endpunkts, indem Sie „/*“ im Pfad angeben.

Bei Cachebereinigungen in Azure Front Door Standard/Premium wird die Groß-/Kleinschreibung nicht beachtet. Darüber hinaus sind Cachebereinigungen abfragezeichenfolgenagnostisch, d. h. beim Bereinigen einer URL werden alle Abfragezeichenfolgenvariationen der URL gelöscht. 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Erstellen einer Instanz von Front Door Standard/Premium](create-front-door-portal.md).

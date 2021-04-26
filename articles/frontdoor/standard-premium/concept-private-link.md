---
title: Sichern ihres Ursprungs mit Private Link in Azure Front Door Standard/Premium (Vorschau)
description: Diese Seite enthält Informationen zum Sichern der Konnektivität mit Ihrem Ursprung mithilfe von Private Link.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: tyao
ms.custom: references_regions
ms.openlocfilehash: c927682231bab1fc92f198c56a2f6aeff2f4c516
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106553531"
---
# <a name="secure-your-origin-with-private-link-in-azure-front-door-standardpremium-preview"></a>Sichern ihres Ursprungs mit Private Link in Azure Front Door Standard/Premium (Vorschau)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Lesen Sie die [Azure Front Door-Dokumente](../front-door-overview.md).

## <a name="overview"></a>Übersicht

Mit [Azure Private Link](../../private-link/private-link-overview.md) können Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf Azure-PaaS-Dienste sowie auf in Azure gehostete Dienste zugreifen. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbone-Netzwerk übertragen und dadurch vom öffentlichen Internet isoliert.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Die Azure Front Door Premium-SKU kann über einen Private Link-Dienst eine Verbindung mit ihrem Ursprung herstellen. Ihre Anwendungen können in Ihrem privaten VNet oder hinter einem PaaS-Dienst (z. B. Web-App und Speicherkonto) gehostet werden. Dadurch entfällt die Notwendigkeit, dass Ihr Ursprung öffentlich zugänglich ist.

:::image type="content" source="../media/concept-private-link/front-door-private-endpoint-architecture.png" alt-text="Architektur von privaten Front Door-Endpunkten":::

Wenn Sie Private Link für Ihren Ursprung in der Azure Front Door Premium-Konfiguration aktivieren, erstellt Front Door in Ihrem Namen einen privaten Endpunkt aus dem regionalen privaten Netzwerk von Front Door. Dieser Endpunkt wird von Azure Front Door verwaltet. Sie erhalten eine Nachricht zur Genehmigungsanforderung für einen privaten Endpunkt von Azure Front Door an Ihrem Ursprung. Nachdem Sie die Anforderung genehmigt haben, wird eine private IP-Adresse aus dem virtuellen Netzwerk von Front Door zugewiesen. Der Datenverkehr zwischen Azure Front Door und Ihrem Ursprung durchläuft die eingerichtete private Verbindung mit dem Backbone des Azure-Netzwerks. Eingehender Datenverkehr zu Ihrem Ursprung ist jetzt gesichert, wenn er von Ihrer Azure Front Door-Instanz stammt.

:::image type="content" source="../media/concept-private-link/enable-private-endpoint.png" alt-text="Aktivieren eines privaten Endpunkts":::

> [!NOTE]
> Nachdem Sie einen Private Link-Ursprung aktiviert und die Verbindung mit dem privaten Endpunkt genehmigt haben, dauert es einige Minuten, bis die Verbindung hergestellt ist. Während dieser Zeit erhalten Anforderungen an den Ursprung eine Front Door-Fehlermeldung. Sobald die Verbindung hergestellt ist, wird die Fehlermeldung nicht mehr angezeigt.

## <a name="limitations"></a>Einschränkungen

Private Azure Front Door-Endpunkte sind in den folgenden Regionen während der öffentlichen Vorschau verfügbar: „USA, Osten“, „USA, Westen 2“ und „Vereinigtes Königreich, Süden“.

Für die kürzeste Wartezeit sollten Sie immer eine Azure-Region auswählen, die Ihrem Ursprung am nächsten ist, wenn Sie den Private Link-Endpunkt von Front Door aktivieren.

Die privaten Endpunkte von Azure Front Door werden von der Plattform verwaltet und unterliegen dem Abonnement von Azure Front Door. Azure Front Door ermöglicht Private Link-Verbindungen mit demselben Kundenabonnement, das zum Erstellen des Front Door-Profils verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Herstellen einer Verbindung zwischen Azure Front Door Premium und Ihrer Web-App über den Private Link-Dienst finden Sie unter [Herstellen einer Verbindung mit einer Web-App über einen privaten Endpunkt](../../private-link/tutorial-private-endpoint-webapp-portal.md).
* Informationen zum Herstellen einer Verbindung zwischen Azure Front Door Premium und Ihrem Speicherkonto über den Private Link-Dienst finden Sie unter [Herstellen einer Verbindung mit einem Speicherkonto über einen privaten Endpunkt](../../private-link/tutorial-private-endpoint-storage-portal.md).

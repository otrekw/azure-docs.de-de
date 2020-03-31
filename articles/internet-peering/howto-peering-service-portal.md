---
title: Aktivieren von Peering Service für direktes Peering über das Portal
titleSuffix: Azure
description: Aktivieren von Peering Service für direktes Peering über das Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: 87a65826a338f4098ae24c33939ea7f9d4475e36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129959"
---
# <a name="enable-peering-service-on-a-direct-peering-using-the-portal"></a>Aktivieren von Peering Service für direktes Peering über das Portal

In diesem Artikel wird beschrieben, wie Sie [Peering Service](overview-peering-service.md) für ein Direct Peering mithilfe des Portals aktivieren.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-peering-service-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor Beginn der Konfiguration die [Voraussetzungen](prerequisites.md).
* Wählen Sie ein Direct Peering in Ihrem Abonnement aus, für das Sie den Peering Service aktivieren möchten. Wenn Sie keins besitzen, konvertieren Sie entweder ein Legacy-Direct Peering, oder erstellen Sie ein neues Direct Peering.
    * Um ein Legacy-Direct Peering zu konvertieren, befolgen Sie die Anweisungen in [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource über das Portal](howto-legacy-direct-portal.md).
    * Um ein neues Direct Peering zu erstellen, befolgen Sie die Anweisungen in [Erstellen oder Ändern eines Direct Peerings über das Portal](howto-direct-portal.md).

## <a name="enable-peering-service-on-a-direct-peering"></a>Aktivieren von Peering Service für direktes Peering

### <a name="view-direct-peering"></a><a name= get></a>Direct Peering anzeigen
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Aktivieren von Direct Peering für Peering Service

Nachdem Sie im vorherigen Schritt Direct Peering geöffnet haben, aktivieren Sie es für Peering Service.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Ändern einer Direct Peering-Verbindung

Wenn Sie die Verbindungseinstellungen ändern müssen, finden Sie weitere Informationen im Abschnitt **Ändern eines Direct Peerings** unter [Erstellen oder Ändern eines Direct Peerings über das Portal.](howto-direct-portal.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
* [Konvertieren einer älteren Instanz für Exchange Peering in eine Azure-Ressource über das Portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Häufig gestellte Fragen finden Sie unter [Peering Service: Häufig gestellte Fragen](service-faqs.md).
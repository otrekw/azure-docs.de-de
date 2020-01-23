---
title: Aktivieren von Peering Service für direktes Peering über das Portal
titleSuffix: Azure
description: Aktivieren von Peering Service für direktes Peering über das Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d6b67c42ef8a5ba5ae98894775d1f56cee39ba8b
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773873"
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

### <a name= get></a>Direct Peering anzeigen
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name= get></a>Aktivieren von Direct Peering für Peering Service

Nachdem Sie im vorherigen Schritt Direct Peering geöffnet haben, aktivieren Sie es für Peering Service.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Ändern einer Direct Peering-Verbindung

Wenn Sie die Verbindungseinstellungen ändern müssen, finden Sie weitere Informationen im Abschnitt **Ändern eines Direct Peerings** unter [Erstellen oder Ändern eines Direct Peerings über das Portal.](howto-direct-portal.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
* [Konvertieren einer älteren Instanz für Exchange Peering in eine Azure-Ressource über das Portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Häufig gestellte Fragen finden Sie unter [Peering Service: Häufig gestellte Fragen](service-faqs.md).
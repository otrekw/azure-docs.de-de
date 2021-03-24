---
title: Aktivieren von Azure Peering Service für Direct Peering über das Azure-Portal
titleSuffix: Azure
description: Aktivieren von Azure Peering Service für Direct Peering über das Azure-Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: a52e6038b622c004dc0d133394cd4f53600b2935
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "84700041"
---
# <a name="enable-azure-peering-service-on-a-direct-peering-by-using-the-azure-portal"></a>Aktivieren von Azure Peering Service für Direct Peering über das Azure-Portal

In diesem Artikel wird beschrieben, wie Sie Azure [Peering Service](overview-peering-service.md) für ein Direct Peering mithilfe des Azure-Portals aktivieren.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-peering-service-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor Beginn der Konfiguration die [Voraussetzungen](prerequisites.md).
* Wählen Sie ein Direct Peering in Ihrem Abonnement aus, für das Sie den Peering Service aktivieren möchten. Wenn Sie keins besitzen, konvertieren Sie entweder ein Legacy-Direct Peering, oder erstellen Sie ein neues Direct Peering:
    * Um ein Legacy-Direct Peering zu konvertieren, befolgen Sie die Anweisungen in [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource mithilfe des Portals](howto-legacy-direct-portal.md).
    * Um ein neues Direct Peering zu erstellen, befolgen Sie die Anweisungen in [Erstellen oder Ändern eines Direct Peerings über das Portal](howto-direct-portal.md).

## <a name="enable-peering-service-on-a-direct-peering"></a>Aktivieren von Peering Service für direktes Peering

### <a name="view-direct-peering"></a><a name= get></a>Direct Peering anzeigen
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Aktivieren von Direct Peering für Peering Service

Nachdem Sie im vorherigen Schritt ein Direct Peering geöffnet haben, aktivieren Sie es für den Peering Service.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Ändern einer Direct Peering-Verbindung

Informationen zum Ändern von Verbindungseinstellungen finden Sie im Abschnitt „Ändern eines Direct Peerings“ unter [Erstellen oder Ändern eines Direct Peerings mithilfe des Portals](howto-direct-portal.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
* [Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Häufig gestellte Fragen finden Sie unter [Peering Service: Häufig gestellte Fragen](service-faqs.md).
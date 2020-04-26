---
title: Aktivieren von Azure Peering Service für Direct Peering mithilfe von PowerShell
titleSuffix: Azure
description: Aktivieren von Azure Peering Service für Direct Peering mithilfe von PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: c029b822d00ce545e3623a6212421a55b2d1971f
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686966"
---
# <a name="enable-azure-peering-service-on-a-direct-peering-by-using-powershell"></a>Aktivieren von Azure Peering Service für Direct Peering mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie mit PowerShell-Cmdlets und dem Azure Resource Manager-Bereitstellungsmodell den Azure [Peering Service](overview-peering-service.md) für ein Direct Peering erstellen.

Sie können diese Anleitung auch im Azure-[Portal](howto-peering-service-portal.md) durchführen, wenn Sie das vorziehen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor Beginn der Konfiguration die [Voraussetzungen](prerequisites.md).
* Wählen Sie ein Direct Peering in Ihrem Abonnement aus, für das Sie den Peering Service aktivieren möchten. Wenn Sie keins besitzen, konvertieren Sie entweder ein Legacy-Direct Peering, oder erstellen Sie ein neues Direct Peering:
    * Um ein Legacy-Direct Peering zu konvertieren, befolgen Sie die Anweisungen in [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource mithilfe von PowerShell](howto-legacy-direct-powershell.md).
    * Um ein neues Direct Peering zu erstellen, befolgen Sie die Anweisungen in [Erstellen oder Ändern eines Direct Peerings mit PowerShell](howto-direct-powershell.md).

### <a name="work-with-azure-powershell"></a>Arbeiten mit Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="enable-peering-service-on-a-direct-peering"></a>Aktivieren von Peering Service für direktes Peering

### <a name="view-direct-peering"></a><a name= get></a>Direct Peering anzeigen
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Aktivieren von Direct Peering für Peering Service

Nachdem Sie im vorherigen Schritt Direct Peering eingerichtet haben, aktivieren Sie es für den Peering Service.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-powershell.md)]

## <a name="modify-a-direct-peering-connection"></a>Ändern einer Direct Peering-Verbindung

Wenn Sie die Verbindungseinstellungen ändern müssen, finden Sie Informationen hierzu im Abschnitt „Ändern eines Direct Peerings“ unter [Erstellen oder Ändern eines Direct Peerings mithilfe von PowerShell](howto-direct-powershell.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern eines Exchange Peerings mithilfe von PowerShell](howto-exchange-powershell.md)
* [Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource mithilfe von PowerShell](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie den folgenden Befehl ausführen:

```powershell
Get-Help Get-AzPeering -detailed
```

Häufig gestellte Fragen finden Sie unter [Peering Service: Häufig gestellte Fragen](service-faqs.md).
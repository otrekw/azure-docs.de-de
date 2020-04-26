---
title: Erstellen oder Ändern eines Exchange Peerings mithilfe von PowerShell
titleSuffix: Azure
description: Erstellen oder Ändern eines Exchange Peerings mithilfe von PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 23c905f148da614c7785b61b76abed191206cd90
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678664"
---
# <a name="create-or-modify-an-exchange-peering-by-using-powershell"></a>Erstellen oder Ändern eines Exchange Peerings mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie mit PowerShell-Cmdlets und dem Resource Manager-Bereitstellungsmodell ein Exchange Peering mit Microsoft erstellen. Der Artikel veranschaulicht auch, wie Sie den Status der Ressource prüfen und die Ressource aktualisieren, löschen oder ihre Bereitstellung aufheben.

Sie können diese Anleitung auch im Azure-[Portal](howto-exchange-portal.md) durchführen, wenn Sie das vorziehen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Exchange Peering](walkthrough-exchange-all.md).
* Falls Sie bereits Exchange Peerings mit Microsoft eingerichtet haben, die nicht in Azure-Ressourcen konvertiert werden, lesen Sie die Informationen unter [Konvertieren eines Legacy-Exchange Peerings in eine Azure-Ressource mithilfe von PowerShell](howto-legacy-exchange-powershell.md).

### <a name="work-with-azure-powershell"></a>Arbeiten mit Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-an-exchange-peering"></a>Erstellen und Bereitstellen eines Exchange Peerings

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-the-list-of-supported-peering-locations-for-exchange-peering"></a><a name=exchange-location></a>Abrufen der Liste mit unterstützten Peeringstandorten für Exchange Peering
[!INCLUDE [exchange-location](./includes/exchange-powershell-create-location.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Erstellen eines Exchange Peerings
[!INCLUDE [exchange-peering](./includes/exchange-powershell-create-connection.md)]

### <a name="get-exchange-peering"></a><a name=get></a>Einrichten von Exchange Peering
[!INCLUDE [peering-exchange-get](./includes/exchange-powershell-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Ändern eines Exchange Peerings
[!INCLUDE [peering-exchange-modify](./includes/exchange-powershell-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name=delete></a>Aufheben der Bereitstellung eines Exchange Peerings

[!INCLUDE [peering-exchange-delete](./includes/delete.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Direct Peering mit PowerShell](howto-direct-powershell.md)
* [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource mit PowerShell](howto-legacy-direct-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie den folgenden Befehl ausführen:

```powershell
Get-Help Get-AzPeering -detailed
```

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

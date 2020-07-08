---
title: Erstellen oder Ändern einer Instanz für Direct Peering mit PowerShell
titleSuffix: Azure
description: Erstellen oder Ändern einer Instanz für Direct Peering mit PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 076332ac61359bc793615c2f7c9ea0e22c667bcd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700296"
---
# <a name="create-or-modify-a-direct-peering-by-using-powershell"></a>Erstellen oder Ändern einer Instanz für Direct Peering mit PowerShell

In diesem Artikel wird beschrieben, wie Sie mit PowerShell-Cmdlets und dem Azure Resource Manager-Bereitstellungsmodell ein direktes Peering mit Microsoft erstellen. Der Artikel veranschaulicht auch, wie Sie den Status der Ressource prüfen und die Ressource aktualisieren, löschen oder ihre Bereitstellung aufheben.

Sie können diese Anleitung auch im Azure-[Portal](howto-direct-portal.md) durchführen, wenn Sie das vorziehen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Direct Peering](walkthrough-direct-all.md).
* Falls Sie bereits Direct Peering-Verbindungen mit Microsoft eingerichtet haben, die nicht in Azure-Ressourcen konvertiert werden, lesen Sie die Informationen unter [Konvertieren eines Legacy-Direct Peerings in eine Azure-Ressource mithilfe der PowerShell](howto-legacy-direct-powershell.md).

### <a name="work-with-azure-powershell"></a>Arbeiten mit Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-a-direct-peering"></a>Erstellen und Bereitstellen eines direkten Peerings

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-the-list-of-supported-peering-locations-for-direct-peering"></a><a name=direct-location></a>Abrufen der Liste mit unterstützten Peeringstandorten für direktes Peering
[!INCLUDE [direct-location](./includes/direct-powershell-create-location.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Erstellen eines direkten Peerings
[!INCLUDE [direct-peering](./includes/direct-powershell-create-connection.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Überprüfen eines direkten Peerings
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Ändern des direkten Peerings
[!INCLUDE [peering-direct-modify](./includes/direct-powershell-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Aufheben der Bereitstellung eines direkten Peerings
[!INCLUDE [peering-direct-delete](./includes/delete.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern eines Exchange Peerings mithilfe von PowerShell](howto-exchange-powershell.md)
* [Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource mithilfe von PowerShell](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie den folgenden Befehl ausführen:

```powershell
Get-Help Get-AzPeering -detailed
```

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

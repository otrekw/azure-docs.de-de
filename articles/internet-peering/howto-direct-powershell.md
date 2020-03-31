---
title: Erstellen oder Ändern eines direkten Peerings mit PowerShell
titleSuffix: Azure
description: Erstellen oder Ändern eines direkten Peerings mit PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 69031550bdab1535213c78f81426fa76e8ea62ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773645"
---
# <a name="create-or-modify-a-direct-peering-using-powershell"></a>Erstellen oder Ändern eines direkten Peerings mit PowerShell

In diesem Artikel wird beschrieben, wie Sie mit PowerShell-Cmdlets und dem Resource Manager-Bereitstellungsmodell ein direktes Peering mit Microsoft erstellen. Der Artikel veranschaulicht auch, wie Sie den Status der Ressource prüfen und die Ressource aktualisieren, löschen oder ihre Bereitstellung aufheben.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe des [Portals](howto-direct-portal.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [exemplarische Vorgehensweise für direktes Peering](walkthrough-direct-all.md).
* Falls Sie bereits ein direktes Peering mit Microsoft eingerichtet haben, die nicht in Azure-Ressourcen konvertiert werden, lesen Sie die Informationen unter [Konvertieren eines älteren direkten Peerings in eine Azure-Ressource mithilfe von PowerShell](howto-legacy-direct-powershell.md).

### <a name="working-with-azure-powershell"></a>Arbeiten mit Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-a-direct-peering"></a>Erstellen und Bereitstellen eines direkten Peerings

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-the-list-of-supported-peering-locations-for-direct-peering"></a><a name=direct-location></a>Abrufen der Liste mit unterstützten Peeringstandorten für direktes Peering
[!INCLUDE [direct-location](./includes/direct-powershell-create-location.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Erstellen eines direkten Peerings
[!INCLUDE [direct-peering](./includes/direct-powershell-create-connection.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Überprüfen des direkten Peerings
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Ändern des direkten Peerings
[!INCLUDE [peering-direct-modify](./includes/direct-powershell-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Aufheben der Bereitstellung eines direkten Peerings
[!INCLUDE [peering-direct-delete](./includes/delete.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern eines Austauschpeerings mithilfe von PowerShell](howto-exchange-powershell.md).
* [Konvertieren eines älteren Austauschpeerings in eine Azure-Ressource mithilfe von PowerShell](howto-legacy-exchange-powershell.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie den folgenden Befehl ausführen:

```powershell
Get-Help Get-AzPeering -detailed
```

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

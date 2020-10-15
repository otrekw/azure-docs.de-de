---
title: Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource mithilfe von PowerShell
titleSuffix: Azure
description: Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource mithilfe von PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 769522238939fe8ee786ae51a1b3b6051604451e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89071668"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-powershell"></a>Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell-Cmdlets ein vorhandenes Legacy-Exchange Peering in eine Azure-Ressource konvertieren.

Sie können diese Anleitung auch im Azure-[Portal](howto-legacy-exchange-portal.md) durchführen, wenn Sie das vorziehen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Exchange Peering](walkthrough-exchange-all.md).

### <a name="work-with-azure-powershell"></a>Arbeiten mit Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-legacy-exchange-peering-for-conversion"></a><a name= get></a>Abrufen des Legacy-Exchange Peerings für die Konvertierung
Dieses Beispiel zeigt, wie Sie eine ältere Instanz für Exchange Peering an einem Peeringstandort in Seattle abrufen:

```powershell
$legacyPeering = Get-AzLegacyPeering -Kind Exchange -PeeringLocation "Seattle"
$legacyPeering
```

Die Antwort kann wie folgt aussehen:
```powershell
    Kind                     : Exchange
    PeeringLocation          : Seattle
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```

### <a name="convert-legacy-peering"></a>Legacy-Peering konvertieren
Dieser Befehl kann dazu verwendet werden, ein Legacy-Exchange Peering in eine Azure-Ressource zu konvertieren:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> Änderungen werden beim Konvertieren einer älteren Peering-Instanz in eine Azure-Ressource nicht unterstützt.
&nbsp;

Diese Beispielantwort wird angezeigt, wenn die End-to-End-Bereitstellung erfolgreich abgeschlossen wurde:

```powershell
    Name                     : SeattleExchangePeering
    Kind                     : Exchange
    Sku                      : Basic_Exchange_Free
    PeeringLocation          : Seattle
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```
## <a name="additional-resources"></a>Zusätzliche Ressourcen
Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie den folgenden Befehl ausführen:

```powershell
Get-Help Get-AzPeering -detailed
```
Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern eines Exchange Peerings mithilfe von PowerShell](howto-exchange-powershell.md)

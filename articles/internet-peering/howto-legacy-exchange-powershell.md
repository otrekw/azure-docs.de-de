---
title: Konvertieren eines älteren Exchange Peerings in eine Azure-Ressource mithilfe von PowerShell
titleSuffix: Azure
description: Konvertieren eines älteren Exchange Peerings in eine Azure-Ressource mithilfe von PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: aa11f329cf0a0cb27d58b940b42731a2ec41c272
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774001"
---
# <a name="convert-a-legacy-exchange-peering-to-azure-resource-using-powershell"></a>Konvertieren eines älteren Exchange Peerings in eine Azure-Ressource mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell-Cmdlets ein vorhandenes Legacy-Exchange Peering in eine Azure-Ressource konvertieren.

Falls Sie es vorziehen, können Sie diese Anleitung auch über das [Portal](howto-legacy-exchange-portal.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [exemplarische Vorgehensweise für das Exchange Peering](walkthrough-exchange-all.md).

### <a name="working-with-azure-powershell"></a>Arbeiten mit Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Konvertieren einer älteren Instanz für Austauschpeering in eine Azure-Ressource

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name= get></a>Abrufen des Legacy- Exchange Peerings für die Konvertierung
Im Folgenden finden Sie das Beispiel zum Abrufen einer älteren Instanz für Exchange Peering an einem Peeringstandort in Seattle:

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
Der Befehl unten kann dazu verwendet werden, ein Legacy-Exchange Peering in eine Azure-Ressource zu konvertieren:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> Beachten Sie, dass Änderungen beim Konvertieren einer älteren Peering-Instanz in eine Azure-Ressource nicht unterstützt werden.&nbsp;

Unten sehen Sie eine Beispielantwort, wenn die End-to-End-Bereitstellung erfolgreich abgeschlossen wurde:

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

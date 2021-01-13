---
title: 'Tutorial: Konfigurieren einer WAF-Richtlinie mit Geofilterung – Azure Front Door'
description: In diesem Tutorial erfahren Sie, wie Sie eine WAF-Richtlinie für die Geofilterung erstellen und Ihrem vorhandenen Front Door-Front-End-Host zuordnen.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/14/2020
ms.author: duau
ms.openlocfilehash: f9b5869f7dd472c50ffb2c1c0dce765200f53882
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91324024"
---
# <a name="tutorial-how-to-set-up-a-geo-filtering-waf-policy-for-your-front-door"></a>Tutorial: Einrichten einer WAF-Richtlinie mit Geofilterung für Ihre Front Door-Instanz
In diesem Tutorial wird veranschaulicht, wie Sie mit Azure PowerShell eine Beispielrichtlinie für die Geofilterung erstellen und sie Ihrem vorhandenen Front Door-Front-End-Host zuordnen. Diese Beispielrichtlinie für die Geofilterung blockiert Anforderungen aus allen Ländern/Regionen, mit Ausnahme der USA.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> - Definieren der Übereinstimmungsbedingung für die Geofilterung
> - Hinzufügen der Übereinstimmungsbedingung für die Geofilterung zu einer Regel
> - Hinzufügen von Regeln zu einer Richtlinie
> - Verknüpfen der WAF-Richtlinie mit dem FrontDoor-Front-End-Host

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen
* Bevor Sie mit der Einrichtung einer Geofilterungsrichtlinie beginnen, müssen Sie zunächst Ihre PowerShell-Umgebung einrichten und ein Front Door-Profil erstellen.
* Gehen Sie zum Erstellen einer Front Door-Instanz gemäß den Anweisungen unter [Schnellstart: Erstellen einer Front Door-Instanz für eine hoch verfügbare globale Webanwendung](quickstart-create-front-door.md) vor.

## <a name="define-geo-filtering-match-condition"></a>Definieren der Übereinstimmungsbedingung für die Geofilterung

Erstellen Sie eine exemplarische Übereinstimmungsbedingung, durch die Anforderungen ausgewählt werden, die nicht aus den USA stammen. Verwenden Sie dazu [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) in den Parametern, wenn Sie eine Übereinstimmungsbedingung erstellen. Die zweistelligen Länder-/Regionscodes für die Zuordnung des Landes bzw. der Region finden Sie [hier](front-door-geo-filtering.md).

```azurepowershell-interactive
$nonUSGeoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable RemoteAddr `
-OperatorProperty GeoMatch `
-NegateCondition $true `
-MatchValue "US"
```
## <a name="add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>Hinzufügen der Übereinstimmungsbedingung für die Geofilterung zu einer Regel mit Aktion und Priorität

Erstellen Sie anschließend unter Verwendung von [New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject) das CustomRule-Objekt `nonUSBlockRule` basierend auf der Übereinstimmungsbedingung, einer Aktion und einer Priorität.  Ein CustomRule-Objekt kann über mehrere MatchCondition-Elemente verfügen.  In diesem Beispiel ist „Action“ auf „Block“ und „Priority“ auf „1“ (höchste Priorität) festgelegt.

```
$nonUSBlockRule = New-AzFrontDoorWafCustomRuleObject `
-Name "geoFilterRule" `
-RuleType MatchRule `
-MatchCondition $nonUSGeoMatchCondition `
-Action Block `
-Priority 1
```
## <a name="add-rules-to-a-policy"></a>Hinzufügen von Regeln zu einer Richtlinie
Suchen Sie mithilfe von `Get-AzResourceGroup` nach dem Namen der Ressourcengruppe, die das Front Door-Profil enthält. Erstellen Sie als Nächstes mithilfe von [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) ein Richtlinienobjekt vom Typ `geoPolicy` mit `nonUSBlockRule` in der angegebenen Ressourcengruppe, die das Front Door-Profil enthält. Geben Sie einen eindeutigen Namen für die Geofilterungsrichtlinie an. 

Im folgenden Beispiel wird der Ressourcengruppenname *FrontDoorQS_rg0* verwendet. Dabei wird davon ausgegangen, dass Sie das Front Door-Profil gemäß der Anleitung im Artikel [Schnellstart: Erstellen einer Front Door-Instanz für eine hoch verfügbare globale Webanwendung](quickstart-create-front-door.md) erstellt haben. Ersetzen Sie im folgenden Beispiel den Richtliniennamen *geoPolicyAllowUSOnly* durch einen eindeutigen Richtliniennamen.

```
$geoPolicy = New-AzFrontDoorWafPolicy `
-Name "geoPolicyAllowUSOnly" `
-resourceGroupName FrontDoorQS_rg0 `
-Customrule $nonUSBlockRule  `
-Mode Prevention `
-EnabledState Enabled
```
## <a name="link-waf-policy-to-a-front-door-frontend-host"></a>Verknüpfen der WAF-Richtlinie mit einem Front Door-Front-End-Host
Verknüpfen Sie das WAF-Richtlinienobjekt mit dem vorhandenen Front Door-Front-End-Host, und aktualisieren Sie die Front Door-Eigenschaften. 

Rufen Sie hierzu zunächst mithilfe von [Get-AzFrontDoor](/powershell/module/az.frontdoor/get-azfrontdoor) Ihr Front Door-Objekt ab. 

```
$geoFrontDoorObjectExample = Get-AzFrontDoor -ResourceGroupName FrontDoorQS_rg0
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```
Legen Sie als Nächstes mithilfe von [Set-AzFrontDoor](/powershell/module/az.frontdoor/set-azfrontdoor) die Eigenschaft „WebApplicationFirewallPolicyLink“ des Front-Ends auf die Ressourcen-ID von `geoPolicy`fest.

```
Set-AzFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> Die Eigenschaft „WebApplicationFirewallPolicyLink“ muss nur einmal festgelegt werden, um eine WAF-Richtlinie mit einem Front Door-Front-End-Host zu verknüpfen. Nachfolgende Richtlinienaktualisierungen werden automatisch auf den Front-End-Host angewendet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie eine Geofilterungsregel konfiguriert, die einer WAF-Richtlinie zugeordnet ist. Anschließend haben Sie die Richtlinie mit einem Front-End-Host Ihrer Front Door-Instanz verknüpft. Wenn Sie die Geofilterungsregel oder WAF-Richtlinie nicht mehr benötigen, müssen Sie zuerst die Zuordnung der Richtlinie zum Front-End-Host aufheben, bevor die WAF-Richtlinie gelöscht werden kann.

:::image type="content" source="media/front-door-geo-filtering/front-door-disassociate-policy.png" alt-text="Aufheben der Zuordnung der WAF-Richtlinie":::

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Tutorial erfahren Sie, wie Sie eine Web Application Firewall für Ihre Front Door-Instanz konfigurieren:

> [!div class="nextstepaction"]
> [Web Application Firewall und Front Door](front-door-waf.md)

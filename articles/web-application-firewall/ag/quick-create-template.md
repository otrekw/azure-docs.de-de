---
title: 'Schnellstart: Erstellen einer Azure WAF v2 für Application Gateway: Azure Resource Manager-Vorlage'
titleSuffix: Azure Application Gateway
description: Hier erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Schnellstartvorlage (ARM-Vorlage) eine Web Application Firewall v2-Instanz für Azure Application Gateway erstellen.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: quickstart
ms.date: 09/16/2020
ms.author: victorh
ms.custom: subject-armqs
ms.openlocfilehash: 4c5d5c4cafee9402e2afd7ddc047fd3b68e84540
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92091320"
---
# <a name="quickstart-create-an-azure-waf-v2-on-application-gateway-using-an-arm-template"></a>Schnellstart: Erstellen einer Azure WAF v2 für Application Gateway mithilfe einer ARM-Vorlage

In dieser Schnellstartanleitung wird eine Azure Resource Manager-Vorlage (ARM-Vorlage) verwendet, um eine Azure Web Application Firewall v2 für Application Gateway zu erstellen.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fag-docs-wafv2%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Überprüfen der Vorlage

Diese Vorlage dient zum Erstellen einer einfachen Web Application Firewall (v2) für Azure Application Gateway. Dies schließt eine öffentliche Front-End-IP-Adresse, HTTP-Einstellungen, eine Regel mit einem einfachen Listener am Port 80 sowie einen Back-End-Pool ein. Eine WAF-Richtlinie mit einer benutzerdefinierten Regel wird erstellt, um Datenverkehr für den Back-End-Pool basierend auf einer IP-Adressübereinstimmung zu blockieren.

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/ag-docs-wafv2/).

:::code language="json" source="~/quickstart-templates/ag-docs-wafv2/azuredeploy.json":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/applicationgateways**](/azure/templates/microsoft.network/applicationgateways)
- [**Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies**](/azure/templates/microsoft.network/ApplicationGatewayWebApplicationFirewallPolicies)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses): Eine für das Anwendungsgateway, zwei für die virtuellen Computer.
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines): Zwei virtuelle Computer.
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): Zwei für die virtuellen Computer.
- [**Microsoft.Compute/virtualMachine/extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions): Zum Konfigurieren von IIS und Webseiten.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die ARM-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Durch die Vorlage werden ein Anwendungsgateway, die Netzwerkinfrastruktur und zwei virtuelle Computer im Back-End-Pool mit IIS erstellt.

   [![In Azure bereitstellen](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fag-docs-wafv2%2Fazuredeploy.json)

2. Wählen Sie Ihre Ressourcengruppe aus, oder erstellen Sie sie.
3. Wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** und anschließend **Kaufen** aus. Die Bereitstellung kann zehn Minuten oder länger dauern.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

IIS ist für die Erstellung des Anwendungsgateways zwar nicht erforderlich, wird aber auf den Back-End-Servern installiert, um die erfolgreiche Erstellung einer WAF (v2) auf dem Anwendungsgateway zu überprüfen.

Testen des Anwendungsgateways mit IIS:

1. Suchen Sie auf der Seite **Übersicht** des Anwendungsgateways nach der öffentlichen IP-Adresse für das Anwendungsgateway.![Notieren der öffentlichen IP-Adresse des Anwendungsgateways](../../application-gateway/media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)Sie können auch **Alle Ressourcen** auswählen, *myAGPublicIPAddress* in das Suchfeld eingeben und den Eintrag dann in den Suchergebnissen auswählen. Azure zeigt die öffentliche IP-Adresse auf der Seite **Übersicht** an.
2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste Ihres Browsers ein, um zu dieser IP-Adresse zu navigieren.
3. Überprüfen Sie die Antwort. Eine Antwort vom Typ **403 Verboten** bestätigt, dass die WAF erfolgreich erstellt wurde und Verbindungen mit dem Back-End-Pool blockiert.
4. Ändern Sie die benutzerdefinierte Regel in **Datenverkehr zulassen**.
  Führen Sie das folgende Azure PowerShell-Skript aus, und geben Sie dabei Ihren Ressourcengruppennamen an:
   ```azurepowershell
   $rg = "<your resource group name>"
   $AppGW = Get-AzApplicationGateway -Name myAppGateway -ResourceGroupName $rg
   $pol = Get-AzApplicationGatewayFirewallPolicy -Name WafPol01 -ResourceGroupName $rg
   $pol[0].customrules[0].action = "allow"
   $rule = $pol.CustomRules
   Set-AzApplicationGatewayFirewallPolicy -Name WafPol01 -ResourceGroupName $rg -CustomRule $rule
   $AppGW.FirewallPolicy = $pol
   Set-AzApplicationGateway -ApplicationGateway $AppGW
   ```

   Aktualisieren Sie mehrmals Ihren Browser. Daraufhin sollten Verbindungen mit „myVM1“ und „myVM2“ angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Anwendungsgateway erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden das Anwendungsgateway und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Erstellen eines Anwendungsgateways mit einer Web Application Firewall über das Azure-Portal](application-gateway-web-application-firewall-portal.md)
---
title: 'Schnellstart: Weiterleiten von Webdatenverkehr mithilfe einer Resource Manager-Vorlage'
titleSuffix: Azure Application Gateway
description: Hier erfahren Sie, wie Sie eine Resource Manager-Vorlage zum Erstellen einer Azure Application Gateway-Instanz verwenden, mit der Webdatenverkehr an virtuelle Computer in einem Back-End-Pool weitergeleitet wird.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 03/23/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 0612371605f6b216eadeef49d9adb1497ba31591
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80148229"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---resource-manager-template"></a>Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway: Resource Manager-Vorlage

In dieser Schnellstartanleitung wird eine Resource Manager-Vorlage verwendet, um eine Azure Application Gateway-Instanz zu erstellen. Anschließend wird das Anwendungsgateway getestet, um sicherzustellen, dass es ordnungsgemäß funktioniert.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Diese Schnellstartanleitung steht auch für das [Azure-Portal](quick-create-portal.md) sowie für [Azure PowerShell](quick-create-powershell.md) und die [Azure-Befehlszeilenschnittstelle](quick-create-cli.md) zur Verfügung.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Der Einfachheit halber wird mit dieser Vorlage ein simples Setup mit einer öffentlichen Front-End-IP-Adresse, einem einfachen Listener zum Hosten einer einzelnen Website auf diesem Anwendungsgateway, einer einfachen Routingregel für Anforderungen und zwei virtuellen Computern im Back-End-Pool erstellt.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/ag-docs-qs/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/ag-docs-qs/azuredeploy.json" range="001-343" highlight="197-297":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/applicationgateways**](/azure/templates/microsoft.network/applicationgateways)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses): Eine für das Anwendungsgateway, zwei für die virtuellen Computer.
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines): Zwei virtuelle Computer.
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): Zwei für die virtuellen Computer.
- [**Microsoft.Compute/virtualMachine/extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions): Zum Konfigurieren von IIS und Webseiten.


### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Resource Manager-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Durch die Vorlage werden ein Anwendungsgateway, die Netzwerkinfrastruktur und zwei virtuelle Computer im Back-End-Pool mit IIS erstellt.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fag-docs-qs%2Fazuredeploy.json"><img src="./media/quick-create-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Wählen Sie Ihre Ressourcengruppe aus (oder erstellen Sie sie), und geben Sie den Benutzernamen und das Kennwort des VM-Administrators ein.
3. Wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** und anschließend **Kaufen** aus. Die Bereitstellung kann 20 Minuten oder länger dauern.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

IIS ist für die Erstellung des Anwendungsgateways zwar nicht erforderlich, wird aber installiert, um die erfolgreiche Erstellung des Anwendungsgateways durch Azure zu überprüfen. Testen des Anwendungsgateways mit IIS:

1. Suchen Sie auf der Seite **Übersicht** des Anwendungsgateways nach der öffentlichen IP-Adresse für das Anwendungsgateway.![Notieren der öffentlichen IP-Adresse des Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)Sie können auch **Alle Ressourcen** auswählen, *myAGPublicIPAddress* in das Suchfeld eingeben und den Eintrag dann in den Suchergebnissen auswählen. Azure zeigt die öffentliche IP-Adresse auf der Seite **Übersicht** an.
2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste Ihres Browsers ein, um zu dieser IP-Adresse zu navigieren.
3. Überprüfen Sie die Antwort. Eine gültige Antwort bestätigt, dass das Anwendungsgateway erfolgreich erstellt wurde und eine Verbindung mit dem Back-End herstellen kann.

   ![Testen des Anwendungsgateways](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

   Aktualisieren Sie den Browser mehrmals. Daraufhin sollten Verbindungen mit „myVM1“ und „myVM2“ angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Anwendungsgateway erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden das Anwendungsgateway und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure CLI](./tutorial-manage-web-traffic-cli.md)

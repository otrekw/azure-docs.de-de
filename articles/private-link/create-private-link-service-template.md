---
title: 'Schnellstart: Erstellen eines Private Link-Diensts in Azure Private Link'
description: In dieser Schnellstartanleitung erstellen Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) einen Private Link-Dienst.
services: private-link
author: asudbring
ms.service: private-link
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 05/29/2020
ms.author: allensu
ms.openlocfilehash: 34993ad3d3d0494f89bd264a8b7194f52129ad7c
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102555056"
---
# <a name="quickstart-create-a-private-link-service-by-using-an-arm-template"></a>Schnellstart: Erstellen eines Private Link-Diensts mithilfe einer ARM-Vorlage

In dieser Schnellstartanleitung erstellen Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) einen Private Link-Dienst.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Diese Schnellstartanleitung steht auch für das [Azure-Portal](create-private-link-service-portal.md), für [Azure PowerShell](create-private-link-service-powershell.md) und für die [Azure-Befehlszeilenschnittstelle](create-private-link-service-cli.md) zur Verfügung.

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-privatelink-service%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Überprüfen der Vorlage

Diese Vorlage erstellt einen Private Link-Dienst.

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-privatelink-service/).

:::code language="json" source="~/quickstart-templates/101-privatelink-service/azuredeploy.json":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks): Es gibt ein virtuelles Netzwerk für jeden virtuellen Computer.
- [**Microsoft.Network/loadBalancers**](/azure/templates/microsoft.network/loadBalancers): Der Lastenausgleich, der die virtuellen Computer verfügbar macht, die den Dienst hosten
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): Es gibt zwei Netzwerkschnittstellen, eine für jeden virtuellen Computer.
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines): Es gibt zwei virtuelle Computer, von denen einer den Dienst hostet und der andere die Verbindung mit dem privaten Endpunkt testet.
- [**Microsoft.Compute/virtualMachines/extensions**](/azure/templates/Microsoft.Compute/virtualMachines/extensions): Die Erweiterung, die einen Webserver installiert
- [**Microsoft.Network/privateLinkServices**](/azure/templates/microsoft.network/privateLinkServices): Der Private Link-Dienst, der den Dienst verfügbar macht
- [**Microsoft.Network/publicIpAddresses**](/azure/templates/microsoft.network/publicIpAddresses): Es gibt zwei öffentliche IP-Adressen, eine für jeden virtuellen Computer.
- [**Microsoft.Network/privateendpoints**](/azure/templates/microsoft.network/privateendpoints): Der private Endpunkt für den Zugriff auf den Dienst

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

So stellen Sie die ARM-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Die Vorlage erstellt einen virtuellen Computer, einen Standardlastenausgleich, einen Private Link-Dienst, einen privaten Endpunkt, ein Netzwerk und einen virtuellen Computer zur Überprüfung.

   [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-privatelink-service%2Fazuredeploy.json)

2. Wählen Sie Ihre Ressourcengruppe aus, oder erstellen Sie sie.
3. Geben Sie den Administratorbenutzernamen für den virtuellen Computer und das zugehörige Kennwort ein.
4. Lesen Sie die Geschäftsbedingungen. Wenn Sie einverstanden sind, wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** > **Kaufen** aus.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

> [!NOTE]
> Die ARM-Vorlage generiert jeweils einen eindeutigen Namen für die VM-Ressource „myConsumerVm<b>{eindeutige ID}</b>“. Ersetzen Sie **{eindeutige ID}** durch den entsprechenden generierten Wert.

### <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt über das Internet eine Verbindung mit dem virtuellen Computer _myConsumerVm{eindeutige ID}_ her:

1.  Geben Sie in die Suchleiste des Portals _myConsumerVm{eindeutige ID}_ ein.

2.  Wählen Sie **Verbinden**. **Verbindung mit virtuellem Computer herstellen** wird geöffnet.

3.  Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (_RDP_) und lädt sie auf Ihren Computer herunter.

4.  Öffnen Sie die heruntergeladene RDP-Datei.

    a. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    b. Geben Sie den Benutzernamen und das Kennwort an, die Sie beim Erstellen des virtuellen Computers angegeben haben.
    
    > [!NOTE]
    > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

5.  Klicken Sie auf **OK**.

6.  Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

7.  Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.

### <a name="access-the-http-service-privately-from-the-vm"></a>Privates Zugreifen auf den HTTP-Dienst über den virtuellen Computer

Hier wird erläutert, wie Sie mithilfe des privaten Endpunkts über den virtuellen Computer eine Verbindung mit dem HTTP-Dienst herstellen.

1.  Navigieren Sie zum Remotedesktop von _myConsumerVm{eindeutige ID}_ .
2.  Öffnen Sie einen Browser, und geben Sie die Adresse des privaten Endpunkts ein: `http://10.0.0.5/`.
3.  Die IIS-Standardseite wird angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Private Link-Dienst erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden der Private Link-Dienst und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Nächste Schritte


Weitere Informationen zu den Diensten, die einen privaten Endpunkt unterstützen, finden Sie unter:
> [!div class="nextstepaction"]
> [Private Link-Verfügbarkeit](private-link-overview.md#availability)

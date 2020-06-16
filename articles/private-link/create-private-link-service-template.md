---
title: 'Azure Private Link-Dienst: ARM-Vorlage'
description: 'Private Link-Dienst: ARM-Vorlage'
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: article
ms.date: 05/29/2020
ms.author: allensu
ms.openlocfilehash: 93a66057ddb0034f7ac9ac62578292ca38f2d2fe
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84236409"
---
# <a name="create-a-private-link-service---resource-manager-template"></a>Erstellen eines Private Link-Diensts: Resource Manager-Vorlage

In diesem Schnellstart erstellen Sie mit einer Resource Manager-Vorlage einen Private Link-Dienst.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Diese Schnellstartanleitung steht auch für das [Azure-Portal](create-private-link-service-portal.md) sowie für [Azure PowerShell](create-private-link-service-powershell.md) und die [Azure-Befehlszeilenschnittstelle](create-private-link-service-cli.md) zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-private-link-service"></a>Erstellen eines Private Link-Diensts

Diese Vorlage erstellt einen Private Link-Dienst.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-privatelink-service/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-privatelink-service/azuredeploy.json" range="001-432" highlight="263-289":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/privateLinkServices**](/azure/templates/microsoft.network/privateLinkServices): Private Link-Dienst, um den Dienst privat verfügbar zu machen.
- [**Microsoft.Network/privateEndpoints**](/azure/templates/microsoft.network/privateendpoints): Privater Endpunkt für den privaten Zugriff auf den Dienst.
- [**Microsoft.Network/loadBalancers**](/azure/templates/microsoft.network/loadBalancers): Lastenausgleich, der die virtuellen Computer verfügbar macht, die den Dienst hosten.
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines): Zwei virtuelle Computer, von denen einer den Dienst hostet und der andere die Verbindung mit dem privaten Endpunkt testet.
- [**Microsoft.Compute/virtualMachines/extensions**](/azure/templates/Microsoft.Compute/virtualMachines/extensions): Erweiterung, die den Webserver installiert.
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks): Eines für jeden virtuellen Computer.
- [**Microsoft.Network/publicIpAddresses**](/azure/templates/microsoft.network/publicIpAddresses): Zwei öffentliche IP-Adressen, eine für jeden virtuellen Computer.
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): Zwei Netzwerkschnittstellen, eine für jeden virtuellen Computer.

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Resource Manager-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Die Vorlage erstellt einen virtuellen Computer, einen Standardlastenausgleich, einen Private Link-Dienst, einen privaten Endpunkt, ein Netzwerk und einen virtuellen Computer zur Überprüfung.

   [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-privatelink-service%2Fazuredeploy.json)

2. Wählen Sie Ihre Ressourcengruppe aus, oder erstellen Sie sie.
3. Geben Sie den Administratorbenutzernamen für den virtuellen Computer und das zugehörige Kennwort ein.
4. Wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** und anschließend **Kaufen** aus.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

> [!NOTE]
> Die ARM-Vorlage generiert einen eindeutigen Namen für die Virtual Machine-Ressource „myConsumerVm<b>{eindeutige ID}</b>“. Ersetzen Sie <b>{eindeutige ID}</b> durch Ihren generierten Wert.

### <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt über das Internet eine Verbindung mit dem virtuellen Computer _myConsumerVm{eindeutige ID}_ her:

1.  Geben Sie in die Suchleiste des Portals _myConsumerVm{eindeutige ID}_ ein.

2.  Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

3.  Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (_RDP_) und lädt sie auf Ihren Computer herunter.

4.  Öffnen Sie die heruntergeladene RDP\*-Datei.

    a. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    b. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.
    
    > [!NOTE]
    > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

5.  Klicken Sie auf **OK**.

6.  Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

7.  Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.

### <a name="access-the-http-service-privately-from-the-vm"></a>Privates Zugreifen auf den HTTP-Dienst über den virtuellen Computer

In diesem Abschnitt stellen Sie über den privaten Endpunkt eine Verbindung von der VM mit dem HTTP-Dienst her.

1.  Auf dem Remotedesktop von _myConsumerVm{eindeutige ID}_
2.  Öffnen Sie einen Browser, und geben Sie die private Endpunktadresse http://10.0.0.5/ ein.
3.  Es wird die IIS-Standardseite angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Private Link-Dienst erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden der Private Link-Dienst und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure Private Link](private-link-overview.md).

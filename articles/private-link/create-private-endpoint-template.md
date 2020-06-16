---
title: Resource Manager-Vorlage für einen privaten Endpunkt in Azure
description: Erfahren Sie mehr über Azure Private Link.
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: article
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: a91415e7e3d91c2950cc4df2235c3d58df284cc0
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235984"
---
# <a name="create-a-private-endpoint---resource-manager-template"></a>Erstellen eines privaten Endpunkts mithilfe einer Resource Manager-Vorlage

In diesem Schnellstart erstellen Sie mit einer Resource Manager-Vorlage einen privaten Endpunkt.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Diese Schnellstartanleitung steht auch für das [Azure-Portal](create-private-endpoint-portal.md) sowie für [Azure PowerShell](create-private-endpoint-powershell.md) und die [Azure-Befehlszeilenschnittstelle](create-private-endpoint-cli.md) zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-private-endpoint"></a>Erstellen eines privaten Endpunkts

Mit dieser Vorlage wird ein privater Endpunkt für einen Azure SQL-Server erstellt.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-private-endpoint-sql/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-private-endpoint-sql/azuredeploy.json" range="001-295" highlight="131-156":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/privateEndpoints:** ](/azure/templates/microsoft.network/privateendpoints) privater Endpunkt für den privaten Zugriff auf den Azure SQL-Server
- [**Microsoft.Network/privateDnsZones:** ](/azure/templates/microsoft.network/privatednszones) wird zum Auflösen der IP-Adresse des privaten Endpunkts verwendet
- [**Microsoft.Network/privateDnsZones/virtualNetworkLinks**](/azure/templates/microsoft.network/privatednszones/virtualnetworklinks)
- [**Microsoft.Network/privateEndpoints/privateDnsZoneGroups:** ](/azure/templates/microsoft.network/privateendpoints/privateDnsZoneGroups) Verknüpfen des privaten Endpunkts mit einer privaten DNS-Zone
- [**Microsoft.Sql/servers:** ](/azure/templates/microsoft.sql/servers) Azure SQL-Server mit der Beispieldatenbank
- [**Microsoft.Sql/servers/databases:** ](/azure/templates/microsoft.sql/servers/databases) Beispieldatenbank
- [**Microsoft.Network/virtualNetworks:** ](/azure/templates/microsoft.network/virtualnetworks) Virtuelles Netzwerk, in dem der private Endpunkt bereitgestellt wird
- [**Microsoft.Network/publicIpAddresses:** ](/azure/templates/microsoft.network/publicIpAddresses) Öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer
- [**Microsoft.Compute/virtualMachines:** ](/azure/templates/microsoft.compute/virtualmachines) Virtueller Computer zum Testen der privaten Verbindung mit dem Azure SQL-Server mithilfe eines privaten Endpunkts
- [**Microsoft.Network/networkInterfaces:** ](/azure/templates/microsoft.network/networkinterfaces) Netzwerkschnittstelle für den virtuellen Computer

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Resource Manager-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Die Vorlage erstellt den privaten Endpunkt, den Azure SQL-Server, die Netzwerkinfrastruktur und die zu überprüfenden virtuellen Computer.

   [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-private-endpoint-sql%2Fazuredeploy.json)

2. Wählen Sie Ihre Ressourcengruppe aus, oder erstellen Sie sie.
3. Geben Sie den Benutzernamen des SQL-Administrators und das entsprechende Kennwort ein.
4. Geben Sie den Administratorbenutzernamen für den virtuellen Computer und das zugehörige Kennwort ein.
5. Wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** und anschließend **Kaufen** aus. Die Bereitstellung kann 20 Minuten oder länger dauern.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

> [!NOTE]
> Die Resource Manager-Vorlage generiert jeweils einen eindeutigen Namen für die VM-Ressource myVm<b>{eindeutige ID}</b> und für die Azure SQL-Serverressource sqlserver<b>{eindeutige ID}</b>. Bitte ersetzen Sie im Folgenden <b>{eindeutige ID}</b> durch den entsprechenden generierten Wert.

### <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt über das Internet eine Verbindung mit dem virtuellen Computer _myVm{eindeutige ID}_ her:

1. Geben Sie in die Suchleiste des Portals _myVm{eindeutige ID}_ ein.

2. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

3. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (_RDP_) und lädt sie auf Ihren Computer herunter.

4. Öffnen Sie die heruntergeladene RDP\*-Datei.

   a. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

   b. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

      > [!NOTE]
      > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

5. Klicken Sie auf **OK**.

6. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

7. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.

### <a name="access-sql-database-server-privately-from-the-vm"></a>Privates Zugreifen auf den SQL-Datenbank-Server vom virtuellen Computer

In diesem Abschnitt stellen Sie eine Verbindung mit dem SQL-Datenbank-Server von der VM her, indem Sie den privaten Endpunkt verwenden.

1.  Öffnen Sie auf dem Remotedesktop von _myVM{eindeutige ID}_ PowerShell.
2.  Geben Sie „nslookup sqlserver{eindeutige ID}.database.windows.net“  ein. Sie erhalten eine ähnliche Meldung wie die folgende:

    ```
      Server:  UnKnown
      Address:  168.63.129.16
      Non-authoritative answer:
      Name:    sqlserver.privatelink.database.windows.net
      Address:  10.0.0.5
      Aliases:  sqlserver.database.windows.net
    ```

3.  Installieren von SQL Server Management Studio
4.  Geben Sie in „Mit Server verbinden“ diese Informationen ein, oder wählen Sie sie aus: Servertyp: Wählen Sie „Datenbank-Engine“ aus.
    Servername: Wählen Sie „sqlserver{eindeutige ID}.database.windows.net“ aus. Benutzername: Geben Sie einen während der Erstellung angegebenen Benutzernamen ein.
    Password (Kennwort): Geben Sie ein während der Erstellung angegebenes Kennwort ein.
    Kennwort speichern: Wählen Sie „Ja“ aus.

5.  Wählen Sie **Verbinden**.
6.  Durchsuchen Sie **Datenbanken** im linken Menü.
7.  (Optional:) Erstellen Sie _sample-db_ oder fragen Sie Informationen daraus ab.
8.  Schließen Sie die Remotedesktopverbindung mit _myVm{eindeutige ID}_ .

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem privaten Endpunkt erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden der private Endpunkt und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure Private Link](private-link-overview.md).

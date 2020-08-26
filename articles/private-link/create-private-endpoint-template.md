---
title: Erstellen eines privaten Endpunkts in Azure Private Link
description: In dieser Schnellstartanleitung erstellen Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) einen privaten Endpunkt.
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: 553d36a86671617417b6b9b1ea47966c3ba3fdf6
ms.sourcegitcommit: e0785ea4f2926f944ff4d65a96cee05b6dcdb792
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88705808"
---
# <a name="quickstart-create-a-private-endpoint-by-using-an-arm-template"></a>Schnellstart: Erstellen eines privaten Endpunkts mithilfe einer ARM-Vorlage

In dieser Schnellstartanleitung erstellen Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) einen privaten Endpunkt.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Diese Schnellstartanleitung steht auch für das [Azure-Portal](create-private-endpoint-portal.md), für [Azure PowerShell](create-private-endpoint-powershell.md) und für die [Azure-Befehlszeilenschnittstelle](create-private-endpoint-cli.md) zur Verfügung.

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-private-endpoint-sql%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Überprüfen der Vorlage

Mit dieser Vorlage wird ein privater Endpunkt für eine Instanz von Azure SQL-Datenbank erstellt.

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-private-endpoint-sql/).

:::code language="json" source="~/quickstart-templates/101-private-endpoint-sql/azuredeploy.json":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Sql/servers**](/azure/templates/microsoft.sql/servers): Die Instanz der SQL-Datenbank mit der Beispieldatenbank
- [**Microsoft.Sql/servers/databases**](/azure/templates/microsoft.sql/servers/databases): Die Beispieldatenbank
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks): Das virtuelle Netzwerk, in dem der private Endpunkt bereitgestellt wird
- [**Microsoft.Network/privateEndpoints**](/azure/templates/microsoft.network/privateendpoints): Der private Endpunkt für den Zugriff auf die Instanz von SQL-Datenbank
- [**Microsoft.Network/privateDnsZones**](/azure/templates/microsoft.network/privatednszones): Die Zone, mit der die IP-Adresse des privaten Endpunkts aufgelöst wird
- [**Microsoft.Network/privateDnsZones/virtualNetworkLinks**](/azure/templates/microsoft.network/privatednszones/virtualnetworklinks)
- [**Microsoft.Network/privateEndpoints/privateDnsZoneGroups**](/azure/templates/microsoft.network/privateendpoints/privateDnsZoneGroups): Die Zonengruppe, mit der der private Endpunkt mit einer privaten DNS-Zone verknüpft wird
- [**Microsoft.Network/publicIpAddresses**](/azure/templates/microsoft.network/publicIpAddresses): Die öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): Die Netzwerkschnittstelle für den virtuellen Computer
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines): Der virtuelle Computer zum Testen der privaten Verbindung mit dem privaten Endpunkt und der Instanz von SQL-Datenbank

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

So stellen Sie die ARM-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Die Vorlage erstellt den privaten Endpunkt, die Instanz von SQL-Datenbank, die Netzwerkinfrastruktur und einen zu überprüfenden virtuellen Computer.

   [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-private-endpoint-sql%2Fazuredeploy.json)

2. Wählen Sie Ihre Ressourcengruppe aus, oder erstellen Sie sie.
3. Geben Sie den Benutzernamen des SQL-Administrators und das entsprechende Kennwort ein.
4. Geben Sie den Administratorbenutzernamen für den virtuellen Computer und das zugehörige Kennwort ein.
5. Lesen Sie die Geschäftsbedingungen. Wenn Sie einverstanden sind, wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** > **Kaufen** aus. Die Bereitstellung kann 20 Minuten oder länger dauern.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

> [!NOTE]
> Die ARM-Vorlage generiert jeweils einen eindeutigen Namen für die VM-Ressource „myVm<b>{eindeutige ID}</b>“ und für die SQL-Datenbank-Ressource „sqlserver<b>{eindeutige ID}</b>“. Ersetzen Sie **{eindeutige ID}** durch den entsprechenden generierten Wert.

### <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt über das Internet eine Verbindung mit dem virtuellen Computer _myVm{eindeutige ID}_ her:

1. Geben Sie in die Suchleiste des Portals _myVm{eindeutige ID}_ ein.

2. Wählen Sie **Verbinden**. **Verbindung mit virtuellem Computer herstellen** wird geöffnet.

3. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (_RDP_) und lädt sie auf Ihren Computer herunter.

4. Öffnen Sie die heruntergeladene RDP-Datei.

   a. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

   b. Geben Sie den Benutzernamen und das Kennwort an, die Sie beim Erstellen des virtuellen Computers angegeben haben.

      > [!NOTE]
      > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

5. Klicken Sie auf **OK**.

6. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

7. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.

### <a name="access-the-sql-database-server-privately-from-the-vm"></a>Privates Zugreifen auf den SQL-Datenbank-Server über den virtuellen Computer

Hier wird erläutert, wie Sie mithilfe des privaten Endpunkts über den virtuellen Computer eine Verbindung mit dem SQL-Datenbank-Server herstellen.

1.  Öffnen Sie auf dem Remotedesktop von _myVM{eindeutige ID}_ PowerShell.
2.  Geben Sie Folgendes ein: nslookup sqlserver{uniqueid}.database.windows.net. 
    Sie erhalten eine Meldung wie die folgende:

    ```
      Server:  UnKnown
      Address:  168.63.129.16
      Non-authoritative answer:
      Name:    sqlserver.privatelink.database.windows.net
      Address:  10.0.0.5
      Aliases:  sqlserver.database.windows.net
    ```

3.  Installieren Sie SQL Server Management Studio.
4.  Geben Sie in  **Mit Server verbinden** diese Informationen ein, oder wählen Sie sie aus:
    - **Servertyp**: Wählen Sie **Datenbank-Engine** aus.
    - **Servername**: Wählen Sie **sqlserver{eindeutige ID}.database.windows.net** aus.
    - **Benutzername**: Geben Sie einen während der Erstellung angegebenen Benutzernamen ein.
    - **Kennwort**: Geben Sie ein während der Erstellung angegebenes Kennwort ein.
    - **Kennwort speichern**: Wählen Sie  **Ja** aus.

5.  Wählen Sie **Verbinden**.
6.  Navigieren Sie über das Menü auf der linken Seite zu **Datenbanken**.
7.  Optional können Sie _sample-db_ erstellen oder Informationen daraus abfragen.
8.  Schließen Sie die Remotedesktopverbindung mit _myVm{eindeutige ID}_ .

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem privaten Endpunkt erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden der private Endpunkt und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Azure Private Link](private-link-overview.md).

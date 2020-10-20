---
title: 'Schnellstart: Erstellen eines Bastion-Hosts über eine VM und Herstellen einer Verbindung über eine private IP-Adresse'
titleSuffix: Azure Bastion
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie einen Azure Bastion-Host über einen virtuellen Computer erstellen und mit einer privaten IP-Adresse eine sichere Verbindung herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: quickstart
ms.date: 10/12/2020
ms.author: cherylmc
ms.openlocfilehash: 6f451e7b115c00bc7b2cf350e00b9f704ab1d29f
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019051"
---
# <a name="quickstart-connect-to-a-virtual-machine-using-a-private-ip-address-and-azure-bastion"></a>Schnellstart: Herstellen einer Verbindung mit einem virtuellen Computer mit einer privaten IP-Adresse und Azure Bastion

In dieser Schnellstartanleitung wird veranschaulicht, wie Sie über Ihren Browser eine Verbindung mit einem virtuellen Computer herstellen und dabei Azure Bastion und das Azure-Portal verwenden. Im Azure-Portal können Sie Bastion über Ihre Azure-VM in Ihrem virtuellen Netzwerk bereitstellen. Nach der Bereitstellung von Bastion können Sie über das Azure-Portal unter Verwendung der privaten IP-Adresse eine Verbindung mit dem virtuellen Computer herstellen. Für Ihren virtuellen Computer ist keine öffentliche IP-Adresse oder spezielle Software erforderlich. Ein Vorteil der Erstellung eines Bastion-Hosts für Ihr VNET direkt über Ihre VM ist, dass viele Einstellungen für Sie bereits eingefügt werden.

Nachdem der Dienst bereitgestellt wurde, ist die RDP/SSH-Umgebung für alle virtuellen Computer des jeweiligen virtuellen Netzwerks verfügbar. Weitere Informationen zu Azure Bastion finden Sie unter [Was ist Azure Bastion?](bastion-overview.md).

## <a name="prerequisites"></a><a name="prereq"></a>Voraussetzungen

* Ein virtuelles Netzwerk
* Ein virtueller Windows-Computer im virtuellen Netzwerk
* Folgende erforderliche Rollen:
  * Rolle „Leser“ für den virtuellen Computer
  * Rolle „Leser“ auf der Netzwerkschnittstellenkarte mit privater IP-Adresse des virtuellen Computers

* Ports: Zum Herstellen einer Verbindung mit dem virtuellen Computer müssen darauf die folgenden Ports geöffnet sein:
  * Eingehende Ports: RDP (3389)

### <a name="example-values"></a>Beispielwerte

|**Name** | **Wert** |
| --- | --- |
| Name |  TestVNet1-bastion |
| Virtuelles Netzwerk |  TestVNet1 (basierend auf der VM) |
| + Subnetzname | AzureBastionSubnet |
| AzureBastionSubnet-Adressen |  10.1.254.0/27 |
| Öffentliche IP-Adresse |  Neu erstellen |
| Name der öffentlichen IP-Adresse | VNet1BastionPIP  |
| SKU der öffentlichen IP-Adresse |  Standard  |
| Zuweisung  | statischen |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Erstellen eines Bastion-Hosts

Wenn Sie im Azure-Portal einen Bastionhost über einen bereits vorhandenen virtuellen Computer erstellen, werden verschiedene Einstellungen automatisch auf Standardwerte für Ihren virtuellen Computer bzw. Ihr virtuelles Netzwerk festgelegt.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu Ihrem virtuellen Computer, und wählen Sie dann **Verbinden** aus.

   :::image type="content" source="./media/quickstart-host-portal/vm-settings.png" alt-text="VM-Einstellungen" lightbox="./media/quickstart-host-portal/vm-settings.png":::
1. Wählen Sie in der Dropdownliste die Option **Bastion** aus.
1. Wählen Sie auf der Seite **TestVM | Verbinden** die Option **Bastion verwenden** aus.

   :::image type="content" source="./media/quickstart-host-portal/select-bastion.png" alt-text="VM-Einstellungen" border="false":::

1. Füllen Sie auf der Seite **Bastion** die folgenden Einstellungsfelder aus:

   * **Name**: Name des Bastionhosts.
   * **Subnetz**: Das Subnetz in Ihrem virtuellen Netzwerk, in dem die Bastion-Ressource bereitgestellt wird. Das Subnetz muss mit dem Namen **AzureBastionSubnet** erstellt werden. Aufgrund dieses Namens weiß Azure, in welchem Subnetz die Bastion-Ressource bereitgestellt werden soll. Dieses Subnetz unterscheidet sich von einem Gatewaysubnetz. Verwenden Sie mindestens ein Subnetz der Größe „/27“ oder ein größeres Subnetz („/27“, „/26“, „/25“ usw.).
   
      * Wählen Sie **Subnetzkonfiguration verwalten** aus.
      * Wähen Sie **AzureBastionSubnet** aus.
      * Passen Sie bei Bedarf den Adressbereich in CIDR-Notation an. Beispiel: 10.1.254.0/27.
      * Passen Sie keine anderen Einstellungen an. Wählen Sie **OK** aus, um die Subnetzänderungen zu akzeptieren und zu speichern, oder wählen Sie oben auf der Seite **x** aus, falls Sie keine Änderungen vornehmen möchten.
1. Klicken Sie in Ihrem Browser auf die Schaltfläche „Zurück“, um zurück zur Seite **Bastion** zu navigieren, und fahren Sie mit dem Angeben der Werte fort.
   * **Öffentliche IP-Adresse**: Der Name der öffentlichen IP-Adressressource.
   * **Öffentliche IP-Adresse:** Dies ist die öffentliche IP-Adresse der Bastion-Ressource für den RDP/SSH-Zugriff (über Port 443). Erstellen Sie eine neue öffentliche IP-Adresse.
1. Wählen Sie **Erstellen** aus, um den Bastionhost zu erstellen. Azure überprüft Ihre Einstellungen und erstellt anschließend den Host. Die Erstellung und Bereitstellung des Hosts und der zugehörigen Ressourcen dauert ungefähr fünf Minuten.

   :::image type="content" source="./media/quickstart-host-portal/validate.png" alt-text="VM-Einstellungen":::

## <a name="connect"></a><a name="connect"></a>Verbinden

Nachdem Bastion im virtuellen Netzwerk bereitgestellt wurde, wird die Seite „Verbinden“ angezeigt.

1. Geben Sie den Benutzernamen und das Kennwort für Ihren virtuellen Computer ein. Wählen Sie dann **Verbinden** aus.

   :::image type="content" source="./media/quickstart-host-portal/connect-vm.png" alt-text="VM-Einstellungen":::
1. Die RDP-Verbindung zu diesem virtuellen Computer über Bastion wird direkt im Azure-Portal (über HTML5) über Port 443 und den Bastion-Dienst geöffnet.

   :::image type="content" source="./media/quickstart-host-portal/connected.png" alt-text="VM-Einstellungen":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nachdem Sie Ihre Arbeit mit dem virtuellen Netzwerk und den virtuellen Computern beendet haben, sollten Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen löschen:

1. Geben Sie oben im Portal den Namen Ihrer Ressourcengruppe im Suchfeld **Suche** ein, und wählen Sie ihn in den Suchergebnissen aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie unter **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN** Ihre Ressourcengruppe ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Bastion-Host für Ihr virtuelles Netzwerk erstellt und anschließend darüber eine sichere Verbindung mit einem virtuellen Computer hergestellt. Anschließend können Sie mit dem folgenden Schritt fortfahren, wenn Sie eine Verbindung mit einer VM-Skalierungsgruppe herstellen möchten.

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einer VM-Skalierungsgruppe mit Azure Bastion](bastion-connect-vm-scale-set.md)

---
title: 'Schnellstart: Konfigurieren von Azure Bastion und Herstellen einer Verbindung mit einer VM über eine private IP-Adresse und einen Browser'
titleSuffix: Azure Bastion
description: Hier erfahren Sie, wie Sie einen Azure Bastion-Host über einen virtuellen Computer erstellen und mit Ihrem Browser über eine private IP-Adresse eine sichere Verbindung mit der VM herstellen können.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: quickstart
ms.date: 06/29/2021
ms.author: cherylmc
ms.openlocfilehash: 67211215b3dac9ad8774dc4e3c67a869bd031646
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113111265"
---
# <a name="quickstart-connect-to-a-vm-securely-through-a-browser-via-private-ip-address"></a>Schnellstart: Herstellen einer sicheren Verbindung mit einer VM über einen Browser und eine private IP-Adresse

Sie können mit Ihrem Browser mithilfe des Azure-Portals und Azure Bastion eine Verbindung mit einem virtuellen Computer (VM) herstellen. In diesem Schnellstartartikel wird gezeigt, wie Sie Azure Bastion auf der Grundlage Ihrer VM-Einstellungen konfigurieren. Nachdem der Dienst bereitgestellt wurde, ist die RDP/SSH-Umgebung für alle virtuellen Computer des jeweiligen virtuellen Netzwerks verfügbar. Die VM benötigt weder öffentliche IP-Adressen, Clientsoftware, Agents noch eine spezielle Konfiguration. Wenn Sie die öffentliche IP-Adresse auf Ihrer VM für keine anderen Zwecke benötigen, können Sie sie entfernen. Stellen Sie anschließend über das Portal mithilfe der privaten IP-Adresse eine Verbindung mit Ihrer VM her. Weitere Informationen zu Azure Bastion finden Sie unter [Was ist Azure Bastion?](bastion-overview.md).

## <a name="prerequisites"></a><a name="prereq"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sollten Sie über keine Organisation verfügen, [können Sie kostenlos eine erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Damit Sie über Ihren Browser mithilfe von Bastion eine Verbindung mit einer VM herstellen können, müssen Sie sich am Azure-Portal anmelden können.

* Ein virtueller Windows-Computer in einem virtuellen Netzwerk Wenn Sie nicht über einen virtuellen Computer verfügen, erstellen Sie einen über [Schnellstart: Erstellen einer VM](../virtual-machines/windows/quick-create-portal.md).

  * Wenn Sie Beispielwerte benötigen, finden Sie diese in den bereitgestellten [Beispielwerten](#values).
  * Wenn Sie bereits über ein virtuelles Netzwerk verfügen, stellen Sie sicher, dass Sie es auf der Registerkarte „Netzwerk“ auswählen, wenn Sie Ihre VM erstellen.
  * Wenn Sie noch nicht über ein virtuelles Netzwerk verfügen, können Sie gleichzeitig mit der VM ein virtuelles Netzwerk erstellen.
  * Sie benötigen keine öffentliche IP-Adresse für diese VM, um eine Verbindung über Azure Bastion herstellen zu können.

* Erforderliche VM-Rollen:
  * Rolle „Leser“ für den virtuellen Computer
  * Rolle „Leser“ für den Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
  
* Erforderliche VM-Ports:
  * Eingehende Ports: RDP (3389)

 >[!NOTE]
 >Die Verwendung von Azure Bastion in Verbindung mit privaten Azure DNS-Zonen wird derzeit nicht unterstützt. Ehe Sie loslegen, stellen Sie bitte sicher, dass das virtuelle Netzwerk, in dem Sie Ihre Bastion-Ressource bereitstellen möchten, nicht mit einer privaten DNS-Zone verknüpft ist.
 >

### <a name="example-values"></a><a name="values"></a>Beispielwerte

Sie können beim Erstellen dieser Konfiguration die folgenden Beispielwerte verwenden oder Ihre eigenen Werte einsetzen.

**Grundlegende VNet- und VM-Werte:**

|**Name** | **Wert** |
| --- | --- |
| Virtueller Computer| TestVM, |
| Ressourcengruppe | TestRG1 |
| Region | USA, Osten |
| Virtuelles Netzwerk | VNet1 |
| Adressraum | 10.1.0.0/16 |
| Subnetze | FrontEnd: 10.1.0.0/24 |

**Azure Bastion-Werte:**

|**Name** | **Wert** |
| --- | --- |
| Name | VNet1-bastion |
| + Subnetzname | AzureBastionSubnet |
| AzureBastionSubnet-Adressen | Ein Subnetz innerhalb Ihres VNet-Adressraums mit einer /27-Subnetzmaske. Beispiel: 10.1.1.0/27.  |
| Öffentliche IP-Adresse |  Neu erstellen |
| Name der öffentlichen IP-Adresse | VNet1-ip  |
| SKU der öffentlichen IP-Adresse |  Standard  |
| Zuweisung  | statischen |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Erstellen eines Bastion-Hosts

Es gibt verschiedene Möglichkeiten, einen Bastionhost zu konfigurieren. In den folgenden Schritten erstellen Sie direkt über Ihre VM einen Bastionhost im Azure-Portal. Wenn Sie einen Host über eine VM erstellen, werden verschiedene Einstellungen automatisch entsprechend Ihres virtuellen Computers und/oder Ihres virtuellen Netzwerks ausgefüllt.

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zur VM, mit der Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**.

   :::image type="content" source="./media/quickstart-host-portal/vm-connect.png" alt-text="Screenshot der VM-Einstellungen" lightbox="./media/quickstart-host-portal/vm-connect.png":::
1. Wählen Sie in der Dropdownliste die Option **Bastion** aus.

   :::image type="content" source="./media/quickstart-host-portal/bastion.png" alt-text="Screenshot der Dropdownliste mit „Bastion“" lightbox="./media/quickstart-host-portal/bastion.png":::
1. Wählen Sie auf der Seite **TestVM | Verbinden** die Option **Bastion verwenden** aus.

   :::image type="content" source="./media/quickstart-host-portal/select-bastion.png" alt-text="Screenshot von „Bastion verwenden“":::

1. Konfigurieren Sie die Werte auf der Seite **Verbindung herstellen über Azure Bastion**.

   * **Schritt 1**: Die Werte werden vorab aufgefüllt, weil Sie den Bastionhost direkt auf der VM erstellen.

   * **Schritt 2**: Der Adressraum wird mit einem empfohlenen Adressraum vorab aufgefüllt. Das AzureBastionSubnet muss über einen Adressraum der Größe „/27“ oder mehr (/26, /25 usw.) verfügen.

   :::image type="content" source="./media/quickstart-host-portal/create-subnet.png" alt-text="Screenshot der Erstellung des Subnetzes für Bastion":::

1. Klicken Sie auf **Subnetz erstellen**, um das AzureBastionSubnet zu erstellen.
1. Nach dem Erstellen des Subnetzes geht die Seite automatisch zu **Schritt 3** über. Verwenden Sie für Schritt 3 die folgenden Werte:

   * **Name**: Name des Bastionhosts.
   * **Öffentliche IP-Adresse:** Wählen Sie **Neu erstellen**.
   * **Name der öffentlichen IP-Adresse**: Der Name der öffentlichen IP-Adressressource.
   * **SKU der öffentlichen IP-Adresse**: Vorkonfiguriert als **Standard**.
   * **Zuweisung**: Vorkonfiguriert als **Statisch**. Sie können keine dynamische Zuweisung für Azure Bastion verwenden.
   * **Ressourcengruppe:** dieselbe Ressourcengruppe wie die VM

   :::image type="content" source="./media/quickstart-host-portal/create-bastion.png" alt-text="Screenshot von Schritt 3":::
1. Nachdem Sie die Werte angegeben haben, wählen Sie **Azure Bastion mit Standardwerten erstellen** aus. Azure überprüft Ihre Einstellungen und erstellt anschließend den Host. Die Erstellung und Bereitstellung des Hosts und der zugehörigen Ressourcen dauert ungefähr fünf Minuten.

## <a name="remove-vm-public-ip-address"></a><a name="remove"></a>Entfernen einer öffentlichen IP-Adresse einer VM

[!INCLUDE [Remove a public IP address from a VM](../../includes/bastion-remove-ip.md)]

## <a name="connect-to-a-vm"></a><a name="connect"></a>Herstellen einer Verbindung mit einem virtuellen Computer

Nachdem Bastion im virtuellen Netzwerk bereitgestellt wurde, wird die Seite „Verbinden“ angezeigt.

1. Geben Sie den Benutzernamen und das Kennwort für Ihren virtuellen Computer ein. Wählen Sie dann **Verbinden** aus.

   :::image type="content" source="./media/quickstart-host-portal/connect.png" alt-text="Screenshot: Dialogfeld „Verbindung über Azure Bastion herstellen“":::
1. Die RDP-Verbindung zu diesem virtuellen Computer über Bastion wird direkt im Azure-Portal (über HTML5) über Port 443 und den Bastion-Dienst geöffnet. 

   * Wenn Sie eine Verbindung herstellen, sieht der Desktop der VM möglicherweise anders aus als im Beispielscreenshot. 
   * Tastenkombinationen während der Verbindung mit einer VM weisen möglicherweise ein anderes Verhalten als Tastenkombinationen auf einem lokalen Computer auf. Wenn Sie beispielsweise über einen Windows-Client eine Verbindung mit einer Windows-VM hergestellt haben, ist STRG+ALT+ENDE die Tastenkombination für STRG+ALT+ENTF auf einem lokalen Computer. Wenn Sie dieselbe Aktion bei einer Verbindung von einem Mac aus mit einer Windows-VM ausführen möchten, lautet die Tastenkombination Fn+STRG+ALT+RÜCKTASTE.

   :::image type="content" source="./media/quickstart-host-portal/connected.png" alt-text="RDP-Verbindung":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nachdem Sie Ihre Arbeit mit dem virtuellen Netzwerk und den virtuellen Computern beendet haben, sollten Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen löschen:

1. Geben Sie oben im Portal den Namen Ihrer Ressourcengruppe im Suchfeld **Suche** ein, und wählen Sie ihn in den Suchergebnissen aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie unter **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN** Ihre Ressourcengruppe ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Bastionhost für Ihr virtuelles Netzwerk erstellt und anschließend über Bastion eine sichere Verbindung mit einem virtuellen Computer hergestellt. Anschließend können Sie mit dem folgenden Schritt fortfahren, wenn Sie eine Verbindung mit einer VM-Skalierungsgruppe herstellen möchten.

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einer VM-Skalierungsgruppe mit Azure Bastion](bastion-connect-vm-scale-set.md)

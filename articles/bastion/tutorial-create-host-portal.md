---
title: 'Tutorial: Erstellen eines Azure Bastion-Hosts: Virtueller Windows-Computer: Portal'
description: In diesem Artikel erfahren Sie, wie Sie einen Azure Bastion-Host erstellen und eine Verbindung mit einem virtuellen Windows-Computer herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: tutorial
ms.date: 02/12/2021
ms.author: cherylmc
ms.openlocfilehash: 60b49e5b6e103a85d79cf8495f2743b22e434c96
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100586801"
---
# <a name="tutorial-configure-bastion-and-connect-to-a-windows-vm-through-a-browser"></a>Tutorial: Konfigurieren von Bastion und Herstellen einer Verbindung mit einer Windows-VM über einen Browser

In diesem Tutorial erfahren Sie, wie Sie über Ihren Browser eine Verbindung mit einem virtuellen Computer herstellen und dabei Azure Bastion und das Azure-Portal verwenden. Im Azure-Portal wird Bastion für Ihr virtuelles Netzwerk bereitgestellt. Nach der Bereitstellung von Bastion wird über das Azure-Portal unter Verwendung der privaten IP-Adresse eine Verbindung mit einem virtuellen Computer hergestellt. Für Ihren virtuellen Computer ist keine öffentliche IP-Adresse oder spezielle Software erforderlich. Nachdem der Dienst bereitgestellt wurde, ist die RDP/SSH-Umgebung für alle virtuellen Computer des jeweiligen virtuellen Netzwerks verfügbar. Weitere Informationen zu Azure Bastion finden Sie unter [Was ist Azure Bastion?](bastion-overview.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Bastionhosts für Ihr VNET
> * Herstellen einer Verbindung mit einem virtuellen Windows-Computer

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein virtuelles Netzwerk
* Ein virtueller Windows-Computer im virtuellen Netzwerk
* Folgende erforderliche Rollen:
  * Rolle „Leser“ für den virtuellen Computer
  * Rolle „Leser“ für den Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
  * Rolle „Leser“ für die Azure Bastion-Ressource

* Ports: Zum Herstellen einer Verbindung mit dem virtuellen Windows-Computer müssen die folgenden Ports auf Ihrer Windows-VM geöffnet sein:
  * Eingehende Ports: RDP (3389)

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Erstellen eines Bastion-Hosts

In diesem Abschnitt erfahren Sie, wie Sie das Bastionhostobjekt in Ihrem VNET erstellen. Dies ist erforderlich, um eine sichere Verbindung mit einem virtuellen Computer im VNET zu erstellen.

1. Wählen Sie auf der Seite **Start** die Option **+ Ressource erstellen** aus.
1. Geben Sie auf der Seite **Neu** im Suchfeld den Suchbegriff **Bastion** ein, und drücken Sie anschließend die **EINGABETASTE**, um zu den Suchergebnissen zu gelangen. Vergewissern Sie beim Ergebnis für **Bastion**, dass der Herausgeber „Microsoft“ lautet.
1. Klicken Sie auf **Erstellen**.
1. Konfigurieren Sie auf der Seite **Bastion-Instanz erstellen** eine neue Bastion-Ressource.

   :::image type="content" source="./media/tutorial-create-host-portal/bastion-basics.png" alt-text="Erstellen eines Bastionhosts" lightbox="./media/tutorial-create-host-portal/bastion-basics.png":::

    * **Abonnement**: Das Azure-Abonnement, das Sie zum Erstellen einer neuen Bastion-Ressource verwenden möchten.
    * **Ressourcengruppe**: Die Azure-Ressourcengruppe, in der die neue Bastion-Ressource erstellt wird. Erstellen Sie eine Ressourcengruppe, wenn noch keine vorhanden ist.
    * **Name**: Der Name der neuen Bastion-Ressource.
    * **Region**: Die öffentliche Azure-Region, in der die Ressource erstellt wird.
    * **Virtuelles Netzwerk:** Das virtuelle Netzwerk, in dem die Bastion-Ressource erstellt wird. Im Rahmen dieses Prozesses können Sie über das Portal ein neues virtuelles Netzwerk erstellen oder eines der vorhandenen virtuellen Netzwerke verwenden. Achten Sie bei Verwendung eines bereits vorhandenen virtuellen Netzwerks darauf, dass der freie Adressraum die Anforderungen des Bastion-Subnetzes erfüllt. Sollte Ihr virtuelles Netzwerk nicht in der Dropdownliste angezeigt werden, überprüfen Sie, ob Sie die richtige Ressourcengruppe ausgewählt haben.
    * **Subnetz**: Nach dem Erstellen oder Auswählen eines virtuellen Netzwerks wird das Subnetzfeld angezeigt. Das Subnetz in Ihrem virtuellen Netzwerk, in dem der neue Bastion-Host bereitgestellt wird. Das Subnetz ist für den Bastionhost dediziert. Wählen Sie **Subnetzkonfiguration verwalten** aus, und erstellen Sie das Azure Bastion-Subnetz. Wählen Sie **+ Subnetz** aus, und erstellen Sie ein Subnetz unter Verwendung der folgenden Richtlinien:

         * Das Subnetz muss **AzureBastionSubnet** heißen.
         * Das Subnetz muss mindestens die Größe „/27“ haben.

      Weitere Angaben sind nicht erforderlich. Wählen Sie **OK** und anschließend oben auf der Seite die Option **Bastion-Instanz erstellen** aus, um zur Bastion-Konfigurationsseite zurückzukehren.
    * **Öffentliche IP-Adresse:** Die öffentliche IP-Adresse der Bastion-Ressource für den RDP-/SSH-Zugriff (über den Port 443). Erstellen Sie eine neue öffentliche IP-Adresse. Die öffentliche IP-Adresse muss sich in der gleichen Region befinden wie die Bastion-Ressource, die Sie erstellen. Diese IP-Adresse hat nichts mit den virtuellen Computern zu tun, mit denen Sie eine Verbindung herstellen möchten. Vielmehr handelt es sich dabei um die öffentliche IP-Adresse für die Bastionhostressource.
    * **Öffentliche IP-Adresse**: Der Name der öffentlichen IP-Adressressource. Für dieses Tutorial können Sie den Standardwert beibehalten.
    * **SKU der öffentlichen IP-Adresse**: Diese Einstellung ist standardmäßig bereits auf **Standard** festgelegt. Azure Bastion verwendet/unterstützt nur die SKU „Standard“ für die öffentliche IP-Adresse.
    * **Zuweisung**: Diese Einstellung ist standardmäßig bereits auf **Statisch** festgelegt.

1. Wählen Sie nach Angabe der Einstellungen die Option **Überprüfen und erstellen** aus. Daraufhin werden die Werte überprüft. Nach erfolgreicher Überprüfung können Sie die Bastion-Ressource erstellen.
1. Klicken Sie auf **Erstellen**.
1. Daraufhin wird eine Meldung mit dem Hinweis angezeigt, dass Ihre Bereitstellung ausgeführt wird. Der Status der Ressourcenerstellung wird auf dieser Seite angezeigt. Die Erstellung und Bereitstellung der Bastion-Ressource dauert etwa fünf Minuten.

## <a name="connect-to-a-vm"></a>Herstellen einer Verbindung mit einem virtuellen Computer

[!INCLUDE [Connect to a Windows VM](../../includes/bastion-vm-rdp.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, löschen Sie Ihre Ressourcen wie folgt:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das **Suchfeld** am oberen Rand des Portals ein. Wenn Ihre Ressourcengruppe in den Suchergebnissen angezeigt wird, wählen Sie sie aus.
1. Wählen Sie die Option **Ressourcengruppe löschen**.
1. Geben Sie unter **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen Ihrer Ressourcengruppe ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Bastionhost erstellt, den Bastionhost einem virtuellen Netzwerk zugeordnet und anschließend eine Verbindung mit einem virtuellen Windows-Computer hergestellt. Falls gewünscht, können Sie Netzwerksicherheitsgruppen mit Ihrem Azure Bastion-Subnetz verwenden. Weitere Informationen finden Sie hier:

> [!div class="nextstepaction"]
> [Arbeiten mit NSGs](bastion-nsg.md)

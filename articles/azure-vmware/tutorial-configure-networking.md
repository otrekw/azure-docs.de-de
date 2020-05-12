---
title: 'Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure'
description: Hier erfahren Sie, wie Sie das erforderliche Netzwerk zum Bereitstellen Ihrer privaten Cloud in Azure erstellen und konfigurieren.
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 41043b98a6e270d8d9f4373de7876b3fcae86747
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82837616"
---
# <a name="tutorial-configure-networking-for-your-vmware-private-cloud-in-azure"></a>Tutorial: Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure

Eine private Azure VMware Solution (AVS)-Cloud erfordert ein virtuelles Netzwerk (VNET). Da AVS Ihr lokales vCenter in der Vorschauversion nicht unterstützt, müssen Sie zusätzliche Schritte für die Integration in Ihre lokale Umgebung durchführen. Die Einrichtung einer ExpressRoute-Leitung und eines Gateways für das virtuelle Netzwerk ist ebenfalls erforderlich und wird in diesem Tutorial behandelt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Gateways für das virtuelle Netzwerk
> * Verbinden der ExpressRoute-Leitung mit dem Gateway
> * Ermitteln der URLs für vCenter und NSX Manager

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Navigieren Sie zu der Ressourcengruppe, die Sie im [vorherigen Tutorial](tutorial-create-private-cloud.md) erstellt haben, und wählen Sie **+ Hinzufügen** aus, um eine neue Ressource zu definieren.

Geben Sie **Virtuelles Netzwerk** in das Feld **Marketplace durchsuchen** ein. Suchen Sie nach der VNET-Ressource.

Wählen Sie auf der Seite „Virtuelles Netzwerk“ die Option **Erstellen** aus, um Ihr virtuelles Netzwerk für die private Cloud einzurichten.

Geben Sie auf der Seite **Virtuelles Netzwerk erstellen** die relevanten Details für Ihr virtuelles Netzwerk ein. Eine Beschreibung der Eigenschaften finden Sie in der folgenden Tabelle:

> [!IMPORTANT]
> Sie müssen einen Adressraum verwenden, der sich **nicht** mit dem Adressraum überlappt, den Sie im vorherigen Tutorial beim Erstellen der privaten Cloud verwendet haben.

Geben Sie auf der Registerkarte **Grundlagen** einen Namen für das virtuelle Netzwerk ein, wählen Sie die entsprechende Region und dann **Weiter: IP-Adressen** aus.

Geben Sie auf der Registerkarte **IP-Adressen** unter **IPv4-Adressraum** den Adressraum ein, den Sie im vorherigen Tutorial erstellt haben.

Wählen Sie **+ Subnetz hinzufügen** aus, und geben Sie auf der Seite **Subnetz hinzufügen** einen Namen und einen entsprechenden Adressbereich für das Subnetz an. Wählen Sie zum Abschluss **Hinzufügen** aus.

Klicken Sie auf **Bewerten + erstellen**.

:::image type="content" source="./media/tutorial-configure-networking/create-virtual-network.png" alt-text="Erstellen eines virtuellen Netzwerks" border="true":::

Überprüfen Sie die Angaben, und wählen Sie dann **Erstellen** aus. Nach Abschluss der Bereitstellung wird Ihr virtuelles Netzwerk in der Ressourcengruppe angezeigt.

## <a name="create-a-virtual-network-gateway"></a>Erstellen eines Gateways für das virtuelle Netzwerk

Im vorherigen Abschnitt haben Sie ein virtuelles Netzwerk erstellt. Jetzt erstellen Sie ein Gateway für das virtuelle Netzwerk.

Wählen Sie in Ihrer Ressourcengruppe **+ Hinzufügen** aus, um eine neue Ressource hinzuzufügen.

Geben Sie **Gateway für virtuelle Netzwerke** in das Feld **Marketplace durchsuchen** ein. Suchen Sie nach der VNET-Ressource.

Wählen Sie auf der Seite **Gateway für virtuelle Netzwerke** die Option **Erstellen** aus.

Geben Sie auf der Registerkarte „Grundlagen“ der Seite **Gateway für virtuelle Netzwerke erstellen** Werte für die Felder an. Eine Beschreibung der Felder finden Sie in der folgenden Tabelle:

| Feld | Wert |
| --- | --- |
| **Abonnement** | Dieser Wert ist bereits mit dem Abonnement aufgefüllt, zu dem die Ressourcengruppe gehört. |
| **Ressourcengruppe** | Dieser Wert ist bereits für die aktuelle Ressourcengruppe aufgefüllt. Es sollte die Ressourcengruppe eingetragen sein, die Sie bei einem vorherigen Test erstellt haben. |
| **Name** | Geben Sie einen eindeutigen Namen für das Gateway des virtuellen Netzwerks ein. |
| **Region** | Wählen Sie den geografischen Standort des Gateways des virtuellen Netzwerks aus. |
| **Gatewaytyp** | Wählen Sie **ExpressRoute** aus. |
| **VPN-Typ** | Wählen Sie **Routenbasiert** aus. |
| **SKU** | Übernehmen Sie den Standardwert : **Standard**. |
| **Virtuelles Netzwerk** | Wählen Sie das zuvor erstellte virtuelle Netzwerk aus. Falls das virtuelle Netzwerk nicht angezeigt wird, vergewissern Sie sich, dass die Region des Gateways mit der Ihres virtuellen Netzwerks übereinstimmt. |
| **Adressbereich für Gatewaysubnetz** | Dieser Wert wird aufgefüllt, wenn Sie das virtuelle Netzwerk auswählen. Lassen Sie den Standardwert unverändert. |
| **Öffentliche IP-Adresse** | Wählen Sie **Neu erstellen**. |

:::image type="content" source="./media/tutorial-configure-networking/create-virtual-network-gateway.png" alt-text="Erstellen eines Gateways" border="true":::

Wählen Sie **Bewerten + erstellen** aus, überprüfen Sie auf der nächsten Seite die Details, und wählen Sie dann **Erstellen** aus, um die Bereitstellung des Gateways des virtuellen Netzwerks zu starten. Fahren Sie nach Abschluss der Bereitstellung mit dem nächsten Abschnitt dieses Tutorials fort, um Ihre ExpressRoute-Verbindung mit dem virtuellen Netzwerk zu verbinden, das Ihre private Cloud enthält.

## <a name="connect-expressroute-to-the-virtual-network-gateway"></a>Verbinden von ExpressRoute mit dem Gateway des virtuellen Netzwerks

In diesem Abschnitt wird schrittweise erläutert, wie Sie eine Verbindung zwischen Ihrer privaten AVS-Cloud und dem von Ihnen erstellten Gateway des virtuellen Netzwerks hinzufügen.

Navigieren Sie zu der privaten Cloud, die Sie im vorherigen Tutorial erstellt haben, wählen Sie unter **Verwalten** die Option **Konnektivität** und dann die Registerkarte **ExpressRoute** aus.

Kopieren Sie den Autorisierungsschlüssel. Falls kein Autorisierungsschlüssel angezeigt wird, müssen Sie einen erstellen, indem Sie **+ Request an authorization key** (Autorisierungsschlüssel anfordern) auswählen.

:::image type="content" source="./media/tutorial-configure-networking/request-auth-key.png" alt-text="Anfordern eines Autorisierungsschlüssels" border="true":::

Navigieren Sie zu dem Gateway des virtuellen Netzwerks, das Sie im vorherigen Schritt erstellt haben, und wählen Sie unter **Einstellungen** die Option **Verbindungen** aus. Wählen Sie auf der Seite **Verbindungen** die Option **+ Hinzufügen** aus.

Geben Sie auf der Seite **Verbindung hinzufügen** Werte für die Felder an. Eine Beschreibung der Felder finden Sie in der folgenden Tabelle:

| Feld | Wert |
| --- | --- |
| **Name**  | Geben Sie einen Namen für die Verbindung ein.  |
| **Verbindungstyp**  | Wählen Sie **ExpressRoute** aus.  |
| **Autorisierung einlösen**  | Stellen Sie sicher, dass dieses Kontrollkästchen aktiviert ist.  |
| **Gateway für virtuelle Netzwerke** | Das Gateway für das virtuelle Netzwerk, das Sie zuvor erstellt haben.  |
| **Autorisierungsschlüssel**  | Kopieren Sie den Autorisierungsschlüssel von der Registerkarte „ExpressRoute“ für Ihre Ressourcengruppe, und fügen Sie ihn ein. |
| **Peerleitungs-URI**  | Kopieren Sie die ExpressRoute-ID von der Registerkarte „ExpressRoute“ für Ihre Ressourcengruppe, und fügen Sie sie ein.  |

Klicken Sie auf **OK**. Dadurch wird die Verbindung zwischen Ihrer ExpressRoute-Leitung und Ihrem virtuellen Netzwerk erstellt.

:::image type="content" source="./media/tutorial-configure-networking/add-connection.png" alt-text="Hinzufügen einer Verbindung" border="true":::

## <a name="locate-the-urls-for-vcenter-and-nsx-manager"></a>Ermitteln der URLs für vCenter und NSX Manager

Damit Sie sich bei vCenter und NSX Manager anmelden können, benötigen Sie die URLs des vCenter-Webclients und der NSX-T Manager-Website. So ermitteln Sie die URLs:

Navigieren Sie zu Ihrer privaten AVS-Cloud, und wählen Sie unter **Verwalten** die Option **Identität** aus. Dort finden Sie die benötigten Informationen.

:::image type="content" source="./media/tutorial-configure-networking/locate-urls.png" alt-text="Ermitteln der vCenter-URLs" border="true":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Gateways für das virtuelle Netzwerk
> * Verbinden der ExpressRoute-Leitung mit dem Gateway
> * Ermitteln der URLs für vCenter und NSX Manager

Im nächsten Tutorial erfahren Sie, wie Sie eine Jumpbox zum Herstellen einer Verbindung mit Ihrer Umgebung erstellen, damit Sie Ihre private Cloud lokal verwalten können.

> [!div class="nextstepaction"]
> [Zugreifen auf die private Cloud](tutorial-access-private-cloud.md)

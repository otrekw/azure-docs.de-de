---
title: 'Schnellstart: Erstellen eines NAT-Gateways: Azure-Portal'
titlesuffix: Azure Virtual Network NAT
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie ein NAT-Gateway mithilfe des Azure-Portals erstellen.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: 8913c956554fa7bf1d0362b44dc6f8031ffd74f9
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77429131"
---
# <a name="quickstart-create-a-nat-gateway-using-the-azure-portal"></a>Schnellstart: Erstellen eines NAT-Gateways mithilfe des Azure-Portals

In dieser Schnellstartanleitung erfahren Sie, wie Sie den Azure Virtual Network NAT-Dienst verwenden. Sie erstellen ein NAT-Gateway, um ausgehende Konnektivität für einen virtuellen Computer in Azure bereitzustellen. 

>[!NOTE] 
>Azure Virtual Network NAT ist zurzeit als öffentliche Vorschauversion verfügbar und nur in bestimmten [Regionen](./nat-overview.md#region-availability) verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

> [!IMPORTANT]
> Greifen Sie nach dem [Aktivieren der Vorschauversion](./nat-overview.md#enable-preview) von Virtual Network NAT für Ihr Abonnement über https://aka.ms/natportal auf das Portal zu.

Melden Sie sich beim [Azure-Portal](https://aka.ms/natportal) an.


### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Sie müssen zunächst die Ressourcengruppe und das virtuelle Netzwerk erstellen, damit Sie einen virtuellen Computer bereitstellen und das NAT-Gateway verwenden können.  

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus, oder suchen Sie über die Marketplace-Suche nach **Virtuelles Netzwerk**.

2. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | Name | Geben Sie **myVNet** ein. |
    | Adressraum | Geben Sie **192.168.0.0/16** ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie „Neu erstellen“ aus: **myResourceGroupNAT**. |
    | Location | Wählen Sie **USA, Osten 2** aus.|
    | Subnetzname | Geben Sie **mySubnet** ein. |
    | Subnetzadressbereich | Geben Sie **192.168.0.0/24** ein. |

3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

### <a name="create-a-vm-to-use-the-nat-gateway"></a>Erstellen eines virtuellen Computers zur Verwendung des NAT-Gateways

Sie erstellen nun einen virtuellen Computer zur Verwendung des NAT-Diensts. Dieser virtuelle Computer verfügt über eine öffentliche IP-Adresse, die als öffentliche IP auf Instanzebene verwendet wird, um den Zugriff auf den virtuellen Computer zu ermöglichen. Der NAT-Dienst erkennt die Flussrichtung und ersetzt in Ihrem Subnetz das Standardziel im Internet. Die öffentliche IP-Adresse des virtuellen Computers wird nicht für ausgehende Verbindungen verwendet.

1. Wählen Sie links oben im Portal **Ressource erstellen** > **Compute** > **Ubuntu Server 18.04 LTS** aus, oder suchen Sie über die Marketplace-Suche nach **Ubuntu Server 18.04 LTS**.

2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupNAT** aus.
   - **Instanzendetails** > **Name des virtuellen Computers**: Geben Sie **myVM** ein.
   - **Instanzendetails** > **Region**: Wählen Sie **USA, Osten 2** aus.
   - **Administratorkonto** > **Authentifizierungstyp**: Wählen Sie **Kennwort** aus.
   - **Administratorkonto**: Geben Sie Informationen für **Benutzername**, **Kennwort** und **Kennwort bestätigen** ein.
   - **Regeln für eingehende Ports** > **Öffentliche Eingangsports**: Wählen Sie **Ausgewählte Ports zulassen** aus.
   - **Regeln für eingehende Ports** > **Eingangsports auswählen**: Wählen Sie **SSH (22)** aus.
   - Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.

3. Stellen Sie auf der Registerkarte **Netzwerk** sicher, dass Folgendes ausgewählt ist:
   - **Virtuelles Netzwerk:** **myVnet**
   - **Subnetz**: **mySubnet**
   - **Öffentliche IP-Adresse**: Wählen Sie **Neu erstellen** aus.  Geben Sie im Fenster **Öffentliche IP-Adresse erstellen** im Feld **Name** den Namen **myPublicIPVM** ein, und wählen Sie unter **SKU** die Option **Standard** aus.  Klicken Sie auf **OK**.
   - **NIC-Netzwerksicherheitsgruppe:** Wählen Sie **Basic** aus.
   - **Öffentliche Eingangsports**: Wählen Sie **Ausgewählte Ports zulassen** aus.
   - **Eingangsports auswählen**: Vergewissern Sie sich, dass **SSH** ausgewählt ist.

4. Legen Sie auf der Registerkarte **Verwaltung** unter **Überwachung** die Option **Startdiagnose** auf **Aus** fest.

5. Klicken Sie auf **Überprüfen + erstellen**. 

6. Überprüfen Sie die Einstellungen, und klicken Sie auf **Erstellen**.

## <a name="create-the-nat-gateway"></a>Erstellen des NAT-Gateways

Sie können mehrere öffentliche IP-Adressressourcen und/oder Präfixes für öffentliche IP-Adressen verwenden. Sie fügen eine öffentliche IP-Ressource, ein Präfix für öffentliche IP-Adressen und eine NAT-Gatewayressource hinzu.

In diesem Abschnitt erfahren Sie, wie Sie mit der NAT-Gatewayressource die folgenden Komponenten des NAT-Diensts erstellen und konfigurieren:
  - Öffentlicher IP-Adresspool und Präfix für öffentliche IP-Adressen für ausgehende Datenflüsse, die von der NAT-Gatewayressource übersetzt werden
  - Ändern des Leerlauftimeouts von vier Minuten (Standardwert) in zehn Minuten

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

1. Wählen Sie links oben im Portal **Ressource erstellen** > **Netzwerk** > **Öffentliche IP-Adresse** aus, oder suchen Sie über die Marketplace-Suche nach **Öffentliche IP-Adresse**.

2. Geben Sie unter **Öffentliche IP-Adresse erstellen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | IP-Version | Wählen Sie **IPv4** aus.
    | SKU | Wählen Sie **Standard** aus.
    | Name | Geben Sie **myPublicIP** ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **myResourceGroupNAT** aus. |
    | Location | Wählen Sie **USA, Osten 2** aus.|

3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

### <a name="create-a-public-ip-prefix"></a>Erstellen des Präfixes für öffentliche IP-Adressen

1. Wählen Sie links oben im Portal **Ressource erstellen** > **Netzwerk** > **Präfix für öffentliche IP-Adresse** aus, oder suchen Sie über die Marketplace-Suche nach **Präfix für öffentliche IP-Adresse**. 

2. Geben Sie auf der Registerkarte **Grundlagen** unter **Präfix für öffentliche IP-Adresse erstellen** die folgenden Werte ein, oder wählen Sie sie aus:
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupNAT**> aus.
   - **Instanzendetails** > **Name**: Geben Sie **myPublicIPprefix** ein.
   - **Instanzendetails** > **Region**: Wählen Sie **USA, Osten 2** aus.
   - **Instanzendetails** > **Präfixgröße**: Wählen Sie **/31 (2 addresses)** (/31 (2 Adressen)) aus.

3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Bewerten + erstellen** aus.

4. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.
   

### <a name="create-a-nat-gateway-resource"></a>Erstellen einer NAT-Gatewayressource

1. Wählen Sie links oben im Portal **Ressource erstellen** > **Netzwerk** > **NAT-Gateway** aus, oder suchen Sie über die Marketplace-Suche nach **NAT-Gateway**.

2. Geben Sie auf der Registerkarte **Grundlagen** unter **Gateway für die Netzwerkadressübersetzung (NAT) erstellen** die folgenden Werte ein, oder wählen Sie sie aus:
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupNAT** aus.
   - **Instanzendetails** > **Name des NAT-Gateways**: Geben Sie **myNATgateway** ein.
   - **Instanzendetails** > **Region**: Wählen Sie **USA, Osten 2** aus.
   - **Instanzendetails** > **Leerlauftimeout (Minuten)** : Geben Sie **10** ein.
   - Wählen Sie die Registerkarte **Öffentliche IP-Adresse** oder **Weiter: Öffentliche IP-Adresse** aus.

3. Geben Sie auf der Registerkarte **Öffentliche IP-Adresse** die folgenden Werte ein, oder wählen Sie sie aus:
   - **Öffentliche IP-Adressen**: Wählen Sie **myPublicIP** aus.
   - **Präfixe für öffentliche IP-Adressen**: Wählen Sie **myPublicIPprefix** aus.
   - Wählen Sie die Registerkarte **Subnetz** oder **Weiter: Subnetz** aus.

4. Geben Sie auf der Registerkarte **Subnetz** die folgenden Werte ein, oder wählen Sie sie aus:
   - **Virtuelles Netzwerk**: Wählen Sie **myResourceGroupNAT** > **myVnet** aus.
   - **Subnetzname**: Aktivieren Sie das Kästchen neben **mySubnet**.

5. Klicken Sie auf **Überprüfen + erstellen**.

6. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

## <a name="discover-the-ip-address-of-the-vm"></a>Ermitteln der IP-Adresse des virtuellen Computers

1. Wählen Sie im Portal auf der linken Seite **Ressourcengruppen** aus.
2. Wählen Sie **myResourceGroupNAT** aus.
3. Wählen Sie **myVM** aus.
4. Kopieren Sie unter **Übersicht** den Wert unter **Öffentliche IP-Adresse**, und fügen Sie ihn in einen Editor ein, damit Sie ihn für den Zugriff auf den virtuellen Computer verwenden können.

>[!IMPORTANT]
>Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in einen Editor ein, damit Sie sie für den Zugriff auf den virtuellen Computer verwenden können.

## <a name="sign-in-to-vm"></a>Anmelden beim virtuellen Computer

Öffnen Sie [Azure Cloud Shell](https://shell.azure.com) im Browser. Verwenden Sie die im vorherigen Schritt abgerufene IP-Adresse, um eine SSH-Verbindung mit dem virtuellen Computer herzustellen.

```azurecli-interactive
ssh <username>@<ip-address-destination>
```

Sie können den NAT-Dienst nun verwenden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, das NAT-Gateway und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie die Ressourcengruppe **myResourceGroupNAT** aus, die das NAT-Gateway enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein NAT-Gateway und einen virtuellen Computer für seine Verwendung erstellt. 

Sehen Sie sich die Metriken in Azure Monitor an, um Informationen zum Betrieb Ihres NAT-Diensts anzuzeigen. Diagnostizieren Sie Probleme wie die Ressourcenüberlastung von verfügbaren SNAT-Ports.  Die Ressourcenüberlastung von SNAT-Ports wird durch Hinzufügen zusätzlicher Ressourcen für öffentliche IP-Adressen und/oder zusätzlicher Präfixressourcen für öffentliche IP-Adressen behoben.


- Erfahren Sie mehr über [Azure Virtual Network NAT](./nat-overview.md).
- Informieren Sie sich über [NAT-Gatewayressourcen](./nat-gateway-resource.md).
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe der Azure CLI](./quickstart-create-nat-gateway-cli.md)
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe von Azure PowerShell](./quickstart-create-nat-gateway-powershell.md)
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe des Azure-Portals](./quickstart-create-nat-gateway-portal.md)
- [Senden Sie Feedback zur Public Preview.](https://aka.ms/natfeedback)
> [!div class="nextstepaction"]


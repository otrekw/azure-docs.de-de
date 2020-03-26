---
title: 'Tutorial: Erstellen und Testen eines NAT-Gateways: Azure-Portal'
titlesuffix: Azure Virtual Network NAT
description: In diesem Tutorial wird veranschaulicht, wie Sie mit dem Azure-Portal ein NAT-Gateway erstellen und den NAT-Dienst testen.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumundD
Customer intent: I want to test a NAT Gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2020
ms.author: allensu
ms.openlocfilehash: 7c26487018b9632b5780dcd7c56ab36f286f8cac
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80059997"
---
# <a name="tutorial-create-a-nat-gateway-using-the-azure-portal-and-test-the-nat-service"></a>Tutorial: Erstellen eines NAT-Gateways mit dem Azure-Portal und Testen des NAT-Diensts

In diesem Tutorial erstellen Sie ein NAT-Gateway, um ausgehende Konnektivität für virtuelle Computer in Azure bereitzustellen. Sie stellen einen virtuellen Quell- und Zielcomputer bereit, um das NAT-Gateway zu testen. Sie testen das NAT-Gateway, indem Sie über eine öffentliche IP-Adresse vom virtuellen Quell- zum Zielcomputer Verbindungen in ausgehender Richtung herstellen.  In diesem Tutorial werden die Quelle und das Ziel der Einfachheit halber in zwei unterschiedlichen virtuellen Netzwerken jeweils in derselben Ressourcengruppe bereitgestellt.

Falls Sie es vorziehen, können Sie diese Schritte auch mit der [Azure CLI](tutorial-create-validate-nat-gateway-cli.md) oder [Azure PowerShell](tutorial-create-validate-nat-gateway-powershell.md) anstatt mit dem Portal ausführen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="prepare-the-source-for-outbound-traffic"></a>Vorbereiten der Quelle für ausgehenden Datenverkehr

In den folgenden Abschnitten wird die Konfiguration einer vollständigen Testumgebung und die Durchführung von Tests Schritt für Schritt für Sie beschrieben. Wir beginnen mit der Quelle, für die die NAT-Gatewayressource verwendet wird, die in einem der nachfolgenden Schritte erstellt wird.

## <a name="virtual-network-and-parameters"></a>Virtuelles Netzwerk und Parameter

Sie müssen zunächst die Ressourcengruppe und das virtuelle Netzwerk erstellen, damit Sie einen virtuellen Computer bereitstellen und das NAT-Gateway verwenden können.

In den Schritten dieses Abschnitts müssen die folgenden Parameter wie folgt ersetzt werden:

| Parameter                   | Wert                |
|-----------------------------|----------------------|
| **\<Ressourcengruppenname>**  | myResourceGroupNAT |
| **\<virtual-network-name>** | myVNetsource          |
| **\<region-name>**          | USA (Ost) 2      |
| **\<IPv4-Adressraum>**   | 192.168.0.0\16          |
| **\<subnet-name>**          | mySubnetsource        |
| **\<subnet-address-range>** | 192.168.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-source-virtual-machine"></a>Erstellen des virtuellen Quellcomputers

Sie erstellen nun einen virtuellen Computer zur Verwendung des NAT-Diensts. Dieser virtuelle Computer verfügt über eine öffentliche IP-Adresse, die als öffentliche IP auf Instanzebene verwendet wird, um den Zugriff auf den virtuellen Computer zu ermöglichen. Der NAT-Dienst erkennt die Flussrichtung und ersetzt in Ihrem Subnetz das Standardziel im Internet. Die öffentliche IP-Adresse des virtuellen Computers wird nicht für ausgehende Verbindungen verwendet.

Zum Testen des NAT-Gateways weisen wir eine Ressource für eine öffentliche IP-Adresse als öffentliche IP-Adresse auf Instanzebene zu, um den externen Zugriff auf diese VM zu ermöglichen. Diese Adresse wird nur für den Zugriff im Rahmen des Tests verwendet.  Wir veranschaulichen, wie der NAT-Dienst Vorrang vor anderen Ausgangsoptionen erhält.

Zu Übungszwecken können Sie diesen virtuellen Computer auch ohne öffentliche IP-Adresse erstellen und dann einen weiteren virtuellen Computer erstellen, der als Jumpbox ohne öffentliche IP-Adresse verwendet wird.

1. Wählen Sie links oben im Portal **Ressource erstellen** > **Compute** > **Ubuntu Server 18.04 LTS** aus, oder suchen Sie über die Marketplace-Suche nach **Ubuntu Server 18.04 LTS**.

2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupNAT** aus.
   - **Instanzendetails** > **Name des virtuellen Computers**: Geben Sie **myVMsource** ein.
   - **Instanzendetails** > **Region**: Wählen Sie **USA, Osten 2** aus.
   - **Administratorkonto** > **Eingabe für Authentifizierung**: Wählen Sie **Kennwort** aus.
   - **Administratorkonto**: Geben Sie Informationen für **Benutzername**, **Kennwort** und **Kennwort bestätigen** ein.
   - **Regeln für eingehende Ports** > **Öffentliche Eingangsports**: Wählen Sie **Ausgewählte Ports zulassen** aus.
   - **Regeln für eingehende Ports** > **Eingangsports auswählen**: Wählen Sie **SSH (22)** aus.
   - Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.

3. Stellen Sie auf der Registerkarte **Netzwerk** sicher, dass Folgendes ausgewählt ist:
   - **Virtuelles Netzwerk**: **myVnetsource**
   - **Subnetz**: **mySubnetsource**
   - **Öffentliche IP-Adresse**: Wählen Sie **Neu erstellen** aus.  Geben Sie im Fenster **Öffentliche IP-Adresse erstellen** im Feld **Name** den Wert **myPublicIPsourceVM** ein. Wählen Sie unter **SKU** die Option **Standard** aus. Übernehmen Sie die restlichen Standardeinstellungen, und klicken Sie auf **OK**.
   - **NIC-Netzwerksicherheitsgruppe:** Wählen Sie **Basic** aus.
   - **Öffentliche Eingangsports**: Wählen Sie **Ausgewählte Ports zulassen** aus.
   - **Eingangsports auswählen**: Vergewissern Sie sich, dass **SSH** ausgewählt ist.

4. Legen Sie auf der Registerkarte **Verwaltung** unter **Überwachung** die Option **Startdiagnose** auf **Aus** fest.

5. Klicken Sie auf **Überprüfen + erstellen**.

6. Überprüfen Sie die Einstellungen, und klicken Sie auf **Erstellen**.

## <a name="create-the-nat-gateway"></a>Erstellen des NAT-Gateways

Sie können für ein NAT-Gateway mehrere Ressourcen bzw. Präfixressourcen für öffentliche IP-Adressen verwenden. Sie fügen eine Ressource für eine öffentliche IP-Adresse, eine Präfixressource für eine öffentliche IP-Adresse und eine NAT-Gatewayressource hinzu.

In diesem Abschnitt erfahren Sie, wie Sie mit der NAT-Gatewayressource die folgenden Komponenten des NAT-Diensts erstellen und konfigurieren:
  - Öffentlicher IP-Adresspool und Präfix für öffentliche IP-Adressen für ausgehende Datenflüsse, die von der NAT-Gatewayressource übersetzt werden
  - Ändern des Leerlauftimeouts von vier Minuten (Standardwert) in zehn Minuten

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

1. Wählen Sie links oben im Portal **Ressource erstellen** > **Netzwerk** > **Öffentliche IP-Adresse** aus, oder suchen Sie über die Marketplace-Suche nach **Öffentliche IP-Adresse**. 

2. Geben Sie unter **Öffentliche IP-Adresse erstellen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | IP-Version | Wählen Sie **IPv4** aus.
    | SKU | Wählen Sie **Standard** aus.
    | Name | Geben Sie **myPublicIPsource** ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **myResourceGroupNAT** aus. |
    | Position | Wählen Sie **USA, Osten 2** aus.|

3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

### <a name="create-a-public-ip-prefix"></a>Erstellen des Präfixes für öffentliche IP-Adressen

1. Wählen Sie links oben im Portal **Ressource erstellen** > **Netzwerk** > **Präfix für öffentliche IP-Adresse** aus, oder suchen Sie über die Marketplace-Suche nach **Präfix für öffentliche IP-Adresse**.

2. Geben Sie auf der Registerkarte **Grundlagen** unter **Präfix für öffentliche IP-Adresse erstellen** die folgenden Werte ein, oder wählen Sie sie aus:
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupNAT**> aus.
   - **Instanzendetails** > **Name**: Geben Sie **myPublicIPprefixsource** ein.
   - **Instanzendetails** > **Region**: Wählen Sie **USA, Osten 2** aus.
   - **Instanzendetails** > **Präfixgröße**: Wählen Sie **/31 (2 addresses)** (/31 (2 Adressen)) aus.

3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Überprüfen + erstellen** aus.

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
   - **Öffentliche IP-Adressen**: Wählen Sie **myPublicIPsource** aus.
   - **Präfixe von öffentlichen IP-Adressen**: Wählen Sie **myPublicIPprefixsource** aus.
   - Wählen Sie die Registerkarte **Subnetz** oder **Weiter: Subnetz** aus.

4. Geben Sie auf der Registerkarte **Subnetz** die folgenden Werte ein, oder wählen Sie sie aus:
   - **Virtuelles Netzwerk**: Wählen Sie **myResourceGroupNAT** > **myVnetsource** aus.
   - **Subnetzname**: Aktivieren Sie das Kästchen neben **mySubnetsource**.

5. Klicken Sie auf **Überprüfen + erstellen**.

6. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

Der gesamte ausgehende Datenverkehr an Ziele im Internet verwendet jetzt den NAT-Dienst.  Eine UDR muss nicht konfiguriert werden.


## <a name="prepare-destination-for-outbound-traffic"></a>Vorbereiten des Ziels für ausgehenden Datenverkehr

Wir erstellen jetzt ein Ziel für den ausgehenden Datenverkehr, der vom NAT-Dienst übersetzt wird, damit Sie einen Test durchführen können.


## <a name="virtual-network-and-parameters-for-destination"></a>Virtuelles Netzwerk und Parameter für das Ziel

Bevor Sie einen virtuellen Computer für das Ziel bereitstellen, müssen Sie ein virtuelles Netzwerk erstellen, in dem der virtuelle Zielcomputer angeordnet werden kann. Unten sind die gleichen Schritte wie für den virtuellen Quellcomputer angegeben, aber es wurden einige kleinere Änderungen zum Verfügbarmachen des Zielendpunkts vorgenommen.

In diesem Abschnitt müssen Sie die folgenden Parameter in den Schritten unten wie folgt ersetzen:

| Parameter                   | Wert                |
|-----------------------------|----------------------|
| **\<Ressourcengruppenname>**  | myResourceGroupNAT |
| **\<virtual-network-name>** | myVNetdestination          |
| **\<region-name>**          | USA (Ost) 2      |
| **\<IPv4-Adressraum>**   | 192.168.0.0\16          |
| **\<subnet-name>**          | mySubnetdestination        |
| **\<subnet-address-range>** | 192.168.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-destination-virtual-machine"></a>Erstellen des virtuellen Zielcomputers

1. Wählen Sie links oben im Portal **Ressource erstellen** > **Compute** > **Ubuntu Server 18.04 LTS** aus, oder suchen Sie über die Marketplace-Suche nach **Ubuntu Server 18.04 LTS**.

2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupNAT** aus.
   - **Instanzendetails** > **Name des virtuellen Computers**: Geben Sie **myVMdestination** ein.
   - **Instanzendetails** > **Region**: Wählen Sie **USA, Osten 2** aus.
   - **Administratorkonto** > **Eingabe für Authentifizierung**: Wählen Sie **Kennwort** aus.
   - **Administratorkonto**: Geben Sie Informationen für **Benutzername**, **Kennwort** und **Kennwort bestätigen** ein.
   - **Regeln für eingehende Ports** > **Öffentliche Eingangsports**: Wählen Sie **Ausgewählte Ports zulassen** aus.
   - **Regeln für eingehende Ports** > **Eingangsports auswählen**: Wählen Sie **SSH (22)** und **HTTP (80)** aus.
   - Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.

3. Stellen Sie auf der Registerkarte **Netzwerk** sicher, dass Folgendes ausgewählt ist:
   - **Virtuelles Netzwerk**: **myVnetdestination**
   - **Subnetz**: **mySubnetdestination**
   - **Öffentliche IP-Adresse**: Wählen Sie **Neu erstellen** aus.  Geben Sie im Fenster **Öffentliche IP-Adresse erstellen** im Feld **Name** den Wert **myPublicIPdestinationVM** ein. Wählen Sie unter **SKU** die Option **Standard** aus. Übernehmen Sie die restlichen Standardeinstellungen, und klicken Sie auf **OK**.
   - **NIC-Netzwerksicherheitsgruppe:** Wählen Sie **Basic** aus.
   - **Öffentliche Eingangsports**: Wählen Sie **Ausgewählte Ports zulassen** aus.
   - **Eingangsports auswählen**: Vergewissern Sie sich, dass **SSH** und **HTTP** ausgewählt sind.

4. Legen Sie auf der Registerkarte **Verwaltung** unter **Überwachung** die Option **Startdiagnose** auf **Aus** fest.

5. Klicken Sie auf **Überprüfen + erstellen**.

6. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Vorbereiten eines Webservers und Testen der Nutzlast auf dem virtuellen Zielcomputer

Als Erstes müssen Sie die IP-Adresse des virtuellen Zielcomputers ermitteln. 

1. Wählen Sie im Portal auf der linken Seite **Ressourcengruppen** aus.
2. Wählen Sie **myResourceGroupNAT** aus.
3. Wählen Sie **myVMdestination** aus.
4. Kopieren Sie unter **Übersicht** den Wert unter **Öffentliche IP-Adresse**, und fügen Sie ihn in einen Editor ein, damit Sie ihn für den Zugriff auf den virtuellen Computer verwenden können.

>[!IMPORTANT]
>Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in einen Editor ein, damit Sie sie in den nachfolgenden Schritten verwenden können. Geben Sie an, dass es sich um den virtuellen Zielcomputer handelt.

### <a name="sign-in-to-destination-vm"></a>Anmelden beim virtuellen Zielcomputer

Öffnen Sie [Azure Cloud Shell](https://shell.azure.com) im Browser. Verwenden Sie die im vorherigen Schritt abgerufene IP-Adresse, um eine SSH-Verbindung mit dem virtuellen Computer herzustellen.

```azurecli-interactive
ssh <username>@<ip-address-destination>
```

Kopieren Sie die folgenden Befehle, und fügen Sie sie ein, nachdem Sie sich angemeldet haben.  

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get -y install nginx && \
sudo ln -sf /dev/null /var/log/nginx/access.log && \
sudo touch /var/www/html/index.html && \
sudo rm /var/www/html/index.nginx-debian.html && \
sudo dd if=/dev/zero of=/var/www/html/100k bs=1024 count=100
```

Mit diesen Befehlen wird der virtuelle Computer aktualisiert, nginx installiert und eine Datei mit 100 KB erstellt. Diese Datei wird mit dem NAT-Dienst vom virtuellen Quellcomputer abgerufen.

Schließen Sie die SSH-Sitzung mit dem virtuellen Zielcomputer.

## <a name="prepare-test-on-source-vm"></a>Vorbereiten des Tests auf dem virtuellen Quellcomputer

Als Erstes müssen Sie die IP-Adresse des virtuellen Quellcomputers ermitteln.

1. Wählen Sie im Portal auf der linken Seite **Ressourcengruppen** aus.
2. Wählen Sie **myResourceGroupNAT** aus.
3. Wählen Sie **myVMsource** aus.
4. Kopieren Sie unter **Übersicht** den Wert unter **Öffentliche IP-Adresse**, und fügen Sie ihn in einen Editor ein, damit Sie ihn für den Zugriff auf den virtuellen Computer verwenden können.

>[!IMPORTANT]
>Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in einen Editor ein, damit Sie sie in den nachfolgenden Schritten verwenden können. Geben Sie an, dass es sich um den virtuellen Quellcomputer handelt.

### <a name="log-into-source-vm"></a>Anmelden beim virtuellen Quellcomputer

Öffnen Sie in Ihrem Browser eine neue Registerkarte für [Azure Cloud Shell](https://shell.azure.com).  Verwenden Sie die im vorherigen Schritt abgerufene IP-Adresse, um eine SSH-Verbindung mit dem virtuellen Computer herzustellen. 

```azurecli-interactive
ssh <username>@<ip-address-source>
```

Kopieren Sie die folgenden Befehle, und fügen Sie sie ein, um den Test des NAT-Diensts vorzubereiten.

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get install -y nload golang && \
echo 'export GOPATH=${HOME}/go' >> .bashrc && \
echo 'export PATH=${PATH}:${GOPATH}/bin' >> .bashrc && \
. ~/.bashrc &&
go get -u github.com/rakyll/hey

```

Mit diesem Befehl wird Ihr virtueller Computer aktualisiert, go installiert, [hey](https://github.com/rakyll/hey) von GitHub installiert und Ihre Shellumgebung aktualisiert.

Sie können den NAT-Dienst nun testen.

## <a name="validate-nat-service"></a>Überprüfen des NAT-Diensts

Während der Anmeldung beim virtuellen Quellcomputer können Sie **cURL** und **hey** verwenden, um Anforderungen für die IP-Zieladresse zu generieren.

Verwenden Sie cURL, um die 100-KB-Datei abzurufen.  Ersetzen Sie **\<ip-address-destination>** im Beispiel unten durch die IP-Zieladresse, die Sie weiter oben kopiert haben.  Der Parameter **--output** gibt an, dass die abgerufene Datei verworfen wird.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

Sie können auch eine Reihe von Anforderungen generieren, indem Sie **hey** verwenden. Ersetzen Sie erneut **\<ip-address-destination>** durch die IP-Zieladresse, die Sie weiter oben kopiert haben.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Mit diesem Befehl werden 100 Anforderungen (jeweils zehn) mit einem Timeout von 30 Sekunden und ohne Wiederverwendung der TCP-Verbindung generiert.  Mit jeder Anforderung werden 100 KB abgerufen.  Am Ende der Ausführung meldet **hey** einige statistische Daten, um anzugeben, welches Ergebnis für den NAT-Dienst erzielt wurde.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, das NAT-Gateway und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie die Ressourcengruppe **myResourceGroupNAT** aus, die das NAT-Gateway enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein NAT-Gateway und einen virtuellen Quell- und Zielcomputer erstellt und dann das NAT-Gateway getestet.

Sehen Sie sich die Metriken in Azure Monitor an, um Informationen zum Betrieb Ihres NAT-Diensts anzuzeigen. Diagnostizieren Sie Probleme wie die Ressourcenüberlastung von verfügbaren SNAT-Ports.  Die Ressourcenüberlastung von SNAT-Ports kann durch das Hinzufügen zusätzlicher Ressourcen bzw. Präfixressourcen für öffentliche IP-Adressen leicht behoben werden.

- Informieren Sie sich über [Virtual Network NAT](./nat-overview.md).
- Informieren Sie sich über [NAT-Gatewayressourcen](./nat-gateway-resource.md).
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe der Azure CLI](./quickstart-create-nat-gateway-cli.md)
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe von Azure PowerShell](./quickstart-create-nat-gateway-powershell.md)
- Schnellstart zur Bereitstellung einer [NAT-Gatewayressource mithilfe des Azure-Portals](./quickstart-create-nat-gateway-portal.md)

> [!div class="nextstepaction"]


---
title: Konfigurieren eines Endpunkts für den internen Lastenausgleich
titleSuffix: Azure Application Gateway
description: In diesem Artikel erfahren Sie, wie Sie Application Gateway mit einer privaten Front-End-IP-Adresse konfigurieren.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: how-to
ms.date: 02/23/2021
ms.author: victorh
ms.openlocfilehash: 224cbe1e34e5915a7fa5fc1cf415c35f86c3abe4
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101711653"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>Konfigurieren einer Application Gateway-Instanz mit einem Endpunkt für den internen Lastenausgleich (Internal Load Balancer, ILB)

Ein Azure Application Gateway kann mit einer VIP mit Internetzugriff oder mit einem internen Endpunkt konfiguriert werden, der nicht über das Internet erreichbar ist. Ein interner Endpunkt verwendet eine private IP-Adresse für das Front-End, das auch als *ILB-Endpunkt (Internal Load Balancer, interner Lastenausgleich)* bezeichnet wird.

Das Konfigurieren des Gateways mit einer privaten Front-End-IP-Adresse ist für interne Branchenanwendungen nützlich, die nicht für das Internet verfügbar gemacht werden. Es ist auch in einer Anwendung mit mehreren Ebenen hilfreich für die Dienste und Ebenen, die sich innerhalb einer Sicherheitsgrenze befinden und nicht für das Internet verfügbar gemacht werden, die aber dennoch Folgendes erfordern:

- Roundrobin-Lastverteilung
- Sitzungsbindungen
- TLS-Abschluss (Transport Layer Security, zuvor SSL, Secure Sockets Layer)

Dieser Artikel führt Sie durch die Schritte zum Konfigurieren einer Application Gateway-Instanz mit einer privaten Front-End-IP-Adresse über das Azure-Portal.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter <https://portal.azure.com> beim Azure-Portal an.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Für die Kommunikation in Azure zwischen den von Ihnen erstellten Ressourcen ist ein virtuelles Netzwerk erforderlich. Erstellen Sie ein neues, oder verwenden Sie ein bereits vorhandenes virtuelles Netzwerk. 

In diesem Beispiel erstellen Sie ein neues virtuelles Netzwerk. Sie können ein virtuelles Netzwerk zum gleichen Zeitpunkt erstellen wie das Anwendungsgateway. Application Gateway-Instanzen werden in separaten Subnetzen erstellt. Dieses Beispiel umfasst zwei Subnetze: eins für das Anwendungsgateway und eins für die Back-End-Server.

1. Erweitern Sie das Portalmenü, und wählen Sie **Ressource erstellen** aus.
2. Klicken Sie auf **Netzwerk** und dann in der Liste der ausgewählten Elemente auf **Application Gateway**.
3. Geben Sie für das Anwendungsgateway den Namen *myAppGateway* und für die neue Ressourcengruppe den Namen *myResourceGroupAG* ein.
4. Wählen Sie als **Region** die Option **USA, Mitte** aus.
5. Wählen Sie als **Dienstebene** die Option **Standard** aus.
6. Wählen Sie unter **Virtuelles Netzwerk konfigurieren** die Option **Neu erstellen** aus, und geben Sie dann die folgenden Werte für das virtuelle Netzwerk ein:
   - *myVNet*: Name des virtuellen Netzwerks
   - *10.0.0.0/16*: Adressraum des virtuellen Netzwerks
   - *myAGSubnet*: Subnetzname
   - *10.0.0.0/24*: Adressraum des Subnetzes
   - *myBackendSubnet*: Back-End-Subnetzname.
   - *10.0.1.0/24*: Adressraum des Back-End-Subnetzes.

    ![Virtuelles Netzwerk erstellen](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)

6. Klicken Sie auf **OK**, um das virtuelle Netzwerk und die Subnetze zu erstellen.
7. Wählen Sie **Weiter: Front-Ends** aus.
8. Wählen Sie als **Front-End-IP-Adresstyp** die Option **Privat** aus.

   Standardmäßig handelt es sich hierbei um die Zuweisung dynamischer IP-Adressen. Die erste verfügbare Adresse des konfigurierten Subnetzes wird als Front-End-IP-Adresse zugewiesen.
   > [!NOTE]
   > Die Art der IP-Adresse (statisch oder dynamisch) kann nach der Zuordnung nicht mehr geändert werden.
9. Wählen Sie **Weiter: Back-Ends** aus.
10. Wählen Sie **Back-End-Pool hinzufügen** aus.
11. Geben Sie als **Name** n den Wert *appGatewayBackendPool* ein.
12. Wählen Sie für **Back-End-Pool ohne Ziele hinzufügen** den Wert **Ja** aus. Die Ziele fügen Sie später hinzu.
13. Wählen Sie **Hinzufügen**.
14. Wählen Sie **Weiter: Konfiguration** aus.
15. Wählen Sie unter **Routingregeln** den Eintrag **Routingregel hinzufügen** aus.
16. Geben Sie als **Regelname** n den Text *Rrule-01* ein.
17. Geben Sie als **Listenername** n den Text *Listener-01* ein.
18. Wählen Sie als **Front-End-IP** die Option **Privat** aus.
19. Übernehmen Sie für die restlichen Optionen die Standardwerte, und wählen Sie die Registerkarte **Back-End-Ziele** aus.
20. Wählen Sie für **Zieltyp** den Wert **Back-End-Pool** aus, und wählen Sie dann **appGatewayBackendPool** aus.
21. Wählen Sie als **HTTP-Einstellung** die Option **Neu hinzufügen** aus.
22. Geben Sie als **HTTP-Einstellungsname** n den Wert *http-setting-01* ein.
23. Wählen Sie für **Back-End-Protokoll** die Option **HTTP** aus.
24. Geben Sie für **Back-End-Port** den Wert *80* ein.
25. Übernehmen Sie die verbleibenden Standardwerte, und wählen Sie **Hinzufügen** aus.
26. Wählen Sie auf der Seite **Routingregel hinzufügen** die Option **Hinzufügen** aus.
27. Klicken Sie auf **Weiter: Tags**.
28. Klicken Sie auf **Weiter: Überprüfen + erstellen**.
29. Überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**, um die Netzwerkressourcen und das Anwendungsgateway zu erstellen. Es kann einige Minuten dauern, bis das Anwendungsgateway erstellt wird. Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="add-backend-pool"></a>Hinzufügen eines Back-End-Pools

Der Back-End-Pool wird zum Weiterleiten von Anforderungen an die Back-End-Server verwendet, die die Anforderung verarbeiten. Das Back-End kann Netzwerkadapter, VM-Skalierungsgruppen, öffentliche IP-Adressen, interne IP-Adressen, vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDNs) und Back-Ends mit mehreren Mandanten (beispielsweise Azure App Service) umfassen. In diesem Beispiel verwenden Sie virtuelle Computer als Ziel-Back-End. Sie können entweder vorhandene virtuelle Computer verwenden oder neue erstellen. In diesem Beispiel erstellen Sie zwei VMs, die von Azure als Back-End-Server für das Anwendungsgateway verwendet werden.

Führen Sie dazu die folgenden Schritte aus:

1. Erstellen Sie zwei neue virtuelle Computer (*myVM* und *myVM2*), die als Back-End-Server verwendet werden.
2. Installieren von IIS auf den virtuellen Computern, um zu überprüfen, ob die Application Gateway-Instanz erfolgreich erstellt wurde
3. Hinzufügen der Back-End-Server zum Back-End-Pool

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers


1. Wählen Sie **Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Virtueller Computer** aus.
4. Geben Sie die folgenden Werte für den virtuellen Computer ein:
   - Wählen Sie Ihr Abonnement aus.
   - Wählen Sie unter **Ressourcengruppe** den Wert *myResourceGroupAG* aus.
   - Geben Sie unter **VM-Name** *myVM* ein.
   - Wählen Sie als **Image** den Wert **Windows Server 2019 Datacenter** aus.
   - Geben Sie einen gültigen **Benutzernamen** ein.
   - Geben Sie ein gültiges **Kennwort** ein.
1. Übernehmen Sie die verbleibenden Standardwerte, und wählen Sie **Weiter: Datenträger** aus.
1. Übernehmen Sie die Standardeinstellungen, und wählen Sie **Weiter: Netzwerk** aus.
1. Stellen Sie sicher, dass als virtuelles Netzwerk **myVNet** und als Subnetz **myBackendSubnet** ausgewählt ist.
1. Übernehmen Sie die verbleibenden Standardwerte, und wählen Sie **Weiter: Verwaltung** aus.
1. Wählen Sie **Deaktivieren** aus, um die Startdiagnose zu deaktivieren.
1. Klicken Sie auf **Überprüfen + erstellen**.
1. Überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und wählen Sie dann **Erstellen** aus. Das Erstellen des virtuellen Computers kann ein paar Minuten dauern. Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.

### <a name="install-iis"></a>Installieren von IIS

1. Öffnen Sie die Cloud Shell, und vergewissern Sie sich, dass **PowerShell** festgelegt ist.
    ![Ein Screenshot, der ein geöffnetes Azure Cloud Shell-Konsolenfenster zeigt, das PowerShell verwendet.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-3.png)
2. Führen Sie den folgenden Befehl aus, um IIS auf dem virtuellen Computer zu installieren:

   ```azurepowershell
   Set-AzVMExtension `
        -ResourceGroupName myResourceGroupAG `
        -ExtensionName IIS `
        -VMName myVM `
        -Publisher Microsoft.Compute `
        -ExtensionType CustomScriptExtension `
        -TypeHandlerVersion 1.4 `
        -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
         -Location CentralUS 

   ```

3. Erstellen Sie einen zweiten virtuellen Computer, und installieren Sie IIS mithilfe der soeben ausgeführten Schritte. Verwenden Sie „VM2“ als VM-Name und für `VMName` in `Set-AzVMExtension`.

### <a name="add-backend-servers-to-backend-pool"></a>Hinzufügen von Back-End-Servern zu Back-End-Pools

1. Wählen Sie **Alle Ressourcen** und dann **myAppGateway** aus.
2. Wählen Sie **Back-End-Pools** und dann **appGatewayBackendPool** aus.
3. Wählen Sie unter **Zieltyp** den Wert **Virtueller Computer** aus, und wählen Sie unter **Ziel** die myVM zugeordnete vNIC aus.
4. Wiederholen Sie den Vorgang, um MyVM2 hinzuzufügen.
   ![Bereich „Back-End-Pool bearbeiten“ mit hervorgehobenen Zieltypen und Zielen.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
5. Wählen Sie **Speichern** aus.

## <a name="create-a-client-virtual-machine"></a>Erstellen einer Client-VM

Die Client-VM wird zum Herstellen einer Verbindung mit dem Back-End-Pool des Anwendungsgateways verwendet.

- Erstellen Sie eine dritte VM anhand der vorherigen Schritte. Verwenden Sie „myVM3“ als VM-Name.

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

1. Wählen Sie auf der Seite „myAppGateway“ die Option **Front-End-IP-Konfigurationen** aus, um die private IP-Adresse des Front-Ends zu notieren.
    ![Bereich „Front-End-IP-Konfigurationen“ mit hervorgehobenem Typ „Privat“.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. Kopieren Sie die private IP-Adresse, und fügen Sie sie auf „myVM3“ in die Adressleiste des Browsers ein, um auf den Back-End-Pool des Anwendungsgateways zuzugreifen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Überwachen der Integrität Ihres Back-End-Pools finden Sie unter [Back-End-Integrität und Diagnoseprotokolle für Application Gateway](application-gateway-diagnostics.md).

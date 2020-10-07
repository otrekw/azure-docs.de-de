---
title: Verwenden von Azure Application Gateway zum Schützen Ihrer Web-Apps in Azure VMware Solution
description: Konfigurieren Sie Azure Application Gateway, um Ihre in Azure VMware Solution ausgeführten Web-Apps sicher verfügbar zu machen.
ms.topic: how-to
ms.date: 07/31/2020
ms.openlocfilehash: ad7cd36d77da41d75ea9dcc18a51d0ffc5540d2a
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91579206"
---
# <a name="use-azure-application-gateway-to-protect-your-web-apps-on-azure-vmware-solution"></a>Verwenden von Azure Application Gateway zum Schützen Ihrer Web-Apps in Azure VMware Solution

[Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) ist ein Lastenausgleich für Webdatenverkehr auf Schicht 7, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können. Er bietet viele Funktionen: Cookiebasierte Sitzungsaffinität, URL-basiertes Routing und Web Application Firewall (WAF) sind nur einige davon. (Eine umfassende Liste der Features finden Sie unter [Azure Application Gateway-Funktionen](../application-gateway/features.md).) Es gibt zwei Versionen im Angebot: v1 und v2. Beide wurden mit Web-Apps getestet, die in Azure VMware Solution ausgeführt werden.

In diesem Artikel wird ein gängiges Szenario erläutert, in dem Application Gateway vor einer Webserverfarm mit einer Reihe von Konfigurationen und Empfehlungen zum Schützen einer unter Azure VMware Solution ausgeführten Web-App verwendet wird. 

## <a name="topology"></a>Topologie
Wie in der folgenden Abbildung dargestellt, kann Application Gateway verwendet werden, um virtuelle Azure-IaaS-Computer, Azure-VM-Skalierungsgruppen oder lokale Server zu schützen. Virtuelle Azure VMware Solution-Computer werden von Application Gateway wie lokale Server behandelt.

![Application Gateway schützt Azure VMware Solution-VMs.](media/protect-azure-vmware-solution-with-application-gateway/app-gateway-protects.png)

> [!IMPORTANT]
> Azure Application Gateway ist derzeit die einzige unterstützte Methode, um Web-Apps verfügbar zu machen, die auf virtuellen Azure VMware Solution-Computern ausgeführt werden.

Das folgende Diagramm zeigt das Testszenario zum Überprüfen von Application Gateway mit Azure VMware Solution-Webanwendungen.

![Application Gateway-Integration mit Azure VMware Solution zum Ausführen von Web-Apps.](media/protect-azure-vmware-solution-with-application-gateway/app-gateway-avs-scenario.png)

Die Application Gateway-Instanz wird auf dem Hub in einem dedizierten Subnetz bereitgestellt. Sie weist eine öffentliche Azure-IP-Adresse auf. Es wird empfohlen, den DDoS-Standardschutz für das virtuelle Netzwerk zu aktivieren. Der Webserver wird in einer privaten Azure VMware Solution-Cloud hinter NSX T0- und T1-Routern gehostet. Azure VMware Solution aktiviert die Kommunikation mit dem Hub und den lokalen Systemen mit [ExpressRoute Global Reach](../expressroute/expressroute-global-reach.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement.
- Eine bereitgestellte und ausgeführte private Azure VMware Solution-Cloud.

## <a name="deployment-and-configuration"></a>Bereitstellung und Konfiguration

1. Suchen Sie im Azure-Portal nach **Application Gateway**, und wählen Sie **Anwendungsgateway erstellen** aus.

2. Geben Sie die grundlegenden Details wie in der folgenden Abbildung an, und wählen Sie dann **Weiter: Front-Ends >** aus. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/create-app-gateway.png" alt-text="Erstellung eines Anwendungsgateways":::

3. Wählen Sie den IP-Adresstyp für das Front-End aus. Wählen Sie für den Typ „Öffentlich“ eine vorhandene öffentliche IP-Adresse aus, oder erstellen Sie eine neue. Wählen Sie **Weiter: Back-Ends >** aus.

    > [!NOTE]
    > Für private Front-Ends werden nur die SKUs „Standard“ und „Web Application Firewall (WAF)“ unterstützt.

4. Fügen Sie als Nächstes einen Back-End-Pool hinzu. Damit wird eine Gruppe von Instanzen beschrieben, die Teil der Anwendung oder des Diensts sind (in diesem Fall virtuelle Computer, die in der Azure VMware Solution-Infrastruktur ausgeführt werden). Geben Sie die Details der in der privaten Azure VMware Solution-Cloud ausgeführten Webserver an, und wählen Sie **Hinzufügen** aus. Wählen Sie dann **Weiter: Konfiguration >** aus.

1. Wählen Sie auf der Registerkarte **Konfiguration** die Option **Routingregel hinzufügen** aus.

6. Geben Sie auf der Registerkarte **Listener** die Details für den Listener an. Wenn HTTPS ausgewählt wurde, muss ein Zertifikat aus einer PFX-Datei oder ein vorhandenes Zertifikat von Azure Key Vault bereitgestellt werden. 

7. Wählen Sie die Registerkarte **Back-End-Ziele** und dann das zuvor erstellte Back-End aus. Wählen Sie für das Feld **HTTP-Einstellungen** die Option **Neu hinzufügen** aus.

8. Konfigurieren Sie die Parameter für die HTTP-Einstellungen. Wählen Sie **Hinzufügen**.

9. Wenn Sie mehrere pfadbasierte Regeln konfigurieren möchten, wählen Sie **Fügen Sie mehrere Ziele zum Erstellen einer pfadbasierten Regel hinzu** aus. 

10. Fügen Sie eine pfadbasierte Regel hinzu, und wählen Sie **Hinzufügen** aus. Wiederholen Sie diese Schritte zum Hinzufügen weiterer pfadbasierter Regeln. 

11. Wenn Sie mit dem Hinzufügen von pfadbasierten Regeln fertig sind, wählen Sie erneut **Hinzufügen** und dann **Weiter: Tags >** aus. 

12. Fügen Sie alle gewünschten Tags hinzu. Wählen Sie **Weiter: Überprüfen + erstellen >** aus.

13. Auf der Application Gateway-Instanz wird eine Überprüfung ausgeführt. Wenn sie erfolgreich ist, wählen Sie **Erstellen** aus, um die Bereitstellung abzuschließen.

## <a name="configuration-examples"></a>Konfigurationsbeispiele

In diesem Abschnitt erfahren Sie, wie Sie Application Gateway mit virtuellen Azure VMware Solution-Computern als Back-End-Pools für die folgenden Anwendungsfälle konfigurieren: 

- [Hosten mehrerer Websites](#hosting-multiple-sites)
- [Routing per URL](#routing-by-url)

### <a name="hosting-multiple-sites"></a>Hosten mehrerer Websites

Sie können mit dem Azure-Portal ein Hosting mehrerer Websites konfigurieren, wenn Sie ein Anwendungsgateway erstellen. In diesem Tutorial definieren Sie Back-End-Adresspools mithilfe von virtuellen Computern, die in einer privaten Azure VMware Solution-Cloud auf einem vorhandenen Anwendungsgateway ausgeführt werden. Das Anwendungsgateway ist Teil eines virtuellen Hubnetzwerks, wie unter [Integrieren von Azure VMware Solution in eine Hub-and-Spoke-Architektur](concepts-hub-and-spoke.md) beschrieben. In diesem Tutorial wird vorausgesetzt, dass Sie mehrere Domänen besitzen, und als Beispiele werden www.contoso.com und www.fabrikam.com verwendet.

1. Erstellen Sie die virtuellen Computer. Erstellen Sie in der privaten Azure VMware Solution-Cloud zwei Pools mit virtuellen Computern. Einer steht für Contoso und der zweite für Fabrikam. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs.png" alt-text="Erstellung eines Anwendungsgateways":::

    Zur Veranschaulichung verwenden wir in diesem Tutorial Windows Server 2016 mit installierter IIS-Rolle (Internetinformationsdienste). Nachdem die virtuellen Computer installiert wurden, führen Sie die folgenden PowerShell-Befehle aus, um IIS auf den einzelnen VMs zu konfigurieren. 

    ```powershell
    Install-WindowsFeature -Name Web-Server
    Add-Content -Path C:\inetpub\wwwroot\Default.htm -Value $($env:computername)
    ```

2. Fügen Sie die Back-End-Pools hinzu. Wählen Sie in einer vorhandenen Application Gateway-Instanz im Menü links **Back-End-Pools** aus, wählen Sie **Hinzufügen** aus, und geben Sie die Details der neuen Pools ein. Wählen Sie im rechten Bereich **Hinzufügen** aus.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-02.png" alt-text="Erstellung eines Anwendungsgateways" lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-02.png":::

3. Erstellen Sie im Abschnitt **Listener** einen neuen Listener für jede Website. Geben Sie die Details für jeden Listener ein, und wählen Sie **Hinzufügen** aus.

4. Wählen Sie im linken Navigationsbereich **HTTP-Einstellungen** aus, und wählen Sie im linken Bereich **Hinzufügen** aus. Geben Sie die Details ein, um eine neue HTTP-Einstellung zu erstellen, und wählen Sie **Speichern** aus.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-03.png" alt-text="Erstellung eines Anwendungsgateways" lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-03.png":::

5. Erstellen Sie die Regeln im linken Menü im Abschnitt **Regeln**. Ordnen Sie jede Regel dem entsprechenden Listener zu. Wählen Sie **Hinzufügen**.

6. Konfigurieren Sie den entsprechenden Back-End-Pool und die HTTP-Einstellungen. Wählen Sie **Hinzufügen**.

7. Testen Sie die Verbindung. Öffnen Sie Ihren bevorzugten Browser, und browsen Sie zu den verschiedenen Websites, die in Ihrer Azure VMware Solution-Umgebung gehostet werden, z. B. http://www.fabrikam.com.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-avs-07.png" alt-text="Erstellung eines Anwendungsgateways":::

### <a name="routing-by-url"></a>Routing per URL

Sie können mit Azure Application Gateway Routingregeln auf Basis von URL-Pfaden konfigurieren. In diesem Tutorial definieren Sie Back-End-Adresspools mithilfe von virtuellen Computern, die in einer privaten Azure VMware Solution-Cloud auf einem vorhandenen Anwendungsgateway ausgeführt werden. Das Anwendungsgateway ist Teil eines virtuellen Hubnetzwerks, wie in der [Dokumentation zur nativen Azure VMware Solution-Integration](concepts-hub-and-spoke.md) beschrieben. Anschließend erstellen Sie Routingregeln, die sicherstellen, dass Webdatenverkehr an die richtigen Server in den Pools gesendet wird.

1. Erstellen Sie die virtuellen Computer. Erstellen Sie in der privaten Azure VMware Solution-Cloud einen Pool mit virtuellen Computern, die die Webfarm darstellen. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs.png" alt-text="Erstellung eines Anwendungsgateways"
    Add-Content -Path C:\inetpub\wwwroot\video\test.htm -Value $($env:computername)
    ```

2. Fügen Sie die Back-End-Pools hinzu. Sie müssen drei neue Back-End-Pools in einer vorhandenen Application Gateway-Instanz hinzufügen. Klicken Sie im Menü auf der linken Seite auf **Back-End-Pools**. Wählen Sie **Hinzufügen** aus, und geben Sie die Details des ersten Pools (**contoso-web**) ein. Fügen Sie eine VM als Ziel hinzu. Wählen Sie **Hinzufügen**. Wiederholen Sie diesen Vorgang für **contoso-images** und **contoso-video**, wobei Sie jeweils eine andere VM als Ziel hinzufügen. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-02.png" alt-text="Erstellung eines Anwendungsgateways" lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-02.png":::

3. Erstellen Sie im Abschnitt **Listener** einen neuen Listener vom Typ Basic an Port 8080.

4. Wählen Sie im linken Navigationsbereich **HTTP-Einstellungen** aus, und wählen Sie im linken Bereich **Hinzufügen** aus. Geben Sie die Details ein, um eine neue HTTP-Einstellung zu erstellen, und wählen Sie **Speichern** aus.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-04.png" alt-text="Erstellung eines Anwendungsgateways":::

5. Erstellen Sie die Regeln im linken Menü im Abschnitt **Regeln**. Ordnen Sie jede Regel dem zuvor erstellten Listener zu. Konfigurieren Sie dann den Haupt-Back-End-Pool und die HTTP-Einstellungen. Wählen Sie **Hinzufügen**.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-07.png" alt-text="Erstellung eines Anwendungsgateways":::

6. Testen Sie die Konfiguration. Greifen Sie im Azure-Portal auf das Anwendungsgateway zu, und kopieren Sie die öffentliche IP-Adresse im Abschnitt **Übersicht**. Öffnen Sie dann ein neues Browserfenster, und geben Sie die URL `http://<app-gw-ip-address>:8080` ein. 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-08.png" alt-text="Erstellung eines Anwendungsgateways":::

    Ändern Sie die URL in `http://<app-gw-ip-address>:8080/images/test.htm`.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-09.png" alt-text="Erstellung eines Anwendungsgateways":::

    Ändern Sie die URL erneut in `http://<app-gw-ip-address>:8080/video/test.htm`.

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-avs-10.png" alt-text="Erstellung eines Anwendungsgateways":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Konfigurationsbeispiele finden Sie in der [Dokumentation zu Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/).

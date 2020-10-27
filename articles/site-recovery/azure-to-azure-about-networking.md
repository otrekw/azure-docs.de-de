---
title: Informationen zu Netzwerken für die Notfallwiederherstellung für virtuelle Azure-Computer mit Azure Site Recovery
description: Bietet eine Übersicht über die Netzwerkkonzepte für die Replikation von Azure-VMs mithilfe von Azure Site Recovery.
services: site-recovery
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 3/13/2020
ms.author: harshacs
ms.openlocfilehash: 1189324cf0bb2731a100032058c7ba9ae4add758
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92332041"
---
# <a name="about-networking-in-azure-vm-disaster-recovery"></a>Informationen zu Netzwerken für die Notfallwiederherstellung für virtuelle Azure-Computer



Dieser Artikel bietet einen Netzwerkleitfaden für die Replikation und Wiederherstellung von Azure-VMs zwischen verschiedenen Regionen mithilfe von [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Vorbereitung

Erfahren Sie, wie Site Recovery die Notfallwiederherstellung für [dieses Szenario](azure-to-azure-architecture.md) bereitstellt.

## <a name="typical-network-infrastructure"></a>Typische Netzwerkinfrastruktur

Das folgende Diagramm zeigt eine typische Azure-Umgebung für Anwendungen, die auf Azure-VMs ausgeführt werden:

![Diagramm einer typischen Azure-Umgebung für Anwendungen, die auf Azure-VMs ausgeführt werden.](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Wenn Ihr lokales Netzwerk über Azure ExpressRoute oder VPN mit Azure verbunden ist, sieht die Umgebung wie folgt aus:

![Kundenumgebung](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Normalerweise werden Netzwerke durch Firewalls und Netzwerksicherheitsgruppen (NSGs) geschützt. Diensttags sollten zur Steuerung der Netzwerkkonnektivität verwendet werden. NSGs sollten mehrere Diensttags zulassen, um die ausgehenden Konnektivität zu steuern.

>[!IMPORTANT]
> Die Verwendung eines authentifizierten Proxys zum Steuern der Netzwerkkonnektivität wird von Site Recovery nicht unterstützt. In diesem Fall kann die Replikation nicht aktiviert werden.

>[!NOTE]
>- Zum Steuern der ausgehenden Konnektivität sollte keine IP-Adressen basierende Filterung durchgeführt werden.
>- IP-Adressen von Azure Site Recovery sollten nicht zur Azure-Routingtabelle hinzugefügt werden, um die ausgehende Konnektivität zu kontrollieren.

## <a name="outbound-connectivity-for-urls"></a>Ausgehende Konnektivität für URLs

Lassen Sie die folgenden Site Recovery-URLs zu, wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Konnektivität verwenden:

**URL** | **Details**
--- | ---
*.blob.core.windows.net | Erforderlich, damit Daten in das Cachespeicherkonto in der Quellregion über die VM geschrieben werden können. Wenn Sie alle Cachespeicherkonten für Ihre VMs kennen, können Sie den Zugriff auf die spezifischen Speicherkonten-URLs (z. B. cache1.blob.core.windows.net and cache2.blob.core.windows.net) anstelle von „*.blob.core.windows.net“ zulassen.
login.microsoftonline.com | Erforderlich für die Autorisierung und Authentifizierung bei den Site Recovery-Dienst-URLs.
*.hypervrecoverymanager.windowsazure.com | Erforderlich, um die Kommunikation mit dem Site Recovery-Dienst über die VM zu ermöglichen.
*.servicebus.windows.net | Erforderlich, damit die Site Recovery-Überwachungs- und -Diagnosedaten über die VM geschrieben werden können.
*.vault.azure.net | Ermöglicht über das Portal Zugriff zum Aktivieren der Replikation für VMs, für die ADE aktiviert ist
*.automation.ext.azure.com | Ermöglicht das Aktivieren automatischer Upgrades für den Mobilitäts-Agent für ein repliziertes Element über das Portal

## <a name="outbound-connectivity-using-service-tags"></a>Ausgehende Konnektivität mithilfe von Diensttags

Wenn Sie NSG zum Steuern der ausgehenden Konnektivität verwenden, müssen diese Diensttags zugelassen werden.

- Für die Speicherkonten in der Quellregion:
    - Erstellen Sie ein [Speicherdiensttag](../virtual-network/security-overview.md#service-tags) basierend auf der NSG-Regel für die Quellregion.
    - Lassen Sie diese Adressen zu, damit Daten von der VM in das Cachespeicherkonto geschrieben werden können.
- Erstellen Sie basierend auf der NSG-Regel ein [Azure Active Directory-Diensttag (AAD)](../virtual-network/security-overview.md#service-tags) für den Zugriff auf alle IP-Adressen für AAD.
- Erstellen Sie eine auf dem EventsHub-Diensttag basierende NSG-Regel für die Zielregion, die den Zugriff auf Site Recovery-Überwachung ermöglicht.
- Erstellen Sie eine auf dem Azure Site Recovery-Diensttag basierende NSG-Regel, um den Zugriff auf den Site Recovery-Dienst in einer beliebigen Region zuzulassen.
- Erstellen Sie eine NSG-Regel, die auf einem AzureKeyVault-Diensttag basiert. Dies ist nur erforderlich, um die Replikation von VMs, für die ADE aktiviert ist, über das Portal zu aktivieren.
- Erstellen Sie eine NSG-Regel, die auf einem GuestAndHybridManagement-Diensttag basiert. Dies ist nur erforderlich, um automatische Upgrades für den Mobilitäts-Agent für ein repliziertes Element über das Portal zu aktivieren.
- Wir empfehlen, die erforderlichen NSG-Regeln in einer Test-Netzwerksicherheitsgruppe zu erstellen und sicherzustellen, dass keine Probleme vorliegen, bevor Sie die Regeln in einer Netzwerksicherheitsgruppe in der Produktionsumgebung erstellen.

## <a name="example-nsg-configuration"></a>Beispielkonfiguration für eine Netzwerksicherheitsgruppe

Dieses Beispiel zeigt, wie NSG-Regeln für eine zu replizierende VM konfiguriert werden.

- Wenn Sie die ausgehende Konnektivität mit NSG-Regeln steuern, verwenden Sie für alle erforderlichen IP-Adressbereiche Regeln zum Zulassen ausgehender HTTPS-Verbindungen für „port:443“.
- Im Beispiel wird davon ausgegangen, dass der VM-Quellstandort „USA, Osten“ ist und der Zielstandort „USA, Mitte“.

### <a name="nsg-rules---east-us"></a>NSG-Regeln – USA, Osten

1. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „Storage.EastUS“, wie im Screenshot unten gezeigt.

      ![Screenshot: Hinzufügen einer ausgehenden Sicherheitsregel für eine Netzwerksicherheitsgruppe für „Storage Punkt East U S“.](./media/azure-to-azure-about-networking/storage-tag.png)

2. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „AzureActiveDirectory“, wie im Screenshot unten gezeigt.

      ![Screenshot: Hinzufügen einer ausgehenden Sicherheitsregel für eine Netzwerksicherheitsgruppe für „Azure AD“.](./media/azure-to-azure-about-networking/aad-tag.png)

3. Erstellen Sie (ähnlich zu den oben erstellten Sicherheitsregeln) eine Sicherheitsregel für ausgehenden HTTPS-Datenverkehr (443) für „EventHub.CentralUS“ für die NSG, die dem Zielstandort entspricht. Dies ermöglicht den Zugriff auf Site Recovery-Überwachung.

4. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „AzureSiteRecovery“. Dies ermöglicht den Zugriff auf den Site Recovery-Dienst in jeder beliebigen Region.

### <a name="nsg-rules---central-us"></a>NSG-Regeln – USA, Mitte

Diese Regeln sind erforderlich, damit nach dem Failover die Replikation von der Zielregion zur Quellregion aktiviert werden kann:

1. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „Storage.CentralUS“.

2. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „AzureActiveDirectory“.

3. Erstellen Sie (ähnlich zu den oben erstellten Sicherheitsregeln) eine Sicherheitsregel für ausgehenden HTTPS-Datenverkehr (443) für „EventHub.EastUS“ für die NSG, die dem Quellstandort entspricht. Dies ermöglicht den Zugriff auf Site Recovery-Überwachung.

4. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „AzureSiteRecovery“. Dies ermöglicht den Zugriff auf den Site Recovery-Dienst in jeder beliebigen Region.

## <a name="network-virtual-appliance-configuration"></a>Konfiguration der virtuellen Netzwerkappliance

Wenn Sie virtuelle Netzwerkappliances zur Steuerung des ausgehenden Netzwerkverkehrs von VMs verwenden, wird die Appliance möglicherweise gedrosselt, wenn der gesamte Replikationsdatenverkehr über die virtuelle Netzwerkappliance läuft. Es wird empfohlen, einen Netzwerk-Dienstendpunkt in Ihrem virtuellen Netzwerk für „Storage“ zu erstellen, damit der Replikationsdatenverkehr nicht an die virtuelle Netzwerkappliance geleitet wird.

### <a name="create-network-service-endpoint-for-storage"></a>Erstellen eines Netzwerk-Dienstendpunkts für Storage
Sie können einen Netzwerk-Dienstendpunkt in Ihrem virtuellen Netzwerk für „Storage“ erstellen, damit der Replikationsdatenverkehr die Azure-Grenze nicht überschreitet.

- Wählen Sie Ihre virtuelles Azure-Netzwerk aus, und klicken Sie auf „Dienstendpunkte“.

    ![storage-endpoint](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- Klicken Sie auf „Hinzufügen“, und die Registerkarte „Dienstendpunkte hinzufügen“ wird geöffnet.
- Wählen Sie unter „Dienst“ die Option „Microsoft.Storage“ und unter dem Feld „Subnetze“ die erforderlichen Subnetze, und klicken Sie auf „Hinzufügen“.

>[!NOTE]
>Beschränken Sie nicht den Zugriff des virtuellen Netzwerks auf Ihre für ASR verwendeten Speicherkonten. Sie sollten den Zugriff für „Alle Netzwerke“ zulassen.

### <a name="forced-tunneling"></a>Tunnelerzwingung

Sie können die Standardsystemroute von Azure für das Adresspräfix 0.0.0.0/0 mit einer [benutzerdefinierten Route](../virtual-network/virtual-networks-udr-overview.md#custom-routes) überschreiben und VM-Datenverkehr auf ein lokales virtuelles Netzwerkgerät umleiten, aber diese Konfiguration wird für die Site Recovery-Replikation nicht empfohlen. Wenn Sie benutzerdefinierte Routen verwenden, sollten Sie [einen VNET-Dienstendpunkt](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in Ihrem virtuellen Netzwerk für „Storage“ erstellen, damit der Replikationsdatenverkehr innerhalb der Azure-Begrenzung bleibt.

## <a name="next-steps"></a>Nächste Schritte
- Schützen Sie Ihre Workloads durch die [Replikation virtueller Azure-Computer](./azure-to-azure-quickstart.md).
- Weitere Informationen zur [Beibehaltung von IP-Adressen](site-recovery-retain-ip-azure-vm-failover.md) für das Failover von virtuellen Azure-Computern.
- Weitere Informationen zur Notfallwiederherstellung von [virtuellen Azure-Computern mit ExpressRoute ](azure-vm-disaster-recovery-with-expressroute.md).

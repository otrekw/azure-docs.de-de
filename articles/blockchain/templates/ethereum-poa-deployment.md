---
title: Bereitstellen der Lösungsvorlage „Ethereum-Proof-of-Authority-Konsortium“ auf Azure
description: Verwenden der Lösung „Ethereum-Proof-of-Authority-Konsortium“, um ein Netzwerk mit vielen Elementen für das Ethereum-Konsortium auf Azure bereitzustellen und zu konfigurieren
ms.date: 03/01/2021
ms.topic: how-to
ms.reviewer: ravastra
ms.custom: contperf-fy21q3
ms.openlocfilehash: 70c9498bae9117585963e111bea4f1e127cab232
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097940"
---
# <a name="deploy-ethereum-proof-of-authority-consortium-solution-template-on-azure"></a>Bereitstellen der Lösungsvorlage „Ethereum-Proof-of-Authority-Konsortium“ auf Azure

Sie können die Azure-Lösungsvorlage [Ethereum-Proof-of-Authority-Konsortium](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-blockchain.azure-blockchain-ethereum) verwenden, um das Bereitstellen, Konfigurieren und Steuern eines Multi-Member-Netzwerks für das Ethereum-Proof-of-Authority-Konsortium zu erleichtern, ohne dass ausführliche Azure- und Ethereum-Kenntnisse vorhanden sein müssen.

Die Lösungsvorlage kann von jedem Konsortiummitglied verwendet werden, um mithilfe Microsoft Azure-Dienste für Compute-, Netzwerk- und Speicherfunktionen einen Blockchainnetzwerkspeicherbedarf anzugeben. Der Netzwerkfußabdruck jedes Konsortiummitglieds umfasst einige Validierungsknoten mit Lastenausgleich, über die eine Anwendung oder ein Benutzer interagieren kann, um Ethereum-Transaktionen zu übermitteln.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="choose-an-azure-blockchain-solution"></a>Auswählen einer Azure Blockchain-Lösung

Bevor Sie sich für die Lösungsvorlage „Ethereum-Proof-of-Authority-Konsortium“ entscheiden, sollten Sie Ihr Szenario mit den gängigen Anwendungsfällen der verfügbaren Azure Blockchain-Optionen vergleichen.

> [!IMPORTANT]
> Erwägen Sie die Verwendung des [Azure Blockchain Service](../service/overview.md) anstelle der Lösungsvorlage für Ethereum unter Azure. Azure Blockchain Service ist ein unterstützter verwalteter Azure-Dienst. Parity Ethereum ist zu einer von der Community gesteuerten Entwicklung und Wartung übergegangen. Weitere Informationen finden Sie unter [Wechsel von Parity Ethereum zu OpenEthereum DAO](https://www.parity.io/parity-ethereum-openethereum-dao/).

Option | Dienstmodell | Gängiger Anwendungsfall
-------|---------------|-----------------
Lösungsvorlagen | IaaS | Lösungsvorlagen sind Azure Resource Manager-Vorlagen, die Sie verwenden können, um eine vollständig konfigurierte Blockchainnetzwerktopologie bereitzustellen. Die Vorlagen stellen Microsoft Azure-Dienste für Compute-, Netzwerk- und Speicherfunktionen für einen bestimmten Blockchainnetzwerktyp bereit und konfigurieren diese. Lösungsvorlagen werden ohne Vereinbarung zum Servicelevel bereitgestellt. Nutzen Sie die [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-blockchain-workbench.html) zur Unterstützung.
[Azure Blockchain Service](../service/overview.md) | PaaS | Mit der Vorschauversion von Azure Blockchain Service wird die Einrichtung, Verwaltung und Governance von Konsortiumblockchainnetzwerken vereinfacht. Setzen Sie Azure Blockchain Service für Lösungen ein, bei denen PaaS, Konsortiumverwaltung oder Datenschutz für Vertrags- und Transaktionsaktivitäten erforderlich ist.
[Azure Blockchain Workbench](../workbench/overview.md) | IaaS und PaaS | Azure Blockchain Workbench (Vorschauversion) ist eine Sammlung mit Azure-Diensten und -Funktionen zum Erstellen und Bereitstellen von Blockchain-Anwendungen, mit denen Geschäftsprozesse und Daten mit anderen Organisationen gemeinsam genutzt werden können. Verwenden Sie Azure Blockchain Workbench, um einen Prototyp für eine Blockchainlösung oder einen Proof of Concept für eine Blockchainanwendung zu erstellen. Azure Blockchain Workbench wird ohne Vereinbarung zum Servicelevel bereitgestellt. Nutzen Sie die [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-blockchain-workbench.html) zur Unterstützung.

## <a name="solution-architecture"></a>Lösungsarchitektur

Mit der Ethereum-Lösungsvorlage können Sie Multi-Member-Netzwerke für Ethereum-Proof-of-Authority-Konsortien für eine oder mehrere Regionen bereitgestellt werden.

![Bereitstellungsarchitektur](./media/ethereum-poa-deployment/deployment-architecture.png)

Jede Bereitstellung von Konsortiumsmitgliedern umfasst Folgendes:

* Virtuelle Computer für die Ausführung der PoA-Validierungssteuerelemente
* Azure Load Balancer für die Verteilung von RPC-, Peering- und Governance-DApp-Anforderungen
* Azure Key Vault zum Schützen der Validierungselementidentitäten
* Azure Storage zum Hosten von persistenten Netzwerkinformationen und Koordinieren des Leasens
* Azure Monitor zum Aggregieren von Protokollen und Leistungsstatistiken
* VNET-Gateway (optional) zum Zulassen von VPN-Verbindungen über private VNETs

Standardmäßig sind die RPC- und Peeringendpunkte über die öffentliche IP-Adresse zugänglich, um eine vereinfachte Konnektivität über Abonnements und Clouds zu ermöglichen. Für die Zugriffssteuerung auf Anwendungsebene können Sie [Parity-Verträge zur Berechtigungsvergabe](https://openethereum.github.io/Permissioning.html) verwenden. Es werden Netzwerke unterstützt, die hinter VPNs mit VNET-Gateways für abonnementübergreifende Konnektivität bereitgestellt werden. Da VPN- und VNET-Bereitstellungen komplexer sind sollten Sie beim Erstellen eines Prototyps für eine Lösung mit einem öffentlichen IP-Modell beginnen.

Docker-Container sorgen für Zuverlässigkeit und Modularität. Azure Container Registry wird verwendet, um im Rahmen jeder Bereitstellung Images mit Versionsangabe zu hosten und bereitzustellen. Die Containerimages bestehen aus:

* Orchestrator: generiert Identitäten und Governanceverträge, speichert Identitäten in einem Identitätsspeicher
* Parity-Client: least Identitäten aus dem Identitätsspeicher, ermittelt Peers und stellt die Verbindung damit her
* EthStats-Agent: erfasst lokale Protokolle und Statistiken per RPC und überträgt Informationen mithilfe von Push an Azure Monitor
* Governance-DApp: Webschnittstelle für die Interaktion mit Governanceverträgen

### <a name="validator-nodes"></a>Validierungsknoten

Beim Proof-of-Authority-Protokoll übernehmen Validierungsknoten die Rolle der herkömmlichen Miningknoten. Jedes Validierungssteuerelement verfügt über eine eindeutige Ethereum-Identität, die die Teilnahme am Blockerstellungsprozess ermöglicht. Jedes Konsortiumsmitglied kann zwei oder mehr Validierungsknoten für fünf Regionen bereitstellen, um Georedundanz zu erzielen. Validierungsknoten kommunizieren mit anderen Validierungsknoten, um sich über den Zustand des zugrunde liegenden Distributed Ledger abzustimmen. Damit im Netzwerk eine faire Beteiligung sichergestellt wird, ist es allen Konsortiumsmitgliedern untersagt, mehr Validierungssteuerelemente als das erste Mitglied des Netzwerks zu verwenden. Wenn das erste Mitglied beispielsweise drei Validierungssteuerelemente bereitstellt, kann jedes Mitglied nur drei Validierungssteuerelemente nutzen.

### <a name="identity-store"></a>Identitätsspeicher

Ein Identitätsspeicher wird im Abonnement jedes Mitglieds bereitgestellt, in dem die generierten Ethereum-Identitäten enthalten sind und geschützt werden. Für jedes Validierungssteuerelement generiert der Orchestrierungscontainer einen privaten Ethereum-Schlüssel für und speichert ihn in Azure Key Vault.

## <a name="deploy-ethereum-consortium-network"></a>Bereitstellen des Ethereum-Konsortiumnetzwerks

In dieser exemplarischen Vorgehensweise gehen wir davon aus, dass Sie ein Multi-Party-Netzwerk für das Ethereum-Konsortium erstellen. Hier sehen Sie ein Beispiel für den Ablauf einer Multi-Party-Bereitstellung:

1. Drei Mitglieder generieren mit MetaMask jeweils ein Ethereum-Konto.
1. *Mitglied A* stellt einen Ethereum-PoA bereit und gibt seine öffentliche Ethereum-Adresse an.
1. *Mitglied A* stellt die Konsortiums-URL für *Mitglied B* und *Mitglied C* bereit.
1. *Mitglied B* und *Mitglied C* stellen Ethereum-PoA bereit und geben ihre öffentliche Ethereum-Adresse und die Konsortiums-URL von *Mitglied A* an.
1. *Mitglied A* wählt per Abstimmung *Mitglied B* als Administrator aus.
1. *Mitglied A* und *Mitglied B* wählen beide *Mitglied C* als Administrator aus.

In den nächsten Abschnitten wird veranschaulicht, wie Sie den Speicherbedarf des ersten Mitglieds im Netzwerk konfigurieren.

### <a name="create-resource"></a>Erstellen von Ressourcen

Klicken Sie im [Azure-Portal](https://portal.azure.com) in der linken oberen Ecke auf **Ressource erstellen**.

Wählen Sie **Blockchain** > **Ethereum Proof-of-Authority Consortium (Preview)** (Blockchain > Ethereum-Proof-of-Authority-Konsortium (Vorschauversion)) aus.

### <a name="basics"></a>Grundlagen

Geben Sie unter **Grundlagen** Werte für die Standardparameter für jede Bereitstellung an.

![Grundlagen](./media/ethereum-poa-deployment/basic-blade.png)

Parameter | BESCHREIBUNG | Beispielwert
----------|-------------|--------------
Neues Netzwerk erstellen oder vorhandenem Netzwerk beitreten? | Sie können ein neues Netzwerk erstellen oder einem bereits vorhandenen Konsortiumnetzwerk beitreten. Für den Beitritt zu einem vorhandenen Netzwerk sind zusätzliche Parameter erforderlich. | Neu erstellen
E-Mail-Adresse | Sie erhalten eine E-Mail-Benachrichtigung mit Informationen zu Ihrer Bereitstellung, wenn die Bereitstellung abgeschlossen ist. | eine gültige E-Mail-Adresse
VM-Benutzername | Benutzername des Administrators der einzelnen bereitgestellten virtuellen Computer | die alphanumerischen Zeichen 1 bis 64
Authentifizierungsart | Die Methode zur Authentifizierung des virtuellen Computers. | Kennwort
Kennwort | Das Kennwort für das Administratorkonto jedes bereitgestellten, virtuellen Computers. Alle VMs verfügen anfänglich über das gleiche Kennwort. Sie können das Kennwort nach der Bereitstellung ändern. | 12 bis 72 Zeichen 
Subscription | Das Abonnement, für das das Konsortiumsnetzwerk bereitgestellt wird. |
Ressourcengruppe| Die Ressourcengruppe, für die das Konsortiumsnetzwerk bereitgestellt wird. | myResourceGroup
Standort | Die Azure-Region für die Ressourcengruppe. | USA, Westen 2

Klicken Sie auf **OK**.

### <a name="deployment-regions"></a>Bereitstellungsregionen

Geben Sie unter *Bereitstellungsregionen* die Anzahl der Regionen und Standorte für die einzelnen Standorte an. Sie können in maximal fünf Regionen Bereitstellungen durchführen. Die erste Region sollte mit dem Ressourcengruppenstandort aus dem Abschnitt *Grundlagen* übereinstimmt. Für Entwicklungs- oder Testnetzwerke können Sie eine einzelne Region pro Mitglied verwenden. Für die Produktion sollten Sie mindestens zwei Regionen bereitstellen, um Hochverfügbarkeit sicherzustellen.

![Bereitstellungsregionen](./media/ethereum-poa-deployment/deployment-regions.png)

Parameter | BESCHREIBUNG | Beispielwert
----------|-------------|--------------
Anzahl der Regionen|Die Anzahl der Regionen, in denen das Konsortiumsnetzwerk bereitgestellt wird.| 2
Erste Region | Die erste Region, in der das Konsortiumsnetzwerk bereitgestellt wird. | USA, Westen 2
Zweite Region | Die zweite Region, in der das Konsortiumsnetzwerk bereitgestellt wird – weitere Regionen werden angezeigt, wenn mindestens zwei Regionen vorhanden sind. | USA (Ost) 2

Klicken Sie auf **OK**.

### <a name="network-size-and-performance"></a>Netzwerkgröße und -leistung

Geben Sie unter *Netzwerkgröße und -leistung* Werte für die Größe des Konsortiumnetzwerks ein. Die Speichergröße des Validierungsknotens gibt die potenzielle Größe der Blockchain vor. Die Größe kann nach der Bereitstellung geändert werden.

![Netzwerkgröße und -leistung](./media/ethereum-poa-deployment/network-size-and-performance.png)

Parameter | BESCHREIBUNG | Beispielwert
----------|-------------|--------------
Anzahl von Validierungsknoten mit Lastenausgleich | Anzahl von Validierungsknoten, die als Teil des Netzwerks bereitgestellt werden | 2
Speicherleistung von Validierungsknoten | Die Art des verwalteten Datenträgers für die einzelnen bereitgestellten Validierungsknoten – Informationen zu den Preisen finden Sie unter [Verwaltete Datenträger – Preise](https://azure.microsoft.com/pricing/details/managed-disks/). | SSD Standard
VM-Größe für Validierungsknoten | Die VM-Größe, die für Validierungsknoten verwendet wird. Informationen zu den Preisen finden Sie unter [Virtuelle Windows-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). | Standard D2 v3

Virtuelle Computer und die Speicherebene haben Auswirkungen auf die Netzwerkleistung.  Mithilfe der folgenden Tabelle können Sie eine kosteneffiziente Option auswählen:

SKU des virtuellen Computers|Speicherebene|Preis|Throughput|Latency
---|---|---|---|---
F1|SSD Standard|niedrig|niedrig|high
D2_v3|SSD Standard|mittel|mittel|mittel
F16s|SSD Premium|high|high|niedrig

Klicken Sie auf **OK**.

### <a name="ethereum-settings"></a>Ethereum-Einstellungen

Geben Sie unter *Ethereum-Einstellungen* die für Ethereum gültigen Konfigurationseinstellungen an.

![Ethereum-Einstellungen](./media/ethereum-poa-deployment/ethereum-settings.png)

Parameter | BESCHREIBUNG | Beispielwert
----------|-------------|--------------
ID des Konsortiumsmitglieds | Die ID, die jedem Mitglied des Konsortiumnetzwerks zugeordnet ist. Sie wird verwendet, um IP-Adressräume zu konfigurieren und Kollisionen zu vermeiden. Bei einem privaten Netzwerk sollte die Mitglieds-ID für verschiedene Organisationen in demselben Netzwerk eindeutig sein.  Eine eindeutige Mitglieds-ID ist erforderlich, auch dieselbe Organisation Bereitstellungen in mehreren Regionen vornimmt. Notieren Sie sich den Wert dieses Parameters, da Sie ihn anderen teilnehmenden Mitgliedern mitteilen müssen, um Konflikte auszuschließen. Der gültige Bereich liegt zwischen 0 und 255. | 0
Netzwerk-ID | Die Netzwerk-ID für das bereitgestellte Ethereum-Konsortiumsnetzwerk. Jedes Ethereum-Netzwerk verfügt über eine eigene Netzwerk-ID, wobei 1 die ID für das öffentliche Netzwerk ist. Der gültige Bereich liegt zwischen 5 und 999.999.999. | 10101010
Ethereum-Administratoradresse | Die Adresse des Ethereum-Kontos, das für die Teilnahme am PoA-Governance-Prozess verwendet wird – Sie können MetaMask verwenden, um eine Ethereum-Adresse zu generieren. |
Erweiterte Optionen | Erweiterte Optionen für Ethereum-Einstellungen | Aktivieren
Deploy using Public IP (Mit öffentlicher IP-Adresse bereitstellen) | Wenn ein privates VNET ausgewählt ist, wird das Netzwerk hinter einer VNet Gateway-Instanz bereitgestellt, und der Peeringzugriff wird aufgehoben. In einem privaten VNET müssen alle Mitglieder eine VNet Gateway-Instanz nutzen, damit die Verbindung kompatibel ist. | Öffentliche IP-Adresse
Block Gas Limit (gas-Limit für Blocks) | Das anfängliche gas-Limit für Blocks im Netzwerk | 50000000
Zeitraum für erneute Blockversiegelung (Sek.) | Die Häufigkeit, mit der leere Blöcke erstellt werden, wenn im Netzwerk keine Transaktionen vorhanden sind. Eine höhere Häufigkeit führt zu einer schnelleren Finalität, jedoch auch zu erhöhten Speicherkosten. | 15
Transaction Permission Contract (Vertrag zu Transaktionsberechtigungen) | Bytecode für den Vertrag zur Berechtigungsvergabe für Transaktionen. Beschränkt die Smart Contract-Bereitstellung und -Ausführung auf eine berechtigungsbasierte Liste mit Ethereum-Konten. |

Klicken Sie auf **OK**.

### <a name="monitoring"></a>Überwachung

Mithilfe der Überwachung können Sie eine Protokollressource für Ihr Netzwerk konfigurieren. Der Überwachungs-Agent sammelt nützliche Metriken und Protokolle aus Ihrem Netzwerk und zeigt diese an. So lassen sich die Netzwerkintegrität oder Debugprobleme schnell überprüfen.

![Azure Monitor](./media/ethereum-poa-deployment/azure-monitor.png)

Parameter | BESCHREIBUNG | Beispielwert
----------|-------------|--------------
Überwachung | Option zum Aktivieren der Überwachung | Aktivieren
Connect to existing Azure Monitor logs (Mit vorhandenen Azure Monitor-Protokollen verbinden) | Option zum Erstellen einer neuen Azure Monitor-Protokollinstanz oder zum Beitritt zu einer vorhandenen Instanz | Neu erstellen
Standort | Die Region, in der die neue Instanz bereitgestellt wird | East US
Existing log analytics workspace ID (Connect to existing Azure Monitor logs = Join Existing) (Vorhandene Log Analytics-Arbeitsbereichs-ID (Mit vorhandener Azure Monitor-Protokollinstanz verbinden = Vorhandener beitreten))|Die Arbeitsbereichs-ID der vorhandenen Azure Monitor-Protokollinstanz||Nicht verfügbar
Existing log analytics primary key (Connect to existing Azure Monitor logs = Join Existing) (Vorhandener Log Analytics-Primärschlüssel (Mit vorhandener Azure Monitor-Protokollinstanz verbinden = Vorhandener beitreten))|Der Primärschlüssel, der zum Herstellen einer Verbindung mit der vorhandenen Azure Monitor-Protokollinstanz verwendet wird.||Nicht verfügbar

Klicken Sie auf **OK**.

### <a name="summary"></a>Zusammenfassung

Klicken Sie sich durch die Zusammenfassung, um die Eingaben zu überprüfen und eine grundlegende Überprüfung vor der Bereitstellung durchzuführen. Vor der Bereitstellung können Sie die Vorlage und die Parameter herunterladen.

Klicken Sie auf **Erstellen**, um die Bereitstellung durchzuführen.

Wenn die Bereitstellung VNet Gateway-Instanzen umfasst, kann diese zwischen 45 und 50 Minuten dauern.

## <a name="deployment-output"></a>Ausgabe der Bereitstellung

Nachdem die Bereitstellung abgeschlossen ist, können Sie über das Azure-Portal auf die erforderlichen Parameter zugreifen.

### <a name="confirmation-email"></a>Bestätigungs-E-Mail

Wenn Sie eine E-Mail-Adresse ([Grundlagen](#basics)) angeben, wird eine E-Mail gesendet, die die Bereitstellungsinformationen und Links zu dieser Dokumentation enthält.

![E-Mail zur Bereitstellung](./media/ethereum-poa-deployment/deployment-email.png)

### <a name="portal"></a>Portal

Nachdem die Bereitstellung erfolgreich abgeschlossen wurde und alle Ressourcen bereitgestellt wurden, können Sie sich die Ausgabeparameter in Ihrer Ressourcengruppe ansehen.

1. Navigieren Sie im Portal zu Ihrer Ressourcengruppe.
1. Klicken Sie auf **Übersicht > Bereitstellungen**.

    ![Übersicht über Ressourcengruppen](./media/ethereum-poa-deployment/resource-group-overview.png)

1. Wählen Sie die Bereitstellung **microsoft-azure-blockchain.azure-blockchain-ether-...** aus.
1. Klicken Sie auf den Abschnitt **Ausgaben**.

    ![Bereitstellungsausgaben](./media/ethereum-poa-deployment/deployment-outputs.png)

## <a name="growing-the-consortium"></a>Vergrößern des Konsortiums

Um Ihr Konsortium zu erweitern, müssen Sie zuerst eine Verbindung mit dem physischen Netzwerk herstellen. Bei einer Bereitstellung hinter einem VPN helfen Ihnen die Informationen im Abschnitt [Herstellen einer Verbindung mit VNet Gateway-Instanzen](#connecting-vnet-gateways) weiter, um die Netzwerkverbindung im Rahmen der Bereitstellung neuer Mitglieder zu konfigurieren. Verwenden Sie nach Abschluss Ihrer Bereitstellung die [Governance-DApp](#governance-dapp), um zum Netzwerkadministrator zu werden.

### <a name="new-member-deployment"></a>Bereitstellung eines neuen Mitglieds

Stellen Sie für das beitretende Mitglied die folgenden Informationen bereit. Diese Informationen finden Sie in der E-Mail, die Sie nach der Bereitstellung erhalten, oder in der Ausgabe zur Bereitstellung im Portal.

* URL der Konsortiumsdaten
* Anzahl von bereitgestellten Knoten
* VNET-Gateway-Ressourcen-ID (bei Verwendung von VPN)

Das bereitstellende Mitglied sollte die gleiche Lösungsvorlage für das Ethereum-Proof-of-Authority-Konsortium verwenden, wenn die Netzwerkverfügbarkeit nach folgender Anleitung bereitgestellt wird:

* Wählen Sie **Vorhandener beitreten**.
* Wählen Sie die gleiche Anzahl von Validierungsknoten wie die anderen Mitglieder des Netzwerks aus, um eine faire Verteilung sicherzustellen.
* Verwenden Sie dieselbe Ethereum-Administratoradresse.
* Nutzen Sie die auf der Registerkarte mit den *Ethereum-Einstellungen* angegebene *URL der Konsortiumsdaten*.
* Wählen Sie unter „Erweitert“ die Option **Private VNet** (Privates VNET) aus, wenn sich der restliche Teil des Netzwerks hinter einem VPN befindet.

### <a name="connecting-vnet-gateways"></a>Herstellen einer Verbindung für VNET-Gateways

Dieser Abschnitt ist nur erforderlich, wenn Sie die Bereitstellung mit einem privaten VNET durchgeführt haben. Sie können diesen Abschnitt überspringen, wenn Sie öffentliche IP-Adressen verwenden.

Bei einem privaten Netzwerk werden die unterschiedlichen Mitglieder über VNET-Gatewayverbindungen vernetzt. Bevor ein Mitglied dem Netzwerk beitreten und Transaktionsdatenverkehr anzeigen lassen kann, muss ein vorhandenes Mitglied die Konfiguration auf seinem VPN-Gateway abschließen, um die Verbindung zu akzeptieren. Die Ethereum-Knoten des beitretenden Mitglieds werden erst ausgeführt, wenn die Verbindung hergestellt wurde. Sie können die Wahrscheinlichkeit eines Single Point of Failure zu reduzieren, indem Sie redundante Netzwerkverbindungen im Konsortium erstellen.

Nachdem das neue Mitglied bereitgestellt wurde, muss das vorhandene Mitglied die bidirektionale Verbindung herstellen, indem es eine VNET-Gatewayverbindung mit dem neuen Mitglied einrichtet. Das vorhandene Mitglied benötigt Folgendes:

* die VNET-Gateway-ResourceID des Mitglieds, das die Verbindung herstellt ([Ausgabe der Bereitstellung](#deployment-output))
* den Schlüssel für die gemeinsame Verbindung

Das vorhandene Mitglied muss das folgende PowerShell-Skript ausführen, um die Verbindung herzustellen. Sie können Azure Cloud Shell verwenden. Die entsprechende Option befindet sich in der rechten oberen Navigationsleiste des Portals.

![Cloud Shell](./media/ethereum-poa-deployment/cloud-shell.png)

```Powershell
$MyGatewayResourceId = "<EXISTING_MEMBER_RESOURCEID>"
$OtherGatewayResourceId = "<NEW_MEMBER_RESOURCEID]"
$ConnectionName = "Leader2Member"
$SharedKey = "<NEW_MEMBER_KEY>"

## $myGatewayResourceId tells me what subscription I am in, what ResourceGroup and the VNetGatewayName
$splitValue = $MyGatewayResourceId.Split('/')
$MySubscriptionid = $splitValue[2]
$MyResourceGroup = $splitValue[4]
$MyGatewayName = $splitValue[8]

## $otherGatewayResourceid tells me what the subscription and VNet GatewayName are
$OtherGatewayName = $OtherGatewayResourceId.Split('/')[8]
$Subscription=Select-AzSubscription -SubscriptionId $MySubscriptionid

## create a PSVirtualNetworkGateway instance for the gateway I want to connect to
$OtherGateway=New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
$OtherGateway.Name = $OtherGatewayName
$OtherGateway.Id = $OtherGatewayResourceId
$OtherGateway.GatewayType = "Vpn"
$OtherGateway.VpnType = "RouteBased"

## get a PSVirtualNetworkGateway instance for my gateway
$MyGateway = Get-AzVirtualNetworkGateway -Name $MyGatewayName -ResourceGroupName $MyResourceGroup

## create the connection
New-AzVirtualNetworkGatewayConnection -Name $ConnectionName -ResourceGroupName $MyResourceGroup -VirtualNetworkGateway1 $MyGateway -VirtualNetworkGateway2 $OtherGateway -Location $MyGateway.Location -ConnectionType Vnet2Vnet -SharedKey $SharedKey -EnableBgp $True
```

## <a name="governance-dapp"></a>Governance-DApp

Das Herzstück von Proof-of-Authority ist die dezentralisierte Governance-Komponente. Da Proof-of-Authority auf einer berechtigungsbasierten Liste mit Netzwerkautoritäten basiert, um das Netzwerk frei von Fehlern zu halten, ist Folgendes wichtig: Es muss ein fairer Mechanismus vorhanden sein, der zum Vornehmen von Änderungen an dieser Berechtigungsliste verwendet werden kann. Jede Bereitstellung verfügt über eine Gruppe von Smart Contracts und ein Portal für On-Chain-Governance-Maßnahmen für diese berechtigungsbasierte Liste. Nachdem eine vorgeschlagene Änderung bei den Konsortiumsmitgliedern die Stimmenmehrheit erzielt hat, wird die Änderung durchgeführt. Durch die Abstimmung können neue Konsensteilnehmer hinzugefügt oder kompromittierte Teilnehmer transparent zu entfernen, um für ein vertrauenswürdiges Netzwerk zu sorgen.

Die Governance-DApp umfasst vorab bereitgestellte [Smart Contracts](https://github.com/Azure-Samples/blockchain/tree/master/ledger/template/ethereum-on-azure/permissioning-contracts) und eine Webanwendung, die zum Steuern der Autoritäten im Netzwerk verwendet werden. Die Autoritäten sind in Administratoridentitäten und Validierungsknoten unterteilt.
Administratoren können die Teilnahme am Konsens an einige Validierungsknoten delegieren. Außerdem können Administratoren andere Administratoren für das Netzwerk auswählen oder abwählen.

![Governance-DApp](./media/ethereum-poa-deployment/governance-dapp.png)

* **Dezentralisierte Governance:** Änderungen der Netzwerkautoritäten werden von ausgewählten Administratoren per On-Chain-Abstimmung verwaltet.
* **Delegation von Validierungssteuerelementen:** Autoritäten können ihre Validierungsknoten verwalten, die für die einzelnen PoA-Bereitstellungen eingerichtet werden.
* **Überwachbarer Änderungsverlauf:** Jede Änderung wird für die Blockchain aufgezeichnet, um für Transparenz und Überwachbarkeit zu sorgen.

### <a name="getting-started-with-governance"></a>Erste Schritte mit Governance

Wenn Sie verschiedene Transaktionen über die Governance-DApp durchführen möchten, müssen Sie ein Ethereum-Wallet nutzen. Die Verwendung eines Wallets wie [MetaMask](https://metamask.io) im Browser stellt den einfachsten Ansatz dar. Da es sich hierbei jedoch um im Netzwerk bereitgestellte Smart Contracts handelt, müssen Sie Ihre Interaktionen mit dem Governancevertrag möglicherweise auch automatisieren.

Navigieren Sie nach der Installation von MetaMask zu der Governance-DApp im Browser.  Sie können die URL im Azure-Portal in der Bereitstellungsausgabe suchen.  Wenn Sie kein Wallet im Browser installiert haben, können Sie keine Aktionen ausführen. Sie können jedoch weiterhin den Administratorstatus anzeigen.  

### <a name="becoming-an-admin"></a>Administrator werden

Wenn Sie das erste Mitglied sind, das im Netzwerk eine Bereitstellung durchgeführt hat, werden Sie automatisch zum Administrator, und Ihre Paritätsknoten werden als Validierungssteuerelemente aufgeführt. Wenn Sie dem Netzwerk beitreten, müssen Sie von einer Mehrheit der vorhandenen Administratoren (mehr als 50 %) zum Administrator gewählt werden. Wenn Sie sich dagegen entscheiden, Administrator zu werden, synchronisieren und überprüfen Ihre Knoten weiterhin die Blockchain. Sie sind jedoch nicht an der Blockerstellung beteiligt. Klicken Sie zum Starten des Abstimmungsprozesses, um Administrator zu werden, auf **Nominierung**, und geben Sie Ihre Ethereum-Adresse und den Alias ein.

![Nominierung](./media/ethereum-poa-deployment/governance-dapp-nominate.png)

### <a name="candidates"></a>Kandidaten

Auf der Registerkarte **Kandidaten** werden Ihnen die aktuellen Bewerber für Administratoren angezeigt.  Sobald ein Kandidat eine Stimmenmehrheit durch die aktuellen Administratoren erreicht, wird er zum Administrator ernannt.  Wenn Sie für einen Kandidaten abstimmen möchten, wählen Sie die Zeile aus, und klicken Sie auf **Vote in** (Abstimmen). Wenn Sie Ihre Meinung zu einer abgegebenen Stimme ändern, können Sie den Kandidaten auswählen und auf **Rescind vote** (Abstimmung widerrufen) klicken.

![Kandidaten](./media/ethereum-poa-deployment/governance-dapp-candidates.png)

### <a name="admins"></a>Administratoren

Auf der Registerkarte **Administratoren** wird die aktuelle Gruppe der Administratoren angezeigt, für die Sie über diese Registerkarte abstimmen können.  Sobald ein Administrator die Unterstützung von mehr als 50 % verliert, wird er als Administrator aus dem Netzwerk entfernt. Alle Validierungsknoten dieses Administrators verlieren ihren Status und werden im Netzwerk zu Transaktionsknoten. Für das Entfernen eines Administrators gibt es verschiedene Gründe. Das Konsortium muss sich jedoch vorab auf eine Richtlinie einigen.

![Administratoren](./media/ethereum-poa-deployment/governance-dapp-admins.png)

### <a name="validators"></a>Validierungssteuerelemente

Durch die Auswahl der Registerkarte **Validierungssteuerelemente** werden die aktuell bereitgestellten Paritätsknoten dieser Instanz mit dem zugehörigen Status (Knotentyp) angezeigt. Jedem Mitglied des Konsortiums in dieser Liste werden andere Validierungssteuerelemente angezeigt, da in dieser Ansicht die aktuell bereitgestellten Mitglieder des Konsortiums dargestellt werden. Handelt es sich um eine neu bereitgestellte Instanz, zu der Sie Ihre Validierungssteuerelemente noch nicht hinzugefügt haben, wird Ihnen die Option **Add Validators** (Validierungssteuerelemente hinzufügen) angezeigt. Durch das Hinzufügen von Validierungssteuerelementen werden automatisch regional ausgewogene Paritätsknoten ausgewählt, die Ihren Validierungssteuerelementen zugewiesen werden. Wenn Sie mit Ihren bereitgestellten Knoten die zulässige Kapazität überschreiten, werden die verbleibenden Knoten zu Transaktionsknoten im Netzwerk.

Die Adresse der einzelnen Validierungssteuerelemente wird automatisch über den [Identitätsspeicher](#identity-store) in Azure zugewiesen.  Wenn ein Knoten ausfällt, wird seine Identität aufgegeben, wodurch ein anderer Knoten in Ihrer Bereitstellung den Platz dieses Knotens übernehmen kann. Dadurch wird sichergestellt, dass Ihre Teilnahme am Konsens hochverfügbar ist.

![Validierungssteuerelemente](./media/ethereum-poa-deployment/governance-dapp-validators.png)

### <a name="consortium-name"></a>Konsortiumsname

Jeder Administrator kann den Namen des Konsortiums aktualisieren.  Klicken Sie links oben auf das Zahnradsymbol, um den Konsortiumsnamen zu aktualisieren.

### <a name="account-menu"></a>Kontomenü

Rechts oben befinden sich Ihr Ethereum-Kontoalias und -Identicon.  Wenn Sie Administrator sind, können Sie Ihren Alias aktualisieren.

![Konto](./media/ethereum-poa-deployment/governance-dapp-account.png)

## <a name="support-and-feedback"></a>Support und Feedback<a id="tutorials"></a>

Neues zu Azure Blockchain: Mit dem [Azure Blockchain-Blog](https://azure.microsoft.com/blog/topics/blockchain/) bleiben Sie in Bezug auf Blockchain-Dienstangebote und Informationen vom Azure Blockchain-Technikteam immer auf dem Laufenden.

Über das [Azure-Feedbackforum für Blockchain](https://aka.ms/blockchainuservoice) können Sie Produktfeedback senden, neue Features anfordern oder über Ideen abstimmen.

### <a name="community-support"></a>Communityunterstützung

Diskutieren Sie mit Microsoft-Technikern und Azure Blockchain-Communityexperten.

* [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-blockchain-workbench.html). Der technische Support für Blockchain-Vorlagen ist auf Bereitstellungsprobleme beschränkt.
* [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-blockchain-workbench)

## <a name="next-steps"></a>Nächste Schritte

Weitere Azure Blockchain-Lösungen finden Sie in der [Dokumentation zu Azure Blockchain](../index.yml).

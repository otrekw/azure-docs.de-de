---
title: Orchestrierungsmodi für VM-Skalierungsgruppen in Azure
description: Es wird beschrieben, wie Sie die Orchestrierungsmodi „Flexibel“ und „Einheitlich“ für VM-Skalierungsgruppen in Azure verwenden.
author: fitzgeraldsteele
ms.author: fisteele
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: extensions
ms.date: 02/12/2021
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 3350ff7aa05232173e5fd3b21451a76a0a40683d
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102043710"
---
# <a name="preview-orchestration-modes-for-virtual-machine-scale-sets-in-azure"></a>Vorschau: Orchestrierungsmodi für VM-Skalierungsgruppen in Azure 

VM-Skalierungsgruppen ermöglichen eine logische Gruppierung von plattformseitig verwalteten virtuellen Computern. Mit Skalierungsgruppen erstellen Sie ein Konfigurationsmodell für virtuelle Computer, fügen automatisch zusätzliche Instanzen basierend auf der CPU- oder Speicherauslastung hinzu oder entfernen sie und führen automatisch ein Upgrade auf die neueste Betriebssystemversion durch. Traditionell ermöglichen Skalierungsgruppen die Erstellung virtueller Computer mit einem VM-Konfigurationsmodell, das zum Zeitpunkt der Erstellung der Skalierungsgruppen bereitgestellt wird. Die Skalierungsgruppen können nur virtuelle Computer verwalten, die implizit basierend auf dem Konfigurationsmodell erstellt werden. 

Mit den Orchestrierungsmodi für Skalierungsgruppen haben Sie eine bessere Kontrolle darüber, wie die Instanzen virtueller Computer von der Skalierungsgruppe verwaltet werden. 

> [!IMPORTANT]
> Der Orchestrierungsmodus wird beim Erstellen der Skalierungsgruppe festgelegt und kann später nicht mehr geändert oder aktualisiert werden.


## <a name="scale-sets-with-uniform-orchestration"></a>Skalierungsgruppen mit einheitlicher Orchestrierung
Optimiert für umfangreiche zustandslose Workloads mit identischen Instanzen.

Für VM-Skalierungsgruppen mit dem Orchestrierungsmodus „Einheitlich“ wird ein VM-Profil oder eine entsprechende Vorlage verwendet, um auf die gewünschte Kapazität hochzuskalieren. Einzelne VM-Instanzen können zwar bis zu einem gewissen Maße verwaltet oder angepasst werden, aber im Modus „Einheitlich“ werden identische VM-Instanzen verwendet. Einzelne einheitliche VM-Instanzen werden über die API-Befehle für die VMs der VM-Skalierungsgruppe verfügbar gemacht. Einzelne Instanzen sind nicht mit den Standardbefehlen der Azure IaaS-VM-API, den Azure-Verwaltungsfeatures, z. B. Azure Resource Manager-Ressourcenmarkierung oder RBAC-Berechtigungen, mit Azure Backup oder mit Azure Site Recovery kompatibel. Im Orchestrierungsmodus „Einheitlich“ ist die Hochverfügbarkeit für Fehlerdomänen garantiert, wenn eine Konfiguration mit weniger als 100 Instanzen genutzt wird. Der Orchestrierungsmodus „Einheitlich“ ist allgemein verfügbar und unterstützt viele verschiedene Features für die Verwaltung und Orchestrierung, z. B. metrikbasierte Autoskalierung, Instanzschutz und automatische Betriebssystemupgrades. 


## <a name="scale-sets-with-flexible-orchestration"></a>Skalierungsgruppen mit flexibler Orchestrierung 
Erzielen Sie Hochverfügbarkeit im großen Stil mit identischen oder mehreren unterschiedlichen VM-Typen.

Im Orchestrierungsmodus „Flexibel“ können Sie im gesamten Azure-VM-Ökosystem eine einheitliche Benutzeroberfläche nutzen. Bei der flexiblen Orchestrierung wird Hochverfügbarkeit garantiert (für bis zu 1.000 VMs), indem VMs auf Fehlerdomänen in einer Region oder einer Verfügbarkeitszone verteilt werden. Dies ermöglicht Ihnen das Aufskalieren Ihrer Anwendung bei gleichzeitiger Fehlerdomänenisolation, die für die Ausführung von quorumbasierten oder zustandsbehafteten Workloads von entscheidender Bedeutung ist. Beispiele:
- Quorumbasierte Workloads
- Open-Source-Datenbanken
- Zustandsbehaftete Anwendungen
- Dienste, für die Hochverfügbarkeit und starke Skalierung erforderlich sind
- Dienste, bei denen VM-Typen gemischt oder Spot- und On-Demand-VMs zusammen genutzt werden sollen
- Vorhandene Verfügbarkeitsgruppen-Anwendungen  

> [!IMPORTANT]
> Das Feature für VM-Skalierungsgruppen im Orchestrierungsmodus „Flexibel“ befindet sich derzeit in der öffentlichen Vorschauphase. Es ist ein Opt-in-Verfahren erforderlich, um die unten beschriebenen Funktionen der öffentlichen Vorschauversion zu nutzen.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="what-has-changed-with-flexible-orchestration-mode"></a>Was hat sich mit dem Orchestrierungsmodus „Flexibel“ geändert?
Einer der Hauptvorteile des Orchestrierungsmodus „Flexibel“ sind die für Azure IaaS-Standard-VMs verfügbaren Orchestrierungsfeatures (anstelle von untergeordneten VMs von Skalierungsgruppen). Dies bedeutet, dass Sie beim Verwalten von Instanzen mit flexibler Orchestrierung alle Standard-VM-APIs verwenden können, anstatt der APIs für die VMs von VM-Skalierungsgruppen im Orchestrierungsmodus „Einheitlich“. Während der Vorschauphase bestehen zwischen dem Verwalten von Instanzen im Orchestrierungsmodus „Flexibel“ und „Einheitlich“ mehrere Unterschiede. Im Allgemeinen lautet unsere Empfehlung, nach Möglichkeit die Standard-APIs für Azure IaaS-VMs zu verwenden. In diesem Abschnitt werden Beispiele für bewährte Methoden zum Verwalten von VM-Instanzen mit flexibler Orchestrierung beschrieben.  

### <a name="assign-fault-domain-during-vm-creation"></a>Zuweisen einer Fehlerdomäne während der VM-Erstellung
Sie können die Anzahl von Fehlerdomänen für die Skalierungsgruppe mit flexibler Orchestrierung auswählen. Wenn Sie eine VM einer flexiblen Skalierungsgruppe hinzufügen, werden die Instanzen von Azure gleichmäßig auf die Fehlerdomänen verteilt. Unsere Empfehlung lautet zwar, die Fehlerdomäne von Azure zuweisen zu lassen, aber für erweiterte oder Problembehandlungsszenarien können Sie dieses Standardverhalten außer Kraft setzen und die Fehlerdomäne angeben, in der die Instanz angeordnet werden soll.

```azurecli-interactive 
az vm create –vmss "myVMSS"  –-platform_fault_domain 1
```

### <a name="instance-naming"></a>Benennung von Instanzen 
Wenn Sie eine VM erstellen und einer flexiblen Skalierungsgruppe hinzufügen, haben Sie über die Azure-Regeln für die Namenskonvention die vollständige Kontrolle über die Instanznamen. Wenn VMs der Skalierungsgruppe über die Autoskalierung automatisch hinzugefügt werden, geben Sie ein Präfix an, und Azure fügt am Ende des Namens dann eine eindeutige Zahl an.

### <a name="query-instances-for-power-state"></a>Abfragen des Energiezustands von Instanzen
Die bevorzugte Methode ist hierbei die Verwendung von Azure Resource Graph zum Abfragen aller VMs einer VM-Skalierungsgruppe. Azure Resource Graph verfügt über effiziente Abfragefunktionen, mit denen Azure-Ressourcen im großen Stil abonnementübergreifend abgefragt werden können. 

``` 
| where type =~ 'Microsoft.Compute/virtualMachines' 
| where properties.virtualMachineScaleSet contains "demo" 
| extend powerState = properties.extended.instanceView.powerState.code 
| project name, resourceGroup, location, powerState 
| order by resourceGroup desc, name desc 
```

Die Funktion zum Abfragen von Ressourcen von [Azure Resource Graph](../governance/resource-graph/overview.md) ist eine bequeme und effiziente Möglichkeit, um Azure-Ressourcen abzufragen, und verringert die Anzahl von API-Aufrufen des Ressourcenanbieters. Azure Resource Graph ist letztlich ein konsistenter Cache, in dem neue oder aktualisierte Ressourcen ggf. bis zu 60 Sekunden lang nicht widergespiegelt werden. Ihre Möglichkeiten:
- Auflisten einer Ressourcengruppe oder eines Abonnements
- Verwenden der Option „Erweitern“ zum Abrufen der Instanzansicht (Zuweisung von Fehlerdomänen, Energie- und Bereitstellungsstatus) für alle VMs Ihres Abonnements
- Verwenden der Get-VM-API und der Befehle zum Abrufen der Modell- und Instanzansicht für eine einzelne Instanz

### <a name="scale-sets-vm-batch-operations"></a>Batchvorgänge für VMs von Skalierungsgruppen
Verwenden Sie die VM-Standardbefehle zum Starten, Beenden, Neustarten oder Löschen von Instanzen (anstelle der APIs für die VMs einer VM-Skalierungsgruppe). Die VM-Batchvorgänge für VM-Skalierungsgruppen (Alle starten, Alle beenden, Reimaging für alle usw.) werden im Orchestrierungsmodus „Flexibel“ nicht verwendet. 

### <a name="monitor-application-health"></a>Überwachen der Anwendungsintegrität 
Beim Überwachen der Anwendungsintegrität kann Ihre Anwendung für Azure einen Heartbeat bereitstellen, damit ermittelt werden kann, ob Ihre Anwendung fehlerfrei oder fehlerhaft ist. Azure kann fehlerhafte VM-Instanzen automatisch austauschen. Bei flexiblen Skalierungsgruppeninstanzen müssen Sie die Erweiterung zur Anwendungsintegrität auf dem virtuellen Computer installieren und konfigurieren. Für einheitliche Skalierungsgruppeninstanzen können Sie entweder die Erweiterung für die Anwendungsintegrität verwenden oder die Integrität mit einem benutzerdefinierten Azure Load Balancer-Integritätstest messen. 

### <a name="list-scale-sets-vm-api-changes"></a>Auflisten der API-Änderungen für VMs einer Skalierungsgruppe 
Mit VM-Skalierungsgruppen können Sie die Instanzen auflisten, die zur Skalierungsgruppe gehören. Bei der flexiblen Orchestrierung wird mit dem Befehl zum Auflisten der VMs einer VM-Skalierungsgruppe eine Liste angezeigt, die die IDs der VMs einer Skalierungsgruppe enthält. Anschließend können Sie die GET-Befehle für die VMs einer VM-Skalierungsgruppe verwenden, um weitere Details zur Zusammenarbeit zwischen Skalierungsgruppe und VM-Instanz abzurufen. Verwenden Sie zum Abrufen der vollständigen Details zur VM die GET-VM-Standardbefehle oder [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview). 

### <a name="retrieve-boot-diagnostics-data"></a>Abrufen von Startdiagnosedaten 
Verwenden Sie die Standard-VM-APIs und -Befehle, um Daten und Screenshots zur Startdiagnose abzurufen. Die Startdiagnose-APIs und -befehle für VMs einer VM-Skalierungsgruppe werden für Instanzen mit dem Orchestrierungsmodus „Flexibel“ nicht verwendet.

### <a name="vm-extensions"></a>VM-Erweiterungen 
Verwenden Sie für Standard-VMs bestimmte Erweiterungen, und keine Erweiterungen, die für Instanzen mit dem Orchestrierungsmodus „Einheitlich“ bestimmt sind.


## <a name="a-comparison-of-flexible-uniform-and-availability-sets"></a>Vergleich der Modi „Flexibel“ und „Einheitlich“ und von Verfügbarkeitsgruppen 
In der folgenden Tabelle werden die Orchestrierungsmodi „Flexibel“ und „Einheitlich“ und Verfügbarkeitsgruppen in Bezug auf ihre Features verglichen.

| Funktion | Von flexibler Orchestrierung unterstützt (Vorschau) | Von einheitlicher Orchestrierung unterstützt (allgemeine Verfügbarkeit) | Von Verfügbarkeitsgruppen unterstützt (allgemeine Verfügbarkeit) |
|-|-|-|-|
|         Typ des virtuellen Computers  | Azure IaaS-Standard-VM (Microsoft.compute /virtualmachines)  | Skalierungsgruppenspezifische VMs (Microsoft.compute /virtualmachinescalesets/virtualmachines)  | Azure IaaS-Standard-VM (Microsoft.compute /virtualmachines)  |
|         Unterstützte SKUs  |            D-Serie, E-Serie, F-Serie, A-Serie, B-Serie, Intel, AMD  |            Alle SKUs  |            Alle SKUs  |
|         Verfügbarkeitszonen  |            Geben Sie optional an, dass alle Instanzen in nur einer Verfügbarkeitszone angeordnet sein sollen. |            Geben Sie an, dass Instanzen in einer, zwei oder drei Verfügbarkeitszonen angeordnet sein sollen.  |            Nicht unterstützt  |
|         Vollständige Kontrolle über VM, NICs, Datenträger  |            Ja  |            Eingeschränkte Kontrolle mit API für VMs der VM-Skalierungsgruppe  |            Ja  |
|         Autoskalierung  |            Nein  |            Ja  |            Nein  |
|         Zuweisung einer VM zu einer bestimmten Fehlerdomäne  |            Ja  |             Nein   |            Nein  |
|         Entfernung von NICs und Datenträgern beim Löschen von VM-Instanzen  |            Nein  |            Ja  |            Nein  |
|         Upgraderichtlinie (VM-Skalierungsgruppen) |            Nein  |            Automatisch, Parallel, Manuell  |            –  |
|         Automatische Betriebssystemupdates (VM-Skalierungsgruppen) |            Nein  |            Ja  |            –  |
|         Sicherheitspatches auf Gastbetriebssystem  |            Ja  |            Nein  |            Ja  |
|         Beendigung von Benachrichtigungen (VM-Skalierungsgruppen) |            Nein  |            Ja  |            –  |
|         Instanzreparatur (VM-Skalierungsgruppen) |            Nein  |            Ja   |            –  |
|         Beschleunigte Netzwerke  |            Ja  |            Ja  |            Ja  |
|         Spot-Instanzen und Preise   |            Ja. Sie können sowohl Spot-Instanzen als auch Instanzen mit regulärer Priorität verwenden.  |            Ja. Entweder nur Spot-Instanzen oder nur reguläre Instanzen.  |            Nein. Nur Instanzen mit regulärer Priorität.  |
|         Mischung von Betriebssystemen  |            Ja. Linux und Windows können in derselben flexiblen Skalierungsgruppe vorhanden sein. |            Nein. Instanzen weisen das gleiche Betriebssystem auf.  |               Ja. Linux und Windows können in derselben flexiblen Skalierungsgruppe vorhanden sein. |
|         Überwachung der Anwendungsintegrität  |            Erweiterung für Anwendungsintegrität  |            Erweiterung für Anwendungsintegrität oder Azure Load Balancer-Test  |            Erweiterung für Anwendungsintegrität  |
|         SSD Ultra-Datenträger   |            Ja  |            Ja, nur für zonale Bereitstellungen  |            Nein  |
|         InfiniBand   |            Nein  |            Ja, nur eine Platzierungsgruppe  |            Ja  |
|         Schreibbeschleunigung   |            Nein  |            Ja  |            Ja  |
|         Näherungsplatzierungsgruppen   |            Ja  |            Ja  |            Ja  |
|         Dedizierte Azure-Hosts   |            Nein  |            Ja  |            Ja  |
|         Basis-SLB   |            Nein  |            Ja  |            Ja  |
|         Azure Load Balancer Standard-SKU |            Ja  |            Ja  |            Ja  |
|         Application Gateway  |            Nein  |            Ja  |            Ja  |
|         Wartungssteuerung   |            Nein  |            Ja  |            Ja  |
|         Auflistung von VMs einer Gruppe  |            Ja  |            Ja  |            Ja, Auflistung der VMs einer Verfügbarkeitsgruppe  |
|         Azure-Warnungen  |            Nein  |            Ja  |            Ja  |
|         VM Insights  |            Nein  |            Ja  |            Ja  |
|         Azure Backup  |            Ja  |            Ja  |            Ja  |
|         Azure Site Recovery  |            Ja, nur PowerShell  |            Ja  |            Ja  |
|         Hinzufügung/Entfernung einer vorhandenen VM für Gruppe  |            Nein  |            Nein  |            Nein  | 


## <a name="register-for-flexible-orchestration-mode"></a>Registrieren für den Orchestrierungsmodus „Flexibel“
Bevor Sie VM-Skalierungsgruppen im Orchestrierungsmodus „Flexibel“ bereitstellen können, müssen Sie Ihr Abonnement zunächst für die Previewfunktion registrieren. Die Registrierung kann mehrere Minuten dauern. Für die Registrierung können Sie die folgenden Azure PowerShell- oder Azure CLI-Befehle verwenden.

### <a name="azure-powershell"></a>Azure PowerShell 
Verwenden Sie das Cmdlet [Register-AzProviderFeature](/powershell/module/az.resources/register-azproviderfeature), um die Vorschauversion für Ihr Abonnement zu aktivieren. 

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName VMOrchestratorMultiFD -ProviderNamespace Microsoft.Compute `
Register-AzProviderFeature -FeatureName VMOrchestratorSingleFD -ProviderNamespace Microsoft.Compute  
```

Die Featureregistrierung kann bis zu 15 Minuten dauern. So überprüfen Sie den Registrierungsstatus: 

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName VMOrchestratorMultiFD -ProviderNamespace Microsoft.Compute 
```

Schließen Sie den Opt-in-Prozess ab, nachdem Sie das Feature für Ihr Abonnement registriert haben, indem Sie die Änderung an den Computeressourcenanbieter weitergeben. 

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute 
```

### <a name="azure-cli-20"></a>Azure CLI 2.0 
Verwenden Sie [az feature register](/cli/azure/feature#az-feature-register), um die Vorschauversion für Ihr Abonnement zu aktivieren. 

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name VMOrchestratorMultiFD
az feature register --namespace microsoft.compute --name VMOrchestratorSingleFD 
```

Die Featureregistrierung kann bis zu 15 Minuten dauern. So überprüfen Sie den Registrierungsstatus: 

```azurecli-interactive
az feature show --namespace Microsoft.Compute --name VMOrchestratorMultiFD 
```

Schließen Sie den Opt-in-Prozess ab, nachdem Sie das Feature für Ihr Abonnement registriert haben, indem Sie die Änderung an den Computeressourcenanbieter weitergeben. 

```azurecli-interactive
az provider register --namespace Microsoft.Compute 
```


## <a name="get-started-with-flexible-orchestration-mode"></a>Erste Schritte mit dem Orchestrierungsmodus „Flexibel“

Führen Sie die ersten Schritte im Orchestrierungsmodus „Flexibel“ für Ihre Skalierungsgruppen über das Azure-Portal, die Azure CLI, Terraform oder die REST-API aus.

### <a name="azure-portal"></a>Azure-Portal

Erstellen Sie im Orchestrierungsmodus „Flexibel“ eine VM-Skalierungsgruppe über das Azure-Portal.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie in der Suchleiste nach **VM-Skalierungsgruppen**, und wählen Sie den Eintrag dann aus. 
1. Wählen Sie auf der Seite **VM-Skalierungsgruppen** die Option **Erstellen** aus.
1. Zeigen Sie auf der Seite **VM-Skalierungsgruppe erstellen** den Abschnitt **Orchestration** an.
1. Wählen Sie unter **Orchestrierungsmodus** die Option **Flexibel** aus.
1. Legen Sie für **Anzahl von Fehlerdomänen** den gewünschten Wert fest.
1. Schließen Sie die Erstellung Ihrer Skalierungsgruppe ab. Weitere Informationen zum Erstellen einer Skalierungsgruppe finden Sie unter [Erstellen einer Skalierungsgruppe im Azure-Portal](quick-create-portal.md#create-virtual-machine-scale-set).

:::image type="content" source="./media/virtual-machine-scale-sets-orchestration-modes/portal-create-orchestration-mode-flexible.png" alt-text="Orchestrierungsmodus im Portal beim Erstellen einer Skalierungsgruppe":::

Fügen Sie als Nächstes der Skalierungsgruppe im Orchestrierungsmodus „Flexibel“ einen virtuellen Computer hinzu.

1. Suchen Sie in der Suchleiste nach **Virtuelle Computer**, und wählen Sie den Eintrag dann aus.
1. Wählen Sie auf der Seite **Virtuelle Computer** die Option **Hinzufügen** aus.
1. Zeigen Sie auf der Registerkarte **Grundlagen** den Abschnitt mit den **Details zur Instanz** an.
1. Fügen Sie Ihre VM der Skalierungsgruppe im Orchestrierungsmodus „Flexibel“ hinzu, indem Sie die Skalierungsgruppe unter **Verfügbarkeitsoptionen** auswählen. Sie können den virtuellen Computer einer Skalierungsgruppe in derselben Region, Zone und Ressourcengruppe hinzufügen.
1. Schließen Sie die Erstellung Ihres virtuellen Computers ab. 

:::image type="content" source="./media/virtual-machine-scale-sets-orchestration-modes/vm-portal-orchestration-mode-flexible.png" alt-text="Hinzufügen einer VM zu einer Skalierungsgruppe im Orchestrierungsmodus „Flexibel“":::


### <a name="azure-cli-20"></a>Azure CLI 2.0
Erstellen einer flexiblen VM-Skalierungsgruppe mit der Azure CLI Im folgenden Beispiel wird die Erstellung einer flexiblen Skalierungsgruppe veranschaulicht. Die Anzahl von Fehlerdomänen wird auf „3“ festgelegt, und ein virtueller Computer wird erstellt und dann der flexiblen Skalierungsgruppe hinzugefügt. 

```azurecli-interactive
vmssflexname="my-vmss-vmssflex"  
vmname="myVM"  
rg="my-resource-group"  

az group create -n "$rg" -l $location  
az vmss create -n "$vmssflexname" -g "$rg" -l $location --orchestration-mode flexible --platform-fault-domain-count 3  
az vm create -n "$vmname" -g "$rg" -l $location --vmss $vmssflexname --image UbuntuLTS 
```

### <a name="terraform"></a>Terraform
Erstellen Sie eine flexible VM-Skalierungsgruppe mit Terraform. Für diesen Prozess ist **Terraform Azurerm provider v2.15.0** oder höher erforderlich. Beachten Sie die folgenden Parameter:
- Wenn keine Zone angegeben ist, kann `platform_fault_domain_count` je nach Region „1“, „2“ oder „3“ lauten.
- Wenn eine Zone angegeben ist, kann `the fault domain count` nur „1“ lauten.
- Der Parameter `single_placement_group` muss für flexible VM-Skalierungsgruppen `false` lauten.
- Falls Sie eine regionale Bereitstellung durchgeführt haben, müssen Sie `zones` nicht angeben.

```terraform
resource "azurerm orchestrated_virtual_machine_scale_set" "tf_vmssflex" {
name = "tf_vmssflex"
location = azurerm_resource_group.myterraformgroup.location
resource_group_name = azurerm_resource_group.myterraformgroup.name
platform_fault_domain_count = 1
single_placement_group = false
zones = ["1"]
}
```


### <a name="rest-api"></a>REST-API

1. Erstellen Sie eine leere Skalierungsgruppe. Die folgenden Parameter sind erforderlich:
    - API-Version 2019-12-01 (oder höher) 
    - Beim Erstellen einer flexiblen Skalierungsgruppe muss die Option für eine einzelne Platzierungsgruppe auf `false` festgelegt werden.

    ```json
    {
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('virtualMachineScaleSetName')]",
    "apiVersion": "2019-12-01",
    "location": "[parameters('location')]",
    "properties": {
        "singlePlacementGroup": false,
        "platformFaultDomainCount": "[parameters('virtualMachineScaleSetPlatformFaultDomainCount')]"
        },
    "zones": "[variables('selectedZone')]"
    }
    ```

2. Fügen Sie der Skalierungsgruppe virtuelle Computer hinzu.
    1. Fügen Sie die Eigenschaft `virtualMachineScaleSet` der Skalierungsgruppe hinzu, die Sie zuvor erstellt haben. Sie müssen die Eigenschaft `virtualMachineScaleSet` bei der VM-Erstellung angeben. 
    1. Sie können die Funktion **copy()** der Azure Resource Manager-Vorlage verwenden, um mehrere VMs gleichzeitig zu erstellen. Weitere Informationen finden Sie im Artikel zur [Ressourceniteration in Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/templates/copy-resources#iteration-for-a-child-resource). 

    ```json
    {
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(parameters('virtualMachineNamePrefix'), copyIndex(1))]",
    "apiVersion": "2019-12-01",
    "location": "[parameters('location')]",
    "copy": {
        "name": "VMcopy",
        "count": "[parameters('virtualMachineCount')]"
        },
    "dependsOn": [
        "
        [resourceID('Microsoft.Compute/virtualMachineScaleSets', parameters('virtualMachineScaleSetName'))]",
        "
        [resourceID('Microsoft.Storage/storageAccounts', variables('diagnosticsStorageAccountName'))]",
        "
        [resourceID('Microsoft.Network/networkInterfaces', concat(parameters('virtualMachineNamePrefix'), copyIndex(1), '-NIC1'))]"
        ],
    "properties": {
        "virtualMachineScaleSet": {
            "id": "[resourceID('Microsoft.Compute/virtualMachineScaleSets', parameters('virtualMachineScaleSetName'))]"
        }
    }
    ```

Ein vollständiges Beispiel finden Sie in [dieser Azure-Schnellstartanleitung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-vmss-flexible-orchestration-mode).


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Welcher Skalierungsgrad wird für die flexible Orchestrierung unterstützt?**

Im Orchestrierungsmodus „Flexibel“ können Sie einer Skalierungsgruppe bis zu 1.000 VMs hinzufügen.

**Was ist in Bezug auf die Verfügbarkeit der flexiblen Orchestrierung der Unterschied zu Verfügbarkeitsgruppen oder zur einheitlichen Orchestrierung?**

|   | Flexible Orchestrierung  | Einheitliche Orchestrierung  | Verfügbarkeitsgruppen  |
|-|-|-|-|
| Übergreifende Bereitstellung in mehreren Verfügbarkeitszonen  | Nein  | Ja  | Nein  |
| Garantien zur Verfügbarkeit von Fehlerdomänen in einer Region  | Ja. Maximal 1.000 Instanzen können auf bis zu drei Fehlerdomänen in der Region verteilt werden. Maximale Anzahl von Fehlerdomänen variiert je nach Region  | Ja, bis zu 100 Instanzen  | Ja, bis zu 200 Instanzen  |
| Platzierungsgruppen  | Im Modus „Flexibel“ werden immer mehrere Platzierungsgruppen genutzt (singlePlacementGroup = false).  | Sie können „Einzelne Platzierungsgruppe“ oder „Mehrere Platzierungsgruppen“ auswählen. | –  |
| Updatedomänen  | Keine. Wartungs- oder Hostupdates werden Fehlerdomäne für Fehlerdomäne durchgeführt.  | Bis zu 5 Updatedomänen  | Bis zu 20 Updatedomänen  |


## <a name="troubleshoot-scale-sets-with-flexible-orchestration"></a>Problembehandlung für Skalierungsgruppen mit flexibler Orchestrierung
Ermitteln Sie die richtige Lösung für Ihr Problembehandlungsszenario. 

```
InvalidParameter. The value 'False' of parameter 'singlePlacementGroup' is not allowed. Allowed values are: True
```

**Ursache:** Das Abonnement ist nicht für den Orchestrierungsmodus „Flexibel“ (öffentliche Vorschauphase) registriert. 

**Lösung:** Befolgen Sie die obige Anleitung, um die Registrierung für den Orchestrierungsmodus „Flexibel“ (öffentliche Vorschauphase) durchzuführen. 

```
InvalidParameter. The specified fault domain count 2 must fall in the range 1 to 1.
```

**Ursache:** Der Parameter `platformFaultDomainCount` ist für die ausgewählte Region oder Zone ungültig. 

**Lösung:** Sie müssen einen gültigen Wert für `platformFaultDomainCount` auswählen. Für zonale Bereitstellungen beträgt der Maximalwert für `platformFaultDomainCount` „1“. Bei regionalen Bereitstellungen ohne Angabe einer Zone variiert der Maximalwert für `platformFaultDomainCount` je nach Region. Lesen Sie die Informationen im Abschnitt zum [Verwalten der Verfügbarkeit von VMs für Skripts](../virtual-machines/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set), um die maximale Anzahl von Fehlerdomänen pro Region zu ermitteln. 

```
OperationNotAllowed. Deletion of Virtual Machine Scale Set is not allowed as it contains one or more VMs. Please delete or detach the VM(s) before deleting the Virtual Machine Scale Set.
```

**Ursache:** Es wird versucht, eine Skalierungsgruppe im Modus „Flexibel“ zu löschen, die mindestens einem virtuellen Computer zugeordnet ist. 

**Lösung:** Löschen Sie alle virtuellen Computer, die der Skalierungsgruppe im Orchestrierungsmodus „Flexibel“ zugeordnet sind. Anschließend können Sie die Skalierungsgruppe löschen.

```
InvalidParameter. The value 'True' of parameter 'singlePlacementGroup' is not allowed. Allowed values are: False.
```
**Ursache:** Das Abonnement ist für den Orchestrierungsmodus „Flexibel“ (Vorschau) registriert, aber der Parameter `singlePlacementGroup` ist auf *True* festgelegt. 

**Lösung:** `singlePlacementGroup` muss auf *False* festgelegt werden. 


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Informieren Sie sich über die Bereitstellung Ihrer Anwendung in einer Skalierungsgruppe.](virtual-machine-scale-sets-deploy-app.md)

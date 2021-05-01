---
title: Migrieren zu Azure Cloud Services (erweiterter Support) mit PowerShell
description: 'Vorgehensweise: Migrieren von Azure Cloud Services (klassisch) zu Azure Cloud Services (erweiterter Support) mit PowerShell'
author: tanmaygore
ms.service: cloud-services-extended-support
ms.subservice: classic-to-arm-migration
ms.reviwer: mimckitt
ms.topic: how-to
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: eea49a41e81e7e580becce815ff91aff6aa430d6
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286741"
---
# <a name="migrate-to-azure-cloud-services-extended-support-using-powershell"></a>Migrieren zu Azure Cloud Services (erweiterter Support) mit PowerShell

In den folgenden Schritten wird gezeigt, wie Sie mit Azure PowerShell-Befehlen von [Cloud Services (klassisch)](../cloud-services/cloud-services-choose-me.md) zu [Cloud Services (erweiterter Support)](overview.md) migrieren.

> [!IMPORTANT]
> Die Migration von Cloud Services (klassisch) zu Cloud Services (erweiterter Support) mithilfe des Migrationstools befindet sich derzeit in der öffentlichen Vorschau. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="1-plan-for-migration"></a>1) Planen der Migration
Die Planung ist der wichtigste Schritt für eine erfolgreiche Migration. Lesen Sie die [Übersicht zu Cloud Services (erweiterte Unterstützung)](overview.md) und den Artikel [Planen der Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/migration-classic-resource-manager-plan.md), bevor Sie mit den Migrationsschritten beginnen. 

## <a name="2-install-the-latest-version-of-powershell"></a>2) Installieren der neuesten Version von PowerShell
Für die Installation von Azure PowerShell stehen zwei Hauptoptionen zur Verfügung: [PowerShell-Katalog](https://www.powershellgallery.com/profiles/azure-sdk/) oder [Webplattform-Installer (WebPI)](https://aka.ms/webpi-azps). WebPI empfängt monatliche Updates. Der PowerShell-Katalog wird fortlaufend aktualisiert. Dieser Artikel basiert auf Azure PowerShell Version 2.1.0.

Installationsanweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0&preserve-view=true).

## <a name="3-ensure-admin-permissions"></a>3) Sicherstellen der Administratorberechtigungen
Um diese Migration auszuführen, müssen Sie im [Azure-Portal](https://portal.azure.com) als Co-Administrator für das Abonnement hinzugefügt werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü **Hub** die Option **Abonnement** aus. Wählen Sie **Alle Dienste** aus, falls die Option nicht angezeigt wird.
3. Suchen Sie den entsprechenden Abonnementeintrag, und prüfen Sie dann das Feld **MEINE ROLLE**. Bei einem Co-Administrator muss der Wert *Kontoadministrator* lauten.

Sollten Sie keinen Co-Admin hinzufügen können, lassen Sie sich von einem Dienstadministrator oder Co-Admin für das Abonnement hinzufügen.

## <a name="4-register-the-classic-provider-and-cloudservice-feature"></a>4) Registrieren des klassischen Anbieters und des CloudService-Features
Starten Sie zunächst eine PowerShell-Eingabeaufforderung. Für die Migration richten Sie Ihre Umgebung sowohl für das klassische Bereitstellungsmodell als auch für Resource Manager ein.

Melden Sie sich an Ihrem Konto für das Resource Manager-Modell an.

```powershell
Connect-AzAccount
```

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab:

```powershell
Get-AzSubscription | Sort Name | Select Name
```

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. In diesem Beispiel wird der Abonnementname standardmäßig auf **My Azure Subscription** festgelegt. Ersetzen Sie den Abonnementnamen im Beispiel durch den Namen Ihres eigenen Abonnements.

```powershell
Select-AzSubscription –SubscriptionName "My Azure Subscription"
```

Registrieren Sie sich mithilfe des folgenden Befehls beim Migrationsressourcenanbieter:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```
> [!NOTE]
> Die Registrierung ist ein einmaliger Schritt, der jedoch einmal ausgeführt werden muss, bevor Sie versuchen, die Migration auszuführen. Ohne Registrierung wird die folgende Fehlermeldung angezeigt:
>
> *BadRequest: Das Abonnement ist nicht für die Migration registriert.*

Registrieren Sie das CloudServices-Feature für Ihr Abonnement. Die Registrierung kann mehrere Minuten dauern. 

```powershell
Register-AzProviderFeature -FeatureName CloudServices -ProviderNamespace Microsoft.Compute
```

Warten Sie fünf Minuten, bis die Registrierung abgeschlossen wird. 

Überprüfen Sie den Genehmigungsstatus des klassischen Anbieters mit dem folgenden Befehl:

```powershell
Get-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Überprüfen Sie den Status der Registrierung wie folgt:  
```powershell
Get-AzProviderFeature -FeatureName CloudServices
```

Stellen Sie sicher, dass der RegistrationState-Wert für beide `Registered` lautet, bevor Sie fortfahren.

Vergewissern Sie sich vor dem Wechsel zum klassischen Bereitstellungsmodell, dass Sie in der Azure-Region Ihrer aktuellen Bereitstellung oder Ihres aktuellen virtuellen Netzwerks über ein ausreichendes Azure Resource Manager-vCPU-Kontingent verfügen. Mit dem folgenden PowerShell-Befehl können Sie die aktuelle Anzahl von vCPUs in Azure Resource Manager überprüfen. Weitere Informationen zu vCPU-Kontingenten finden Sie unter [Grenzwerte und der Azure Resource Manager](../azure-resource-manager/management/azure-subscription-service-limits.md#managing-limits).

In diesem Beispiel wird die Verfügbarkeit in der Region **USA, Westen** geprüft. Ersetzen Sie den Regionsnamen im Beispiel durch den Namen Ihrer eigenen Region.

```powershell
Get-AzVMUsage -Location "West US"
```

Melden Sie sich jetzt bei Ihrem Konto für das klassische Bereitstellungsmodell an.

```powershell
Add-AzureAccount
```

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab:

```powershell
Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. In diesem Beispiel wird das Abonnement standardmäßig auf **My Azure Subscription** festgelegt. Ersetzen Sie den Abonnementnamen im Beispiel durch den Namen Ihres eigenen Abonnements.

```powershell
Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```


## <a name="5-migrate-your-cloud-services"></a>5) Migrieren Ihrer Cloud Services-Instanzen 
* [Migrieren eines Clouddiensts (nicht in einem virtuellen Netzwerk)](#51-option-1---migrate-a-cloud-service-not-in-a-virtual-network)
* [Migrieren eines Clouddiensts (in einem virtuellen Netzwerk)](#51-option-2---migrate-a-cloud-service-in-a-virtual-network)

> [!NOTE]
> Alle hier beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.


### <a name="51-option-1---migrate-a-cloud-service-not-in-a-virtual-network"></a>5.1) Option 1: Migrieren eines Clouddiensts (nicht in einem virtuellen Netzwerk)
Verwenden Sie den folgenden Befehl, um die Liste der Clouddienste abzurufen. Wählen Sie dann den Clouddienst aus, den Sie migrieren möchten.

```powershell
Get-AzureService | ft Servicename
```

Rufen Sie den Bereitstellungsnamen für den Clouddienst ab. In diesem Beispiel lautet der Dienstname **My Service**. Ersetzen Sie den Dienstnamen im Beispiel durch den Namen Ihres eigenen Diensts.

```powershell
$serviceName = "My Service"
$deployment = Get-AzureDeployment -ServiceName $serviceName
$deploymentName = $deployment.DeploymentName
```

Überprüfen Sie zuerst mit dem folgenden Befehl, ob Sie den Clouddienst migrieren können:

```powershell
$validate = Move-AzureService -Validate -ServiceName $serviceName -DeploymentName $deploymentName -CreateNewVirtualNetwork
$validate.ValidationMessages
 ```

Der folgende Befehl zeigt ggf. Warnungen und Fehler an, die die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit dem Vorbereitungsschritt fortfahren.

```powershell
Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName -CreateNewVirtualNetwork
```

### <a name="51-option-2---migrate-a-cloud-service-in-a-virtual-network"></a>5.1) Option 2: Migrieren eines Clouddiensts (in einem virtuellen Netzwerk)

Um einen Clouddienst in einem virtuellen Netzwerk zu migrieren, migrieren Sie das virtuelle Netzwerk. Der Clouddienst wird automatisch mit dem virtuellen Netzwerk migriert.

> [!NOTE]
> Der Name des virtuellen Netzwerks kann von dem Namen abweichen, der im neuen Portal angezeigt wird. Im neuen Azure-Portal wird der Name als `[vnet-name]` angezeigt, der tatsächliche Name des virtuellen Netzwerks ist jedoch vom Typ `Group [resource-group-name] [vnet-name]`. Suchen Sie vor dem Start der Migration anhand des Befehls `Get-AzureVnetSite | Select -Property Name` den tatsächlichen Namen des virtuellen Netzwerks, oder zeigen Sie ihn im alten Azure-Portal an. 

In diesem Beispiel wird der Name des virtuellen Netzwerks auf **myVnet** festgelegt. Ersetzen Sie den Namen des virtuellen Netzwerks im Beispiel durch den Namen Ihres eigenen virtuellen Netzwerks.

```powershell
$vnetName = "myVnet"
```

Überprüfen Sie zuerst mithilfe des folgenden Befehls, ob Sie das virtuelle Netzwerk migrieren können:

```powershell
Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Der folgende Befehl zeigt ggf. Warnungen und Fehler an, die die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit dem folgenden Vorbereitungsschritt fortfahren:

```powershell
Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Überprüfen Sie die Konfiguration für den vorbereiteten Clouddienst entweder mit Azure PowerShell oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

```powershell
Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

```powershell
Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den Abschnitt [Änderungen nach der Migration](in-place-migration-overview.md#post-migration-changes), um Änderungen in Bereitstellungsdateien, die Automatisierung und andere Attribute der neuen Cloud Services-Bereitstellung (erweiterter Support) anzuzeigen. 

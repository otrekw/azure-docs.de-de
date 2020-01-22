---
title: Bring-Your-Own-Subscription-Images in Azure für Red Hat Enterprise Linux | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Bring-Your-Own-Subscription-Images für Red Hat Enterprise Linux in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 1/14/2020
ms.author: alsin
ms.openlocfilehash: 911d86dd7cb03479d9bde49d8fce0f7861e32e27
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980136"
---
# <a name="red-hat-enterprise-linux-bring-your-own-subscription-gold-images-in-azure"></a>Bring-Your-Own-Subscription-Gold-Images für Red Hat Enterprise Linux in Azure
Red Hat Enterprise Linux (RHEL)-Images sind in Azure über nutzungsbasierte Bezahlung (Pay-As-You-Go, PAYG) oder ein Bring-Your-Own-Subscription (BYOS)-Modell (Red Hat Gold-Image) verfügbar. Dieses Dokument enthält eine Übersicht über die Red Hat Gold-Images in Azure.

## <a name="important-points-to-consider"></a>Wichtige zu berücksichtigende Punkte

- Die in diesem Programm bereitgestellten Red Hat Gold-Images sind produktionsbereite RHEL-Images, die den RHEL-PAYG-Images im Azure-Katalog oder Azure Marketplace ähneln.

- Die Images befolgen unsere aktuellen Richtlinien, die unter [Red Hat Enterprise Linux-Images in Azure](./redhat-images.md) beschrieben werden.

- Standard-Supportrichtlinien gelten für VMs, die aus diesen Images erstellt wurden.

- Die VMs, die von Red Hat Gold-Images bereitgestellt werden, enthalten keine RHEL-Gebühren, die mit RHEL-PAYG-Images verknüpft sind.

- Die Images sind nicht berechtigt, daher müssen Sie für die Registrierung und zum Abonnieren der VMs den Abonnement-Manager verwenden, um Updates direkt von Red Hat zu erhalten.

- Es ist derzeit nicht möglich, dynamisch zwischen BYOS- und PAYG-Abrechnungsmodellen für Linux-Images zu wechseln. Es ist erforderlich, den virtuellen Computer aus dem entsprechenden Image erneut bereitzustellen, um das Abrechnungsmodell zu wechseln.

## <a name="requirements-and-conditions-to-access-the-red-hat-gold-images"></a>Anforderungen und Bedingungen für den Zugriff auf die Red Hat Gold-Images

1. Machen Sie sich mit den Bestimmungen des [Red Hat Cloud Access-Programms](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) vertraut, und aktivieren Sie Ihre Red Hat-Abonnements im [Red Hat-Abonnement-Manager](https://access.redhat.com/management/cloud) für den Cloudzugriff. Sie müssen die Azure-Abonnements parat haben, die für den Cloudzugriff registriert werden.

1. Wenn Sie Red Hat-Abonnements für den Cloudzugriff aktiviert haben, die die entsprechenden Berechtigungsanforderungen erfüllen, wird Ihren Azure-Abonnements automatisch Zugriff auf Gold-Images ermöglicht.

### <a name="expected-time-for-image-access"></a>Erwartete Zeit für den Imagezugriff

Nachdem Sie die Schritte zum Aktivieren des Cloudzugriffs abgeschlossen haben, wird die Berechtigung für die Red Hat Gold-Images von Red Hat überprüft. Wenn die Überprüfung erfolgreich ist, können Sie innerhalb von drei Stunden auf die Gold-Images zugreifen.

## <a name="use-the-red-hat-gold-images-from-the-azure-portal"></a>Verwenden der Red Hat Gold-Images im Azure-Portal

1. Nachdem Ihr Azure-Abonnement Zugriff auf die Red Hat Gold-Images erhalten hat, können Sie sie im [Azure-Portal](https://portal.azure.com) suchen, indem Sie zu **Ressource erstellen** navigieren und dann **Alle anzeigen** auswählen.

1. Am oberen Rand der Seite sehen Sie, dass Sie über private Angebote verfügen.

    ![Private Angebote im Marketplace](./media/rhel-byos-privateoffers.png)

1. Sie können auf den violetten Link klicken oder bis zum Ende der Seite scrollen, um Ihre privaten Angebote anzuzeigen.

1. Die restliche Bereitstellung auf der Benutzeroberfläche unterscheidet sich nicht von anderen vorhandenen Red Hat-Images. Wählen Sie Ihre RHEL-Version aus, und befolgen Sie die Anweisungen zum Bereitstellen Ihrer VM. Mit diesem Prozess können Sie im letzten Schritt auch die Bedingungen für das Image akzeptieren.

>[!NOTE]
>Mit den bisherigen Schritten wird ihr Red Hat Gold-Image nicht für die programmgesteuerte Bereitstellung aktiviert – ein zusätzlicher Schritt ist erforderlich, wie im Abschnitt „Zusätzliche Informationen“ weiter unten beschrieben wird.

Der Rest dieses Dokuments konzentriert sich auf die CLI-Methode zum Bereitstellen und Akzeptieren von Bestimmungen für das Image. Die Benutzeroberfläche und die CLI sind vollkommen gleichwertig, was das Endergebnis (ein bereitgestellter virtueller Red Hat Gold-Computer) betrifft.

## <a name="use-the-red-hat-gold-images-from-the-azure-cli"></a>Verwenden der Red Hat Gold-Images in der Azure CLI
In den folgenden Anweisungen wird der erste Bereitstellungsprozess für einen virtuellen RHEL-Computer mithilfe der Azure CLI erläutert. In dieser Anleitung wird davon ausgegangen, dass die [Azure CLI installiert](https://docs.microsoft.com/cli/azure/install-azure-cli) ist.

>[!IMPORTANT]
>Stellen Sie sicher, dass Sie in den Verweisen zu Herausgeber, Angebot, Plan und Image für alle folgenden Befehle nur Kleinbuchstaben verwenden.

1. Überprüfen Sie, ob Sie sich im gewünschten Abonnement befinden:
    ```azurecli
    az account show -o=json
    ```

1. Erstellen Sie eine Ressourcengruppe für Ihren virtuellen Computer mit dem Red Hat Gold-Image:
    ```azurecli
    az group create --name <name> --location <location>
    ```

1. Akzeptieren Sie die Bestimmungen für das Image:
    ```azurecli
    az vm image terms accept --publisher redhat --offer rhel-byos --plan <SKU value here> -o=jsonc

    # Example:
    az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm75 -o=jsonc

    OR

    az vm image terms accept --urn RedHat:rhel-byos:rhel-lvm8:8.0.20190620
    ```
    >[!NOTE]
    >Diese Bestimmungen müssen *einmal pro Azure-Abonnement und pro Image-SKU* angenommen werden.

1. (Optional) Überprüfen Sie Ihre VM-Bereitstellung mit dem folgenden Befehl:
    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate

    # Example:
    az vm create -n rhel-byos-vm -g rhel-byos-group --image RedHat:rhel-byos:rhel-lvm75:7.5.20190620
    ```

1. Stellen Sie Ihre VM bereit, indem Sie den gleichen Befehl wie oben ausführen, aber ohne das `--validate`-Argument:
    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate
    ```

1. Stellen Sie eine SSH-Verbindung mit Ihrem virtuellen Computer her, und überprüfen Sie, ob Sie ein nicht berechtigtes Image haben. Führen Sie dazu `sudo yum repolist` aus (verwenden Sie für RHEL 8 `sudo dnf repolist`). In der Ausgabe werden Sie aufgefordert, den virtuellen Computer mit dem Abonnement-Manager bei Red Hat zu registrieren.

>[!NOTE]
>Unter RHEL 8 sind `dnf` und `yum` austauschbar. Weitere Informationen hierzu finden Sie im [RHEL 8 Admin Guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/installing-software-with-yum_configuring-basic-system-settings).


## <a name="use-the-red-hat-gold-images-from-powershell"></a>Verwenden der Red Hat Gold-Images in PowerShell
Das folgende Skript ist ein Beispiel. Sie müssen Ressourcengruppe, Speicherort, VM-Name, Anmeldeinformationen und andere Variablen der jeweiligen Konfiguration entsprechend ersetzen. Herausgeber und Planinformationen müssen in Kleinbuchstaben angegeben werden.

```powershell-interactive
    # Variables for common values
    $resourceGroup = "testbyos"
    $location = "canadaeast"
    $vmName = "test01"

    # Define user name and blank password
    $securePassword = ConvertTo-SecureString 'TestPassword1!' -AsPlainText -Force
    $cred = New-Object System.Management.Automation.PSCredential("azureuser",$securePassword)
    Get-AzureRmMarketplaceTerms -Publisher RedHat -Product rhel-byos -Name rhel-lvm75 | SetAzureRmMarketplaceTerms -Accept

    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroup -Location $location

    # Create a subnet configuration
    $subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

    # Create a virtual network
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location
    $location `-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

    # Create a public IP address and specify a DNS name
    $pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location
    $location `-Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

    # Create an inbound network security group rule for port 22
    $nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name
    myNetworkSecurityGroupRuleSSH -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -
    DestinationAddressPrefix * `-DestinationPortRange 22 -Access Allow

    # Create a network security group
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location
    $location `-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH

    # Create a virtual network card and associate with public IP address and NSG
    $nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -
    Location $location `-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

    # Create a virtual machine configuration
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D3_v2 |
    Set-AzureRmVMOperatingSystem -Linux -ComputerName $vmName -Credential $cred |
    Set-AzureRmVMSourceImage -PublisherName redhat -Offer rhel-byos -Skus rhel-lvm75 -Version latest | Add-     AzureRmVMNetworkInterface -Id $nic.Id
    Set-AzureRmVMPlan -VM $vmConfig -Publisher redhat -Product rhel-byos -Name "rhel-lvm75"

    # Configure SSH Keys
    $sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
    Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

    # Create a virtual machine
    New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

## <a name="additional-information"></a>Zusätzliche Informationen
- Wenn Sie versuchen, einen virtuellen Computer in einem Abonnement bereitzustellen, das nicht für dieses Angebot aktiviert ist, erhalten Sie die folgende Fehlermeldung. Wenden Sie sich an Microsoft oder Red Hat, um Ihr Abonnement zu aktivieren.
    ```
    "Offer with PublisherId: redhat, OfferId: rhel-byos, PlanId: rhel-lvm75 is private and can not be purchased by subscriptionId: GUID"
    ```

- Wenn Sie eine Momentaufnahme aus dem RHEL-BYOS-Image erstellen und das Image in [Shared Image Gallery](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries) veröffentlichen, müssen Sie Planinformationen bereitstellen, die mit der ursprünglichen Quelle der Momentaufnahme übereinstimmen. Der Befehl könnte z. B. wie folgt aussehen (beachten Sie die Planparameter in der letzten Zeile):
    ```azurecli
    az vm create –image \
    "/subscriptions/GUID/resourceGroups/GroupName/providers/Microsoft.Compute/galleries/GalleryName/images/ImageName/versions/1.0.0" \
    -g AnotherGroupName --location EastUS2 -n VMName \
    --plan-publisher redhat --plan-product rhel-byos --plan-name rhel-lvm75
    ```

- Wenn Sie VMs mithilfe der Automatisierung aus den RHEL-BYOS-Images bereitstellen, müssen Sie Planparameter angeben, die den oben gezeigten ähneln. Wenn Sie z. B. Terraform verwenden, würden Sie die Planinformationen in einem [plan-Block](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine.html#plan) bereitstellen.

## <a name="next-steps"></a>Nächste Schritte
* Schritt-für-Schritt-Anleitungen und Programmdetails für den Cloudzugriff finden Sie in der [Red Hat-Dokumentation für den Cloudzugriff](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/index).
* Erfahren Sie mehr über die [Azure Red Hat-Updateinfrastruktur](./redhat-rhui.md).
* Weitere Informationen zu allen Red Hat-Images in Azure finden Sie auf der [Dokumentationsseite](./redhat-images.md).
* Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).
* Weitere Dokumentation zu den RHEL Gold-Images finden Sie in der [Red Hat-Dokumentation](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/using_red_hat_gold_images#con-gold-image-azure).

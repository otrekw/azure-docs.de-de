---
title: 'Schnellstart: Erstellen einer Data Science VM: Resource Manager-Vorlage'
titleSuffix: Azure Data Science Virtual Machine
description: In dieser Schnellstartanleitung verwenden Sie eine Azure Resource Manager-Vorlage, um in kurzer Zeit eine Data Science Virtual Machine bereitzustellen.
services: machine-learning
author: lobrien
ms.author: laobri
ms.custom: subject-armqs
ms.date: 06/10/2020
ms.service: machine-learning
ms.subservice: data-science-vm
ms.topic: quickstart
ms.openlocfilehash: 7e3e45d96839be06deec238deccdc9873492d68f
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84660248"
---
# <a name="quickstart-create-an-ubuntu-data-science-virtual-machine-using-a-resource-manager-template"></a>Schnellstart: Erstellen einer Data Science Virtual Machine für Ubuntu mit einer Resource Manager-Vorlage
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In dieser Schnellstartanleitung wird veranschaulicht, wie Sie mit einer Azure Resource Manager-Vorlage eine Data Science Virtual Machine für Ubuntu 18.04 erstellen. Bei Data Science Virtual Machines handelt es sich um cloudbasierte virtuelle Computer, auf denen bereits eine Suite mit Data Science- und Machine Learning-Frameworks und -Tools geladen ist. Bei der Bereitstellung auf GPU-basierten Computeressourcen werden alle Tools und Bibliotheken für die Nutzung der GPU konfiguriert. 

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/AMLFree), bevor Sie beginnen

* Um die CLI-Befehle in diesem Dokument in Ihrer **lokalen Umgebung** zu verwenden, benötigen Sie die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-vm-ubuntu-DSVM-GPU-or-CPU/). Die vollständige Vorlage für diesen Artikel ist zu lang und kann hier nicht angezeigt werden. Unter [azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-ubuntu-DSVM-GPU-or-CPU/azuredeploy.json) finden Sie die vollständige Vorlage. Der Teil mit den spezifischen Angaben für die DSVM ist hier angegeben:

:::code language="json" source="~/quickstart-templates/101-vm-ubuntu-DSVM-GPU-or-CPU/azuredeploy.json" range="235-276":::

Die folgenden Ressourcen sind in der Vorlage definiert:

* [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines): Dient zum Erstellen eines cloudbasierten virtuellen Computers. In dieser Vorlage wird der virtuelle Computer als Data Science Virtual Machine konfiguriert, auf der Ubuntu 18.04 ausgeführt wird.

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage 

Um die Vorlage an der Azure-Befehlszeilenschnittstelle zu verwenden, melden Sie sich an, und wählen Sie Ihr Abonnement aus (Siehe dazu [Anmelden bei Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)). Führen Sie dann Folgendes aus:

```azurecli-interactive
read -p "Enter the name of the resource group to create:" resourceGroupName &&
read -p "Enter the Azure location (e.g., centralus):" location &&
read -p "Enter the authentication type (must be 'password' or 'sshPublicKey') :" authenticationType &&
read -p "Enter the login name for the administrator account (may not be 'admin'):" adminUsername &&
read -p "Enter administrator account secure string (value of password or ssh public key):" adminPasswordOrKey &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-ubuntu-DSVM-GPU-or-CPU/azuredeploy.json" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri $templateUri --parameters adminUsername=$adminUsername authenticationType=$authenticationType adminPasswordOrKey=$adminPasswordOrKey && 
echo "Press [ENTER] to continue ..." &&
read
```

Wenn Sie den Befehl oben ausführen, geben Sie dies ein:

1. Den Namen der Ressourcengruppe, die Sie erstellen möchten und die die DSVM und die zugehörigen Ressourcen enthalten soll. 
1. Den Azure-Standort, an dem die Bereitstellung erfolgen soll
1. Den Authentifizierungstyp, den Sie verwenden möchten (geben Sie die Zeichenfolge `password` oder `sshPublicKey` ein).
1. Die Anmelde-ID des Administratorkontos (Wert darf nicht `admin` lauten).
1. Den Wert für das Kennwort oder den öffentlichen SSH-Schlüssel des Kontos.

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Zeigen Sie Ihre Data Science Virtual Machine wie folgt an:

1. Besuchen Sie https://portal.azure.com. 
1. Anmelden 
1. Wählen Sie die Ressourcengruppe aus, die Sie soeben erstellt haben.

Die Informationen zur Ressourcengruppe werden angezeigt: 

:::image type="content" source="media/dsvm-tutorial-resource-manager/resource-group-home.png" alt-text="Screenshot: Grundlegende Ressourcengruppe mit einer DSVM":::

Klicken Sie auf die Virtual Machine-Ressource, um auf die Seite mit den zugehörigen Informationen zuzugreifen. Sie enthält Informationen zur VM, z. B. Verbindungsdetails. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie diesen virtuellen Computer, falls Sie ihn nicht nutzen möchten. Da die DSVM noch weiteren Ressourcen zugeordnet ist, z. B. einem Speicherkonto, kann es ratsam sein, die gesamte erstellte Ressourcengruppe zu löschen. Sie können die Ressourcengruppe im Portal löschen, indem Sie auf die Schaltfläche „Löschen“ klicken und den Befehl bestätigen. Alternativ können Sie die Ressourcengruppe an der Befehlszeilenschnittstelle löschen, mit diesem Befehl: 

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mit einer Azure Resource Manager-Vorlage eine Data Science Virtual Machine erstellt. 

> [!div class="nextstepaction"]
> [Beispielprogramme und exemplarische Vorgehensweisen für ML](dsvm-samples-and-walkthroughs.md)

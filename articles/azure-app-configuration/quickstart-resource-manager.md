---
title: 'Schnellstart: Automatisierte VM-Bereitstellung mit Azure App Configuration'
description: In dieser Schnellstartanleitung wird veranschaulicht, wie Sie das Azure PowerShell-Modul und Azure Resource Manager-Vorlagen verwenden, um einen Azure App Configuration-Speicher bereitzustellen. Verwenden Sie die Werte im Speicher, um eine VM bereitzustellen.
author: lisaguthrie
ms.author: lcozzens
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-app-configuration
ms.custom:
- mvc
- subject-armqs
ms.openlocfilehash: 9b609d4571d6240f428a0210aa5108ff19dc753b
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88235178"
---
# <a name="quickstart-automated-vm-deployment-with-app-configuration-and-resource-manager-template-arm-template"></a>Schnellstart: Automatisierte VM-Bereitstellung mit App Configuration und einer Resource Manager-Vorlage (ARM-Vorlage)

Hier erfahren Sie, wie Sie mit Azure Resource Manager-Vorlagen und Azure PowerShell einen Azure App Configuration-Speicher bereitstellen, dem Speicher Schlüsselwerte hinzufügen und mithilfe der Schlüsselwerte im Speicher eine Azure-Ressource bereitstellen (in diesem Beispiel eine Azure-VM).

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="review-the-templates"></a>Überprüfen der Vorlagen

Die in diesem Schnellstart verwendeten Vorlagen stammen aus den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/). Die [erste Vorlage](https://azure.microsoft.comresources/templates/101-app-configuration-store/) erstellt einen App Configuration-Speicher:

:::code language="json" source="~/quickstart-templates/101-app-configuration-store/azuredeploy.json" range="1-37" highlight="27-35":::

In der Vorlage ist eine einzelne Azure-Ressource definiert:

- [Microsoft.AppConfiguration/configurationStores](/azure/templates/microsoft.appconfiguration/2019-10-01/configurationstores): Erstellen eines App Configuration-Speichers

Die [zweite Vorlage](https://azure.microsoft.com/resources/templates/101-app-configuration/) erstellt anhand der Schlüsselwerte im Speicher eine VM. Vor diesem Schritt müssen Sie über das Portal oder mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI) Schlüsselwerte hinzufügen.

:::code language="json" source="~/quickstart-templates/101-app-configuration/azuredeploy.json" range="1-217" highlight="77, 181,189":::

## <a name="deploy-the-templates"></a>Bereitstellen der Vorlagen

### <a name="create-an-app-configuration-store"></a>Erstellen eines App Configuration-Speichers

1. Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen. Die Vorlage erstellt einen App Configuration-Speicher.

    [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store%2Fazuredeploy.json)

1. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.

    - **Abonnement**: Wählen Sie ein Azure-Abonnement zum Erstellen des App Configuration-Speichers aus.
    - **Ressourcengruppe**: Klicken Sie auf **Neu erstellen**, um eine neue Ressourcengruppe zu erstellen, oder verwenden Sie eine vorhandene Ressourcengruppe.
    - **Region**: Wählen Sie einen Standort für die Ressourcengruppe aus.  Beispiel: **USA, Osten**
    - **Config Store Name** (Name des App Configuration-Speichers): Geben Sie einen neuen Namen für den App Configuration-Speicher ein.
    - **Speicherort**: Geben Sie den Speicherort des App Configuration-Speichers an.  Verwenden Sie den Standardwert.
    - **SKU-Name**: Geben Sie den SKU-Namen des App Configuration-Speichers an. Verwenden Sie den Standardwert.

1. Klicken Sie auf **Überprüfen + erstellen**.
1. Vergewissern Sie sich, dass **Überprüfung erfolgreich** auf der Seite angezeigt wird, und wählen Sie dann **Erstellen** aus.

Notieren Sie sich den Namen der Ressourcengruppe und den Namen des App Configuration-Speichers.  Sie benötigen diese Werte beim Bereitstellen der VM.
### <a name="add-vm-configuration-key-values"></a>Hinzufügen von Schlüsselwerten für die VM-Konfiguration

Nachdem Sie einen App Configuration-Speicher erstellt haben, können Sie ihm über das Azure-Portal oder mithilfe der Azure CLI Schlüsselwerte hinzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wechseln Sie dann zum neu erstellten App Configuration-Speicher.
1. Wählen Sie im Menü auf der linken Seite die Option **Konfigurations-Explorer** aus.
1. Wählen Sie **Erstellen** aus, um die folgenden Schlüssel-Wert-Paare hinzuzufügen:

   |Schlüssel|Wert|Bezeichnung|
   |-|-|-|
   |windowsOsVersion|2019-Datacenter|Vorlage|
   |diskSizeGB|1023|Vorlage|

   Lassen Sie das Feld **Inhaltstyp** leer.

Informationen zur Verwendung der Azure CLI finden Sie unter [Verwenden von Schlüssel-Wert-Paaren in einem Azure App Configuration-Speicher](./scripts/cli-work-with-keys.md).

### <a name="deploy-vm-using-stored-key-values"></a>Bereitstellen eines virtuellen Computers mit gespeicherten Schlüsselwerten

Nachdem Sie dem Speicher Schlüsselwerte hinzugefügt haben, können Sie mit einer Azure Resource Manager-Vorlage einen virtuellen Computer bereitstellen. In der Vorlage wird auf die von Ihnen erstellten Schlüssel **windowsOsVersion** und **diskSizeGB** verwiesen.

> [!WARNING]
> ARM-Vorlagen können nicht auf Schlüssel in einem App Configuration-Speicher verweisen, für den Private Link aktiviert ist.

1. Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen. Die Vorlage erstellt anhand gespeicherter Schlüsselwerte im App Configuration-Speicher eine VM.

    [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration%2Fazuredeploy.json)

1. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.

    - **Abonnement**: Wählen Sie das Azure-Abonnement zum Erstellen der VM aus.
    - **Ressourcengruppe**: Geben Sie entweder die gleiche Ressourcengruppe wie für den App Configuration-Speicher an, oder wählen Sie **Neu erstellen** aus, um eine neue Ressourcengruppe zu erstellen.
    - **Region**: Wählen Sie einen Standort für die Ressourcengruppe aus.  Beispiel: **USA, Osten**
    - **Standort**: Geben Sie den Standort der VM an. Verwenden Sie den Standardwert.
    - **Administratorbenutzername**: Geben Sie einen Administratorbenutzernamen für die VM an.
    - **Administratorkennwort**: Geben Sie ein Administratorkennwort für die VM an.
    - **Domänennamenbezeichnung**: Geben Sie einen eindeutigen Domänennamen an.
    - **Name des Speicherkontos**: Geben Sie einen eindeutigen Namen für ein Speicherkonto an, das der VM zugeordnet ist.
    - **App Config Store Resource Group** (Ressourcengruppe des App Configuration-Speichers): Geben Sie die Ressourcengruppe an, in der Ihr App Configuration-Speicher enthalten ist.
    - **App Config Store Name** (Name des App Configuration-Speichers): Geben Sie den Namen Ihres Azure App Configuration-Speichers an.
    - **VM Sku Key** (Schlüssel für die VM-SKU): Geben Sie **windowsOsVersion** an.  Dies ist der Schlüsselwertname, den Sie dem Speicher hinzugefügt haben.
    - **Disk Size Key** (Schlüssel für die Datenträgergröße): Geben Sie **diskSizeGB** an. Dies ist der Schlüsselwertname, den Sie dem Speicher hinzugefügt haben.

1. Klicken Sie auf **Überprüfen + erstellen**.
1. Vergewissern Sie sich, dass **Überprüfung erfolgreich** auf der Seite angezeigt wird, und wählen Sie dann **Erstellen** aus.

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wechseln Sie dann zur neu erstellten VM.
1. Wählen Sie im Menü auf der linken Seite **Übersicht** aus, und überprüfen Sie, ob **2019-Datacenter** als **SKU** angezeigt wird.
1. Wählen Sie im Menü auf der linken Seite **Datenträger** aus, und überprüfen Sie, ob die Größe des Datenträgers **2013** ist.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den App Configuration-Speicher, den virtuellen Computer und alle zugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Falls Sie planen, den App Configuration-Speicher oder den virtuellen Computer weiter zu nutzen, können Sie das Löschen überspringen. Wenn Sie diesen Auftrag nicht weiter verwenden möchten, löschen Sie alle in diesem Schnellstart erstellten Ressourcen, indem Sie das folgende Cmdlet ausführen:

```azurepowershell-interactive
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen virtuellen Computer bereitgestellt, indem Sie eine Azure Resource Manager-Vorlage und Schlüsselwerte aus Azure App Configuration verwendet haben.

Fahren Sie mit dem folgenden Artikel fort, um sich über die Erstellung anderer Anwendungen mit Azure App Configuration zu informieren:

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen einer ASP.NET Core-App mit Azure App Configuration](quickstart-aspnet-core-app.md)

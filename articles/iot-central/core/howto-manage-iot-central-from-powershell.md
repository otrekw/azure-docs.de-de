---
title: Verwalten von IoT Central über Azure PowerShell | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre IoT Central-Anwendungen über Azure PowerShell erstellen und verwalten.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/27/2020
ms.topic: how-to
ms.custom: devx-track-azurepowershell
manager: philmea
ms.openlocfilehash: 832147fd06dbc7439500b911efbbde6d9b1d81fd
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92123061"
---
# <a name="manage-iot-central-from-azure-powershell"></a>Verwalten von IoT Central über Azure PowerShell

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Statt IoT Central-Anwendungen auf der Website Seite [IoT Central-Anwendungs-Manager](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie [Azure PowerShell](/powershell/azure/) verwenden, um Ihre Anwendungen zu verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie Azure PowerShell auf Ihrem lokalen Computer ausführen möchten, lesen Sie [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps). Wenn Sie Azure PowerShell lokal ausführen, verwenden Sie das Cmdlet **Connect-AzAccount**, um sich bei Azure anzumelden, bevor Sie die Cmdlets verwenden, die in diesem Artikel aufgeführt sind.

> [!TIP]
> Wenn Sie Ihre PowerShell-Befehle in einem anderen Azure-Abonnement ausführen müssen, finden Sie weitere Informationen unter [Ändern des aktiven Abonnements](/powershell/azure/manage-subscriptions-azureps?view=azps-3.4.0#change-the-active-subscription).

## <a name="install-the-iot-central-module"></a>Installieren des IoT Central-Moduls

Führen Sie den folgenden Befehl aus, um zu überprüfen, ob das [IoT Central-Modul](/powershell/module/az.iotcentral/) in Ihre PowerShell-Umgebung installiert ist:

```powershell
Get-InstalledModule -name Az.I*
```

Wenn die Liste der installierten Module den Eintrag **Az.IotCentral** nicht enthält, führen Sie den folgenden Befehl aus:

```powershell
Install-Module Az.IotCentral
```

## <a name="create-an-application"></a>Erstellen einer Anwendung

Verwenden Sie das Cmdlet [New-AzIotCentralApp](/powershell/module/az.iotcentral/New-AzIotCentralApp), um eine IoT Central-Anwendung in Ihrem Azure-Abonnement zu erstellen. Beispiel:

```powershell
# Create a resource group for the IoT Central application
New-AzResourceGroup -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Location "East US"
```

```powershell
# Create an IoT Central application
New-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Name "myiotcentralapp" -Subdomain "mysubdomain" `
  -Sku "ST1" -Template "iotc-pnp-preview" `
  -DisplayName "My Custom Display Name"
```

Das Skript erstellt zuerst für die Anwendung eine Ressourcengruppe in der Region „East US“ (USA, Osten). In der folgenden Tabelle sind die Parameter beschrieben, die für den **New-AzIotCentralApp**-Befehl verwendet werden:

|Parameter         |BESCHREIBUNG |
|------------------|------------|
|ResourceGroupName |Die Ressourcengruppe, die die Anwendung enthält. Diese Ressourcengruppe muss bereits in Ihrem Abonnement vorhanden sein. |
|Position |Standardmäßig wird in diesem Cmdlet der Standort aus der Ressourcengruppe verwendet. Sie können eine IoT Central-Anwendung derzeit in den geografischen Regionen **Australien**, **Asien, Osten**, **Europa**, **USA**, **Vereinigtes Königreich** und **Japan** erstellen. |
|Name              |Der Name der Anwendung im Azure-Portal. |
|Unterdomäne         |Die Unterdomäne in der URL der Anwendung. Im Beispiel lautet die Anwendungs-URL `https://mysubdomain.azureiotcentral.com`. |
|Sku               |Derzeit können Sie **ST1** oder **ST2** verwenden. Weitere Informationen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/). |
|Vorlage          | Die zu verwendende Anwendungsvorlage. Ausführlichere Informationen finden Sie in der unten stehenden Tabelle. |
|DisplayName       |Der Name der Anwendung, wie er in der Benutzeroberfläche angezeigt wird. |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-iot-central-applications"></a>Anzeigen Ihrer IoT Central-Anwendungen

Verwenden Sie das Cmdlet [Get-AzIotCentralApp](/powershell/module/az.iotcentral/Get-AzIotCentralApp), um Ihre IoT Central-Anwendungen aufzulisten und die Metadaten anzuzeigen.

## <a name="modify-an-application"></a>Ändern einer Anwendung

Verwenden Sie das Cmdlet [Set-AzIotCentralApp](/powershell/module/az.iotcentral/set-aziotcentralapp), um die Metadaten einer IoT Central-Anwendung zu aktualisieren. Den Anzeigenamen Ihrer Anwendung können Sie beispielsweise wie folgt ändern:

```powershell
Set-AzIotCentralApp -Name "myiotcentralapp" `
  -ResourceGroupName "MyIoTCentralResourceGroup" `
  -DisplayName "My new display name"
```

## <a name="remove-an-application"></a>Entfernen einer Anwendung

Verwenden Sie das Cmdlet [Remove-AzIotCentralApp](/powershell/module/az.iotcentral/Remove-AzIotCentralApp), um eine IoT Central-Anwendung zu löschen. Beispiel:

```powershell
Remove-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
 -Name "myiotcentralapp"
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über Azure PowerShell verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)
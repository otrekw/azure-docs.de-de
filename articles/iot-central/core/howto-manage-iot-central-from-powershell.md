---
title: Verwalten von IoT Central über Azure PowerShell | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre IoT Central-Anwendungen über Azure PowerShell erstellen und verwalten.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 07/11/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: c731dae02e9013fc436d6f30d8c8b2ab384968a0
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453988"
---
# <a name="manage-iot-central-from-azure-powershell"></a>Verwalten von IoT Central über Azure PowerShell

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Statt IoT Central-Anwendungen auf der Website Seite [IoT Central-Anwendungs-Manager](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) verwenden, um Ihre Anwendungen zu verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie Azure PowerShell auf Ihrem lokalen Computer ausführen möchten, lesen Sie [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps). Wenn Sie Azure PowerShell lokal ausführen, verwenden Sie das Cmdlet **Connect-AzAccount**, um sich bei Azure anzumelden, bevor Sie die Cmdlets verwenden, die in diesem Artikel aufgeführt sind.

## <a name="install-the-iot-central-module"></a>Installieren des IoT Central-Moduls

Führen Sie den folgenden Befehl aus, um zu überprüfen, ob das [IoT Central-Modul](https://docs.microsoft.com/powershell/module/az.iotcentral/) in Ihre PowerShell-Umgebung installiert ist:

```powershell
Get-InstalledModule -name Az.I*
```

Wenn die Liste der installierten Module den Eintrag **Az.IotCentral** nicht enthält, führen Sie den folgenden Befehl aus:

```powershell
Install-Module Az.IotCentral
```

## <a name="create-an-application"></a>Erstellen einer Anwendung

Verwenden Sie das Cmdlet [New-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/New-AzIotCentralApp), um eine IoT Central-Anwendung in Ihrem Azure-Abonnement zu erstellen. Beispiel:

```powershell
# Create a resource group for the IoT Central application
New-AzResourceGroup -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Location "East US"
```

```powershell
# Create an IoT Central application
New-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Name "myiotcentralapp" -Subdomain "mysubdomain" `
  -Sku "S1" -Template "iotc-demo@1.0.0" `
  -DisplayName "My Custom Display Name"
```

Das Skript erstellt zuerst für die Anwendung eine Ressourcengruppe am Standort „USA, Osten“. In der folgenden Tabelle sind die Parameter beschrieben, die für den **New-AzIotCentralApp**-Befehl verwendet werden:

|Parameter         |BESCHREIBUNG |
|------------------|------------|
|ResourceGroupName |Die Ressourcengruppe, die die Anwendung enthält. Diese Ressourcengruppe muss bereits in Ihrem Abonnement vorhanden sein. |
|Location |Standardmäßig wird in diesem Cmdlet der Standort aus der Ressourcengruppe verwendet. Sie können derzeit eine IoT Central-Anwendung an den Standorten **USA**, **Australien**, **Asien, Osten** oder **Europa** erstellen.  |
|Name              |Der Name der Anwendung im Azure-Portal. |
|Unterdomäne         |Die Unterdomäne in der URL der Anwendung. Im Beispiel lautet die Anwendungs-URL https://mysubdomain.azureiotcentral.com. |
|Sku               |Zurzeit ist **S1** (Standard-Tarif) der einzige Wert. Weitere Informationen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/). |
|Vorlage          | Die zu verwendende Anwendungsvorlage. Weitere Informationen finden Sie in der Tabelle unten: |
|DisplayName       |Der Name der Anwendung, wie er in der Benutzeroberfläche angezeigt wird. |

**Anwendungsvorlage mit allgemein verfügbaren Features**

| Vorlagenname            | BESCHREIBUNG |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | Erstellt eine leere Anwendung, die Sie mit Ihren eigenen Gerätevorlagen und Geräten füllen können.


**Anwendungsvorlagen mit öffentlichen Previewfunktionen**

| Vorlagenname            | BESCHREIBUNG |
| ------------------------ | ----------- |
| iotc-pnp-preview@1.0.0   | Mit dieser Vorlage wird eine leere Plug & Play-Vorschauanwendung erstellt, die Sie mit Ihren eigenen Gerätevorlagen und Geräten füllen können. |
| iotc-condition@1.0.0     | Mit dieser Vorlage wird eine Anwendung mit einer In-Store-Analyse erstellt (Bedingungsüberwachungsvorlage). Verwenden Sie diese Vorlage, um eine Verbindung mit der Speicherumgebung herzustellen und sie zu überwachen. |
| iotc-consumption@1.0.0   | Mit dieser Vorlage wird eine Anwendung mit einer Vorlage zur Überwachung des Wasserverbrauchs erstellt. Verwenden Sie diese Vorlage, um den Wasserfluss zu überwachen und zu steuern. |
| iotc-distribution@1.0.0  | Mit dieser Vorlage wird eine Anwendung mit Vorlage für die digitale Verteilung erstellt. Verwenden Sie diese Vorlage, um die Effizienz des Warenausgangs aus dem Lager durch die Digitalisierung wichtiger Ressourcen und Aktionen zu verbessern. |
| iotc-inventory@1.0.0     | Mit dieser Vorlage wird eine Anwendung mit einer Vorlage für die intelligente Bestandsverwaltung erstellt. Verwenden Sie diese Vorlage, um Empfang, Produktbewegung, Zykluszählung und Nachverfolgung von Sensoren zu automatisieren. |
| iotc-logistics@1.0.0     | Mit dieser Vorlage wird eine Anwendung mit einer Vorlage für vernetzte Logistik erstellt. Verwenden Sie diese Vorlage, um Ihre Sendung auf dem Luft-, Wasser- oder Landweg mit Standort- und Zustandsüberwachung in Echtzeit zu verfolgen. |
| iotc-meter@1.0.0         | Mit dieser Vorlage wird eine Anwendung mit einer Vorlage für die intelligente Verbrauchseinheitenüberwachung erstellt. Verwenden Sie diese Vorlage zum Überwachen des Energieverbrauchs und des Netzwerkstatus, und identifizieren Sie Trends, um den Kundensupport und Verwaltungsvorgänge für intelligente Verbrauchseinheiten zu verbessern.  |
| iotc-patient@1.0.0       | Mit dieser Vorlage wird eine Anwendung mit einer Vorlage für die fortlaufende Patientenüberwachung erstellt. Verwenden Sie diese Vorlage, um die Patientenversorgung zu erweitern, Wiedereinweisungen zu vermeiden und Krankheiten zu behandeln. |
| iotc-power@1.0.0         | Mit dieser Vorlage wird eine Anwendung mit einer Vorlage für die Überwachung von Solarpaneln erstellt. Verwenden Sie diese Vorlage, um den Status von Solarpaneln zu überwachen und Trends bei der Energieerzeugung zu erkennen. |
| iotc-quality@1.0.0       | Mit dieser Vorlage wird eine Anwendung mit einer Vorlage zur Überwachung der Wasserqualität erstellt. Verwenden Sie diese Vorlage, um die Wasserqualität digital zu überwachen.|
| iotc-store@1.0.0         | Mit dieser Vorlage wird eine Anwendung mit einer In-Store-Analyse erstellt (Auftragsabschlussvorlage). Verwenden Sie diese Vorlage, um den Auftragsabschlussflow in Ihrem Unternehmen zu überwachen und zu verwalten. |
| iotc-waste@1.0.0         | Mit dieser Vorlage wird eine Anwendung mit einer Vorlage für eine vernetzte Abfallwirtschaft erstellt. Verwenden Sie diese Vorlage, um Müllcontainer zu überwachen und Mitarbeiter für deren Leerung rechtzeitig loszuschicken. |

> [!NOTE]
> Die Vorlagen für Vorschauanwendungen sind derzeit nur an den Standorten **Europa** und **USA** verfügbar.

## <a name="view-your-iot-central-applications"></a>Anzeigen Ihrer IoT Central-Anwendungen

Verwenden Sie das Cmdlet [Get-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Get-AzIotCentralApp), um Ihre IoT Central-Anwendungen aufzulisten und die Metadaten anzuzeigen.

## <a name="modify-an-application"></a>Ändern einer Anwendung

Verwenden Sie das Cmdlet [Set-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/set-aziotcentralapp), um die Metadaten einer IoT Central-Anwendung zu aktualisieren. Den Anzeigenamen Ihrer Anwendung können Sie beispielsweise wie folgt ändern:

```powershell
Set-AzIotCentralApp -Name "myiotcentralapp" `
  -ResourceGroupName "MyIoTCentralResourceGroup" `
  -DisplayName "My new display name"
```

## <a name="remove-an-application"></a>Entfernen einer Anwendung

Verwenden Sie das Cmdlet [Remove-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Remove-AzIotCentralApp), um eine IoT Central-Anwendung zu löschen. Beispiel:

```powershell
Remove-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
 -Name "myiotcentralapp"
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über Azure PowerShell verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)

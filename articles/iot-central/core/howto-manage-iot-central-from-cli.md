---
title: Verwalten von IoT Central über Azure CLI | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre IoT Central-Anwendung über die Befehlszeilenschnittstelle erstellen und verwalten. Sie können die Anwendung über die Befehlszeilenschnittstelle anzeigen, ändern und entfernen.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/27/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: df24a2dc6e9bd058a2f8b1355b8760653ed3128a
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365520"
---
# <a name="manage-iot-central-from-azure-cli"></a>Verwalten von IoT Central über Azure CLI

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Statt IoT Central-Anwendungen auf der Website Seite [IoT Central-Anwendungs-Manager](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie [Azure CLI](/cli/azure/) verwenden, um Ihre Anwendungen zu verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie Azure CLI auf Ihrem lokalen Computer ausführen möchten, lesen Sie [Installieren der Azure CLI](/cli/azure/install-azure-cli). Wenn Sie die Azure CLI lokal ausführen, verwenden Sie den Befehl **az login**, um sich bei Azure anzumelden, bevor Sie die Befehle verwenden, die in diesem Artikel aufgeführt sind.

> [!TIP]
> Wenn Sie Ihre CLI-Befehle in einem anderen Azure-Abonnement ausführen müssen, finden Sie weitere Informationen unter [Ändern des aktiven Abonnements](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#change-the-active-subscription).

## <a name="install-the-extension"></a>Installieren der Erweiterung

Die Befehle in diesem Artikel gehören zur **azure-iot**-CLI-Erweiterung. Führen Sie zum Installieren der Erweiterung den folgenden Befehl aus:

```azurecli-interactive
az extension add --name azure-iot
```

## <a name="create-an-application"></a>Erstellen einer Anwendung

Verwenden Sie den Befehl [az iotcentral app create](/cli/azure/iotcentral/app#az-iotcentral-app-create), um eine IoT Central-Anwendung in Ihrem Azure-Abonnement zu erstellen. Beispiel:

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iotcentral app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku ST1 --template "iotc-pnp-preview@1.0.0" \
  --display-name "My Custom Display Name"
```

Diese Befehle erstellen zuerst für die Anwendung eine Ressourcengruppe in der Region „East US“ (USA, Osten). In der folgenden Tabelle sind die Parameter beschrieben, die für den **az iotcentral app create**-Befehl verwendet werden:

| Parameter         | BESCHREIBUNG |
| ----------------- | ----------- |
| resource-group    | Die Ressourcengruppe, die die Anwendung enthält. Diese Ressourcengruppe muss bereits in Ihrem Abonnement vorhanden sein. |
| location          | Standardmäßig wird in diesem Befehl der Standort aus der Ressourcengruppe verwendet. Sie können eine IoT Central-Anwendung derzeit in den geografischen Regionen **Australien**, **Asien, Osten**, **Europa**, **USA**, **Vereinigtes Königreich** und **Japan** erstellen. |
| name              | Der Name der Anwendung im Azure-Portal. |
| subdomain         | Die Unterdomäne in der URL der Anwendung. Im Beispiel lautet die Anwendungs-URL `https://mysubdomain.azureiotcentral.com`. |
| sku               | Derzeit können Sie **ST1** oder **ST2** verwenden. Weitere Informationen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/). |
| Vorlage          | Die zu verwendende Anwendungsvorlage. Ausführlichere Informationen finden Sie in der unten stehenden Tabelle. |
| display-name      | Der Name der Anwendung, wie er in der Benutzeroberfläche angezeigt wird. |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-applications"></a>Anzeigen Ihrer Anwendungen

Verwenden Sie den Befehl [az iotcentral app list](/cli/azure/iotcentral/app#az-iotcentral-app-list), um Ihre IoT Central-Anwendungen aufzulisten und die Metadaten anzuzeigen.

## <a name="modify-an-application"></a>Ändern einer Anwendung

Verwenden Sie den Befehl [az iotcentral app update](/cli/azure/iotcentral/app#az-iotcentral-app-update), um die Metadaten einer IoT Central-Anwendung zu aktualisieren. Den Anzeigenamen Ihrer Anwendung können Sie beispielsweise wie folgt ändern:

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>Entfernen einer Anwendung

Verwenden Sie den Befehl [az iotcentral app delete](/cli/azure/iotcentral/app#az-iotcentral-app-delete), um eine IoT Central-Anwendung zu löschen. Beispiel:

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über die Azure CLI verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)

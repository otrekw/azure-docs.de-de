---
title: 'Schnellstart: Einrichten von Hochverfügbarkeit mit Azure Front Door: Azure CLI'
description: Dieser Schnellstart zeigt Ihnen, wie Sie mit Azure Front Door eine hochverfügbare und leistungsstarke globale Webanwendung mit der Azure CLI erstellen können.
services: front-door
author: duongau
manager: KumudD
ms.assetid: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/19/2021
ms.author: duau
ms.openlocfilehash: 3567d5af31b0c7bc2443e3d02426a5bb7aba06f7
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107862001"
---
# <a name="quickstart-create-a-front-door-for-a-highly-available-global-web-application-using-azure-cli"></a>Schnellstart: Erstellen einer hochverfügbaren globalen Front Door-Webanwendung mit der Azure CLI

Führen Sie erste Schritte mit Azure Front Door aus, indem Sie die Azure CLI verwenden, um eine hochverfügbare und leistungsstarke globale Webanwendung zu erstellen.

Front Door leitet Webdatenverkehr an bestimmte Ressourcen in einem Back-End-Pool weiter. Sie haben die Front-End-Domäne definiert, einem Back-End-Pool Ressourcen hinzugefügt und eine Routingregel erstellt. In diesem Artikel wird eine einfache Konfiguration eines Back-End-Pools mit zwei Web-App-Ressourcen und einer einzelnen Routingregel mit einem Standardpfad verwendet, der „/*“ entspricht.

:::image type="content" source="media/quickstart-create-front-door/environment-diagram.png" alt-text="Diagramm: Front Door-Bereitstellungsumgebung mit der Azure CLI" border="false":::

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure CLI (lokal installiert) oder Azure Cloud Shell
- Stellen Sie sicher, dass die Front Door-Erweiterung in der Azure-Befehlszeilenschnittstelle hinzugefügt ist.

```azurecli-interactive 
az extension add --name front-door
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.28 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

In Azure ordnen Sie verwandte Ressourcen einer Ressourcengruppe zu. Sie können entweder eine vorhandene Ressourcengruppe verwenden oder eine neue erstellen.

Für diesen Schnellstart benötigen Sie zwei Ressourcengruppen. Eine Ressourcengruppe in *USA, Mitte* und eine zweite in *USA, Süden-Mitte*.

Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe:

```azurecli-interactive
az group create \
    --name myRGFDCentral \
    --location centralus

az group create \
    --name myRGFDEast \
    --location eastus
```

## <a name="create-two-instances-of-a-web-app"></a>Erstellen von zwei Instanzen einer Web-App

Für diesen Schnellstart sind zwei Instanzen einer Webanwendung erforderlich, die in verschiedenen Azure-Regionen ausgeführt werden. Beide Webanwendungsinstanzen werden im Aktiv/Aktiv-Modus ausgeführt, sodass jede von ihnen Datenverkehr verarbeiten kann.

Wenn Sie noch nicht über eine Web-App verfügen, verwenden Sie das folgende Skript, um zwei Beispiel-Web-Apps einzurichten.

### <a name="create-app-service-plans"></a>Erstellen von App Service-Plänen

Bevor Sie die Web-Apps erstellen können, benötigen Sie zwei App Service-Pläne: einen in *USA, Mitte* und einen zweiten in *USA, Osten*.

Erstellen Sie App Service-Pläne mithilfe von [az appservice plan create](/cli/azure/appservice/plan#az_appservice_plan_create&preserve-view=true):

```azurecli-interactive
az appservice plan create \
--name myAppServicePlanCentralUS \
--resource-group myRGFDCentral

az appservice plan create \
--name myAppServicePlanEastUS \
--resource-group myRGFDEast
```

### <a name="create-web-apps"></a>Erstellen von Web-Apps

Wenn Sie die folgenden Befehle ausführen, wird in jedem App Service-Plan im vorherigen Schritt eine Web-App erstellt. Web-App-Namen müssen global eindeutig sein.

Erstellen Sie eine Web-App mit [az webapp create](/cli/azure/webapp#az_webapp_create&preserve-view=true):

```azurecli-interactive
az webapp create \
--name WebAppContoso-1 \
--resource-group myRGFDCentral \
--plan myAppServicePlanCentralUS 

az webapp create \
--name WebAppContoso-2 \
--resource-group myRGFDEast \
--plan myAppServicePlanEastUS
```

Notieren Sie sich den Standardhostnamen jeder Web-App, damit Sie die Back-End-Adressen definieren können, wenn Sie Front Door-Instanz im nächsten Schritt bereitstellen.

## <a name="create-the-front-door"></a>Erstellen der Front Door-Instanz

Erstellen Sie eine einfache Front Door-Instanz mit Standardeinstellungen für den Lastenausgleich, einem Integritätstest und Routingregeln, indem Sie Folgendes ausführen:

Erstellen Sie eine Front Door-Instanz mit [az network front-door create](/cli/azure/network/front-door#az_network_front_door_create&preserve-view=true):

```azurecli-interactive
az network front-door create \
--resource-group myRGFDCentral \
--name contoso-frontend \
--accepted-protocols http https \
--backend-address webappcontoso-1.azurewebsites.net webappcontoso-2.azurewebsites.net 
```

**--resource-group:** Geben Sie eine Ressourcengruppe an, in der Sie die Front Door-Instanz bereitstellen möchten.

**--name:** Geben Sie einen global eindeutigen Namen für Ihre Azure Front Door-Instanz an. 

**--accepted-protocols:** Mögliche Werte sind **http** und **https**. Wenn Sie beide Werte verwenden möchten, geben Sie beide durch ein Leerzeichen getrennt an.

**--backend-address:** Definieren Sie hier die Hostnamen für beide Web-Apps durch ein Leerzeichen getrennt.

Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, notieren Sie sich den Hostnamen im Abschnitt *frontEndpoints*.

## <a name="test-the-front-door"></a>Testen der Front Door-Instanz

Öffnen Sie einen Webbrowser, und geben Sie den Hostnamen ein, den Sie mit den Befehlen abgerufen haben. Front Door leitet Ihre Anforderung an eine der Back-End-Ressourcen weiter.

:::image type="content" source="./media/quickstart-create-front-door-cli/front-door-testing-page.png" alt-text="FrontDoor-Testseite":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit Front Door erstellten Ressourcen nicht mehr benötigen, löschen Sie beide Ressourcengruppen. Wenn Sie die Ressourcengruppe löschen, werden auch die Front Door-Instanz und alle zugehörigen Ressourcen gelöscht. 

Führen Sie [az group delete](/cli/azure/group#az_group_delete&preserve-view=true) aus, um die Ressourcengruppe zu löschen:

```azurecli-interactive
az group delete \
--name myRGFDCentral 

az group delete \
--name myRGFDEast
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes erstellt:
* Front Door
* Zwei Web-Apps

Fahren Sie mit den Front Door-Tutorials fort, um zu erfahren, wie Sie Ihrer Front Door-Instanz eine benutzerdefinierte Domäne hinzufügen.

> [!div class="nextstepaction"]
> [Hinzufügen einer benutzerdefinierten Domäne](front-door-custom-domain.md)

---
title: 'Tutorial: Bereitstellen eines vSphere-Clusters in Azure'
description: Hier erfahren Sie, wie Sie mithilfe von Azure VMware Solution einen vSphere-Cluster in Azure bereitstellen.
ms.topic: tutorial
ms.date: 09/07/2020
ms.openlocfilehash: 69a29a459ba283bb34169112ac2fa174ac6a14af
ms.sourcegitcommit: 8791f69d44150767807d215cafc4076f3ed43f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89512364"
---
# <a name="tutorial-deploy-an-azure-vmware-solution-private-cloud-in-azure"></a>Tutorial: Bereitstellen einer privaten Azure VMware Solution-Cloud in Azure

Azure VMware Solution ermöglicht das Bereitstellen eines vSphere-Clusters in Azure. Die Bereitstellung muss mindestens drei Hosts umfassen. Später können nacheinander weitere Host hinzugefügt werden. Pro Cluster sind bis zu 16 Hosts möglich. 

Da Sie Ihre private Cloud zunächst nicht mit Ihrer lokalen vCenter-Instanz verwalten können, müssen Sie zusätzliche Konfigurationsschritte für eine lokale vCenter-Instanz, ein virtuelles Netzwerk und mehr ausführen sowie eine Verbindung mit diesen Komponenten herstellen. Diese Verfahren und die damit zusammenhängenden Voraussetzungen werden in diesem Tutorial behandelt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer privaten Cloud von Azure VMware Solution
> * Vergewissern, dass die private Cloud bereitgestellt wurde

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Geeignete Administratorrechte und Berechtigung zum Erstellen einer privaten Cloud.
- Vergewissern Sie sich, dass das Netzwerk ordnungsgemäß konfiguriert ist, wie unter [Netzwerkprüfliste für die Azure-VMware-Lösung (Azure VMware Solution, AVS)](tutorial-network-checklist.md) beschrieben.

## <a name="register-the-resource-provider"></a>Registrieren des Ressourcenanbieters

[!INCLUDE [register-resource-provider-steps](includes/register-resource-provider-steps.md)]


## <a name="create-a-private-cloud"></a>Erstellen einer privaten Cloud

Sie können eine private Azure VMware Solution-Cloud über das [Azure-Portal](#azure-portal) oder mithilfe der [Azure-Befehlszeilenschnittstelle](#azure-cli) erstellen.

### <a name="azure-portal"></a>Azure-Portal

[!INCLUDE [create-avs-private-cloud-azure-portal](includes/create-avs-private-cloud-azure-portal-steps.md)]

### <a name="azure-cli"></a>Azure CLI

Zum Erstellen einer privaten Azure VMware Solution-Cloud können Sie anstelle des Azure-Portals auch die Azure CLI über Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose interaktive Shell, in der häufig verwendete Azure-Tools vorinstalliert sind und die für die Verwendung mit Ihrem Konto konfiguriert wurde. 

#### <a name="open-azure-cloud-shell"></a>Öffnen von Azure Cloud Shell

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock die Option **Ausprobieren** aus. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/bash](https://shell.azure.com/bash) navigieren. Wählen Sie **Kopieren** aus, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die **EINGABETASTE**, um ihn auszuführen.

#### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mithilfe des Befehls [az group create](/cli/azure/group) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```
azurecli-interactive
az group create --name myResourceGroup --location eastus
```

#### <a name="create-a-private-cloud"></a>Erstellen einer privaten Cloud

Geben Sie einen Ressourcengruppennamen, einen Namen für die private Cloud, einen Standort und die Größe des Clusters an.

| Eigenschaft  | BESCHREIBUNG  |
| --------- | ------------ |
| **-g** (Ressourcengruppenname)     | Der Name der Ressourcengruppe für die Ressourcen Ihrer privaten Cloud        |
| **-n** (Name der privaten Cloud)     | Der Name Ihrer privaten Azure VMware Solution-Cloud        |
| **--location**     | Der für Ihre private Cloud verwendete Standort         |
| **--cluster-size**     | Die Größe des Clusters. Der Mindestwert ist „3“.         |
| **--network-block**     | Der für die private Cloud zu verwendende CIDR-IP-Adressenblock des Netzwerks. Der Adressblock darf sich nicht mit Adressblöcken überlappen, die in anderen virtuellen Netzwerken in Ihrem Abonnement und Ihren lokalen Netzwerken verwendet werden.        |
| **--sku** | Der SKU-Wert AV36 |

```
azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22 --sku AV36
```

## <a name="delete-a-private-cloud-azure-portal"></a>Löschen einer privaten Cloud (Azure-Portal)

Wenn Sie über eine private Azure VMware Solution-Cloud verfügen, die Sie nicht mehr benötigen, können Sie sie löschen. Beim Löschen einer privaten Cloud werden alle Cluster zusammen mit allen zugehörigen Komponenten gelöscht.

Navigieren Sie hierzu im Azure-Portal zu Ihrer privaten Cloud, und wählen Sie **Löschen** aus. Geben Sie auf der Bestätigungsseite den Namen der privaten Cloud ein, und wählen Sie **Ja** aus.

> [!CAUTION]
> Das Löschen der privaten Cloud kann nicht rückgängig gemacht werden. Die Daten können nach dem Löschen der privaten Cloud nicht wiederhergestellt werden, da alle aktiven Workloads und Komponenten beendet und sämtliche Daten und Konfigurationseinstellungen der privaten Cloud zerstört werden (einschließlich der öffentlichen IP-Adressen). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer privaten Cloud von Azure VMware Solution
> * Vergewissern, dass die private Cloud bereitgestellt wurde

Im nächsten Tutorial erfahren Sie, wie Sie im Rahmen der Einrichtung der lokalen Verwaltung für Ihre privaten Cloudcluster ein virtuelles Netzwerk für die Verwendung mit Ihrer privaten Cloud erstellen.

> [!div class="nextstepaction"]
> [Erstellen eines virtuellen Netzwerks](tutorial-configure-networking.md)

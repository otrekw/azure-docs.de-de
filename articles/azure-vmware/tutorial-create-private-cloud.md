---
title: 'Tutorial: Bereitstellen eines vSphere-Clusters in Azure'
description: Hier erfahren Sie, wie Sie mithilfe der Azure-VMware-Lösung (Azure VMware Solution, AVS) einen vSphere-Cluster in Azure bereitstellen.
ms.topic: tutorial
ms.date: 07/15/2020
ms.openlocfilehash: 4f3b33ea401c62124ae5f8a4c881d86d2f19b40c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079416"
---
# <a name="tutorial-deploy-an-avs-private-cloud-in-azure"></a>Tutorial: Bereitstellen einer privaten AVS-Cloud in Azure

Die Azure-VMware-Lösung (Azure VMware Solution, AVS) ermöglicht das Bereitstellen eines vSphere-Clusters in Azure. Die Bereitstellung muss mindestens drei Hosts umfassen. Später können nacheinander weitere Host hinzugefügt werden. Pro Cluster sind bis zu 16 Hosts möglich. 

Da Sie Ihre private Cloud zunächst nicht mit Ihrer lokalen vCenter-Instanz verwalten können, müssen Sie zusätzliche Konfigurationsschritte für eine lokale vCenter-Instanz, ein virtuelles Netzwerk und mehr ausführen sowie eine Verbindung mit diesen Komponenten herstellen. Diese Verfahren und die damit zusammenhängenden Voraussetzungen werden in diesem Tutorial behandelt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer privaten AVS-Cloud
> * Vergewissern, dass die private Cloud bereitgestellt wurde

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Geeignete Administratorrechte und Berechtigung zum Erstellen einer privaten Cloud.
- Vergewissern Sie sich, dass das Netzwerk ordnungsgemäß konfiguriert ist, wie unter [Netzwerkprüfliste für die Azure-VMware-Lösung (Azure VMware Solution, AVS)](tutorial-network-checklist.md) beschrieben.

## <a name="register-the-resource-provider"></a>Registrieren des Ressourcenanbieters

Für die Verwendung von AVS müssen Sie zunächst den Ressourcenanbieter für Ihr Abonnement registrieren.

```
azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

Informationen zu weiteren Möglichkeiten für die Ressourcenanbieterregistrierung finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md).


## <a name="create-a-private-cloud"></a>Erstellen einer privaten Cloud

Sie können eine private AVS-Cloud über das [Azure-Portal](#azure-portal) oder mithilfe der [Azure-Befehlszeilenschnittstelle](#azure-cli) erstellen.

### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie **Neue Ressource erstellen** aus. Geben Sie im Textfeld **Marketplace durchsuchen** den Text `Azure VMware Solution` ein, und wählen Sie in der Liste die Option **Azure-VMware-Lösung** aus. Wählen Sie im Fenster **Azure-VMware-Lösung** die Option **Erstellen** aus.

1. Geben Sie auf der Registerkarte **Grundlegende Einstellungen** Werte für die Felder ein. In der folgenden Tabelle werden die Eigenschaften der Felder aufgeführt:

   | Feld   | Wert  |
   | ---| --- |
   | **Abonnement** | Das Abonnement, das Sie für die Bereitstellung verwenden möchten.|
   | **Ressourcengruppe** | Die Ressourcengruppe für die Ressourcen Ihrer privaten Cloud. |
   | **Location** | Wählen Sie einen Standort aus (beispielsweise **USA, Osten**).|
   | **Ressourcenname** | Der Name Ihrer privaten AVS-Cloud. |
   | **SKU** | Wählen Sie den folgenden SKU-Wert aus: AV36 |
   | **Hosts** | Die Anzahl von Hosts, die dem privaten Cloudcluster hinzugefügt werden sollen. Der Standardwert ist 3, er kann jedoch nach der Bereitstellung erhöht oder verringert werden.  |
   | **vCenter admin password** (vCenter-Administratorkennwort) | Geben Sie ein Cloudadministratorkennwort ein. |
   | **NSX-T manager password** (NSX-T-Manager-Kennwort) | Geben Sie ein NSX-T-Administratorkennwort ein. |
   | **Adressblock** | Geben Sie einen IP-Adressblock für das CIDR-Netzwerk für die private Cloud ein, etwa 10.175.0.0/22. |

   :::image type="content" source="./media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="Erstellen einer privaten Cloud" border="true":::

1. Wählen Sie abschließend **Überprüfen + erstellen** aus. Überprüfen Sie auf dem nächsten Bildschirm die eingegebenen Informationen. Sind alle Informationen korrekt, wählen Sie **Erstellen** aus.

   > [!NOTE]
   > Dieser Schritt dauert etwa zwei Stunden. 

1. Vergewissern Sie sich, dass die Bereitstellung erfolgreich war. Navigieren Sie zu der von Ihnen erstellten Ressourcengruppe, und wählen Sie Ihre private Cloud aus.  Wenn die Bereitstellung abgeschlossen ist, wird der Status **Erfolgreich** angezeigt. 

   :::image type="content" source="./media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Vergewissern, dass die private Cloud bereitgestellt wurde" border="true":::

### <a name="azure-cli"></a>Azure CLI

Zum Erstellen einer privaten AVS-Cloud können Sie anstelle des Azure-Portals auch die Azure CLI über Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose interaktive Shell, in der häufig verwendete Azure-Tools vorinstalliert sind und die für die Verwendung mit Ihrem Konto konfiguriert wurde. 

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
| **-n** (Name der privaten Cloud)     | Der Name Ihrer privaten AVS-Cloud.        |
| **--location**     | Der für Ihre private Cloud verwendete Standort         |
| **--cluster-size**     | Die Größe des Clusters. Der Mindestwert ist „3“.         |
| **--network-block**     | Der für die private Cloud zu verwendende CIDR-IP-Adressenblock des Netzwerks. Der Adressblock darf sich nicht mit Adressblöcken überlappen, die in anderen virtuellen Netzwerken in Ihrem Abonnement und Ihren lokalen Netzwerken verwendet werden.        |
| **--sku** | Der SKU-Wert AV36 |

```
azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22 --sku AV36
```

## <a name="delete-a-private-cloud-azure-portal"></a>Löschen einer privaten Cloud (Azure-Portal)

Wenn Sie über eine private AVS-Cloud verfügen, die Sie nicht mehr benötigen, können Sie sie löschen. Beim Löschen einer privaten Cloud werden alle Cluster zusammen mit allen zugehörigen Komponenten gelöscht.

Navigieren Sie hierzu im Azure-Portal zu Ihrer privaten Cloud, und wählen Sie **Löschen** aus. Geben Sie auf der Bestätigungsseite den Namen der privaten Cloud ein, und wählen Sie **Ja** aus.

> [!CAUTION]
> Das Löschen der privaten Cloud kann nicht rückgängig gemacht werden. Die Daten können nach dem Löschen der privaten Cloud nicht wiederhergestellt werden, da alle aktiven Workloads und Komponenten beendet und sämtliche Daten und Konfigurationseinstellungen der privaten Cloud zerstört werden (einschließlich der öffentlichen IP-Adressen). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer privaten AVS-Cloud
> * Vergewissern, dass die private Cloud bereitgestellt wurde

Im nächsten Tutorial erfahren Sie, wie Sie im Rahmen der Einrichtung der lokalen Verwaltung für Ihre privaten Cloudcluster ein virtuelles Netzwerk für die Verwendung mit Ihrer privaten Cloud erstellen.

> [!div class="nextstepaction"]
> [Erstellen eines virtuellen Netzwerks](tutorial-configure-networking.md)

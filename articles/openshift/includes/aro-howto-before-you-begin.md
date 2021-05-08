---
author: sabbour
ms.author: asabbour
ms.date: 4/5/2020
ms.openlocfilehash: 1fded0ad08af4b1e5d915e32e09087c1a78bd318
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107520558"
---
### <a name="create-the-cluster"></a>Erstellen Sie den Cluster.

Sehen Sie sich das Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](../tutorial-create-cluster.md) an. Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens die Azure CLI-Version 2.6.0 ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

### <a name="connect-to-the-cluster"></a>Herstellen einer Verbindung mit dem Cluster

Zum Verwalten eines Azure Red Hat OpenShift-Clusters verwenden Sie [oc](https://docs.openshift.com/container-platform/4.7/cli_reference/openshift_cli/getting-started-cli.html), den OpenShift-Befehlszeilen-Client.

> [!NOTE]
> Es wird empfohlen, die [OpenShift-Befehlszeile](../tutorial-connect-cluster.md) auf [Azure Cloud Shell](https://shell.azure.com/) zu installieren und sie für alle unten aufgeführten Befehlszeilenvorgänge zu verwenden. Starten Sie Ihre Shell über shell.azure.com oder durch Klicken auf den Link:
>
> [![Start einbetten](https://docs.microsoft.com/azure/includes/media/cloud-shell-try-it/hdi-launch-cloud-shell.png "Starten von Azure Cloud Shell")](https://shell.azure.com/bash)

Befolgen Sie das Tutorial zum Installieren der CLI, dem Abrufen der Clusteranmeldeinformationen und dem [Herstellen einer Verbindung mit dem Cluster](../tutorial-connect-cluster.md) mithilfe der Webkonsole und der OpenShift CLI.

Nachdem Sie sich angemeldet haben, sollte eine Meldung mit dem Text angezeigt werden, dass Sie das `default`-Projekt verwenden.

```output
Login successful.

You have access to 61 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
```

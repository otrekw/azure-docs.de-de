---
title: Installieren der Clienttools
description: Installieren von azdata, kubectl, der Azure CLI, psql, Azure Data Studio (Insider) und der Arc-Erweiterung für Azure Data Studio
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: ecf0f54913f980d879b562eb4aa8063acf6c4772
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92320252"
---
# <a name="install-client-tools-for-deploying-and-managing-azure-arc-enabled-data-services"></a>Installieren von Clienttools zum Bereitstellen und Verwalten von Azure Arc-fähigen Datendiensten

> [!IMPORTANT]
> Wenn Sie auf ein neues monatliches Release aktualisieren, stellen Sie sicher, dass Sie auch Updates auf die neueste Version von Azure Data Studio, des [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]-Tools und der [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] sowie der Azure Arc-Erweiterungen für Azure Data Studio ausführen.

Dieses Dokument führt Sie durch die Schritte zum Installieren der [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)], von Azure Data Studio Azure, der Azure CLI (az) und des Kubernetes CLI-Tools (kubectl) auf Ihrem Clientcomputer.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="tools-for-creating-and-managing-azure-arc-enabled-data-services"></a>Tools zum Erstellen und Verwalten von Azure Arc-fähigen Datendiensten 

In der folgenden Tabelle werden die allgemeinen Tools aufgelistet, die für die Erstellung und Verwaltung von Azure Arc-fähigen Datendiensten erforderlich sind, und es wird beschrieben, wie diese Tools installiert werden:

| Tool | Erforderlich | BESCHREIBUNG | Installation |
|---|---|---|---|
| [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] | Ja | Befehlszeilentool für die Installation und Verwaltung eines Big Data-Clusters. [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] enthält außerdem ein Befehlszeilen-Hilfsprogramm zum Herstellen einer Verbindung mit sowie zum Abfragen von Azure SQL- und SQL Server-Instanzen sowie Postgres-Servern mithilfe der Befehle `azdata sql query` (Ausführen einer einzelnen Abfrage über die Befehlszeile), `azdata sql shell` (eine interaktive Shell), `azdata postgres query` und `azdata postgres shell`. | [Installieren](/sql/azdata/install/deploy-install-azdata?toc=/azure/azure-arc/data/toc.json&bc=/azure/azure-arc/data/breadcrumb/toc.json) |
| Azure Data Studio | Ja | Umfassendes Tool zum Herstellen einer Verbindung mit einer Vielzahl von Datenbanken, einschließlich Azure SQL, SQL Server, PostrgreSQL und MySQL. Erweiterungen für Azure Data Studio bieten eine Verwaltungsfunktion für Azure Arc-fähige Datendienste. | [Installieren](/sql/azure-data-studio/download-azure-data-studio) |
| [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]-Erweiterung für Azure Data Studio | Ja | Erweiterung für Azure Data Studio, mit der die [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] installiert wird, sofern dies noch nicht geschehen ist.| Installation aus dem Erweiterungskatalog in Azure Data Studio.|
| Azure Arc-Erweiterung für Azure Data Studio | Ja | Erweiterung für Azure Data Studio, die Verwaltungsfunktionen für Azure Arc-fähige Datendienste bereitstellt. Es gibt eine Abhängigkeit von der [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]-Erweiterung für Azure Data Studio. | Installation aus dem Erweiterungskatalog in Azure Data Studio.|
| PostgreSQL-Erweiterung in Azure Data Studio | Nein | PostgreSQL-Erweiterung für Azure Data Studio, die Verwaltungsfunktionen für PostgreSQL bereitstellt. | <!--{need link} [Install](../azure-data-studio/data-virtualization-extension.md) --> Installation aus dem Erweiterungskatalog in Azure Data Studio.|
| Azure CLI (az)<sup>1</sup> | Ja | Moderne Befehlszeilenschnittstelle zum Verwalten von Azure-Diensten. Wird mit AKS-Bereitstellungen und zum Hochladen von Azure Arc-fähigen Datendienstinventar- und -abrechnungsdaten verwendet. ([Weitere Informationen](/cli/azure/?view=azure-cli-latest&preserve-view=true)). | [Installieren](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) |
| Kubernetes CLI (kubectl)<sup>2</sup> | Ja | Befehlszeilentool zum Verwalten des zugrunde liegenden Kubernetes-Clusters ([weitere Informationen](https://kubernetes.io/docs/tasks/tools/install-kubectl/)). | [Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-powershell-from-psgallery) \| [Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management) |
| curl <sup>3</sup> | Für einige Beispielskripts erforderlich. | Befehlszeilentool zum Übertragen von Daten mit URLs. | [Windows](https://curl.haxx.se/windows/) \| Linux: Installieren von curl-Paket |
| oc | Für Red Hat OpenShift- und Azure Red Hat OpenShift-Bereitstellungen erforderlich |`oc` ist die OpenShift-Befehlszeilenschnittstelle. | [Installing the CLI](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli) (Installieren der Befehlszeilenschnittstelle)



<sup>1</sup> Sie müssen die Azure CLI-Version 2.0.4 oder höher verwenden. Führen Sie bei Bedarf `az --version` aus, um die Version zu ermitteln.

<sup>2</sup> Sie müssen die `kubectl`-Version 1.13 oder höher verwenden. Außerdem sollte die Version von `kubectl` um eins höher oder niedriger als die Nebenversion Ihres Kubernetes-Clusters sein. Wenn Sie eine bestimmte Version auf dem `kubectl`-Client installieren möchten, finden Sie weitere Informationen unter [Installieren von `kubectl` mit curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl) (Verwenden Sie unter Windows 10 cmd.exe und nicht Windows PowerShell zum Ausführen von curl).

<sup>3</sup> Wenn Sie PowerShell verwenden, ist curl ein Alias für das Cmdlet Invoke-WebRequest.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen des Azure Arc-Datencontrollers](create-data-controller.md)
---
title: Konfigurieren eines privaten Endpunkts
titleSuffix: Azure Machine Learning
description: Verwenden Sie Azure Private Link, um sicher von einem virtuellen Netzwerk aus auf Ihren Azure Machine Learning-Arbeitsbereich zuzugreifen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/30/2020
ms.openlocfilehash: 4ba7ec73ac70723e21b6acad571d62d14edd250a
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91828131"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace"></a>Konfigurieren von Azure Private Link für einen Azure Machine Learning-Arbeitsbereich

In diesem Dokument erfahren Sie, wie Sie Azure Private Link mit Ihrem Azure Machine Learning-Arbeitsbereich verwenden. Informationen zum Erstellen eines virtuellen Netzwerks für Azure Machine Learning finden Sie unter [Übersicht über die Isolation virtueller Netzwerke und Datenschutz](how-to-network-security-overview.md).

Azure Private Link ermöglicht Ihnen das Herstellen einer Verbindung mit Ihrem Arbeitsbereich über einen privaten Endpunkt. Bei einem privaten Endpunkt handelt es sich um eine Gruppe privater IP-Adressen in Ihrem virtuellen Netzwerk. Sie können dann den Zugriff auf Ihren Arbeitsbereich so einschränken, dass er nur über die privaten IP-Adressen erfolgt. Private Link hilft dabei, das Risiko einer Datenexfiltration zu verringern. Weitere Informationen zu privaten Endpunkten finden Sie im Artikel zu [Azure Private Link](/azure/private-link/private-link-overview).

> [!IMPORTANT]
> Azure Private Link hat keine Auswirkungen auf die Azure-Steuerungsebene (Verwaltungsvorgänge) aus, etwa das Löschen des Arbeitsbereichs oder das Verwalten von Computeressourcen. Beispiele: Erstellen, Aktualisieren oder Löschen eines Computeziels. Diese Vorgänge werden ganz normal über das öffentliche Internet ausgeführt. Bei Vorgängen auf Datenebene, etwa bei der Verwendung von Azure Machine Learning Studio, APIs (einschließlich veröffentlichter Pipelines) oder des SDK, wird der private Endpunkt verwendet.
>
> Wenn Sie Mozilla Firefox verwenden, treten möglicherweise Probleme beim Zugriff auf den privaten Endpunkt für Ihren Arbeitsbereich auf. Dieses Problem kann im Zusammenhang mit DNS über HTTPS in Mozilla auftreten. Die Verwendung von Microsoft Edge von Google Chrome wird als Problemumgehung empfohlen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie vorhaben, einen durch Private Link aktivierten Arbeitsbereich mit einem kundenseitig verwalteten Schlüssel zu verwenden, müssen Sie dieses Feature mithilfe eines Supporttickets anfordern. Weitere Informationen finden Sie unter [Verwalten und Erhöhen von Kontingenten](how-to-manage-quotas.md#private-endpoint-and-private-dns-quota-increases).

## <a name="limitations"></a>Einschränkungen

Die Verwendung eines Azure Machine Learning-Arbeitsbereichs mit Private Link ist in den Azure Government- und Azure China 21Vianet-Regionen nicht verfügbar.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Erstellen eines Arbeitsbereichs, der einen privaten Endpunkt verwendet

Verwenden Sie eine der folgenden Methoden, um einen Arbeitsbereich mit einem privaten Endpunkt zu erstellen:

> [!TIP]
> Mit der Azure Resource Manager-Vorlage kann bei Bedarf ein neues virtuelles Netzwerk erstellt werden. Die anderen Methoden erfordern ein vorhandenes virtuelles Netzwerk.

# <a name="resource-manager-template"></a>[Resource Manager-Vorlage](#tab/azure-resource-manager)

Die Azure Resource Manager-Vorlage unter [https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced) bietet eine einfache Möglichkeit zum Erstellen eines Arbeitsbereichs mit einem privaten Endpunkt und einem virtuellen Netzwerk.

Informationen zur Verwendung dieser Vorlage, einschließlich privater Endpunkte, finden Sie unter [Verwenden einer Azure Resource Manager-Vorlage zum Erstellen eines Arbeitsbereichs für Azure Machine Learning](how-to-create-workspace-template.md).

# <a name="python"></a>[Python](#tab/python)

Das Azure Machine Learning Python SDK bietet die Klasse [PrivateEndpointConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.privateendpointconfig?view=azure-ml-py), die mit [Workspace.create()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---tags-none--friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--adb-workspace-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--private-endpoint-config-none--private-endpoint-auto-approval-true--exist-ok-false--show-output-true-) verwendet werden kann, um einen Arbeitsbereich mit einem privaten Endpunkt zu erstellen. Diese Klasse erfordert ein vorhandenes virtuelles Netzwerk.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Die [Azure CLI-Erweiterung für maschinelles Lernen](reference-azure-machine-learning-cli.md) stellt den Befehl [az ml workspace create](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_create) bereit. Mit den folgenden Parametern für diesen Befehl kann ein Arbeitsbereich mit einem privaten Netzwerk erstellt werden, aber er erfordert ein vorhandenes virtuelles Netzwerk:

* `--pe-name`: Der Name des erstellten privaten Endpunkts.
* `--pe-auto-approval`: Gibt an, ob private Endpunktverbindungen mit dem Arbeitsbereich automatisch genehmigt werden sollen.
* `--pe-resource-group`: Die für den privaten Endpunkt zu erstellende Ressourcengruppe. Muss dieselbe Gruppe sein, die auch das virtuelle Netzwerk enthält.
* `--pe-vnet-name`: Das vorhandene virtuelle Netzwerk, in dem der private Endpunkt erstellt werden soll.
* `--pe-subnet-name`: Der Name des Subnetzes, in dem der private Endpunkt erstellt werden soll. Der Standardwert ist `default`.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Die Registerkarte __Netzwerk__ im Azure Machine Learning Studio ermöglicht Ihnen die Konfiguration eines privaten Endpunkts. Es erfordert jedoch ein vorhandenes virtuelles Netzwerk. Weitere Informationen finden Sie unter [Erstellen von Arbeitsbereichen im Portal](how-to-manage-workspace.md).

---

## <a name="using-a-workspace-over-a-private-endpoint"></a>Verwenden eines Arbeitsbereichs über einen privaten Endpunkt

Da die Kommunikation mit dem Arbeitsbereich nur aus dem virtuellen Netzwerk zulässig ist, müssen alle Entwicklungsumgebungen, in denen der Arbeitsbereich verwendet wird, Mitglieder des virtuellen Netzwerks sein. Beispiel: ein virtueller Computer im virtuellen Netzwerk.

> [!IMPORTANT]
> Um eine vorübergehende Unterbrechung der Konnektivität zu vermeiden, empfiehlt Microsoft, den DNS-Cache auf Computern, die eine Verbindung mit dem Arbeitsbereich herstellen, nach dem Aktivieren von Private Link zu leeren. 

Weitere Informationen zu Azure Virtual Machines finden Sie in der [Dokumentation zu Virtual Machines](/azure/virtual-machines/).


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Schützen Ihres Azure Machine Learning-Arbeitsbereichs finden Sie im Artikel [Übersicht über die Isolation virtueller Netzwerke und Datenschutz](how-to-network-security-overview.md).

* Wenn Sie vorhaben, eine benutzerdefinierte DNS-Lösung in Ihrem virtuellen Netzwerk zu verwenden, finden Sie weitere Informationen unter [Verwenden eines Arbeitsbereichs mit einem benutzerdefinierten DNS-Server](how-to-custom-dns.md).

---
title: 'Erstellen einer Azure Time Series Insights Gen2-Umgebung mit der Azure CLI: Azure Time Series Insights Gen2 | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie eine Umgebung in Azure Time Series Insights Gen2 mit der Azure CLI einrichten.
author: tedvilutis
ms.author: tvilutis
manager: cnovak
ms.reviewer: orspodek
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: how-to
ms.date: 03/15/2021
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 36a6da683912ac004b948b9a334da1758ab903fd
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110791859"
---
# <a name="create-an-azure-time-series-insights-gen2-environment-using-the-azure-cli"></a>Erstellen einer Azure Time Series Insights Gen2-Umgebung mit der Azure CLI

Dieses Dokument führt Sie durch die Erstellung einer neuen Time Series Insights Gen2-Umgebung.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen eines Azure-Speicherkontos für den [kalten Speicher](./concepts-storage.md#cold-store) Ihrer Umgebung. Dieses Konto ist für die langfristige Aufbewahrung und Analyse von Verlaufsdaten konzipiert.

> [!NOTE]
> Ersetzen Sie `mytsicoldstore` in Ihrem Code durch einen eindeutigen Namen für Ihr Cold Storage-Konto.

Erstellen Sie zuerst das Speicherkonto:

```azurecli-interactive
storage=mytsicoldstore
rg=-my-resource-group-name
az storage account create -g $rg -n $storage --https-only
key=$(az storage account keys list -g $rg -n $storage --query [0].value --output tsv
```

## <a name="creating-the-environment"></a>Erstellen der Umgebung

Nachdem das Speicherkonto erstellt und sein Name und der Verwaltungsschlüssel den Variablen zugewiesen wurden, führen Sie den folgenden Befehl aus, um die Azure Time Series Insights-Umgebung zu erstellen:

> [!NOTE]
> Ersetzen Sie in Ihrem Code die Folgenden durch eindeutige Namen für Ihr Szenario:
>
> * `my-tsi-env` mit dem Namen Ihrer Umgebung.
> * `my-ts-id-prop` mit dem Namen der Zeitreihen-ID-Eigenschaft.

> [!IMPORTANT]
> Die Zeitreihen-ID Ihrer Umgebung ähnelt dem Partitionsschlüssel einer Datenbank. Die Zeitreihen-ID fungiert auch als Primärschlüssel für das Zeitreihenmodell.
>
> Weitere Informationen finden Sie unter [Bewährte Methoden für die Auswahl einer Time Series-ID](./how-to-select-tsid.md).

```azurecli-interactive
az tsi environment gen2 create --name "my-tsi-env" --location eastus2 --resource-group $rg --sku name="L1" capacity=1 --time-series-id-properties name=my-ts-id-prop type=String --warm-store-configuration data-retention=P7D --storage-configuration account-name=$storage management-key=$key
```

## <a name="remove-an-azure-time-series-insights-environment"></a>Entfernen einer Azure Time Series Insights-Umgebung

Sie können mit der Azure CLI eine einzelne Ressource, wie z. B. eine Time Series Insights Umgebung, oder eine Ressourcengruppe und alle zugehörigen Ressourcen einschließlich sämtlicher Time Series Insights-Umgebungen löschen.

Um [eine Time Series Insights-Umgebung zu löschen](/cli/azure/tsi/environment#az_tsi_environment_delete), führen Sie den folgenden Befehl aus:

```azurecli-interactive
az tsi environment delete --name "my-tsi-env" --resource-group $rg
```

Um [das Speicherkonto zu löschen](/cli/azure/storage/account#az_storage_account_delete), führen Sie den folgenden Befehl aus:

```azurecli-interactive
az storage account delete --name $storage --resource-group $rg
```

Führen Sie zum [Löschen einer Ressourcengruppe und aller dazugehörigen Ressourcen](/cli/azure/group#az_group_delete) den folgenden Befehl aus:

```azurecli-interactive
az group delete --name $rg
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Ereignisquellen der Streamingerfassung](./concepts-streaming-ingestion-event-sources.md) für Ihre Azure Time Series Insights Gen2-Umgebung.
* Informationen zum [Herstellen einer Verbindung mit einem IoT-Hub](./how-to-ingest-data-iot-hub.md).

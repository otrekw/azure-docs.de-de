---
title: Erstellen eines Azure Media Services-Kontos
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie ein Azure Media Services-Konto erstellen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/15/2020
ms.author: juliako
ms.openlocfilehash: e32a9053e4ab7cc3618f7b50b7291a660a14e1b1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87053495"
---
# <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

Um mit dem Verschlüsseln, Codieren, Analysieren, Verwalten und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Das Media Services-Konto muss einem oder mehreren Speicherkonten zugeordnet werden.

> [!NOTE]
> Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Zur Vermeidung zusätzlicher Wartezeit und Kosten für ausgehenden Datenverkehr wird dringend empfohlen, Speicherkonten zu verwenden, die sich an demselben Ort wie das Media Services-Konto befinden.

In diesem Artikel werden die Schritte zum Erstellen eines neuen Azure Media Services-Kontos beschrieben. Wählen Sie eine der folgenden Registerkarten aus:

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Über das Azure-Portal können Sie schnell ein Azure Media Services-Konto erstellen. Sie können mit Ihrem Konto auf Mediendienste zugreifen, die Funktionen zum Speichern, Verschlüsseln, Codieren, Verwalten und Streamen von Medieninhalten in Azure bereitstellen.

Derzeit können Sie das [Azure-Portal](https://portal.azure.com/) für Folgendes verwenden:

* Verwalten von Media Services v3-[Liveereignissen](live-events-outputs-concept.md) 
* Anzeigen (nicht verwalten) von [Medienobjekten](assets-concept.md) der Version 3 
* [Abrufen von Informationen über den Zugriff auf APIs](./access-api-howto.md) 

Verwenden Sie für alle anderen Verwaltungsaufgaben (etwa für [Transformationen und Aufträge](transforms-jobs-concept.md) und [Inhaltsschutz](content-protection-overview.md)) die [REST-API](https://aka.ms/ams-v3-rest-ref), die [CLI](https://aka.ms/ams-v3-cli-ref) oder eins der unterstützten [SDKs](media-services-apis-overview.md#sdks).

In diesem Artikel wird beschrieben, wie Sie ein Media Services-Konto im Azure-Portal erstellen.

### <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Klicken Sie auf **+ Ressource erstellen** > **Medien** > **Media Services**.
1. Geben Sie im Abschnitt **Erstellen eines Media Services-Kontos** die erforderlichen Werte ein.
    
    | Name | BESCHREIBUNG |
    | ---|---|
    |**Kontoname**|Geben Sie den Namen des neuen Media Services-Kontos ein. Der Name eines Media Services-Kontos darf nur Kleinbuchstaben oder Ziffern ohne Leerzeichen enthalten und muss aus 3 bis 24 Zeichen bestehen.|
    |**Abonnement**|Wenn Sie über mehrere Abonnements verfügen, wählen Sie eines aus der Liste der Azure-Abonnements aus, auf die Sie Zugriff haben.|
    |**Ressourcengruppe**|Wählen Sie die neue oder vorhandene Ressource aus. Eine Ressourcengruppe ist eine Sammlung von Ressourcen mit gleichem Lebenszyklus, gleichen Berechtigungen und gleichen Richtlinien. [Hier](../../azure-resource-manager/management/overview.md#resource-groups)erhalten Sie weitere Informationen.|
    |**Location**|Wählen Sie die geografische Region aus, in der die Medien- und Metadaten-Datensätze für Ihr Media Services-Konto gespeichert werden sollen. Diese Region wird zum Verarbeiten und Streamen Ihrer Medien verwendet. Im Dropdownlistenfeld werden nur die verfügbaren Media Services-Regionen angezeigt. |
    |**Speicherkonto**|Wählen Sie ein Speicherkonto aus, das als Blobspeicher für die Medieninhalte aus Ihrem Media Services-Konto dienen soll. Sie können ein vorhandenes Speicherkonto in derselben geografischen Region wie Ihr Media Services-Konto auswählen oder ein neues Speicherkonto erstellen. Ein neues Speicherkonto wird in derselben Region erstellt. Für Namen von Speicherkonten gelten die gleichen Regeln wie für Namen von Media Services-Konten.<br/><br/>Sie müssen über ein **primäres** Speicherkonto verfügen. Darüber hinaus können Sie beliebig viele **sekundäre** Speicherkonten an Ihr Media Services-Konto anfügen. Sie können das Azure-Portal verwenden, um sekundäre Speicherkonten hinzuzufügen. Weitere Informationen finden Sie unter [Azure Storage-Konten mit Azure Media Services-Konten](storage-account-concept.md).<br/><br/>Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Zur Vermeidung zusätzlicher Wartezeit und Kosten für ausgehenden Datenverkehr wird dringend empfohlen, Speicherkonten zu verwenden, die sich an demselben Ort wie das Media Services-Konto befinden.|
    
1. Wählen Sie **An Dashboard anheften** aus, um den Status der Kontobereitstellung anzuzeigen.
1. Klicken Sie unten im Formular auf **Erstellen** .

    Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein **Standard**-Streamingendpunkt mit dem Status **Beendet** hinzugefügt. Um mit dem Streamen Ihrer Inhalte zu beginnen und die [dynamische Paketerstellung](dynamic-packaging-overview.md) und [dynamische Verschlüsselung](content-protection-overview.md) zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Zustand **Wird ausgeführt** aufweisen. 

## <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="set-the-azure-subscription"></a>Einrichten des Azure-Abonnements

Geben Sie im folgenden Befehle die ID des Azure-Abonnements an, das Sie für das Media Services-Konto verwenden möchten. Sie können eine Liste der Abonnements anzeigen, auf die Sie Zugriff haben, indem Sie zu [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) navigieren.

```azurecli
az account set --subscription mySubscriptionId
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mithilfe des folgenden Befehls eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Ressourcen wie Azure Media Services-Konten und die zugehörigen Speicherkonten bereitgestellt und verwaltet werden.

Sie können `amsResourceGroup` durch Ihren Wert ersetzen.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. Weitere Informationen zur Verwendung von Speicherkonten in Media Services finden Sie unter [Storage accounts](storage-account-concept.md) (Storage-Konten).

Sie müssen über ein **primäres** Storage-Konto verfügen. Darüber hinaus können Sie beliebig viele **sekundäre** Storage-Konten an Ihr Media Services-Konto anfügen. Media Services unterstützt Konten des Typs **Allgemein v2** (GPv2) oder **Allgemein v1** (GPv1). Reine Blobkonten sind als **primäre** Konten nicht zulässig. Weitere Informationen zu Storage-Konten finden Sie unter [Optionen für Azure Storage-Konten](../../storage/common/storage-account-overview.md). 

In diesem Beispiel erstellen Sie ein universelles LRS-Standardkonto der Version 2. Falls Sie mit Speicherkonten experimentieren möchten, verwenden Sie `--sku Standard_LRS`. Wenn Sie eine SKU für die Produktion auswählen, sollten Sie jedoch die Verwendung von `--sku Standard_RAGRS` erwägen, da diese Option geografische Replikation zum Gewährleisten der Geschäftskontinuität bietet. Weitere Informationen finden Sie unter [Storage accounts](/cli/azure/storage/account?view=azure-cli-latest) (Speicherkonten).
 
Der folgende Befehl erstellt ein Storage-Konto, das dem Media Services-Konto zugeordnet werden soll. Im folgenden Skript können Sie `storageaccountforams` durch Ihren Wert ersetzen. `amsResourceGroup` muss dem Wert entsprechen, den Sie für die Ressourcengruppe im vorherigen Schritt angegeben haben. Der Speicherkontoname muss kürzer als 24 Zeichen sein.

```azurecli
az storage account create --name storageaccountforams \  
  --kind StorageV2 \
  --sku Standard_LRS \
  -l westus2 \
  -g amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

Der folgende Azure CLI-Befehl erstellt ein neues Media Services-Konto. Sie können die folgenden Werte ersetzen: `amsaccount`, `storageaccountforams` (muss dem Wert für Ihr Speicherkonto entsprechen) und `amsResourceGroup` (muss dem Wert für Ihre Ressourcengruppe entsprechen).

```azurecli
az ams account create --name amsaccount \
   -g amsResourceGroup --storage-account storageaccountforams \
   -l westus2 
```

### <a name="see-also"></a>Weitere Informationen

* [Azure-Befehlszeilenschnittstelle](/cli/azure/ams?view=azure-cli-latest)
* [Anfügen eines sekundären Speichers an ein Media Services-Konto](/cli/azure/ams/account/storage?view=azure-cli-latest#az-ams-account-storage-add)

---

## <a name="next-steps"></a>Nächste Schritte

[Streamen einer Datei](stream-files-dotnet-quickstart.md)

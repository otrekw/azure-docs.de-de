---
title: Erstellen eines Azure Media Services-Kontos
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie ein Azure Media Services-Konto erstellen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/15/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 23a26e01c8329fa384d29443a143b173c58a2c6e
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88539119"
---
# <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

Um mit dem Verschlüsseln, Codieren, Analysieren, Verwalten und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Das Media Services-Konto muss einem oder mehreren Speicherkonten zugeordnet werden. In diesem Artikel werden die Schritte zum Erstellen eines neuen Azure Media Services-Kontos beschrieben.

> [!NOTE]
> Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Zur Vermeidung zusätzlicher Wartezeit und Kosten für ausgehenden Datenverkehr wird dringend empfohlen, Speicherkonten zu verwenden, die sich an demselben Ort wie das Media Services-Konto befinden.

 Sie können entweder das Azure-Portal oder die CLI verwenden, um ein Media Services-Konto zu erstellen. Wählen Sie die Registerkarte für die Methode aus, die Sie verwenden möchten.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="portal"></a>[Portal](#tab/portal/)

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

<!-- Move this section. This section should be moved to conceptual.  It doesn't belong in task based -->
Über das Azure-Portal können Sie schnell ein Azure Media Services-Konto erstellen. Sie können mit Ihrem Konto auf Mediendienste zugreifen, die Funktionen zum Speichern, Verschlüsseln, Codieren, Verwalten und Streamen von Medieninhalten in Azure bereitstellen.

Derzeit können Sie das [Azure-Portal](https://portal.azure.com/) für Folgendes verwenden:

* Verwalten von Media Services v3-[Liveereignissen](live-events-outputs-concept.md) 
* Anzeigen (nicht verwalten) von [Medienobjekten](assets-concept.md) der Version 3 
* [Abrufen von Informationen über den Zugriff auf APIs](./access-api-howto.md) 

Verwenden Sie für alle anderen Verwaltungsaufgaben (etwa für [Transformationen und Aufträge](transforms-jobs-concept.md) und [Inhaltsschutz](content-protection-overview.md)) die [REST-API](https://aka.ms/ams-v3-rest-ref), die [CLI](https://aka.ms/ams-v3-cli-ref) oder eins der unterstützten [SDKs](media-services-apis-overview.md#sdks).
<!-- Move this section. This section should be moved to conceptual.  It doesn't belong in task based -->

### <a name="use-the-azure-portal-to-create-a-media-services-account"></a>Erstellen eines Media Services-Kontos über das Azure-Portal

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

## <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli/)

## <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

<!-- NOTE: The following are in the includes file and are reused in other How To articles. All task based content should be in the includes folder with the task- prefix prepended to the file name. -->

### <a name="set-the-azure-subscription"></a>Einrichten des Azure-Abonnements

[!INCLUDE [Set the Azure subscription with CLI](./includes/task-set-azure-subscription-cli.md)]

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create a resource group with CLI](./includes/task-create-resource-group-cli.md)]

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

[!INCLUDE [Create a storage account with CLI](./includes/task-create-storage-account-cli.md)]

### <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

[!INCLUDE [Create a Media Services account with CLI](./includes/task-create-media-services-account-cli.md)]

### <a name="see-also"></a>Weitere Informationen

* [Azure-Befehlszeilenschnittstelle](/cli/azure/ams?view=azure-cli-latest)
* [Anfügen eines sekundären Speichers an ein Media Services-Konto](/cli/azure/ams/account/storage?view=azure-cli-latest#az-ams-account-storage-add)

---

## <a name="next-steps"></a>Nächste Schritte

[Streamen einer Datei](stream-files-dotnet-quickstart.md)

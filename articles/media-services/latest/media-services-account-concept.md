---
title: Verwalten von Azure Media Services v3-Konten | Microsoft-Dokumentation
description: Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. In diesem Artikel wird erläutert, wie Azure Media Services v3-Konten verwaltet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 08579f7ba952bb4ebcba1595508612affb852528
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75980380"
---
# <a name="manage-azure-media-services-v3-accounts"></a>Verwalten von Azure Media Services v3-Konten

Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Weitere Informationen finden Sie unter [Speicherkonten](storage-account-concept.md).

## <a name="moving-a-media-services-account-between-subscriptions"></a>Verschieben eines Media Services-Kontos zwischen Abonnements 

Wenn Sie ein Media Services-Konto in ein neues Abonnement verschieben möchten, müssen Sie zunächst die gesamte Ressourcengruppe, die das Media Services-Konto enthält, in das neue Abonnement verschieben. Dabei müssen alle angefügten Ressourcen verschoben werden: Azure Storage-Konten, Azure CDN-Profile usw. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md). Wie bei allen Ressourcen in Azure kann das Verschieben einer Ressourcengruppe einige Zeit in Anspruch nehmen.

> [!NOTE]
> Media Services v3 unterstützt das mehrinstanzenfähige Modell.

### <a name="considerations"></a>Überlegungen

* Erstellen Sie vor der Migration zu einem anderen Abonnement Sicherungen aller Daten in Ihrem Konto.
* Beenden Sie alle Streamingendpunkte und Livestreamingressourcen. Während der Verschiebung der Ressourcengruppe können die Benutzer nicht auf Ihre Inhalte zugreifen. 

> [!IMPORTANT]
> Starten Sie den Streamingendpunkt erst, nachdem der Verschiebevorgang erfolgreich abgeschlossen wurde.

### <a name="troubleshoot"></a>Problembehandlung 

Wenn ein Media Services-Konto oder ein zugehöriges Azure Storage-Konto nach dem Verschieben der Ressourcengruppe „getrennt“ wird, versuchen Sie, die Speicherkontoschlüssel zu rotieren. Sollte das Rotieren der Speicherkontoschlüssel das Problem mit dem getrennten Media Services-Kontos nicht beheben, stellen Sie im Media Services-Konto über das Menü „Support und Problembehandlung“ eine neue Supportanfrage.  

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines Kontos](create-account-cli-quickstart.md)

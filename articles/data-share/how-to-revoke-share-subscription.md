---
title: Widerrufen eines Freigabeabonnements in Azure Data Share
description: Hier erfahren Sie, wie Sie ein Freigabeabonnement von einem Empfänger mithilfe von Azure Data Share widerrufen.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: how-to
ms.date: 07/10/2019
ms.openlocfilehash: c1c049d467cdf07962719f0dc257acbf47631aaf
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119683"
---
# <a name="how-to-revoke-a-consumers-share-subscription-in-azure-data-share"></a>Widerrufen eines Freigabeabonnements eines Consumers in Azure Data Share

In diesem Artikel wird erläutert, wie ein Freigabeabonnement für einen oder mehrere Consumer über Azure Data Share widerrufen wird. Hierdurch wird der Consumer daran gehindert, weitere Momentaufnahmen auszulösen. Wenn der Consumer noch keine Momentaufnahme ausgelöst hat, werden die Daten nach dem Aufheben des Freigabeabonnements nicht mehr empfangen. Wenn zuvor eine Momentaufnahme ausgelöst wurde, verbleiben die letzten Daten im Konto des Consumers.

## <a name="navigate-to-a-sent-data-share"></a>Navigation zu einer gesendeten Datenfreigabe

Navigieren Sie in Azure Data Share zur gesendeten Freigabe, und wählen Sie die Registerkarte **Freigabeabonnements** aus.

![Aufheben eines Freigabeabonnements](./media/how-to/how-to-revoke-share-subscription/revoke-share-subscription.png) 

Aktivieren Sie die Felder neben den Empfängern, deren Freigabeabonnements Sie löschen möchten, und klicken Sie auf **Widerrufen**. Der Consumer erhält für seine Daten keine Aktualisierungen mehr.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Überwachen Ihrer Datenfreigaben](how-to-monitor.md).
---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.custom: seo-python-october2019
ms.openlocfilehash: 317e47144a716bd1bea5549aed1db0a02250d353
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77466088"
---
## <a name="what-is-queue-storage"></a>Was ist der Warteschlangenspeicher?

Die Warteschlangenspeicherung in Azure ist ein Dienst zur Speicherung großer Anzahlen von Nachrichten, auf die von überall auf der Welt mit authentifizierten Anrufen über HTTP oder HTTPS zugegriffen werden kann. Eine einzelne Warteschlangennachricht kann bis zu 64 KB groß sein, und eine Warteschlange kann Millionen von Nachrichten enthalten. Deren Anzahl ist nur durch die Kapazität des Speicherkontos begrenzt. Warteschlangenspeicher wird häufig verwendet, um ein Arbeits-Backlog zur asynchronen Verarbeitung zu erstellen.

## <a name="queue-service-concepts"></a>Konzepte des Warteschlangendiensts

Der Azure-Warteschlangendienst umfasst die folgenden Komponenten:

![Azure-Warteschlangendienst – Komponenten](./media/storage-queue-concepts-include/azure-queue-service-components.png)

* **URL-Format:** Warteschlangen sind über das folgende URL-Format adressierbar: http://`<storage account>`.queue.core.windows.net/`<queue>`
  
    Mit der folgenden URL kann eine der Warteschlangen im Diagramm adressiert werden:  
  
    `http://myaccount.queue.core.windows.net/incoming-orders`

* **Speicherkonto:** Alle Zugriffe auf den Azure-Speicher erfolgen über ein Speicherkonto. Weitere Informationen zu Speicherkonten finden Sie in der [Übersicht zu Speicherkonten][../articles/storage/common/storage-account-overview.md].
* **Warteschlange:** Eine Warteschlange enthält einen Satz von Nachrichten. Alle Nachrichten müssen sich in Warteschlangen befinden. Beachten Sie, dass der Warteschlangenname nur aus Kleinbuchstaben bestehen darf. Informationen zum Benennen von Warteschlangen finden Sie unter [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **Nachricht:** Eine Nachricht in einem beliebigen Format und mit einer Größe von bis zu 64 KB. Eine Nachricht kann maximal sieben Tage in der Warteschlange verbleiben. Für Version 2017-07-29 oder höhere Versionen kann die maximale Gültigkeitsdauer eine beliebige positive Zahl sein. Mit -1 wird angegeben, dass die Nachricht nicht abläuft. Wird dieser Parameter ausgelassen, beträgt die Standardgültigkeitsdauer sieben Tage.


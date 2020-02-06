---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.openlocfilehash: 82d122ed236dc72ced7ebafe2301ef5f1143897f
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963653"
---
Verwenden Sie für jede Funktions-App ein separates Speicherkonto, um die Leistung zu maximieren. Dies ist besonders wichtig, wenn Sie über Durable Functions oder durch Event Hub ausgelöste Funktionen verfügen, die eine große Menge an Speichertransaktionen generieren. Wenn Ihre Anwendungslogik mit Azure Storage interagiert (direkt mit dem Storage SDK oder über eine der Speicherbindungen), sollten Sie ein dediziertes Speicherkonto verwenden. Wenn Sie z. B. über eine von Event Hub ausgelöste Funktion verfügen, die Daten in Blob Storage schreibt, verwenden Sie zwei Speicherkonten &mdash; eines für die Funktions-App und ein weiteres für die Blobs, die von der Funktion gespeichert werden.
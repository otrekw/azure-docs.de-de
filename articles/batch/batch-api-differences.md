---
title: Unterschiede zwischen Verwaltungs-APIs und Dienst-APIs – Azure Batch | Microsoft-Dokumentation
description: APIs können auf den verschiedenen Ebenen des Azure Batch-Diensts verwendet werden.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 181515c0f497af8ffadcb909c13e51a40bfbf3b0
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78672758"
---
# <a name="service-level-and-management-level-apis"></a>APIs auf Dienst- und Verwaltungsebene

Azure Batch verfügt über zwei Sätze von APIs, einen für die Dienstebene und einen für die Verwaltungsebene. Ihre Namen sind oft ähnlich, aber sie führen zu unterschiedlichen Ergebnissen. Wenn Sie Aktivitätsprotokolle wünschen, müssen Sie die Verwaltungs-APIs verwenden. APIs auf Dienstebene umgehen die Azure-Ressourcenverwaltungsschicht und werden nicht protokolliert.


Batch-Verwaltung und Batch-Dienst verfügen z. B. über APIs für Pool. 
- Diese API zum Löschen des Pools ist direkt auf das Batch-Konto ausgerichtet: https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- Diese API zum Löschen des Pools https://docs.microsoft.com/rest/api/batchmanagement/pool/delete ist auf die Schicht „management.azure.com“ ausgerichtet.


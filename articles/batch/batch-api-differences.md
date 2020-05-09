---
title: Unterschiede zwischen Verwaltungs-APIs und Dienst-APIs
description: APIs können auf den verschiedenen Ebenen des Azure Batch-Diensts verwendet werden.
ms.topic: conceptual
ms.date: 02/26/2020
ms.custom: seodec18
ms.openlocfilehash: 33b8c5980aba1090155d6b136c6707e928666abf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115396"
---
# <a name="service-level-and-management-level-apis"></a>APIs auf Dienst- und Verwaltungsebene

Azure Batch verfügt über zwei Sätze von APIs, einen für die Dienstebene und einen für die Verwaltungsebene. Ihre Namen sind oft ähnlich, aber sie führen zu unterschiedlichen Ergebnissen. Wenn Sie Aktivitätsprotokolle wünschen, müssen Sie die Verwaltungs-APIs verwenden. APIs auf Dienstebene umgehen die Azure-Ressourcenverwaltungsschicht und werden nicht protokolliert.


Batch-Verwaltung und Batch-Dienst verfügen z. B. über APIs für Pool. 
- Diese API zum Löschen des Pools ist direkt auf das Batch-Konto ausgerichtet: https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- Diese API zum Löschen des Pools https://docs.microsoft.com/rest/api/batchmanagement/pool/delete ist auf die Schicht „management.azure.com“ ausgerichtet.


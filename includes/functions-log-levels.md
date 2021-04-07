---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/04/2020
ms.author: glenga
ms.openlocfilehash: 72588ccfd9ad00305cbdeaf8132999731d86b9b1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92164909"
---
Jedem Protokoll wird eine *Protokollebene* zugewiesen. Der Wert ist eine ganze Zahl, die die relative Wichtigkeit angibt:

|LogLevel    |Code| BESCHREIBUNG |
|------------|---|--------------|
|Trace       | 0 |Protokolle, die die ausführlichsten Meldungen enthalten. Diese Meldungen enthalten möglicherweise sensible Anwendungsdaten. Sie sind standardmäßig deaktiviert und sollten nie in einer Produktionsumgebung aktiviert werden.|
|Debuggen       | 1 | Protokolle, die für interaktive Untersuchungen während der Entwicklung verwendet werden. Diese Protokolle sollten hauptsächlich für das Debuggen hilfreiche Informationen enthalten. Sie besitzen keinen langfristigen Nutzen. |
|Information | 2 | Protokolle, die den allgemeinen Ablauf der Anwendung nachverfolgen. Diese Protokolle sollten einen langfristigen Nutzen besitzen. |
|Warnung     | 3 | Dies sind Protokolle, die ein ungewöhnliches oder unerwartetes Ereignis im Anwendungsfluss hervorheben, jedoch nicht bewirken, dass die Anwendung beendet wird. |
|Fehler       | 4 | Dies sind Protokolle, die hervorheben, wenn der aktuelle Ausführungsfluss aufgrund eines Fehlers beendet wird. Diese Fehler sollten auf einen Fehler im Zusammenhang mit der aktuellen Aktivität und nicht auf einen anwendungsweiten Fehler hinweisen. |
|Kritisch    | 5 | Protokolle, die einen nicht behebbaren Anwendungs- oder Systemabsturz beschreiben oder einen schwerwiegenden Fehler, der unmittelbare Aufmerksamkeit erfordert. |
|Keine        | 6 | Hiermit wird die Protokollierung für die angegebene Kategorie deaktiviert. |

Durch die Konfiguration der [*host.json*-Datei](../articles/azure-functions/functions-host-json.md) wird bestimmt, welcher Protokollierungsgrad von einer Funktions-App an Application Insights gesendet wird.  

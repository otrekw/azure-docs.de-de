---
title: Behandeln von Problemen mit Fehlercodes in Azure Stream Analytics
description: Beheben Sie Probleme in Azure Stream Analytics mit internen Fehlercodes.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: c8c792d548fc953cb5d059f322ad1f3139c91562
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595119"
---
# <a name="azure-stream-analytics-internal-error-codes"></a>Interne Fehlercodes in Azure Stream Analytics

Unerwartetes Verhalten Ihres Azure Stream Analytics-Auftrags können Sie mithilfe von Aktivitätsprotokollen und Ressourcenprotokollen debuggen. In diesem Artikel ist die Beschreibung für jeden internen Fehlercode aufgeführt. Interne Fehler sind allgemeine Fehler, die innerhalb der Stream Analytics-Plattform ausgelöst werden, wenn Stream Analytics nicht unterscheiden kann, ob es sich um einen Fehler der internen Verfügbarkeit oder einen Fehler im System handelt.

## <a name="kafkainvalidrequest"></a>KafkaInvalidRequest

* **Ursache:** Die an den Kafka-Server gesendete Anforderung ist ungültig.

## <a name="kafkainputerror"></a>KafkaInputError

* **Ursache:** Bei der Kafka-Eingabe ist ein Problem aufgetreten.

## <a name="cosmosdboutputbatchsizetoolarge"></a>CosmosDBOutputBatchSizeTooLarge

* **Ursache:** Die zum Schreiben in Cosmos DB verwendete Batchgröße ist zu groß. 
* **Empfehlung**: Wiederholen Sie den Vorgang mit einer kleineren Batchgröße.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung für Eingangsverbindungen](stream-analytics-troubleshoot-input.md)
* [Problembehandlung von Azure Stream Analytics-Ausgaben](stream-analytics-troubleshoot-output.md)
* [Problembehandlung von Azure Stream Analytics-Abfragen](stream-analytics-troubleshoot-query.md)
* [Problembehandlung von Azure Stream Analytics mit Ressourcenprotokollen](stream-analytics-job-diagnostic-logs.md)
* [Datenfehler in Azure Stream Analytics](data-errors.md)
---
title: Wiederholen von Tasks nach einem Fehler
description: Suchen nach Fehlern und Wiederhole von Tasks.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: azure-batch
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/15/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 94ed936e619461a2dbf7ec837c2d80e21c01c88e
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77919990"
---
# <a name="detecting-and-handling-batch-service-errors"></a>Erkennen und Behandeln von Batch-Dienstfehlern

Denken Sie unbedingt daran, bei der Arbeit mit einer REST-Dienst-API auf Fehler zu prüfen. Es ist nicht ungewöhnlich, dass beim Ausführen von Batchaufträgen Fehler auftreten.

## <a name="common-errors"></a>Häufige Fehler 

- Netzwerkfehler: Hierbei handelt es sich um Anforderungen, die Batch niemals erreicht haben, oder die Batch-Antwort hat den Client nicht rechtzeitig erreicht.
- Interne Serverfehler: Dies sind standardmäßige HTTP-Antworten mit dem Statuscode 5xx.
- Drosselung kann zu Fehlern wie HTTP-Antworten mit dem Statuscode 429 oder 503 mit dem Retry-after-Header führen.
- 4xx-Fehler, die Fehler wie AlreadyExists und InvalidOperation enthalten. Dies bedeutet, dass sich die Ressource nicht im richtigen Zustand für den Statusübergang befindet.

Ausführliche Informationen zu den verschiedenen Arten von Fehlercodes und bestimmten Fehlercodes finden Sie unter [Status- und Fehlercodes für Batch](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).

## <a name="when-to-retry"></a>Wiederholungsversuche

Sie werden von den Batch-APIs benachrichtigt, wenn ein Fehler auftritt. Sie können alle wiederholt werden, und alle enthalten einen globalen Wiederholungshandler für diesen Zweck. Es empfiehlt sich, diesen integrierten Mechanismus zu verwenden.

Nach einem Fehler sollten Sie eine Zeit lang (mehrere Sekunden zwischen Wiederholungsversuchen) warten, bevor Sie den Vorgang wiederholen. Wenn Wiederholungsversuche zu häufig oder zu schnell sind, führt der Wiederholungshandler Drosselung aus.

### <a name="for-more-information"></a>Weitere Informationen finden Sie unter  

[Batch-APIs und -Tools](batch-apis-tools.md): Links zu API-Referenzinformationen. Die .NET-API verfügt z. B. über eine [RetryPolicyProvider-Klasse]( https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.retrypolicyprovider?view=azure-dotnet), in der die erforderliche Wiederholungsrichtlinie angegeben werden sollte. 

Ausführliche Informationen zu jeder API und ihren standardmäßigen Wiederholungsrichtlinien finden Sie unter [Batchstatus und Fehlercodes](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).

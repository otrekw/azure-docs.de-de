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
ms.openlocfilehash: eda567fda13d6caca679d0ce4947e042eca9530d
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77651932"
---
# <a name="detecting-and-handling-batch-service-errors"></a>Erkennen und Behandeln von Batch-Dienstfehlern

Denken Sie unbedingt daran, bei der Arbeit mit einer REST-Dienst-API auf Fehler zu prüfen. Es ist nicht ungewöhnlich, dass beim Ausführen von Batchaufträgen Fehler auftreten.

## <a name="common-errors"></a>Häufige Fehler 

- Netzwerkfehler: Hierbei handelt es sich um Anforderungen, die Batch niemals erreicht haben, oder die Batch-Antwort hat den Client nicht rechtzeitig erreicht.
- Interne Serverfehler: Dies sind standardmäßige HTTP-Antworten mit dem Statuscode 5xx.
- Drosselung kann zu Fehlern wie HTTP-Antworten mit dem Statuscode 429 oder 503 mit dem Retry-after-Header führen.
- 4xx-Fehler, die Fehler wie AlreadyExists und InvalidOperation enthalten. Dies bedeutet, dass sich die Ressource nicht im richtigen Zustand für den Statusübergang befindet.

## <a name="when-to-retry"></a>Wiederholungsversuche

Sie werden von den Batch-APIs benachrichtigt, wenn ein Fehler auftritt. Sie können alle wiederholt werden, und alle enthalten einen globalen Wiederholungshandler für diesen Zweck. Es empfiehlt sich, diesen integrierten Mechanismus zu verwenden.

Nach einem Fehler sollten Sie eine Zeit lang (mehrere Sekunden zwischen Wiederholungsversuchen) warten, bevor Sie den Vorgang wiederholen. Wenn Wiederholungsversuche zu häufig oder zu schnell sind, führt der Wiederholungshandler Drosselung aus.


Ausführliche Informationen zu jeder API und ihren standardmäßigen Wiederholungsrichtlinien finden Sie unter [Batchstatus und Fehlercodes](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).

---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: e4147fbb27c8538f801f6c49f8b535a283faf50f
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325902"
---
Die Clientanwendung muss wissen, ob eine Äußerung für die Anwendung unsinnig oder ungeeignet ist. Die Absicht **None** (Keine) wird jeder Anwendung bei ihrer Erstellung hinzugefügt, um zu bestimmen, ob eine Äußerung nicht von der Clientanwendung beantwortet werden sollte.

Wenn LUIS für eine Äußerung die Absicht **None** (Keine) zurückgibt, kann Ihre Clientanwendung fragen, ob der Benutzer die Unterhaltung beenden oder weitere Anweisungen zum Fortsetzen der Unterhaltung angeben möchte. 

Wenn Sie die Absicht **None** leer lassen, wird eine Äußerung, die außerhalb der Motivdomäne vorhergesagt werden sollte, in einer der vorhandenen Motivdomänenabsichten vorhergesagt. Das Ergebnis ist, dass die Clientanwendung, z. B. ein Chatbot, auf der Grundlage einer falschen Vorhersage falsche Vorgänge ausführt. 

1. Klicken Sie im linken Bereich auf **Intents** (Absichten).

1. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, die für Ihre App zur Pizzabestellung jedoch nicht relevant sind:

    |`None`-Beispieläußerungen|
    |--|
    |`Barking dogs are annoying`|
    |`Order a pizza for me`|
    |`Penguins in the ocean`|

    Diese Beispiele sollten keine Wörter verwenden, die Sie in Ihrer Themendomäne erwarten, z. B. `pizza`, `cheese`, `crust`, `pickup`, `deliver`.
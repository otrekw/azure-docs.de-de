---
title: include file
description: include file
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.openlocfilehash: 1509d175979bb65c467424db5de967f56825a3f9
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91545159"
---
Die Clientanwendung muss wissen, ob eine Äußerung für die Anwendung unsinnig oder ungeeignet ist. Die Absicht **None** (Keine) wird jeder Anwendung bei ihrer Erstellung hinzugefügt, um zu bestimmen, ob eine Äußerung nicht von der Clientanwendung beantwortet werden sollte.

Wenn LUIS für eine Äußerung die Absicht **None** (Keine) zurückgibt, kann Ihre Clientanwendung fragen, ob der Benutzer die Unterhaltung beenden oder weitere Anweisungen zum Fortsetzen der Unterhaltung angeben möchte.

Wenn Sie die Absicht **None** leer lassen, wird eine Äußerung, die außerhalb der Motivdomäne vorhergesagt werden sollte, in einer der vorhandenen Motivdomänenabsichten vorhergesagt. Das Ergebnis ist, dass die Clientanwendung, z. B. ein Chatbot, auf der Grundlage einer falschen Vorhersage falsche Vorgänge ausführt.

1. Klicken Sie im linken Bereich auf **Intents** (Absichten).

1. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, die für Ihre App zur Pizzabestellung jedoch nicht relevant sind:

    |`None`-Beispieläußerungen|
    |--|
    |`Barking dogs are annoying`|
    |`Penguins in the ocean`|

    In diesen Beispielen dürfen keine Wörter verwendet werden, die Sie in Ihrer Themendomäne erwarten (beispielsweise `pizza`, `cheese`, `crust`, `pickup` oder `deliver`).
---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.author: diberry
ms.openlocfilehash: 956aa308bf1cb3736c491031239661ec6b295ddb
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77279714"
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
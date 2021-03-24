---
title: 'Filtern von Obszönitäten: Translator'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die Filterung von Obszönitäten, um in Azure Cognitive Services Translator den Grad der Obszönität für Ihren übersetzten Text zu bestimmen.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: lajanuar
ms.openlocfilehash: 9f389d669e69dbfa6ec6d4d0b4716d2367443f17
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98896798"
---
# <a name="add-profanity-filtering-with-the-translator"></a>Hinzufügen der Filterung von Obszönitäten mit Translator

In der Regel behält der Übersetzungsdienst Obszönitäten, die im Quelltext vorhanden sind, in der Übersetzung bei. Der Grad an Obszönitäten und der Kontext, bei dem Wörter als obszön angesehen werden, unterscheiden sich je nach Kultur. Daher kann der Obszönitätsgrad in der Zielsprache unter Umständen verstärkt oder reduziert werden.

Wenn Sie keine Obszönitäten in der Übersetzung wünschen (unabhängig davon, ob diese im Quelltext vorhanden sind), können Sie die Obszönitäten-Filteroption in der Translate()-Methode verwenden. Mit dieser Option können Sie auswählen, ob Obszönitäten gelöscht oder entsprechend gekennzeichnet werden sollen oder ob keine Aktion durchgeführt werden soll.

Für die Translate()-Methode wird der Parameter „options“ verwendet, der das neue Element „ProfanityAction“ enthält. Die akzeptierten Werte für ProfanityAction sind „NoAction“, „Marked“ und „Deleted“.

## <a name="accepted-values-of-profanityaction-and-examples"></a>Akzeptierte Werte für ProfanityAction und Beispiele
|ProfanityAction-Wert | Aktion | Beispiel: Ausgangssprache – Japanisch | Beispiel: Zielsprache – Englisch|
| :---|:---|:---|:---|
| NoAction | Standard. Entspricht dem Fall, in dem die Option nicht festgelegt wird. Die Obszönität wird aus der Ausgangs- in die Zielsprache übernommen. | 彼は変態です。 | He is a jerk. (Er ist ein Vollidiot.) |
| Marked | Obszöne Wörter werden von den XML-Tags \<profanity> und \</profanity> umschlossen. | 彼は変態です。 | He is a \<profanity>jerk\</profanity>. (Er ist ein <profanity>Vollidiot</profanity>.) |
| Deleted | Obszöne Begriffe werden aus der Ausgabe entfernt, und es wird kein Ersatzbegriff bereitgestellt. | 彼は。 | He is a. (Er ist ein.) |

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Anwenden der Filterung von Obszönitäten in Ihrem Translator-Aufruf](reference/v3-0-translate.md)

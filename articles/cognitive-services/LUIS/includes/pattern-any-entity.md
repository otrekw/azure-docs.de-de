---
title: include file
description: include file
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.openlocfilehash: 5eb2517b4b71b61dd70406f345c005d007d9e3ef
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91535955"
---
Mit der Entität „pattern.any“ können Sie Freiformdaten finden, bei denen die Formulierung der Entität erschwert, das Ende der Entität vom Rest der Äußerung zu unterscheiden.

Eine Personalwesen-App hilft Mitarbeitern, Unternehmensformulare zu finden.

|Äußerung|
|--|
|Wo ist **HRF-123456**?|
|Wer hat **HRF-123234** erstellt?|
|Ist **HRF-456098** in Französisch veröffentlicht?|

Jedes Formular verfügt jedoch sowohl über einen formatierten Name, der in der obigen Tabelle verwendet wird, als auch einen Anzeigenamen, z.B. `Request relocation from employee new to the company 2018 version 5`.

Äußerungen mit dem Anzeigenamen sehen folgendermaßen aus:

|Äußerung|
|--|
|Wo ist **Request relocation from employee new to the company 2018 version 5**?|
|Wer hat **Request relocation from employee new to the company 2018 version 5** erstellt?|
|Wurde **Request relocation from employee new to the company 2018 version 5** in Französisch veröffentlicht?|

Die unterschiedlichen Längen enthalten Wörter, die es LUIS erschweren, das Ende von Entitäten zu erkennen. Mit einer Pattern.any-Entität in einem Muster können Sie den Anfang und das Ende des Formularnamens angeben, damit LUIS diesen ordnungsgemäß extrahiert.

|Beispiel für eine Vorlagenäußerung|
|--|
|Wo ist {FormName}[?]|
|Wer hat {FormName} erstellt[?]|
|Wurde {FormName} in Französisch veröffentlicht[?]|

Sehen Sie sich die [Referenzinformationen](../reference-entity-pattern-any.md) zu „Pattern.any“ an.
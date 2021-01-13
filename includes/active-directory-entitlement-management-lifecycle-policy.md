---
title: include file
description: include file
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 10/15/2019
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: e971af5d415ea788f97ab11b58541e629c7eb0ed
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "76694297"
---
## <a name="lifecycle"></a>Lebenszyklus

Auf der Registerkarte **Lebenszyklus** geben Sie an, wann die Zuweisung eines Benutzers für das Zugriffspaket abläuft. Sie können auch angeben, ob Benutzer ihre Zuweisungen verlängern können.

1. Legen Sie im Abschnitt **Ablauf** die Option **Zugriffspaket läuft ab** auf **An Datum**, **Anzahl Tage** oder **Nie** fest.

    Wählen Sie für **An Datum** ein Ablaufdatum in der Zukunft aus.

    Geben Sie für **Anzahl Tage** eine Zahl zwischen 0 und 3.660 Tagen an.

    Basierend auf Ihrer Auswahl läuft die Zuweisung eines Benutzers zum Zugriffspaket an einem bestimmten Datum, nach einer bestimmten Anzahl von Tagen nach der Genehmigung oder nie ab.

1. Klicken Sie auf **Erweiterte Ablaufeinstellungen anzeigen**, um zusätzliche Einstellungen anzuzeigen.

    ![Zugriffspaket – Lebenszyklusablaufeinstellungen](./media/active-directory-entitlement-management-lifecycle-policy/expiration.png)

1. Um Benutzern zu erlauben, ihre Zuweisungen zu erweitern, setzen Sie **Benutzern die Verlängerung des Zugriffs erlauben** auf **Ja**.

    Wenn Verlängerungen in der Richtlinie zulässig sind, erhält der Benutzer 14 Tage und auch 1 Tag vor Ablauf seiner Zugriffspaketzuweisung eine E-Mail, die ihn auffordert, die Zuweisung verlängern zu lassen. Wenn der Benutzer eine Anforderung zum Erweitern des Zugriffs sendet, muss das Verlängerungsdatum vor oder nach dem Ablauf der Zuweisungen liegen, wie in der Richtlinie definiert wurde, mit der dem Benutzer Zugriff auf das Zugriffspaket gewährt wurde. Wenn die Richtlinie beispielsweise angibt, dass Zuweisungen am 30. Juni ablaufen sollen, ist die maximale Verlängerung, die ein Benutzer anfordern kann, der 30. Juni.

    Wenn der Zugriff eines Benutzers verlängert wird, kann er das Zugriffspaket nach dem angegebenen Verlängerungsdatum (Datum, das in der Zeitzone des Benutzers festgelegt ist, der die Richtlinie erstellt hat) nicht mehr anfordern.

1. Wenn eine Genehmigung zum Gewähren einer Verlängerung erforderlich sein soll, legen Sie **Genehmigung erforderlich, um Verlängerung zu gewähren** auf **Ja** fest.

    Es werden die auf der Registerkarte **Anforderungen** festgelegten Genehmigungseinstellungen verwendet.

1. Klicken Sie auf **Weiter** oder **Aktualisieren**.

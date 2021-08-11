---
title: Sicherheitserkennungspaket für Azure Application Insights
description: Überwachen Sie Anwendungen mit Azure Application Insights und der intelligenten Erkennung auf potenzielle Sicherheitsprobleme.
ms.topic: conceptual
ms.date: 12/12/2017
ms.openlocfilehash: 2db677b5e08c7b890f66dfa1d5cc227da4dd8a34
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2021
ms.locfileid: "111536779"
---
# <a name="application-security-detection-pack-preview"></a>Paket zur Anwendungsicherheitserkennung (Vorschau)

Die intelligente Erkennung analysiert die von Ihrer Anwendung generierten Telemetriedaten automatisch und erkennt potenzielle Sicherheitsprobleme. Dadurch können Sie potenzielle Sicherheitsprobleme identifizieren. Sie können diese Probleme minimieren, indem Sie die Anwendung reparieren oder die erforderlichen Sicherheitsmaßnahmen ergreifen.

Diese Funktion erfordert keine spezielle Einrichtung, abgesehen von der [Konfiguration Ihrer App für das Senden von Telemetriedaten](./usage-overview.md).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wann erhalte ich diese Art von Benachrichtigung der intelligenten Erkennung?
Es gibt drei Arten von Sicherheitsproblemen, die erkannt werden:
1. Unsicherer URL-Zugriff: Auf eine URL in der Anwendung kann über HTTP und HTTPS zugegriffen werden. In der Regel sollte eine URL, die HTTPS-Anforderungen akzeptiert, keine HTTP-Anforderungen akzeptieren. Diese Erkennung kann auf einen Fehler oder ein Sicherheitsproblem in Ihrer Anwendung hinweisen.
2. Unsicheres Formular: Ein Formular (oder eine andere „POST“-Anforderung) in der Anwendung verwendet HTTP statt HTTPS. Die Verwendung von HTTP kann die vom Formular gesendeten Benutzerdaten gefährden.
3. Verdächtige Benutzeraktivität: Derselbe Benutzer greift ungefähr zu derselben Zeit aus mehreren Ländern oder Regionen auf die Anwendung zu. Beispiel: Derselbe Benutzer hat innerhalb einer Stunde von Spanien und den USA aus auf die Anwendung zugegriffen. Diese Erkennung gibt einen potenziell böswilligen Zugriffsversuch auf Ihre Anwendung an.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Liegt in meiner App definitiv ein Sicherheitsproblem vor?
Eine Benachrichtigung bedeutet nicht zwingend, dass in Ihrer App ein Sicherheitsproblem vorliegt. Eine Erkennung eines der oben genannten Szenarien kann in vielen Fällen auf ein Sicherheitsproblem hinweisen. In anderen Fällen kann die Erkennung einen natürlichen geschäftlichen Grund haben und ignoriert werden.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>Wie behebe ich die Erkennung „Unsicherer URL-Zugriff“?
1. **Selektierung.** Die Benachrichtigung enthält die Anzahl von Benutzern, die auf unsichere URLs zugegriffen haben, und die URL, die am häufigsten von unsicheren Zugriffen betroffen war. Mithilfe dieser Informationen können Sie dem Problem eine Priorität zuweisen.
3. **Umfang.** Wie viel Prozent der Benutzer haben auf unsichere URLs zugegriffen? Wie viele URLs waren betroffen? Diese Informationen können Sie der Benachrichtigung entnehmen.
4. **Diagnose.** Die Erkennung enthält die Liste der unsicheren Anforderungen, die Liste der betroffenen URLs und die Liste der betroffenen Benutzer, sodass Sie das Problem anhand dieser Informationen weiter diagnostizieren können.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>Wie behebe ich die Erkennung „Unsicheres Formular“?
1. **Selektierung.** Die Benachrichtigung enthält die Anzahl der unsicheren Formulare und die Anzahl der Benutzer, deren Daten potenziell gefährdet sind. Mithilfe dieser Informationen können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Welches Formular war an der größten Anzahl unsicherer Übertragungen beteiligt, und wie ist die zeitliche Verteilung unsicherer Übertragungen? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Die Erkennung enthält die Liste der unsicheren Formulare und eine Aufschlüsselung der unsicheren Übertragungen für jedes Formular, sodass Sie das Problem anhand dieser Informationen weiter diagnostizieren können.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>Wie behebe ich die Erkennung „Verdächtige Benutzeraktivität“?
1. **Selektierung.** Die Benachrichtigung enthält die Anzahl der verschiedenen Benutzer, die das verdächtige Verhalten dargelegt haben. Mithilfe dieser Informationen können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Aus welchen Ländern oder Regionen stammen die verdächtigen Anforderungen? Welcher Benutzer war am verdächtigsten? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Die Erkennung enthält die Liste der verdächtigen Benutzer und die Liste der Länder oder Regionen für jeden Benutzer, sodass Sie das Problem anhand dieser Informationen weiter diagnostizieren können.

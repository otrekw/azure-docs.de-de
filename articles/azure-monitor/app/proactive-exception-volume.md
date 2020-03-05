---
title: Anormaler Anstieg in Ausnahmevolume – Azure Application Insights
description: Überwachen Sie Anwendungsausnahmen auf ungewöhnliche Muster im Ausnahmevolume mit der intelligenten Erkennung in Azure Application Insights.
ms.topic: conceptual
ms.date: 12/08/2017
ms.openlocfilehash: a08fae4774a8afb9959f55ea3196cd1a45c33439
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671765"
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Anormaler Anstieg in Ausnahmevolume (Vorschau)

Application Insights analysiert die in Ihrer Anwendung ausgelösten Ausnahmen automatisch und kann Sie vor ungewöhnlichen Mustern in Ihrer Ausnahmetelemetrie warnen.

Diese Funktion erfordert keine spezielle Einrichtung, abgesehen von der [Konfiguration von Ausnahmemeldungen](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) für Ihre App. Sie ist aktiv, wenn Ihre App genügend Ausnahmetelemetriedaten generiert.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wann erhalte ich diese Art von Benachrichtigung der intelligenten Erkennung?
Sie können diese Art von Benachrichtigung erhalten, wenn Ihre App eine anormale Zunahme der Anzahl von Ausnahmen eines bestimmten Typs während eines Tages aufweist, im Vergleich zu einer Baseline, die über die letzten sieben Tage berechnet wurde.
Machine Learning-Algorithmen werden verwendet, um den Anstieg der Anzahl der Ausnahmen zu erkennen, während gleichzeitig ein natürliches Wachstum der Anwendungsnutzung berücksichtigt wird.

## <a name="does-my-app-definitely-have-a-problem"></a>Liegt wirklich ein Problem mit meiner App vor?
Nein, eine Benachrichtigung bedeutet nicht zwingend, dass ein Problem mit Ihrer App vorliegt. Obwohl eine übermäßige Anzahl von Ausnahmen in der Regel auf ein Anwendungsproblem hinweist, haben diese Ausnahmen möglicherweise keine Auswirkungen und werden von Ihrer Anwendung ordnungsgemäß verarbeitet.

## <a name="how-do-i-fix-it"></a>Wie behebe ich das Problem?
Die Benachrichtigungen umfassen Diagnoseinformationen zur Unterstützung des Diagnoseprozesses:
1. **Selektierung.** Diese Benachrichtigung zeigt an, wie viele Benutzer oder wie viele Anforderungen betroffen sind. Dadurch können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Betrifft das Problem den gesamten Datenverkehr oder nur bestimmte Vorgänge? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Die Erkennung enthält Informationen über die Methode, von der die Ausnahme ausgelöst wurde, und den Ausnahmetyp. Zur weiteren Diagnose des Problems können Sie auch die entsprechenden Elemente und Berichte verwenden, die mit weiterführenden Informationen verknüpft sind.

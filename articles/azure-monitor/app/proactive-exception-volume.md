---
title: Anormaler Anstieg in Ausnahmevolume – Azure Application Insights
description: Überwachen Sie Anwendungsausnahmen auf ungewöhnliche Muster im Ausnahmevolume mit der intelligenten Erkennung in Azure Application Insights.
ms.topic: conceptual
ms.date: 12/08/2017
ms.openlocfilehash: 4220415a0ab907c2cdf9268a59a4e85400cad86c
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2021
ms.locfileid: "111536692"
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Anormaler Anstieg in Ausnahmevolume (Vorschau)

>[!NOTE]
>Sie können Ihre Application Insight-Ressourcen zu der auf Warnungen basierenden intelligenten Erkennung (Vorschauversion) migrieren. Bei der Migration werden Warnungsregeln für die verschiedenen Module der intelligenten Erkennung erstellt. Nach der Erstellung können Sie diese Regeln wie alle anderen Azure Monitor-Warnungsregeln verwalten und konfigurieren. Sie können auch Aktionsgruppen für diese Regeln konfigurieren, was mehrere Methoden für das Ausführen von Aktionen oder für das Auslösen von Benachrichtigungen bei neuen Erkennungen ermöglicht.
>
> Weitere Informationen finden Sie unter [Migration zu intelligenten Erkennungswarnungen](../alerts/alerts-smart-detections-migration.md).

Die intelligente Erkennung analysiert die in Ihrer Anwendung ausgelösten Ausnahmen automatisch und kann Sie vor ungewöhnlichen Mustern in Ihrer Ausnahmetelemetrie warnen.

Diese Funktion erfordert keine spezielle Einrichtung, abgesehen von der [Konfiguration von Ausnahmemeldungen](./asp-net-exceptions.md#set-up-exception-reporting) für Ihre App. Sie ist aktiv, wenn Ihre App genügend Ausnahmetelemetriedaten generiert.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wann erhalte ich diese Art von Benachrichtigung der intelligenten Erkennung?
Sie erhalten diesen Typ von Benachrichtigung, wenn Ihre App einen ungewöhnlichen Anstieg bei der Anzahl von Ausnahmen eines bestimmten Typs während eines Tages zeigt. Diese Anzahl wird mit einer Baseline verglichen, die über die letzten sieben Tage berechnet wurde.
Mithilfe von Algorithmen für maschinelles Lernen wird der Anstieg bei der Anzahl der Ausnahmen erkannt, wobei gleichzeitig ein natürliches Wachstum der Anwendungsnutzung berücksichtigt wird.

## <a name="does-my-app-definitely-have-a-problem"></a>Liegt wirklich ein Problem mit meiner App vor?
Nein, eine Benachrichtigung bedeutet nicht zwingend, dass ein Problem mit Ihrer App vorliegt. Obwohl eine übermäßige Anzahl von Ausnahmen in der Regel auf ein Anwendungsproblem hinweist, haben diese Ausnahmen möglicherweise keine Auswirkungen und werden von Ihrer Anwendung ordnungsgemäß verarbeitet.

## <a name="how-do-i-fix-it"></a>Wie behebe ich das Problem?
Die Benachrichtigungen umfassen Diagnoseinformationen zur Unterstützung des Diagnoseprozesses:
1. **Selektierung.** Diese Benachrichtigung zeigt an, wie viele Benutzer oder wie viele Anforderungen betroffen sind. Mithilfe dieser Informationen können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Betrifft das Problem den gesamten Datenverkehr oder nur bestimmte Vorgänge? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Die Erkennung enthält Informationen zur Methode, von der die Ausnahme ausgelöst wurde, und zum Ausnahmetyp. Zur weiteren Diagnose des Problems können Sie auch die entsprechenden Elemente und Berichte verwenden, die mit weiterführenden Informationen verknüpft sind.

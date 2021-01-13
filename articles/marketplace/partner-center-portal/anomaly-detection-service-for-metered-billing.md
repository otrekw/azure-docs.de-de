---
title: 'Anomalieerkennungsdienst für getaktete Abrechnung: Microsoft Azure Marketplace'
description: Erfahren Sie mehr über die Erkennung von Anomalien, den Zeitpunkt des Versands von Benachrichtigungen, den Umgang damit und über Supportoptionen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.author: mingshen
author: mingshen-ms
ms.date: 06/10/2020
ms.openlocfilehash: 909b818c60932f5b1d6071ea1ccf0ee4c76173b4
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92280530"
---
# <a name="anomaly-detection-service-for-metered-billing"></a>Anomalieerkennungsdienst für getaktete Abrechnung

Mit dem [Messdienst in Marketplace](marketplace-metering-service-apis-faq.md) können Sie Angebote im Rahmen des kommerziellen Marketplace-Programms erstellen, die nicht gemäß Standardeinheiten abgerechnet werden. Bei der getakteten Abrechnung werden Nutzungsereignisse Ihrer Kunden an Microsoft gesendet, woraufhin wir die Abrechnung auf Grundlage dieser Nutzung vornehmen.

Falsch gemeldete Nutzungsdaten können verschiedene Ursachen haben, wie z. B. Programmfehler, falsche Konfigurationen in Ihrer Nutzungserfassung oder Betrug. Falsch gemeldete Nutzungsdaten können zu falschen Kundenabrechnungen und Abrechnungsstreitigkeiten führen.

Um das Risiko abzufedern, arbeitet unser Dienst zur Erkennung von Anomalien mit Algorithmen für maschinelles Lernen, um das normale Verhalten bei getakteter Abrechnung zu bestimmen, die Nutzung bei getakteter Abrechnung zu analysieren und Anomalien mit minimalem Benutzereingriff aufzuzeigen.

Sie werden benachrichtigt, wenn bei Ihrer Nutzung mit getakteter Abrechnung Anomalien festgestellt werden. Dies bietet Ihnen die Gelegenheit, eine Anomalie zu untersuchen und uns zu benachrichtigen, wenn sich bestätigt, dass es sich bei einer Anomalie um ein echtes Problem handelt. An diesem Punkt können Maßnahmen ergriffen werden, um das Problem bei der Kundenabrechnung proaktiv anzugehen.

Zusätzlich zu plötzlichen Spitzen, Schwankungen und Trendänderungen bei Nutzung mit getakteter Abrechnung berücksichtigt unser Modell auch saisonale Effekte. Da die getaktete Abrechnung mittels Überschreitungsdaten kommuniziert wird, ist unser Modell auch in der Lage, lange Zeiträume fehlender Daten problemlos zu bewältigen.

Es folgen Beispiele für Ergebnisse bei Erkennung von Anomalien. Der erwartete Bereich wird als gelbes Band gezeigt. Akzeptable Nutzung mit getakteter Abrechnung wird als grüne Sterne innerhalb des Bands gezeigt. Nutzung mit getakteter Abrechnung außerhalb des Bands wird als roter Punkt gezeigt.  

Außerhalb eines vorhersagbaren Trends erkannte Anomalien:

![Veranschaulicht Anomalien, die außerhalb eines vorhersehbaren Trends erkannt wurden.](media/anomaly-1.png)

Außerhalb eines wiederkehrenden zyklischen Trends erkannte Anomalien:

![Veranschaulicht Anomalien, die außerhalb eines wiederkehrenden zyklischen Trends erkannt wurden.](media/anomaly-2.png)

In einem Aufwärtstrend erkannte Anomalien:

![Veranschaulicht Anomalien, die in einem Aufwärtstrend erkannt wurden.](media/anomaly-3.png)

## <a name="how-anomaly-detection-service-works"></a>Funktionsweise des Dienst zur Erkennung von Anomalien

Die Erkennung von Anomalien ist für die gesamte Nutzung mit getakteter Abrechnung automatisch aktiviert. Wenn Sie die Nutzungsereignisse an Microsoft übermitteln, erstellt der Dienst zur Erkennung von Anomalien auf Grundlage bisheriger Nutzungsdaten ein Modell erwarteter Werte. Dieses Modell wird wöchentlich ausgeführt.

Die Anomalieerkennung funktioniert messtakt- und kundenbezogen. Das bedeutet, dass für jede Verbrauchseinheit mit jedem Kunden ein Modell trainiert wird, das auf dem bisherigen Nutzungsmuster dieser Verbrauchseinheit dieses Kunden basiert.

Das Modell beruht auf der Generierung retrospektiver Konfidenzintervalle. Die Zeitreihenvorhersage ist ein verallgemeinertes additives Modell, das aus einem Trendvorhersage- und einem Saisonalitätsteil besteht. Da das Modell als Regressionsaufgabe formuliert ist, kann es mit langen Zeiträumen fehlender Daten problemlos umgehen. Wenn eine Beobachtung außerhalb der vorhergesagten Konfidenzintervalle liegt, bedeutet dies, dass die Beobachtung nicht auf Grundlage von Verlaufsmustern bei getakteter Abrechnung erklärt werden und daher eine Anomalie darstellen kann.

## <a name="anomaly-detection-notification"></a>Benachrichtigung bei einer Anomalieerkennung

Wir versenden Benachrichtigungen bei Erkennung von Anomalien wöchentlich per E-Mail. Diese E-Mail enthält alle in dieser Woche festgestellten Anomalien für alle Verbrauchseinheiten und Kunden. Diese E-Mail wird an die Kontakte in den Abteilungen **Entwicklung** und **Support** gesendet, die Sie bei der Erstellung des Angebots angegeben haben.

Es wird von Ihnen erwartet, dass Sie untersuchen, ob festgestellte Anomalien echte Probleme darstellen, und sich in diesem Fall an Microsoft wenden, um die falschen Nutzungsdaten zu melden (siehe den Abschnitt „Support“ weiter unten).

Wenn Sie bestätigen, dass es sich bei den festgestellten Anomalien um normale Nutzung handelt, müssen Sie nichts weiter unternehmen. Wenn eine Anomalie jedoch ein potenziell hohes finanzielles Risiko darstellt, können wir Sie kontaktieren, um uns die Nutzung bestätigen zu lassen.  

## <a name="when-and-how-to-get-support"></a>Wann und wie erhalte ich Support?

Falls Sie falsche Nutzungsdaten an Microsoft gesendet haben und dies dazu führt, dass dem Kunden zu wenig berechnet wurde bzw. wird, wird dem Kunden die zu wenig gemeldete Nutzung nicht durch Microsoft nachberechnet, und Sie werden nicht für diese Nutzung bezahlt. Der durch die fehlerhafte Meldung entstandene Umsatzverlust muss von Ihnen getragen werden.

In einem der folgenden Fälle können Sie per Supportticket eine Rückerstattung oder eine Abrechnungsanpassung für Ihre Kunden anfordern:

- Sie haben festgestellt, dass es sich bei einer der von uns gefundenen Anomalien um ein echtes Problem handelt und dass die falsch gemeldete Nutzung dazu führen würde, dass dem Kunden **zu viel** berechnet wird.
- Sie stellen fest, dass Sie falsche Nutzungsangaben an uns übermittelt haben, die dazu führen würden, dass dem Kunden **zu viel** berechnet wird.
- Sie möchten eine Rückerstattung der Gebühr für die Nutzung nach getakteter Abrechnung Ihres Kunden anfordern.

So senden Sie ein Ticket

1. Wechseln Sie zur Seite „Support“. Geben Sie in das Feld **Beschreiben Sie Ihr Problem** „falsche Nutzungsdaten“ ein.
2. Wählen Sie in den Supportthemen in der Dropdownliste der Suchergebnisse eine der folgenden Optionen aus:
    - **Commercial Marketplace** > **Getaktete Abrechnung** > **Falsche Nutzung für Azure-Anwendungsangebot gesendet** oder
    - **Commercial Marketplace** > **Getaktete Abrechnung** > **Falsche Nutzung für SaaS-Angebot gesendet**
3. Wählen Sie unter **Nächster Schritt** die Schaltfläche **Lösungen anzeigen** aus, um sich bei Partner Center anzumelden und ein Supportticket zu übermitteln.

Weitere Supportoptionen für Herausgeber finden Sie unter [Support für das Programm „Kommerzieller Marketplace“ im Partner Center](../support.md).

## <a name="next-step"></a>Nächster Schritt

- Erfahren Sie mehr zur [Messdienst-API in Marketplace](marketplace-metering-service-apis.md).

---
title: Optimieren von Azure-Workloads mithilfe der Advisor-Bewertung
description: Verwenden der Advisor-Bewertung zur optimalen Nutzung von Azure
ms.topic: article
ms.date: 09/09/2020
ms.openlocfilehash: 29d8480f501a78c1668b52034f439f998419f9d9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91335618"
---
# <a name="optimize-azure-workloads-using-advisor-score"></a>Optimieren von Azure-Workloads mithilfe der Advisor-Bewertung

## <a name="introduction-to-advisor-score"></a>Einführung in die Advisor-Bewertung

Azure Advisor bietet Empfehlungen zu bewährten Methoden für Ihre Workloads. Diese Empfehlungen sind personalisiert und umsetzbar, um Ihnen bei Folgendem zu helfen:
* Verbessern des Status Ihrer Workloads und Optimieren Ihrer Azure-Bereitstellungen
* Proaktives Vermeiden der häufigsten Probleme durch Befolgen der bewährten Methoden
* Bewerten Ihrer Azure-Workloads anhand der fünf Säulen des [Microsoft Azure Well-Architected Framework](https://docs.microsoft.com/azure/architecture/framework/)

Als ein zentrales Feature von Advisor ist die **Advisor-Bewertung** entwickelt worden, um Ihnen zu helfen, diese Ziele effektiv und effizient zu erreichen. 

Für die optimale Nutzung von Azure ist es entscheidend zu verstehen, an welcher Position Sie sich bei der Optimierung Ihres Workloads befinden, welche Dienste/Ressourcen angemessen verbraucht werden und welche nicht. Darüber hinaus sollten Sie wissen, wie Sie auf der Grundlage von Empfehlungen Prioritäten setzen können, um das Ergebnis zu maximieren. Es ist ebenfalls wichtig, den Fortschritt, den Sie auf dieser Optimierungsjourney machen, nachzuverfolgen und zu melden. Mit der **Advisor-Bewertung** können Sie all diese Punkte mit unserer neuen Gamification-Umgebung ganz einfach erreichen. Als Ihr persönlicher Cloudberater bewertet Azure Advisor kontinuierlich Ihre Nutzungstelemetrie und Ressourcenkonfiguration, um auf bewährte Methoden der Branche zu prüfen. Advisor fasst seine Ergebnisse dann zu einem einzelnen Ergebnis zusammen, sodass Sie auf einen Blick erkennen können, ob Sie die erforderlichen Schritte unternehmen, um zuverlässige, sichere und kosteneffiziente Lösungen zu erstellen. Das Advisor-Ergebnis besteht aus einer Gesamtbewertung, die weiter in fünf Kategorien unterteilt werden kann, eine für jede Kategorie von Azure Advisor, die die fünf Säulen des Well-Architected Framework darstellen. Sie können den im Laufe der Zeit erzielten Fortschritt nachverfolgen, indem Sie Ihre Gesamtbewertung und die Bewertung in der Kategorie mit täglichem, wöchentlichem und monatlichem Trend anzeigen, und Sie können Vergleichstests festlegen, die Ihnen helfen, Ihre Ziele zu erreichen. 

 ![Benutzeroberfläche für die Advisor-Bewertung](./media/advisor-score-1.png)

## <a name="how-to-consume-advisor-score"></a>Nutzen der Advisor-Bewertung
Advisor zeigt Ihre gesamte Advisor-Bewertung und die Aufschlüsselung nach Advisor-Kategorien in Prozent an. Eine Bewertung von 100 % in einer beliebigen Kategorie bedeutet, dass alle Ihre von Advisor bewerteten Ressourcen die bewährten Methoden befolgen, die von Advisor empfohlen wurden. Am anderen Ende des Spektrums bedeutet ein Wert von 0 %, dass keine Ihrer von Advisor bewerteten Ressourcen den Empfehlungen von Advisor folgt. Mithilfe dieser Bewertungseinheiten können Sie ganz einfach den folgenden Flow erreichen:
* **Advisor-Bewertung**, um Ihnen dabei zu helfen, die Grundwerte Ihrer Workload und Abonnements auf Basis der Advisor-Bewertung zu ermitteln. Sie können sich auch die historischen Trends ansehen, um zu verstehen, wie Ihr Trend verläuft.
* **Advisor-Kategoriebewertungen** der einzelnen Empfehlungen geben an, durch welche ausstehenden Empfehlungen sich die Bewertung am meisten verbessert. Diese Werte geben sowohl die Gewichtung der Empfehlung als auch die vorhergesagte Einfachheit der Umsetzung an, um die größtmögliche Zeitersparnis sicherzustellen und Ihnen bei der Priorisierung zu helfen.
* **Auswirkung der Kategoriebewertung** der einzelnen Empfehlungen helfen Ihnen bei der Priorisierung Ihrer Aktionen zur Problembehebung für die einzelnen Kategorien.

Der Anteil der einzelnen Empfehlungen an der Kategoriebewertung wird auf der Seite der Advisor-Bewertung im Azure-Portal eindeutig angezeigt. Sie können jede Kategoriebewertung um den in der Spalte für die Auswirkung der Kategoriebewertung aufgeführten Prozentpunkt erhöhen. Dieser Wert gibt sowohl die Gewichtung der Empfehlung innerhalb der Kategorie als auch die vorhergesagte einfache Umsetzbarkeit an, um die potenziell einfach zu erledigenden Aufgaben zu ermitteln. Wenn Sie sich auf die Empfehlungen mit der größten Bewertungsauswirkung konzentrieren, können Sie mit der Zeit die größten Fortschritte erzielen.  

![Auswirkung der Advisor-Bewertung](./media/advisor-score-2.png)

Für den Fall, dass Advisor-Empfehlungen für eine einzelne Ressource nicht relevant sind, können Sie diese Empfehlungen zurückstellen oder verwerfen, sodass sie bei der nächsten Aktualisierung von der Berechnung der Bewertung ausgeschlossen werden. Advisor verwendet diese Eingabe auch als zusätzliches Feedback zur Verbesserung des Modells.

## <a name="how-is-advisor-score-calculated"></a>Wie wird die Advisor-Bewertung berechnet?
Advisor zeigt die Kategoriebewertungen und die Advisor-Gesamtbewertung als Prozentwerte an. Eine Bewertung von 100 % in einer beliebigen Kategorie bedeutet, dass alle Ihre *von Advisor bewerteten* Ressourcen die bewährten Methoden befolgen, die von Advisor empfohlen wurden. Am anderen Ende des Spektrums bedeutet ein Wert von 0 %, dass keine Ihrer von Advisor bewerteten Ressourcen den Empfehlungen von Advisor folgt. 
**Jede der fünf Kategorien hat eine höchstmögliche potenzielle Bewertung von 100.** Die Advisor-Gesamtbewertung errechnet sich aus der Summe aller anwendbaren Kategorien, geteilt durch die Summe der höchsten potenziellen Bewertung aus allen anwendbaren Kategorien. Für die meisten Abonnements bedeutet dies, dass Advisor die Bewertung aus den einzelnen Kategorien addiert und durch 500 teilt. **Die Bewertung für die einzelnen Kategorien wird jedoch nur berechnet, wenn Sie Ressourcen verwenden, die von Advisor bewertet werden.**

**Beispiel für die Berechnung der Advisor-Bewertung**
* Bewertung für ein Einzelabonnement: Dies ist der einfache Mittelwert aller Advisor-Kategoriebewertungen für Ihr Abonnement. Wenn die Advisor-Kategoriebewertungen wie folgt lauten: Kosten = 73, Zuverlässigkeit = 85, optimaler Betrieb = 77, Leistung = 100, ergibt die **Advisor-Bewertung** (73 + 85 + 77 + 100) / (4 × 100) = 0,84 oder 84 %.
* Bewertung für mehrere Abonnements: Wenn mehrere Abonnements ausgewählt werden, wird die generierte Advisor-Gesamtbewertung aus den gewichteten aggregierten Kategoriebewertungen errechnet. Hierbei wird jede Advisor-Kategoriebewertung basierend auf den von den Abonnements genutzten Ressourcen aggregiert. Der aus den gewichteten aggregierten Kategoriebewertungen gebildete einfache Mittelwert ergibt die Gesamtbewertung für die Abonnements. 


### <a name="scoring-methodology"></a>Bewertungsmethodik: 
Die Berechnung der Advisor-Bewertung lässt sich in vier Schritten zusammenfassen:
1. Advisor berechnet die **Einzelhandelskosten der betroffenen Ressourcen**, d. h. der Ressourcen in Ihren Abonnements, die in Advisor über mindestens eine Empfehlung verfügen.
2. Advisor berechnet die **Einzelhandelskosten der bewerteten Ressourcen**, d. h. der von Advisor überwachten Ressourcen, unabhängig davon, ob sie Empfehlungen aufweisen. 
3. Für jeden Empfehlungstyp berechnet Advisor das **Verhältnis fehlerfreier Ressourcen**, d. h. die Einzelhandelskosten der betroffenen Ressourcen geteilt durch die Einzelhandelskosten der bewerteten Ressourcen.
4. Advisor wendet drei zusätzliche Gewichtungen auf das Verhältnis fehlerfreier Ressourcen in den einzelnen Kategorien an:
  * Empfehlungen mit größerer Auswirkung werden höher gewichtet als Empfehlungen mit geringerer Auswirkung.
  * Ressourcen mit langfristigen Empfehlungen werden bei Ihrer Bewertung höher bewertet.
  * Die in Advisor zurückgestellten oder verworfenen Ressourcen werden vollständig aus der Berechnung Ihrer Bewertung entfernt. 
    
Advisor wendet dieses Modell auf einer Advisor-Kategorieebene an (Security verwendet das [Secure Score](https://docs.microsoft.com/azure/security-center/secure-score-security-controls#introduction-to-secure-score)-Modell), wodurch wir für jede Kategorie eine Advisor-Bewertung erhalten, und darüber hinaus ergibt ein einfacher Mittelwert die endgültige Advisor-Bewertung.


## <a name="advisor-score-faq"></a>Häufig gestellte Fragen zur Advisor-Bewertung
* **Wie oft wird die Bewertung aktualisiert?**
Ihre Bewertung wird mindestens einmal pro Tag aktualisiert. 
* **Warum enthalten einige Empfehlungen einen leeren Wert für „-“ in der Spalte für die Auswirkung der Kategoriebewertung?** Advisor fügt neue Empfehlungen oder Empfehlungen mit aktuellen Änderungen nicht sofort dem Bewertungsmodell hinzu. Nach einer kurzen Evaluierungszeit (in der Regel einige Wochen) werden sie in Bewertungen berücksichtigt. 
* **Warum ist die Auswirkung der Kostenbewertung bei einigen Empfehlungen größer, auch wenn sie geringere potenzielle Einsparungen aufweisen?**
Die Kostenbewertung gibt sowohl Ihre potenziellen Einsparungen durch nicht ausgelastete Ressourcen als auch die vorhergesagte einfache Umsetzbarkeit dieser Empfehlungen an. Beispielsweise werden betroffene Ressourcen, die längere Zeit nicht genutzt wurden, stärker gewichtet, auch wenn die potenziellen Einsparungen geringer sind. 
* **Warum erhalte ich keine Bewertung für einzelne oder mehrere Kategorien oder Abonnements?**
Eine Bewertung wird nur für die Kategorien und Abonnements generiert, die Ressourcen enthalten, die von Advisor bewertet werden.
* **Was ist, wenn eine Empfehlung nicht relevant ist?**
Wenn Sie eine Empfehlung von Advisor verwerfen, wird sie bei der Berechnung Ihrer Bewertung nicht berücksichtigt. Das Verwerfen von Empfehlungen hilft Advisor auch, die Qualität der Empfehlungen zu verbessern.
* **Warum hat sich meine Bewertung geändert?** Ihre Bewertung kann sich ändern, wenn Sie betroffene Ressourcen durch die Anwendung der von Advisor empfohlenen bewährten Methoden korrigieren. Wenn Sie oder jemand mit Berechtigungen für Ihr Abonnement Ressourcen geändert oder neue Ressourcen erstellt hat, kann es auch zu Schwankungen bei Ihrer Bewertung kommen, da Ihre Bewertung auf einem Verhältnis der durch Kosten betroffenen Ressourcen relativ zu den Gesamtkosten aller Ressourcen basiert.
* **Wie berechnet Advisor die Einzelhandelskosten für Ressourcen in einem Abonnement?**
Advisor verwendet die nutzungsbasierten Tarife, die auf der Preisübersichtsseite von Azure.com veröffentlicht sind und keine anwendbaren Rabatte widerspiegeln, multipliziert mit der Anzahl der Nutzungen am letzten Tag, an dem die Ressource zugewiesen wurde. Durch das Auslassen von Rabatten bei der Berechnung der Ressourcenkosten ist die Advisor-Bewertung zwischen Abonnements, Mandanten und Registrierungen vergleichbar, bei denen die Rabatte variieren können. 
* **Muss ich die Empfehlungen in Advisor anzeigen, um Punkte für meine Bewertung zu erhalten?** Nein. Ihre Bewertung gibt an, ob Sie von Advisor empfohlene bewährte Methoden umsetzen, auch wenn Sie die bewährten Methoden proaktiv übernehmen und die Empfehlungen nie in Advisor anzeigen.
* **Unterscheidet die Bewertungsmethodik zwischen Produktions- und Dev/Test-Workloads?**
Nein, bisher nicht, aber Sie können Empfehlungen zu einzelnen Ressourcen verwerfen, wenn diese Ressourcen für Entwicklung und Tests verwendet werden und die Empfehlung nicht zutrifft.
* **Kann ich Bewertungen zwischen einem Abonnement mit 100 Ressourcen und einem Abonnement mit 100.000 Ressourcen vergleichen?**
Die Bewertungsmethodik wurde entwickelt, um die Anzahl der Ressourcen innerhalb einer Kombination aus Abonnement und Dienst zu steuern, sodass Abonnements mit weniger Ressourcen höhere oder niedrigere Bewertungen erhalten können als Abonnements mit mehr Ressourcen. 
* **Was bedeutet es, wenn in der Spalte für die Auswirkung der Bewertung „In Kürze verfügbar“ angezeigt wird?**
Das bedeutet, dass es sich um eine neue Empfehlung handelt und wir noch daran arbeiten, sie in das Advisor-Bewertungsmodell zu integrieren. Sobald diese neue Empfehlung bei der Berechnung der Bewertung berücksichtigt wird, wird der Bewertungswert der Auswirkung für die Empfehlung angezeigt.  
* **Hängt meine Bewertung von meinen Ausgaben für Azure ab?**
Nein, die Bewertung gibt nicht zwangsläufig Ihre Ausgaben an. Unnötige Ausgaben führen zu einer geringeren Kostenbewertung.

## <a name="how-to-access-advisor"></a>Zugreifen auf Advisor
Die Advisor-Bewertung befindet sich im Azure-Portal in der öffentlichen Vorschau. Wechseln Sie zum Advisor-Abschnitt. Dort finden Sie die Advisor-Bewertung als zweiten Menüeintrag im linken Navigationsmenü. 

![Einstiegspunkt für die Advisor-Bewertung](./media/advisor-score-3.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie unter:
* [Einführung in Advisor](advisor-overview.md)
* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Reduzieren der Dienstkosten mithilfe des Azure Advisors](advisor-cost-recommendations.md)
* [Verbessern der Leistung von Azure-Anwendungen mit dem Azure Advisor](advisor-performance-recommendations.md)
* [Sicherheitsempfehlungen von Advisor](advisor-security-recommendations.md)
* [Sicherstellen des optimalen Betriebs mit dem Azure Advisor](advisor-operational-excellence-recommendations.md)

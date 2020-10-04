---
title: Optimieren von Azure-Workloads mithilfe der Advisor-Bewertung
description: Verwenden der Advisor-Bewertung zur optimalen Nutzung von Azure
ms.topic: article
ms.date: 09/09/2020
ms.openlocfilehash: 720a2b358e35d776a7233452eee2bd69b521654f
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90056233"
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
* **Advisor-Bewertung**, um Ihnen dabei zu helfen, die Grundwerte Ihrer Workload/Abonnements auf Basis der Advisor-Bewertung zu ermitteln. Sie können sich auch die historischen Trends ansehen, um zu verstehen, wie Ihr Trend verläuft.
* **Advisor-Kategoriebewertung**, um zu verstehen, welche Kategorien mehr Aufmerksamkeit erfordern und Sie bei der Priorisierung unterstützen.
* **Potenzielle Bewertungssteigerung** der einzelnen Empfehlungen, um Ihnen bei der Priorisierung Ihrer Aktionen zur Problembehebung für die einzelnen Kategorien zu helfen.

Der Beitrag der einzelnen Empfehlungen zu Ihrer Bewertung wird auf der Übersichtsseite im Azure-Portal eindeutig angezeigt. Sie können Ihre Bewertung erhöhen, indem Sie die bewährten Methoden übernehmen, und Sie können die Empfehlungen mit der größten **potenziellen Bewertungssteigerung** priorisieren, um in der Ihnen zur Verfügung stehenden Zeit am schnellsten voranzukommen.  

![Auswirkung der Advisor-Bewertung](./media/advisor-score-2.png)

Da die Bewertungsmethodik von Advisor den kostenaufwendigen Ressourcen mit langfristigen Empfehlungen zusätzliches Gewicht beimisst, können Sie den größten Fortschritt erzielen, wenn Sie die Ressourcen mit den höchsten Einzelhandelskosten zuerst korrigieren. Für den Fall, dass Advisor-Empfehlungen für eine einzelne Ressource nicht relevant sind, können Sie diese Empfehlungen verwerfen, um sie von der Berechnung der Bewertung auszuschließen und Feedback an Advisor senden, um dessen Empfehlungen zu verbessern. 

## <a name="how-is-advisor-score-calculated"></a>Wie wird die Advisor-Bewertung berechnet?
Advisor zeigt die Kategoriebewertungen und die Advisor-Gesamtbewertung als Prozentwerte an. Eine Bewertung von 100 % in einer beliebigen Kategorie bedeutet, dass alle Ihre *von Advisor bewerteten* Ressourcen die bewährten Methoden befolgen, die von Advisor empfohlen wurden. Am anderen Ende des Spektrums bedeutet ein Wert von 0 %, dass keine Ihrer von Advisor bewerteten Ressourcen den Empfehlungen von Advisor folgt. 
**Jede der fünf Kategorien hat eine höchstmögliche potenzielle Bewertung von 100.** Die Advisor-Gesamtbewertung errechnet sich aus der Summe aller anwendbaren Kategorien, geteilt durch die Summe der höchsten potenziellen Bewertung aus allen anwendbaren Kategorien. Für die meisten Abonnements bedeutet dies, dass Advisor die Bewertung aus den einzelnen Kategorien addiert und durch 500 teilt. **Die Bewertung für die einzelnen Kategorien wird jedoch nur berechnet, wenn Sie Ressourcen verwenden, die von Advisor bewertet werden.**

### <a name="scoring-methodology"></a>Bewertungsmethodik: 
Die Berechnung der Advisor-Bewertung lässt sich in vier Schritten zusammenfassen:
1. Advisor berechnet die **täglichen Einzelhandelskosten der betroffenen Ressourcen**, d. h. der Ressourcen in Ihren Abonnements, die in Advisor über mindestens eine Empfehlung verfügen.
2. Advisor berechnet die **täglichen Einzelhandelskosten der bewerteten Ressourcen**, d. h. der von Advisor überwachten Ressourcen, unabhängig davon, ob sie Empfehlungen aufweisen. 
3. Für jeden Empfehlungstyp berechnet Advisor das **Verhältnis fehlerfreier Ressourcen**, d. h. die Kosten der betroffenen Ressourcen geteilt durch die Kosten der bewerteten Ressourcen.
4. Advisor wendet drei zusätzliche Gewichtungen auf das Verhältnis fehlerfreier Ressourcen in den einzelnen Kategorien an:
* Empfehlungen mit größerer Auswirkung werden höher gewichtet als Empfehlungen mit geringerer Auswirkung.
* Ressourcen mit langfristigen Empfehlungen werden bei Ihrer Bewertung höher bewertet.
* Die in Advisor verworfenen Ressourcen werden vollständig aus der Berechnung Ihrer Bewertung entfernt. 
    
Advisor wendet dieses Modell auf einer Advisor-Kategorieebene an (Security verwendet das [Secure Score](https://docs.microsoft.com/azure/security-center/secure-score-security-controls#introduction-to-secure-score)-Modell), wodurch wir für jede Kategorie eine Advisor-Bewertung erhalten, und darüber hinaus ergibt ein einfacher Mittelwert die endgültige Advisor-Bewertung.


## <a name="advisor-score-faq"></a>Häufig gestellte Fragen zur Advisor-Bewertung
* **Wie oft wird die Bewertung aktualisiert?**
Ihre Bewertung wird mindestens einmal pro Tag aktualisiert. 
* **Muss ich die Empfehlungen in Advisor anzeigen, um Punkte für meine Bewertung zu erhalten?**
Nein Ihre Bewertung spiegelt wider, ob Sie von Advisor empfohlene bewährte Methoden übernehmen, auch wenn Sie diese Empfehlungen nie in Advisor anzeigen und bewährte Methoden proaktiv übernehmen.  
* **Wie berechnet Advisor die täglichen Einzelhandelskosten für Ressourcen in einem Abonnement?**
Advisor verwendet die *nutzungsbasierten* Tarife, die auf der Preisübersichtsseite von Azure.com veröffentlicht sind und keine anwendbaren Rabatte widerspiegeln, multipliziert mit der Anzahl der Nutzungen am letzten Tag, an dem die Ressource zugewiesen wurde. Durch das Auslassen von Rabatten bei der Berechnung der Ressourcenkosten ist die Advisor-Bewertung zwischen Abonnements, Mandanten und Registrierungen vergleichbar, bei denen die Rabatte variieren können. 
* **Was ist, wenn eine Empfehlung nicht relevant ist?**
Wenn Sie eine Empfehlung von Advisor verwerfen, wird sie bei der Berechnung Ihrer Bewertung nicht berücksichtigt. Das Verwerfen von Empfehlungen hilft Advisor auch, die Qualität der Empfehlungen zu verbessern.
* **Warum hat sich meine Bewertung geändert?** Ihre Bewertung kann sich ändern, wenn Sie betroffene Ressourcen durch die Anwendung der von Advisor empfohlenen bewährten Methoden korrigieren. Wenn Sie oder jemand mit Berechtigungen für Ihr Abonnement Ressourcen geändert oder neue Ressourcen erstellt hat, kann es auch zu Schwankungen bei Ihrer Bewertung kommen, da Ihre Bewertung auf einem Verhältnis der durch Kosten betroffenen Ressourcen relativ zu den Gesamtkosten aller Ressourcen basiert.
* **Hängt meine Bewertung von meinen Ausgaben für Azure ab?**
Nein Die Bewertung dient zur Steuerung der Größe einer Kombination aus Abonnement und Dienst. 
* **Unterscheidet die Bewertungsmethodik zwischen Produktions- und Dev/Test-Workloads?**
Nein, bisher nicht, aber Sie können Empfehlungen zu einzelnen Ressourcen verwerfen, wenn diese Ressourcen für Entwicklung und Tests verwendet werden und die Empfehlung nicht zutrifft.
* **Kann ich Bewertungen zwischen einem Abonnement mit 100 Ressourcen und einem Abonnement mit 100.000 Ressourcen vergleichen?**
Die Bewertungsmethodik wurde entwickelt, um die Anzahl der Ressourcen innerhalb einer Kombination aus Abonnement und Dienst zu steuern, sodass Abonnements mit weniger Ressourcen höhere oder niedrigere Bewertungen erhalten können als Abonnements mit mehr Ressourcen. 

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

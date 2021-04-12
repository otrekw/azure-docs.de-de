---
title: Secure Score in Azure Security Center
description: Beschreibung des Secure Scores von Azure Security Center und seiner Sicherheitskontrollen
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetd: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2021
ms.author: memildin
ms.openlocfilehash: 2f5d98dd9bf893065f2bf9c37cbec4384d0f7c94
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105727139"
---
# <a name="secure-score-in-azure-security-center"></a>Secure Score in Azure Security Center

## <a name="introduction-to-secure-score"></a>Einführung in Secure Score

Azure Security Center hat zwei Hauptziele: 

- Ihre aktuelle Sicherheitslage besser verstehen
- Ihre Sicherheit effizient und effektiv verbessern

Der zentrale Aspekt von Security Center, mit dem Sie diese Ziele erreichen können, ist die **Sicherheitsbewertung**.

Security Center führt eine ständige Bewertung Ihrer Ressourcen, Abonnements und Organisation in Bezug auf Sicherheitsprobleme durch. Anschließend werden alle Ergebnisse in einer einzigen Bewertung zusammengefasst, sodass Sie auf einen Blick Ihre aktuelle Sicherheitssituation erkennen können: je höher die Bewertung, desto geringer das ermittelte Risiko.

Die Sicherheitsbewertung wird auf den Seiten des Azure-Portals als Prozentwert angezeigt, aber auch die zugrunde liegenden Werte sind übersichtlich dargestellt:

:::image type="content" source="./media/secure-score-security-controls/single-secure-score-via-ui.png" alt-text="Sicherheitsbewertung insgesamt, wie im Portal angezeigt":::

Um Ihre Sicherheit zu verbessern, sehen Sie auf der Seite „Empfehlungen“ von Security Center nach, welche Maßnahmen erforderlich sind, um Ihre Bewertung zu steigern. Jede Empfehlung enthält Anweisungen, die Ihnen helfen sollen, das spezifische Problem zu beheben.

Die Empfehlungen sind nach **Sicherheitskontrollen** gruppiert. Jede Sicherheitskontrolle ist eine logische Gruppe verwandter Sicherheitsempfehlungen, die anfällige Angriffsflächen widerspiegelt. Ihre Bewertung verbessert sich nur, wenn Sie *alle* Empfehlungen für eine einzelne Ressource innerhalb einer Sicherheitskontrolle umsetzen. Um zu sehen, wie gut jede einzelne Angriffsfläche in Ihrer Organisation geschützt ist, sehen Sie sich die Bewertungen für die einzelnen Sicherheitskontrollen an.

Weitere Informationen finden Sie unter [Berechnen Ihrer Sicherheitsbewertung](secure-score-security-controls.md#how-your-secure-score-is-calculated). 

## <a name="how-your-secure-score-is-calculated"></a>Berechnen Ihrer Sicherheitsbewertung 

Der jeweilige Beitrag der einzelnen Sicherheitskontrollen zur gesamten Sicherheitsbewertung ist auf der Seite mit Empfehlungen eindeutig angegeben.

[![Mit dem erweiterten Secure Score (Vorschauversion) werden Sicherheitskontrollen eingeführt](media/secure-score-security-controls/security-controls.png)](media/secure-score-security-controls/security-controls.png#lightbox)

Um alle zu erzielenden Punkte für eine Sicherheitskontrolle zu erhalten, müssen alle Ihre Ressourcen allen Sicherheitsempfehlungen innerhalb der Sicherheitskontrolle entsprechen. Beispielsweise enthält Security Center mehrere Empfehlungen zum Schützen Ihrer Verwaltungsports. Sie müssen alle Empfehlungen umsetzen, um eine Änderung der Sicherheitsbewertung zu erzielen.

Beispielsweise wird die Sicherheitskontrolle „Systemupdates anwenden“ mit maximal sechs Punkten bewertet. Dieser Wert ist in der QuickInfo zur potenziellen Steigerung der Sicherheitskontrolle angegeben:

[![Sicherheitskontrolle „Systemupdates anwenden“](media/secure-score-security-controls/apply-system-updates-control.png)](media/secure-score-security-controls/apply-system-updates-control.png#lightbox)

Der maximal mögliche Wert für das Steuerelement „Systemupdates anwenden“ ist immer 6. In diesem Beispiel gibt es 50 Ressourcen. Der maximale Wert wird daher durch 50 geteilt, woraus sich ergibt, dass jede Ressource 0,12 Punkte beiträgt. 

* **Potenzielle Steigerung** (0,12 x 8 fehlerhafte Ressourcen = 0,96): Die Anzahl der noch verfügbaren Punkte des Steuerelements. Wenn Sie alle Empfehlungen für dieses Steuerelement umsetzen, steigt die Bewertung um 2 % (in diesem Fall um 0,96 Punkte, die auf 1 Punkt aufgerundet werden). 
* **Aktuelle Bewertung** (0,12 x 42 fehlerfreie Ressourcen = 5,04): Die aktuelle Bewertung für dieses Steuerelement. Jedes Kontrollelement trägt zur Gesamtbewertung bei. In diesem Beispiel trägt das Steuerelement 5,04 Punkte zur aktuellen Gesamtbewertung bei.
* **Maximale Bewertung**: Die maximale Anzahl von Punkten, die Sie durch Erfüllen aller Empfehlungen innerhalb eines Kontrollelements erhalten können. Die maximale Bewertung für ein Kontrollelement gibt die relative Bedeutung dieses Kontrollelements an. Verwenden Sie die maximale Bewertung, um die Probleme zu selektieren, die zuerst behandelt werden müssen. 


### <a name="calculations---understanding-your-score"></a>Berechnungen – Verstehen der Bewertung

|Metrik|Formel und Beispiel|
|-|-|
|**Aktuelle Bewertung der Sicherheitskontrolle**|<br>![Gleichung zum Berechnen der Bewertung einer Sicherheitskontrolle](media/secure-score-security-controls/secure-score-equation-single-control.png)<br><br>Jede einzelne Sicherheitskontrolle trägt zur Sicherheitsbewertung bei. Jede Ressource, die von einer Empfehlung innerhalb des Kontrollelements betroffen ist, trägt zur aktuellen Bewertung des Kontrollelements bei. Die aktuelle Bewertung für jedes Kontrollelement ist ein Maß für den Status der Ressourcen *innerhalb* des Kontrollelements.<br>![QuickInfos mit den Werten, die beim Berechnen der aktuellen Bewertung der Sicherheitskontrolle verwendet werden](media/secure-score-security-controls/security-control-scoring-tooltips.png)<br>In diesem Beispiel wird die maximale Bewertung von 6 durch 78 dividiert, da dies die Summe aus fehlerfreien und fehlerhaften Ressourcen ist.<br>6 / 78 = 0,0769<br>Die Multiplikation dieses Ergebnisses mit der Anzahl der fehlerfreien Ressourcen (4) ergibt die aktuelle Bewertung:<br>0,0769 * 4 = **0,31**<br><br>|
|**Sicherheitsbewertung**<br>Ein Abonnement|<br>![Gleichung zum Berechnen der Sicherheitsbewertung eines Abonnements](media/secure-score-security-controls/secure-score-equation-single-sub.png)<br><br>![Sicherheitsbewertung eines einzelnen Abonnements, wenn alle Kontrollen aktiviert sind](media/secure-score-security-controls/secure-score-example-single-sub.png)<br>In diesem Beispiel gibt es ein einzelnes Abonnement, für das alle Sicherheitskontrollen verfügbar sind (potenzielle Höchstbewertung sind 60 Punkte). Die Bewertung gibt 28 Punkte der möglichen 60 Punkte an. Die verbleibenden 32 Punkte sind in den Zahlen für die potenzielle Bewertungssteigerung der Sicherheitskontrollen enthalten.<br>![Liste der Kontrollen und potenziellen Bewertungssteigerung](media/secure-score-security-controls/secure-score-example-single-sub-recs.png)|
|**Sicherheitsbewertung**<br>Mehrere Abonnements|<br>![Gleichung zum Berechnen der Sicherheitsbewertung für mehrere Abonnements](media/secure-score-security-controls/secure-score-equation-multiple-subs.png)<br><br>Wenn Sie die kombinierte Bewertung für mehrere Abonnements berechnen, enthält Security Center eine *Gewichtung* für jedes Abonnement. Die relativen Gewichtungen für Ihre Abonnements werden von Security Center basierend auf Faktoren wie der Anzahl der Ressourcen bestimmt.<br>Die aktuelle Bewertung für die einzelnen Abonnements wird auf die gleiche Weise berechnet wie für ein einziges Abonnement, aber dann wird die Gewichtung wie in der Gleichung dargestellt angewendet.<br>Beim Anzeigen mehrerer Abonnements werden von Secure Score alle Ressourcen in allen aktivierten Richtlinien ausgewertet und nach deren kombinierter Auswirkung auf die Höchstbewertung der einzelnen Sicherheitskontrollen gruppiert.<br>![Sicherheitsbewertung für mehrere Abonnements, wenn alle Sicherheitskontrollen aktiviert sind](media/secure-score-security-controls/secure-score-example-multiple-subs.png)<br>Die kombinierte Bewertung ist **kein** Durchschnittswert, sondern vielmehr die Auswertung des Status aller Ressourcen in allen Abonnements.<br>Wenn Sie die Seite mit den Empfehlungen aufrufen und die potenziell verfügbaren Punkte addieren, werden Sie feststellen, dass es sich hierbei um die Differenz zwischen der aktuellen Bewertung (24) und der verfügbaren Höchstbewertung (60) handelt.|


### <a name="which-recommendations-are-included-in-the-secure-score-calculations"></a>Welche Empfehlungen sind in den Berechnungen zur Sicherheitsbewertung enthalten?

Nur integrierte Empfehlungen haben eine Auswirkung auf die Sicherheitsbewertung.

Empfehlungen, die als **Vorschau** gekennzeichnet sind, werden nicht in die Berechnungen Ihrer Sicherheitsbewertung einbezogen. Sie sollten nach Möglichkeit weiterhin korrigiert werden, damit sie nach Ablauf des Vorschauzeitraums zu Ihrer Bewertung beitragen.

Beispiel für eine Vorschauempfehlung:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Empfehlung mit dem Flag „Vorschau“":::

## <a name="improve-your-secure-score"></a>Verbessern Ihrer Sicherheitsbewertung

Zum Verbessern Ihrer Sicherheitsbewertung setzen Sie die Sicherheitsempfehlungen in Ihrer Empfehlungsliste um. Sie können jede Empfehlung manuell für die einzelnen Ressourcen umsetzen oder die Option **Schnelle Problembehebung** verwenden (falls verfügbar), um eine Empfehlung für eine Gruppe von Ressourcen schnell anzuwenden. Weitere Informationen finden Sie unter [Umsetzen von Empfehlungen](security-center-remediate-recommendations.md).

Eine andere Möglichkeit, Ihre Bewertung zu verbessern und sicherzustellen, dass Ihre Benutzer keine Ressourcen erstellen, die sich negativ auf Ihre Bewertung auswirken, besteht darin, die Erzwingungs- und Ablehnungsoptionen für die entsprechenden Empfehlungen zu konfigurieren. Weitere Informationen finden Sie unter [Verhindern von Fehlkonfigurationen mit den Optionen zum Erzwingen/Ablehnen für Empfehlungen](prevent-misconfigurations.md).

## <a name="security-controls-and-their-recommendations"></a>Sicherheitskontrollen und deren Empfehlungen

In der folgenden Tabelle sind die Sicherheitskontrollen in Azure Security Center aufgelistet. Für jede Sicherheitskontrolle ist die maximale Anzahl von Punkten angegeben, die Ihrer Sicherheitsbewertung hinzugefügt wird, wenn Sie *alle* in der Sicherheitskontrolle aufgeführten Empfehlungen für *alle* Ihre Ressourcen umsetzen. 

Die Sicherheitsempfehlungen, die mit Security Center bereitgestellt werden, sind auf die verfügbaren Ressourcen in der Umgebung der jeweiligen Organisation zugeschnitten. Die Empfehlungen können durch [Deaktivieren von Richtlinien](tutorial-security-policy.md#disable-security-policies-and-disable-recommendations) und [Ausnehmen bestimmter Ressourcen von einer Empfehlung](exempt-resource.md) weiter angepasst werden. 
 
Die zugewiesenen Azure Policy-Initiativen sollten von jeder Organisation sorgfältig geprüft werden. 

> [!TIP]
> Ausführliche Informationen zur Überprüfung und Bearbeitung von Initiativen finden Sie unter [Arbeiten mit Sicherheitsrichtlinien](tutorial-security-policy.md). 

Die Standardsicherheitsinitiative von Security Center basiert zwar auf branchenüblichen bewährten Methoden und Standards, es gibt jedoch Szenarien, in denen die unten aufgeführten integrierten Empfehlungen möglicherweise nicht uneingeschränkt für Ihre Organisation geeignet sind. Daher ist es manchmal erforderlich, die Standardinitiative anzupassen (ohne die Sicherheit zu beeinträchtigen). Dadurch wird sichergestellt, dass sie mit den Richtlinien Ihrer Organisation sowie mit Branchenstandards, gesetzlichen Vorgaben und Benchmarks in Einklang steht, die Sie erfüllen müssen.<br><br>
<div class="foo">

<style type="text/css"> .tg  {border-collapse:collapse;border-spacing:0;} .tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px; overflow:hidden;padding:10px 5px;word-break:normal;} .tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:18px; font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;} .tg .tg-cly1{text-align:left;vertical-align:middle} .tg .tg-lboi{border-color:inherit;text-align:left;vertical-align:middle} </style>

[!INCLUDE [security-center-controls-and-recommendations](../../includes/asc/security-control-recommendations.md)]

</div>




## <a name="secure-score-faq"></a>Häufig gestellte Fragen zu Secure Score

### <a name="if-i-address-only-three-out-of-four-recommendations-in-a-security-control-will-my-secure-score-change"></a>Ändert sich meine Sicherheitsbewertung, wenn ich nur drei von vier Empfehlungen in einer Sicherheitskontrolle umsetze?
Nein. Die Bewertung ändert sich erst, wenn Sie alle Empfehlungen für eine einzelne Ressource umgesetzt haben. Um die maximale Bewertung für ein Kontrollelement zu erhalten, müssen Sie alle Empfehlungen für alle Ressourcen umsetzen.

### <a name="if-a-recommendation-isnt-applicable-to-me-and-i-disable-it-in-the-policy-will-my-security-control-be-fulfilled-and-my-secure-score-updated"></a>Wenn eine Empfehlung für mich nicht anwendbar ist und ich sie in der Richtlinie deaktiviere, ist dann die Sicherheitskontrolle erfüllt, und wird meine Sicherheitsbewertung entsprechend aktualisiert?
Ja. Es wird empfohlen, Empfehlungen zu deaktivieren, wenn sie in Ihrer Umgebung nicht anwendbar sind. Anweisungen zum Deaktivieren einer bestimmten Empfehlung finden Sie unter [Deaktivieren von Sicherheitsrichtlinien](./tutorial-security-policy.md#disable-security-policies-and-disable-recommendations).

### <a name="if-a-security-control-offers-me-zero-points-towards-my-secure-score-should-i-ignore-it"></a>Wenn durch eine Sicherheitskontrolle keine Punkte für meine Sicherheitsbewertung erzielt werden, soll ich diese ignorieren?
In einigen Fällen wird als maximale Bewertung für ein Kontrollelement ein Wert größer 0 (null) angezeigt, jedoch ist die Auswirkung gleich null. Wenn der inkrementelle Wert für das Korrigieren von Ressourcen unerheblich ist, wird auf null gerundet. Ignorieren Sie diese Empfehlungen aber nicht, da Sie dennoch Sicherheitsverbesserungen bieten. Die einzige Ausnahme ist das Kontrollelement „Zusätzliche bewährte Methode“. Durch das Umsetzen dieser Empfehlungen erhöht sich die Bewertung nicht, doch Sie verbessern die Gesamtsicherheit.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden Secure Score und die damit eingeführten Sicherheitskontrollen beschrieben. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md)
- [Umsetzen von Empfehlungen in Azure Security Center](security-center-remediate-recommendations.md)
- [Anzeigen der GitHub-basierten Tools für die programmgesteuerte Arbeit mit Sicherheitsbewertungen](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score)


> [!div class="nextstepaction"]
> [Zugreifen auf Ihre Sicherheitsbewertung und Nachverfolgen dieser Bewertung](secure-score-access-and-track.md)
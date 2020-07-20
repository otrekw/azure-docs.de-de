---
title: Leitfaden zur Kostenverwaltung für Azure Lab Services
description: Informieren Sie sich über die verschiedenen Möglichkeiten, die Kosten für Lab Services anzuzeigen.
author: rbest
ms.author: rbest
ms.date: 06/26/2020
ms.topic: article
ms.openlocfilehash: fbbaf4a3646260fc09467e214b82fd0213415635
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445303"
---
# <a name="cost-management-for-azure-lab-services"></a>Kostenverwaltung für Azure Lab Services

Kostenverwaltung kann in zwei verschiedene Bereiche aufgeteilt werden: Kostenschätzung und Kostenanalyse.  Die Kostenschätzung wird beim Einrichten des Labs durchgeführt, um sicherzustellen, dass die anfängliche Struktur des Labs in das erwartete Budget passt.  Die Kostenanalyse erfolgt in der Regel am Ende des Monats, um die Kosten zu analysieren und die für den nächsten Monat erforderlichen Aktionen zu bestimmen.

## <a name="estimating-the-lab-costs"></a>Schätzen der Labkosten

Jedes Lab-Dashboard verfügt über einen Abschnitt **Kosten und Abrechnung**, in dem eine grobe Schätzung der Kosten des Labs für den Monat festgelegt wird.  Die Kostenschätzung rechnet die Stundennutzung mit der maximalen Anzahl von Benutzern nach den geschätzten Kosten pro Stunde zusammen.  Damit die Schätzung so genau wie möglich ist, richten Sie das Lab einschließlich des [Zeitplans](how-to-create-schedules.md) ein, und das Dashboard spiegelt die geschätzten Kosten wider.  

Diese Schätzung umfasst möglicherweise nicht alle möglichen Kosten. Einige Ressourcen sind nicht eingeschlossen.  Die Kosten für die Vorlagenvorbereitung fließen nicht in die Kostenschätzung ein.  Der Zeitaufwand zum Erstellen der Vorlage kann erheblich variieren. Die Kosten für die Ausführung der Vorlage sind mit den gesamten Labkosten pro Stunde identisch. Alle Kosten für die [Shared Image Gallery](how-to-use-shared-image-gallery.md) sind nicht im Lab-Dashboard enthalten, da ein Katalog von mehreren Labs gemeinsam genutzt werden kann.  Schließlich werden Stunden, die beim Starten eines Computers durch den Labersteller anfallen, von dieser Schätzung ausgeschlossen.

> [!div class="mx-imgBorder"]
> ![Dashboard-Kostenschätzung](./media/cost-management-guide/dashboard-cost-estimation.png)

## <a name="analyzing-previous-months-usage"></a>Analysieren der Nutzung in vorherigen Monaten

Die Kostenanalyse dient zum Analysieren der Nutzung in vorherigen Monaten, um ggf. Anpassungen für das Lab zu ermitteln.  Die Aufschlüsselung der Kosten in der Vergangenheit finden Sie in der [Abonnementkostenanalyse](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis).  Im Azure-Portal können Sie „Abonnements“ in das obere Suchfeld eingeben und dann die Option „Abonnements“ auswählen.  

> [!div class="mx-imgBorder"]
> ![Abonnementsuche](./media/cost-management-guide/subscription-search.png)

Wählen Sie das Abonnement aus, das Sie überprüfen möchten.

> [!div class="mx-imgBorder"]
> ![Abonnementauswahl](./media/cost-management-guide/subscription-select.png)

 Wählen Sie im linken Bereich unter **Kostenverwaltung** die Option „Kostenanalyse“ aus.

 > [!div class="mx-imgBorder"]
> ![Abonnementkostenanalyse](./media/cost-management-guide/subscription-cost-analysis.png)

Dieses Dashboard ermöglicht eine gründliche Kostenanalyse, die die Möglichkeit einschließt, nach einem Zeitplan in verschiedene Dateitypen zu exportieren.  Die Kostenverwaltung bietet zahlreiche Funktionen, um weitere Informationen zu erhalten. Lesen Sie hierzu [Was ist die Azure-Kostenverwaltung und -Abrechnung?](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview).

Filtern nach Ressourcentyp: In `microsoft.labservices/labaccounts` werden nur die Kosten angezeigt, die Lab Services zugeordnet sind.

## <a name="understanding-the-usage"></a>Grundlegendes zur Nutzung

Im Folgenden finden Sie ein Beispiel für die Kostenanalyse.

> [!div class="mx-imgBorder"]
> ![Abonnementkostenanalyse](./media/cost-management-guide/cost-analysis.png)

Standardmäßig gibt es sechs Spalten: Ressource, Ressourcentyp, Speicherort, Ressourcengruppenname, Tags, Kosten.  Die Spalte „Ressource“ enthält Informationen über das Labkonto, den Labnamen und den virtuellen Computer.  Die Zeilen mit „Labkonto/Labname/Standard“ enthalten die Kosten für das Lab, wie in der zweiten und dritten Zeile zu sehen ist.  Die Kosten der verwendeten VMs sind unter „Labkonto/Labname/Standard/VM-Name“ aufgeführt.  Wenn Sie in diesem Beispiel die erste Zeile zur zweiten Zeile addieren, beide mit „aaalab/dockerlab“ beginnend, erhalten Sie die Gesamtkosten für das Lab „dockerlab“ im Labkonto „aaalab“.

Um Informationen über die Shared Image Gallery zu erhalten, ändern Sie den Ressourcentyp in `Microsoft.Compute/Galleries`, sodass Sie die Gesamtkosten für die Image Gallery erfahren.  Je nachdem, wo der Katalog gespeichert ist, werden die Shared Image Galleries möglicherweise nicht in den Kosten angezeigt.

> [!NOTE]
> Die Shared Image Gallery ist mit einem Labkonto verbunden.  Dies bedeutet, dass mehrere Labs dasselbe Image verwenden könnten.

## <a name="separating-costs"></a>Aufteilen von Kosten

Einige Universitäten haben Labkonto und Ressourcengruppe zur Differenzierung der einzelnen Klassen verwendet.  Jede Klasse verfügt über ein eigenes Labkonto und eine eigene Ressourcengruppe. Fügen Sie im Bereich Kostenanalyse einen Filter auf Basis des Ressourcengruppenamens mit dem entsprechenden Ressourcengruppenamen für die Klasse hinzu, sodass nur die Kosten für diese Klasse angezeigt werden.  Dies ermöglicht beim Anzeigen der Kosten eine klarere Abgrenzung zwischen den verschiedenen Klassen.  Das Feature [geplanter Export](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data) der Kostenanalyse ermöglicht, die Kosten der einzelnen Klassen in separate Dateien herunterzuladen.

## <a name="managing-costs"></a>Kostenverwaltung

Abhängig vom Typ der Klasse gibt es Möglichkeiten, die Kosten zu verwalten, um zu verhindern, dass die VMs ausgeführt werden, ohne dass ein Student den Computer verwendet.

### <a name="auto-shutdown-on-disconnect"></a>Automatisches Herunterfahren beim Trennen

Bei der Erstellung des Labs kann der Labbesitzer die virtuellen Computer im Lab so festlegen, dass [heruntergefahren wird, wenn die RDP-Verbindung mit dem virtuellen Computer getrennt ist](how-to-enable-shutdown-disconnect.md).  Diese Einstellung ist für das Szenario gedacht, dass der Student die Verbindung trennt, aber vergisst, den virtuellen Computer anzuhalten.

### <a name="quota-vs-scheduled-time"></a>Kontingentzeit und geplante Zeit

Wenn der Labbesitzer den Unterschied zwischen [Kontingentzeit](classroom-labs-concepts.md#quota) und [geplanter Zeit](classroom-labs-concepts.md#schedules) kennt, kann er das Lab so konfigurieren, dass es den Anforderungen des Professors und der Studenten besser gerecht wird.  Die geplante Zeit ist eine festgelegte Zeit, in der alle Studenten-VMs gestartet wurden und für Verbindungen verfügbar sind.  „Üblicherweise geplant“ wird verwendet, wenn alle Studenten ihren eigenen virtuellen Computer haben und die Anweisungen des Professors zu einer festgelegten Zeit während des Tages befolgen, wie z. B. in Unterrichtsstunden.  Der Nachteil ist, dass alle Studenten-VMs gestartet werden und auch dann Kosten anfallen, wenn sich ein Student nicht bei der VM angemeldet hat.  Die Kontingentzeit ist die Zeit, die den einzelnen Studierenden zugewiesen wird, und die sie nach eigenem Ermessen verwenden können, und wird häufig für das unabhängige Studium verwendet. Die VM wird erst gestartet, wenn der Student sie startet.  

Ein Lab kann entweder Kontingentzeit, geplante Zeit oder eine Kombination aus beidem verwenden. Wenn eine Klasse die geplante Zeit nicht benötigt, sollten Sie nur die Kontingentzeit verwenden, um die VMs am effektivsten zu nutzen.

### <a name="scheduled-event---stop-only"></a>Geplantes Ereignis – „Nur beenden“

Im Zeitplan können Sie einen Ereignistyp „Nur beenden“ hinzufügen, mit dem alle Computer zu einem bestimmten Zeitpunkt beendet werden.  Einige Labbesitzer haben für jeden Tag um Mitternacht ein „Nur beenden“-Ereignis festgelegt, um die Kosten und die Kontingentnutzung zu reduzieren, wenn ein Student vergessen hat, den virtuellen Computer herunterzufahren, den er verwendet.  Dieser Ereignistyp hat den Nachteil, dass alle VMs heruntergefahren werden, auch wenn der Student die VM verwendet.

### <a name="other-costs-related-to-labs"></a>Andere Kosten im Zusammenhang mit Labs 

Es gibt Kosten, die nicht Lab Services zugeordnet sind, aber an einen Labdienst gebunden werden können.  Eine Shared Image Gallery kann mit Labs verbunden sein, wird aber nicht unter den Lab Services-Kosten angezeigt und verursacht Kosten.  Wenn Sie die Gesamtkosten senken möchten, sollten Sie nicht verwendete Images aus dem Katalog entfernen, da die Images Speicherkosten erben.  Labs können über ein virtuelles Netzwerk (VNET) Verbindungen mit anderen Azure-Ressourcen haben. Wenn ein Lab entfernt wird, entfernen Sie auch das VNET und die anderen Ressourcen.

## <a name="conclusion"></a>Zusammenfassung

Die oben aufgeführten Informationen bieten Ihnen hoffentlich einen besseren Einblick in die Nutzungskosten und die Verwendung der Tools zur Kostensenkung.

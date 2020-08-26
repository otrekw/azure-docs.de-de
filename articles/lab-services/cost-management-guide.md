---
title: Leitfaden zur Kostenverwaltung für Azure Lab Services
description: Informieren Sie sich über die verschiedenen Möglichkeiten, die Kosten für Lab Services anzuzeigen.
author: rbest
ms.author: rbest
ms.date: 08/16/2020
ms.topic: article
ms.openlocfilehash: 0aaa454df05cd8981b314abe238163caced7864c
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88604592"
---
# <a name="cost-management-for-azure-lab-services"></a>Kostenverwaltung für Azure Lab Services

Kostenverwaltung kann in zwei verschiedene Bereiche aufgeteilt werden: Kostenschätzung und Kostenanalyse.  Die Kostenschätzung wird beim Einrichten des Labs durchgeführt, um sicherzustellen, dass die anfängliche Struktur des Labs in das erwartete Budget passt.  Die Kostenanalyse erfolgt in der Regel am Ende des Monats, um die Kosten zu analysieren und die für den nächsten Monat erforderlichen Aktionen zu bestimmen.

## <a name="estimate-the-lab-costs"></a>Schätzen der Labkosten

Jedes Lab-Dashboard verfügt über einen Abschnitt **Kosten und Abrechnung**, in dem eine grobe Schätzung der Kosten des Labs für den Monat festgelegt wird.  Die Kostenschätzung rechnet die Stundennutzung mit der maximalen Anzahl von Benutzern nach den geschätzten Kosten pro Stunde zusammen.  Damit die Schätzung so genau wie möglich ist, richten Sie das Lab einschließlich des [Zeitplans](how-to-create-schedules.md) ein, und das Dashboard spiegelt die geschätzten Kosten wider.  

Diese Schätzung umfasst möglicherweise nicht alle möglichen Kosten. Einige Ressourcen sind nicht eingeschlossen.  Die Kosten für die Vorlagenvorbereitung fließen nicht in die Kostenschätzung ein.  Der Zeitaufwand zum Erstellen der Vorlage kann erheblich variieren. Die Kosten für die Ausführung der Vorlage sind mit den gesamten Labkosten pro Stunde identisch. Alle Kosten für die [Shared Image Gallery](how-to-use-shared-image-gallery.md) sind nicht im Lab-Dashboard enthalten, da ein Katalog von mehreren Labs gemeinsam genutzt werden kann.  Schließlich werden Stunden, die beim Starten eines Computers durch den Labersteller anfallen, von dieser Schätzung ausgeschlossen.

> [!div class="mx-imgBorder"]
> ![Dashboard-Kostenschätzung](./media/cost-management-guide/dashboard-cost-estimation.png)

## <a name="analyze-previous-months-usage"></a>Analysieren der Nutzung in vorherigen Monaten

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

## <a name="understand-the-usage"></a>Erläuterungen der Auslastung

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

### <a name="maximize-cost-control-with-auto-shutdown-settings"></a>Maximieren der Kostenkontrolle durch Einstellungen zum automatischen Herunterfahren

Die Kostenkontrolle durch automatisches Herunterfahren ermöglicht es Ihnen, unnötige Nutzungsstunden virtueller Computer in den Labs zu vermeiden. Die Kombination der folgenden drei Features zum automatischen Herunterfahren und Trennen deckt den größten Teil der Fälle ab, in denen Benutzer die Ausführung der virtuellen Computer versehentlich nicht beenden:

> [!div class="mx-imgBorder"]
> ![Abonnementkostenanalyse](./media/cost-management-guide/auto-shutdown-disconnect.png)

Diese Einstellungen können sowohl auf Labkontoebene als auch auf Labebene konfiguriert werden. Wenn die Einstellungen auf Labkontoebene aktiviert sind, werden sie auf alle Labs im Labkonto angewendet. Für alle neuen Labkonten sind diese Einstellungen standardmäßig aktiviert. 

#### <a name="details-about-auto-shutdown-settings"></a>Details zu den Einstellungen für automatisches Herunterfahren

* Automatisches Trennen der Benutzer von virtuellen Computern, die vom Betriebssystem als Computer im Leerlauf erkannt werden (nur Windows).

    > [!NOTE]
    > Diese Einstellung ist nur für virtuelle Windows-Computer verfügbar.

    Wenn die Einstellung aktiviert ist, wird der Benutzer von allen Computern im Lab getrennt, wenn das Windows-Betriebssystem die Sitzung als Sitzung im Leerlauf erachtet (einschließlich der Vorlagen-VMs). Für die [Definition von Leerlauf durch das Windows-Betriebssystem](https://docs.microsoft.com/windows/win32/taskschd/task-idle-conditions#detecting-the-idle-state) werden zwei Kriterien verwendet: 

    * Benutzerabwesenheit: keine Tastatur- oder Mauseingabe.
    * Kein Ressourcenverbrauch: Alle Prozessoren und alle Datenträger befanden sich für einen bestimmten Zeitraum (in Prozent) im Leerlauf.

    Bevor Benutzer getrennt werden, wird auf dem virtuellen Computer eine Meldung wie die folgende angezeigt: 

    > [!div class="mx-imgBorder"]
    > ![Abonnementkostenanalyse](./media/cost-management-guide/idle-timer-expired.png)
    
    Der virtuelle Computer wird immer noch ausgeführt, wenn der Benutzer getrennt wurde. Wenn der Benutzer durch Anmelden die Verbindung mit dem virtuellen Computer wiederherstellt, sind Fenster oder Dateien, die vor der Trennung geöffnet waren, oder Inhalte, die vor der Trennung nicht gespeichert wurden, noch vorhanden. Da der virtuelle Computer ausgeführt wird, gilt er in diesem Status weiterhin als aktiv, und es fallen Kosten an. 
    
    Verwenden Sie zum automatischen Herunterfahren von virtuellen Windows-Computern, die nicht getrennt sind, die beiden Einstellungen **Benutzerverbindung bei Leerlauf der VMs trennen** und **VMs bei Trennen der Benutzerverbindung herunterfahren**.

    Angenommen, Sie konfigurieren die Einstellungen wie folgt:
    
    * Benutzerverbindung bei Leerlauf der VMs trennen – 15 Minuten nach Feststellung des Leerlaufzustands
    * VMs bei Trennen der Benutzerverbindung herunterfahren – 5 minutes after user disconnects (5 Minuten nach Trennen durch Benutzer)
    
    Die virtuellen Windows-Computer werden automatisch 20 Minuten nach der letzten Verwendung durch den Benutzer heruntergefahren. 
    
    > [!div class="mx-imgBorder"]
    > ![Abonnementkostenanalyse](./media/cost-management-guide/vm-idle-diagram.png)
* Automatisches Herunterfahren virtueller Computer, wenn Benutzer die Verbindung trennen (Windows und Linux).
    
    Diese Einstellung wird für virtuelle Windows- und Linux-Computer unterstützt. Wenn sie aktiviert ist, erfolgt das automatische Herunterfahren unter folgenden Bedingungen:
    
    * Für Windows – die Remotedesktopverbindung (RDP) ist getrennt.
    * Für Linux – die SSH-Verbindung ist getrennt.
    
    > [!NOTE]
    > Es werden nur [bestimmte Distributionen und Versionen von Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/diagnostics-linux#supported-linux-distributions) unterstützt.
    
    Sie können angeben, wie lange die VMs auf eine erneute Verbindungsherstellung des Benutzers warten sollen, bevor sie automatisch heruntergefahren werden. 
* Automatisches Herunterfahren von virtuellen Computern, die gestartet wurden, mit denen jedoch von den Benutzern keine Verbindung hergestellt wurde.
     
    Möglicherweise startet ein Benutzer in einem Lab einen virtuellen Computer und stellt keine Verbindung mit ihm her. Beispiel:
    
    * Durch einen Zeitplan im Lab werden alle virtuellen Computer für eine Unterrichtssitzung gestartet, einige Studenten bleiben jedoch dem Unterricht fern und stellen keine Verbindung mit ihren Computern her.  
    * Ein Benutzer startet einen virtuellen Computer, vergisst aber, eine Verbindung herzustellen. 
    
    Durch die Einstellung „VMs herunterfahren, wenn Benutzer keine Verbindung herstellen“ werden diese Fälle behandelt und die virtuellen Computer automatisch heruntergefahren.  
    
Informationen zum Konfigurieren und Aktivieren des automatischen Herunterfahrens von virtuellen Computern bei Verbindungstrennung finden Sie in den folgenden Artikeln:

* [Konfigurieren des automatischen Herunterfahrens von virtuellen Computern für ein Labkonto](how-to-configure-lab-accounts.md)
* [Konfigurieren des automatischen Herunterfahrens von virtuellen Computern für ein Lab](how-to-enable-shutdown-disconnect.md)

### <a name="quota-vs-scheduled-time"></a>Kontingentzeit und geplante Zeit

Wenn der Labbesitzer den Unterschied zwischen [Kontingentzeit](classroom-labs-concepts.md#quota) und [geplanter Zeit](classroom-labs-concepts.md#schedules) kennt, kann er das Lab so konfigurieren, dass es den Anforderungen des Professors und der Studenten besser gerecht wird.  Die geplante Zeit ist eine festgelegte Zeit, in der alle Studenten-VMs gestartet wurden und für Verbindungen verfügbar sind.  „Üblicherweise geplant“ wird verwendet, wenn alle Studenten ihren eigenen virtuellen Computer haben und die Anweisungen des Professors zu einer festgelegten Zeit während des Tages befolgen, wie z. B. in Unterrichtsstunden.  Der Nachteil ist, dass alle Studenten-VMs gestartet werden und auch dann Kosten anfallen, wenn sich ein Student nicht bei der VM angemeldet hat.  Die Kontingentzeit ist die Zeit, die den einzelnen Studierenden zugewiesen wird, und die sie nach eigenem Ermessen verwenden können, und wird häufig für das unabhängige Studium verwendet. Die VM wird erst gestartet, wenn der Student sie startet.  

Ein Lab kann entweder Kontingentzeit, geplante Zeit oder eine Kombination aus beidem verwenden. Wenn eine Klasse die geplante Zeit nicht benötigt, sollten Sie nur die Kontingentzeit verwenden, um die VMs am effektivsten zu nutzen.

### <a name="scheduled-event---stop-only"></a>Geplantes Ereignis – „Nur beenden“

Im Zeitplan können Sie einen Ereignistyp „Nur beenden“ hinzufügen, mit dem alle Computer zu einem bestimmten Zeitpunkt beendet werden.  Einige Labbesitzer haben für jeden Tag um Mitternacht ein „Nur beenden“-Ereignis festgelegt, um die Kosten und die Kontingentnutzung zu reduzieren, wenn ein Student vergessen hat, den virtuellen Computer herunterzufahren, den er verwendet.  Dieser Ereignistyp hat den Nachteil, dass alle VMs heruntergefahren werden, auch wenn der Student die VM verwendet.

### <a name="other-costs-related-to-labs"></a>Andere Kosten im Zusammenhang mit Labs 

Es gibt Kosten, die nicht Lab Services zugeordnet sind, aber an einen Labdienst gebunden werden können.  Eine Shared Image Gallery kann mit Labs verbunden sein, wird aber nicht unter den Lab Services-Kosten angezeigt und verursacht Kosten.  Wenn Sie die Gesamtkosten senken möchten, sollten Sie nicht verwendete Images aus dem Katalog entfernen, da die Images Speicherkosten erben.  Labs können über ein virtuelles Netzwerk (VNET) Verbindungen mit anderen Azure-Ressourcen haben. Wenn ein Lab entfernt wird, entfernen Sie auch das VNET und die anderen Ressourcen.

## <a name="conclusion"></a>Zusammenfassung

Die oben aufgeführten Informationen bieten Ihnen hoffentlich einen besseren Einblick in die Nutzungskosten und die Verwendung der Tools zur Kostensenkung.

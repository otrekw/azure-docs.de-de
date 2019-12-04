---
title: Bewährte Methoden – Azure Batch
description: Erhalten Sie Informationen über bewährte Methoden und nützliche Tipps für das Entwickeln Ihrer Azure Batch-Lösung.
author: laurenhughes
ms.author: lahugh
ms.date: 11/22/2019
ms.service: batch
ms.topic: article
manager: gwallace
ms.openlocfilehash: 19c5b6acaeddb915af49cf62a884da0678075f15
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2019
ms.locfileid: "74534878"
---
# <a name="azure-batch-best-practices"></a>Azure Batch: bewährte Methoden

In diesem Artikel werden bewährte Methoden für die effektive und effiziente Verwendung des Azure Batch-Diensts behandelt. Diese bewährten Methoden leiten sich aus unseren Erfahrungen mit Batch und den Erfahrungen von Batch-Kunden ab. Es ist wichtig, diesen Artikel zu verstehen, um Fehler beim Entwurf, potenzielle Leistungsprobleme und Antimuster bei der Entwicklung für und Verwendung von Batch zu vermeiden.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> - Welche bewährten Methoden gibt es?
> - Gründe für die Verwendung der bewährten Methoden
> - Mögliche Folgen, wenn Sie die bewährte Methode nicht einhalten
> - So halten Sie die bewährten Methoden ein

## <a name="pools"></a>Pools

Batch-Pools sind die Computeressourcen zum Ausführen von Aufträgen im Batch-Dienst. Die folgenden Abschnitte bieten Anleitungen zu den wichtigsten bewährten Methoden, die Sie bei der Arbeit mit Batch-Pools einhalten sollten.

### <a name="pool-configuration-and-naming"></a>Poolkonfiguration und Benennung

- **Poolzuordnungsmodus**  
    Beim Erstellen eines Batch-Kontos können Sie zwischen zwei Modi der Poolzuordnung wählen: **Batch-Dienst** und **Benutzerabonnement**. In den meisten Fällen sollten Sie den standardmäßigen Batchdienstmodus wählen, in dem Pools im Hintergrund von Batch verwalteten Abonnements zugeordnet werden. Im alternativen Benutzerabonnementmodus werden virtuelle Batchcomputer und andere Ressourcen direkt in Ihrem Abonnement erstellt, wenn ein Pool erstellt wird. Benutzerabonnementkonten werden hauptsächlich zum Ermöglichen einer wichtigen, aber kleinen Teilmenge von Szenarien verwendet. Weitere Informationen zum Benutzerabonnementmodus finden Sie unter [Zusätzliche Konfiguration für den Benutzerabonnementmodus](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode).

- **Berücksichtigen Sie die Ausführungszeit von Aufträgen und Aufgaben beim Festlegen der Zuordnung von Aufträgen zum Pool.**  
    Wenn Sie Aufträge haben, die hauptsächlich aus kurzen Aufgaben bestehen, und wenn die erwartete Gesamtzahl der Aufgaben gering ist, sodass auch die insgesamt zu erwartende Ausführungszeit des Auftrags nicht lang ist, sollten Sie nicht für jeden Auftrag einen neuen Pool zuordnen. Die Zuordnungszeit der Knoten verringert die Laufzeit des Auftrags.

- **Pools sollten mehr als einen Computeknoten besitzen.**  
    Es ist nicht garantiert, dass einzelne Knoten immer verfügbar sind. Wenn auch ungewöhnlich, so können Hardwareausfälle, Betriebssystemupdates und eine Menge anderer Probleme dazu führen, dass einzelne Knoten offline sind. Wenn Ihr Batch-Workload deterministischen, garantierten Fortschritt erfordert, sollten Sie Pools mit mehreren Knoten zuordnen.

- **Verwenden Sie Ressourcennamen nicht wieder.**  
    Batch-Ressourcen (Aufträge, Pools usw.) kommen und gehen häufig im Laufe der Zeit. Beispielsweise erstellen Sie vielleicht am Montag einen Pool, den Sie am Dienstag löschen, um dann am Donnerstag einen weiteren Pool zu erstellen. Jeder neue Ressource, die Sie erstellen, sollte einen eindeutigen Name erhalten, den Sie zuvor noch nicht verwendet haben. Hierzu können Sie eine GUID (entweder als vollständiger Ressourcennamen oder als Teil davon) verwenden oder die Erstellungszeit der Ressource in den Ressourcennamen einbetten. Batch unterstützt [DisplayNam-](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.jobspecification.displayname?view=azure-dotnet), womit Sie einer Ressource einen von Menschen lesbaren Namen geben können, auch wenn die tatsächliche Ressourcen-ID nicht wirklich menschenfreundlich ist. Durch die Verwendung eindeutiger Namen können Sie leichter unterscheiden, welche spezifische Ressource in Protokollen und Metriken etwas bewirkt hat. Außerdem entfällt hierdurch jede eventuelle Mehrdeutigkeit, wenn Sie jemals eine Supportanfrage für eine Ressource einreichen müssen.

- **Kontinuität während Wartung und Ausfall von Pools.**  
    Es ist optimal, wenn Ihre Aufträge Pools dynamisch verwenden. Wenn Ihre Aufträge denselben Pool für alles verwenden, besteht die Möglichkeit, dass Ihre Aufträge nicht ausgeführt werden, wenn ein Problem mit dem Pool auftritt. Dies ist besonders wichtig für zeitkritische Workloads. Um dieses Problem zu beheben, wählen oder erstellen Sie einen Pool dynamisch, wenn Sie die einzelnen Aufträge planen, oder implementieren Sie eine Methode, um den Poolnamen außer Kraft zu setzen, sodass fehlerhafte Pools umgangen werden können.

- **Geschäftskontinuität während Wartung und Ausfall von Pools**  
    Es gibt viele mögliche Ursachen, die möglicherweise verhindern, dass ein Pool auf die gewünschte Größe anwächst, z. B. interne Fehler, Kapazitätseinschränkungen usw. Aus diesem Grund sollten Sie bereit sein, Aufträge nötigenfalls einem anderen Pool zuzuweisen (möglicherweise mit einer anderen VM-Größe; Batch unterstützt dies mittels [UpdateJob](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.protocol.joboperationsextensions.update?view=azure-dotnet)). Vermeiden Sie die Verwendung einer statischen Pool-ID mit der Erwartung, dass Sie nie gelöscht und nie geändert wird.

### <a name="pool-lifetime-and-billing"></a>Lebensdauer und Abrechnung für Pools

Die Poollebensdauer kann abhängig von der Zuordnungsmethode und den auf die Poolkonfiguration angewendeten Optionen variieren. Pools können eine beliebige Lebensdauer sowie eine jederzeit variable Anzahl von Computeknoten im Pool besitzen. Es liegt in ihrer Verantwortung, die Computeknoten im Pool entweder explizit oder durch vom Dienst bereitgestellte Funktionen (Autoskalierung oder AutoPool) zu verwalten.

- **Pools aktuell halten.**  
    Sie sollten die Größe Ihrer Pools alle paar Monate auf Null setzen, um sicherzustellen, dass Sie die neuesten Updates und Fehlerbehebungen für den Knoten-Agent erhalten. Ihr Pool empfängt keine Updates für den Knoten-Agent, es sei denn, er wird neu erstellt oder seine Größe auf 0 Computeknoten geändert. Vor dem erneuten Erstellen oder Ändern der Größe Ihres Pools wird empfohlen, alle Knoten-Agent-Protokolle zu Debuggingzwecken herunterzuladen, wie im Abschnitt [Knoten](#nodes) erläutert.

- **Neuerstellung von Pools**  
    In ähnlicher Weise ist es nicht empfehlenswert, Ihre Pools täglich zu löschen und neu zu erstellen. Erstellen Sie stattdessen einen neuen Pool, und aktualisieren Sie die vorhandenen Aufträge so, dass Sie auf den neuen Pool verweisen. Nachdem alle Aufgaben in den neuen Pool verschoben wurden, löschen Sie den alten Pool.

- **Effizienz und Abrechnung von Pools**  
    Für Batch selbst fallen keine zusätzlichen Gebühren an, aber es fallen Gebühren für die verwendeten Computeressourcen an. Ihnen wird jeder Computeknoten im Pool in Rechnung gestellt, unabhängig davon, in welchem Zustand er sich befindet. Dies schließt alle Gebühren ein, die für die Ausführung des Knotens erforderlich sind, z. B. Speicher- und Netzwerkkosten. Weitere Informationen zu bewährten Methoden finden Sie unter [Kostenanalyse und Budget für Azure Batch](budget.md).

### <a name="pool-allocation-failures"></a>Fehler bei der Poolzuordnung

Fehler bei der Poolzuordnung können jederzeit während der ersten Zuordnung oder bei nachfolgenden Größenänderungen auftreten. Dies kann an einer vorübergehenden Kapazitätserschöpfung in einer Region oder an Fehlern in anderen Azure-Diensten liegen, auf denen Batch basiert. Ihr Kernkontingent ist keine Garantie, sondern ein Limit.

### <a name="unplanned-downtime"></a>Ungeplante Ausfallzeiten

Es ist möglich, dass es bei Batch-Pools in Azure zu Ausfallzeiten kommt. Es ist wichtig, dies im Hinterkopf zu behalten, wenn Sie Ihr Szenario oder Ihren Workflow für Batch planen und entwickeln.

Wenn ein Knoten ausfällt, versucht Batch automatisch, diese Computeknoten in Ihrem Auftrag wiederherzustellen. Dadurch wird möglicherweise eine Neuplanung aller laufenden Aufgaben auf dem Knoten ausgelöst, der wiederhergestellt wird. Weitere Informationen zu unterbrochenen Aufgaben finden Sie unter [Entwerfen für Wiederholungsversuche](#designing-for-retries-and-re-execution).

- **Abhängigkeit von Azure-Regionen**  
    Es wird empfohlen, keine Abhängigkeit von einer einzelnen Azure-Region einzurichten, wenn Sie eine zeitkritische oder Produktionsworkload haben. In seltenen Fällen kann es Probleme geben, die sich auf eine ganze Region auswirken können. Wenn Ihre Verarbeitung beispielsweise zu einem bestimmten Zeitpunkt gestartet werden muss, sollten Sie erwägen, den Pool in ihrer primären Region zentral hochzuskalieren, und zwar *eine ganze Weile im Voraus vor der Startzeit*. Wenn diese Poolskalierung fehlschlägt, können Sie als Plan B einen Pool in einer (oder mehreren) Sicherungsregion(en) zentral hochskalieren. Pools über mehrere Konten hinweg in unterschiedlichen Regionen bieten eine bereite, leicht zugängliche Sicherung, wenn bei einem anderen Pool etwas schiefgeht. Weitere Informationen finden Sie unter [Entwerfen Ihrer Anwendung für Hochverfügbarkeit](high-availability-disaster-recovery.md).

## <a name="jobs"></a>Aufträge

Ein Auftrag ist ein Container, der auf die Aufnahme Hunderter, Tausender oder sogar von Millionen von Aufgaben ausgelegt ist.

- **Aufnehmen vieler Aufgaben in einen Auftrag**  
    Die Verwendung eines Auftrags zum Ausführen einer einzelnen Aufgabe ist ineffizient. Beispielsweise ist es effizienter, einen einzelnen Auftrag mit 1000 Aufgaben zu verwenden, statt 100 Aufträge zu erstellen, die jeweils 10 Aufgaben enthalten. Das Ausführen von 1000 Aufträgen mit jeweils einer einzelnen Aufgabe wäre der am wenigsten effiziente, langsamste und teuerste Ansatz.  

    Entwerfen Sie keine Batch-Lösung, die Tausende gleichzeitig aktive Aufträge erfordert. Es gibt kein Kontingent für Aufgaben. Wenn Sie also so viele Aufgaben unter so wenigen Aufträgen wie möglich ausführen, werden Ihre [Kontingente für Aufträge und die Zeitplanung von Aufträgen](batch-quota-limit.md#resource-quotas) effizient verwendet.

- **Lebensdauer von Aufträgen**  
    Ein Batch-Auftrag hat eine unbegrenzte Lebensdauer, bis er aus dem System gelöscht wird. Der Zustand eines Auftrags bestimmt, ob er mehr Aufgaben für die Planung annehmen kann oder nicht. Ein Auftrag wechselt nicht automatisch in den Zustand „Abgeschlossen“, es sei denn, er wurde explizit beendet. Dies kann automatisch über die Eigenschaft [onAllTasksComplete](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.onalltaskscomplete?view=azure-dotnet) oder [maxWallClockTime-](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints) ausgelöst werden.

Es gibt ein standardmäßiges [Kontingent für aktive Aufträge und die Zeitplanung von Aufträgen](batch-quota-limit.md#resource-quotas). Aufträge und Auftragszeitpläne im Zustand „Abgeschlossen“ werden nicht auf dieses Kontingent angerechnet.

## <a name="tasks"></a>Aufgaben

Aufgaben sind einzelne Arbeitseinheiten, die einen Auftrag bilden. Tasks werden vom Benutzer übermittelt und von Batch auf Computeknoten geplant. Beim Erstellen und Ausführen von Tasks gibt es einige zu berücksichtigende Aspekte. In den folgenden Abschnitten werden gängige Szenarien erläutert und wie Sie Ihre Aufgaben so entwerfen, dass sie mit Probleme umgehen können und effizient ausgeführt werden.

- **Speichern von Aufgabendaten als Teil der Aufgabe.**  
    Computeknoten sind von Natur aus kurzlebig. Es gibt viele Funktionen in Batch wie AutoPool und Autoskalierung, die das Verschwinden von Knoten begünstigen. Wenn Knoten einen Pool verlassen (aufgrund einer Größenänderung oder des Löschens eine Pools), werden alle Dateien auf diesen Knoten ebenfalls gelöscht. Aus diesem Grund wird empfohlen, dass die Ausgabe vor Abschluss einer Aufgabe von dem Knoten, auf dem Sie ausgeführt wird, in einen permanenten Speicher verschoben wird. Ähnlich sollten Sie, wenn ein Task fehlschlägt, auch die Protokolle in einen permanenten Speicher verschieben, die zur Diagnose des Fehlers erforderlich sind. Batch verfügt über integrierte Unterstützung für Azure Storage zum Hochladen von Daten über [OutputFiles](batch-task-output-files.md) sowie eine Vielzahl von freigegebenen Dateisystemen, oder Sie können auch den Upload selbst in Ihren Aufgaben durchführen.

### <a name="task-lifetime"></a>Gültigkeitsdauer von Tasks

- **Löschen von Aufgaben, wenn Sie abgeschlossen sind.**  
    Löschen Sie Aufgaben, wenn Sie nicht mehr benötigt werden, oder legen Sie eine Einschränkung durch eine [retentionTime](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.retentiontime?view=azure-dotnet)-Aufgabe fest. Wenn eine `retentionTime` festgelegt wird, bereinigt Batch automatisch den von der Aufgabe verwendeten Speicherplatz, wenn die `retentionTime` abläuft.  

    Durch das Löschen von Aufgaben werden zwei Dinge erreicht. Es wird sichergestellt, dass sich nicht übermäßig Aufgaben im Auftrag ansammeln, was das Abfragen/Auffinden der Aufgabe erschwert, an der Sie interessiert sind (da Sie die abgeschlossenen Aufgaben filtern müssen). Es werden ferner die entsprechenden Aufgabendaten auf dem Knoten bereinigt (sofern `retentionTime` nicht bereits erreicht wurde). Dadurch wird sichergestellt, dass Ihre Knoten nicht mit Aufgabendaten aufgefüllt werden und der Speicherplatz knapp wird.

### <a name="task-submission"></a>Aufgabenübermittlung

- **Übermitteln einer großen Anzahl von Aufgaben in einer Sammlung.**  
    Aufgaben können einzeln oder in Sammlungen übermittelt werden. Übermitteln Sie Aufgaben in [Sammlungen](https://docs.microsoft.com/rest/api/batchservice/task/addcollection) von bis zu 100 Stück gleichzeitig, wenn Sie eine Massenübermittlung von Aufgaben durchführen, um den Aufwand und die Übermittlungszeiten zu verringern.

### <a name="task-execution"></a>Aufgabenausführung

- **Auswählen der maximalen Anzahl von Aufgaben pro Knoten**  
    Batch unterstützt das Überabonnieren von Aufgaben auf Knoten (Ausführen von mehr Aufgaben, als ein Knoten über Kerne verfügt). Es liegt an Ihnen, sicherzustellen, dass ihre Aufgaben in die Knoten in Ihrem Pool „passen“. Es kann beispielsweise zu einer verschlechterten Erfahrung kommen, wenn Sie versuchen, acht Aufgaben zu planen, die jeweils 25 % CPU-Auslastung auf einem Knoten verbrauchen (in einem Pool mit `maxTasksPerNode = 8`).

### <a name="designing-for-retries-and-re-execution"></a>Entwerfen für Wiederholungsversuche und erneute Ausführung

Aufgaben können von Batch automatisch wiederholt werden. Es gibt zwei Arten von Wiederholungsversuchen: benutzergesteuert und intern. Benutzergesteuerte Wiederholungsversuche werden über die [maxTaskRetryCount](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.maxtaskretrycount?view=azure-dotnet) der Aufgabe angegeben. Wenn ein in der Aufgabe angegebenes Programm mit einem Exitcode ungleich 0 (null) beendet wird, wird die Aufgabe so oft, wie im Wert `maxTaskRetryCount` angegeben, wiederholt.

Zwar ist dies selten, doch eine Aufgabe kann aufgrund von Fehlern auf dem Computeknoten intern wiederholt werden, z. B. wenn der interne Zustand nicht aktualisiert werden kann oder ein Fehler auf dem Knoten auftritt, während die Aufgabe ausgeführt wird. Die Aufgabe wird, falls möglich, auf demselben Computeknoten bis zu einem internen Limit wiederholt, bevor die Aufgabe aufgegeben und dann zur Neuplanung durch Batch verzögert wird, potenziell auf einem anderen Computeknoten.

- **Erstellen von „Durable Tasks“ (Permanente Aufgaben)**  
    Aufgaben sollten so entworfen werden, dass Sie Fehlern widerstehen und eine Wiederholung ermöglichen. Dies gilt besonders wichtig für zeitintensive Aufgaben. Stellen Sie zu diesem Zweck sicher, dass Aufgaben dasselbe einzige Ergebnis generieren, auch wenn Sie mehrmals ausgeführt werden. Eine Möglichkeit, dies zu erreichen, besteht darin, ihre Aufgaben „zielsuchend“ auszuführen. Eine andere Möglichkeit besteht darin sicherzustellen, dass ihre Aufgaben idempotent sind (Aufgaben haben immer dasselbe Ergebnis, egal wie oft Sie ausgeführt werden).

    Ein gängiges Beispiel hierfür ist eine Aufgabe zum Kopieren von Dateien auf einen Computeknoten. Ein einfacher Ansatz ist eine Aufgabe, die bei jeder Ausführung alle angegebenen Dateien kopiert, was ineffizient und nicht robust ist. Erstellen Sie stattdessen eine Aufgabe, um sicherzustellen, dass sich die Dateien auf dem Computeknoten befinden, eine Aufgabe, die keine Dateien wiederholt kopiert, die bereits vorhanden sind. Auf diese Weise setzt die Aufgabe an der Stelle fort, an der Sie aufgehört hatte oder unterbrochen wurde.

- **Knoten mit niedriger Priorität**  
    Beim Ausführen Ihrer Aufgaben auf dedizierten Knoten oder Knoten mit niedriger Priorität gibt es keine Unterschiede beim Design. Unabhängig davon, ob eine Aufgabe bei der Ausführung auf einem Knoten mit niedriger Priorität vorzeitig entfernt oder aufgrund eines Fehlers auf einem dedizierten Knoten unterbrochen wird, werden beide Situationen durch das Entwerfen einer gegenüber Fehlern robusten Aufgabe abgemildert.

- **Aufgabenausführungszeit**  
    Vermeiden Sie Aufgaben mit kurzer Ausführungszeit. Aufgaben, die nur für eine bis zwei Sekunden ausgeführt werden, sind nicht ideal. Sie sollten versuchen, eine signifikante Menge an Arbeit in einer einzelnen Aufgabe zu verrichten (mindestens 10 Sekunden, bis zu Stunden oder Tage). Wenn jede Aufgabe für eine Minute (oder mehr) ausgeführt wird, ist der Planungsaufwand als Anteil an der gesamten Computezeit gering.

## <a name="nodes"></a>Nodes

- **Startaufgaben sollten idempotent sein**  
    Ähnlich wie bei anderen Aufgaben, sollte die Knotenstartaufgabe idempotent sein, da Sie bei jedem Neustart des Knotens erneut ausgeführt wird. Bei einer idempotenten Aufgabe handelt es sich um eine Aufgabe, die bei mehreren Wiederholungen ein konsistentes Ergebnis erzeugt.

- **Verwalten zeitintensiver Dienste über die Betriebssystem-Diensteschnittstelle.**  
    In manchen Fällen ist es erforderlich, einen anderen Agent parallel zum Batch-Agent im Knoten auszuführen, z. B. um Daten vom Knoten zu erfassen und zu melden. Wir empfehlen, diese Agents als Betriebssystemdienste bereitzustellen, z. B. als Windows-Dienst oder als `systemd`-Dienst von Linux.  

    Bei der Ausführung dieser Dienste dürfen sie keine Dateisperren für Dateien in von Batch verwalteten Verzeichnissen auf dem Knoten einrichten, da Batch sonst diese Verzeichnisse wegen der Dateisperren nicht löschen kann. Wenn Sie beispielsweise einen Windows-Dienst in einer Startaufgabe installieren, anstatt den Dienst direkt aus dem Arbeitsverzeichnis der Startaufgabe zu starten, kopieren Sie die Dateien an einen anderen Ort (wenn die Dateien vorhanden sind, überspringen Sie einfach das Kopieren). Installieren Sie den Dienst von diesem Ort aus. Wenn Batch Ihre Startaufgabe erneut ausführt, wird das Arbeitsverzeichnis der Startaufgabe gelöscht und erneut erstellt. Dies funktioniert, weil der Dienst Dateisperren im anderen Verzeichnis und nicht im Arbeitsverzeichnis der Startaufgabe eingerichtet hat.

- **Vermeiden der Erstellung von Verzeichnisverbindungen**  
    Verzeichnisverbindungen, manchmal auch als feste Verzeichnisverknüpfungen bezeichnet, sind während der Aufgaben- und Auftragsbereinigung schwierig zu behandeln. Verwenden Sie symlinks (symbolische Verknüpfungen) anstelle von festen Verknüpfungen.

- **Erfassen der Batch-Agent-Protokolle, wenn ein Problem vorliegt**  
    Wenn Sie ein Problem bemerken, das mit dem Verhalten eines Knotens oder einer Aufgabe, die auf einem Knoten ausgeführt wird, zusammenhängt, empfiehlt es sich, die Batch-Agent-Protokolle vor dem Aufheben der Zuordnung der fraglichen Knoten zu erfassen. Die Batch-Agent-Protokolle lassen sich mithilfe der API zum Hochladen von Batch-Dienstprotokollen erfassen. Diese Protokolle können als Teil eines Supporttickets an Microsoft bereitgestellt werden und helfen bei der Behandlung und Behebung von Problemen.

## <a name="security"></a>Sicherheit

### <a name="security-isolation"></a>Sicherheitsisolierung

Zum Zwecke der Isolierung, falls Ihr Szenario die Isolierung von Aufträgen voneinander erfordert, sollten Sie diese Aufträge isolieren, indem Sie sie in separaten Pools platzieren. Ein Pool ist die Sicherheitsisolierungsgrenze in Batch, und standardmäßig sind zwei Pools nicht gegenseitig sichtbar und können auch nicht miteinander kommunizieren. Vermeiden Sie die Verwendung separater Batch-Konten als Isolierungsmethode.

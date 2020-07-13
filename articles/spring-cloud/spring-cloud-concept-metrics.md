---
title: Grundlegendes zu Metriken für Azure Spring Cloud
description: Hier erfahren Sie, wie Sie Metriken in Azure Spring Cloud überprüfen.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: brendm
ms.openlocfilehash: ed8a46683d79211559b4af7972c6d1bb784e1f02
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84434160"
---
# <a name="understand-metrics-for-azure-spring-cloud"></a>Grundlegendes zu Metriken für Azure Spring Cloud

Der Metrik-Explorer von Azure ist eine Komponente des Microsoft Azure-Portals, die das Zeichnen von Diagrammen, das visuelle Korrelieren von Trends und das Untersuchen von Spitzen und Tiefen in Metriken ermöglicht. Verwenden Sie den Metrik-Explorer zum Untersuchen der Integrität und Auslastung Ihrer Ressourcen. 

In Azure Spring Cloud gibt es zwei Ansichten für Metriken.
* Übersichtsseite mit Diagrammen für jede Anwendung
* Seite mit allgemeinen Metriken

 ![Metrikdiagramme](media/metrics/metrics-1.png)

Über die Diagramme auf der Seite **Übersicht** einer Anwendung kann der Status einer Anwendung schnell überprüft werden. Auf der allgemeinen Seite **Metriken** können alle Metriken eingesehen werden. Sie können auf der allgemeinen Seite „Metriken“ Ihre eigenen Diagramme erstellen und an das Dashboard anheften.

## <a name="application-overview-page"></a>Übersichtsseite der Anwendung
Wählen Sie in **Apps** eine App aus, damit Diagramme für sie auf der Seite „Übersicht“ angezeigt werden.  

 ![Anwendungsmetriken in der App-Verwaltung](media/metrics/metrics-2.png)

Die Seite **Anwendungsübersicht** jeder Anwendung enthält ein Metrikdiagramm, das eine schnelle Überprüfung des Anwendungsstatus ermöglicht.  

 ![Die Seite „Übersicht“ der Anwendungsmetriken](media/metrics/metrics-3.png)

In Azure Spring Cloud stehen Ihnen diese fünf Metrikdiagramme zur Verfügung, die minütlich aktualisiert werden:

* **HTTP-Serverfehler**: Fehleranzahl für an Ihre App gesendete HTTP-Anforderungen
* **Eingehende Daten**: Von Ihrer App empfangene Byte
* **Ausgehende Daten**: Von Ihrer App gesendete Byte
* **Anforderungen**: Von Ihrer App empfangene Anforderungen
* **Durchschnittliche Antwortzeit**: Durchschnittliche Antwortzeit Ihrer App

Für die Diagramme kann ein Zeitbereich von einer Stunde bis zu sieben Tagen ausgewählt werden.

## <a name="common-metrics-page"></a>Seite mit allgemeinen Metriken

Wenn Sie im linken Navigationsbereich auf **Metriken** klicken, werden Sie auf die Seite mit allgemeinen Metriken weitergeleitet.

Wählen Sie zuerst Metriken aus, die Sie sich ansehen möchten:

![Auswählen einer Metrikansicht](media/metrics/metrics-4.png)

Details zu allen Metrikoptionen finden Sie im nachfolgenden [Abschnitt](#user-metrics-options).

Wählen Sie dann für jede Metrik einen Aggregationstyp aus:

![Metrikaggregation](media/metrics/metrics-5.png)

Der Aggregationstyp gibt an, wie metrische Punkte im Diagramm nach Zeit aggregiert werden können. Jede Minute gibt es einen Rohmetrikpunkt, und der Vorabaggregationstyp innerhalb einer Minute ist durch den Metriktyp vordefiniert.
* Summe: Summieren aller Werte als Zielausgabe.
* Durchschnitt: Verwenden des Durchschnittswerts im Zeitraum als Zielausgabe
* Maximum/Minimum: Verwenden des maximalen/minimalen Werts im Zeitraum als Zielausgabe

Der Zeitbereich kann auch von den letzten 30 Minuten bis zu den letzten 30 Tagen oder einem benutzerdefinierten Zeitbereich angepasst werden.

![Ändern des Zeitbereichs für Metriken](media/metrics/metrics-6.png)

In der Standardansicht werden die Metriken aller Anwendungen des Azure Spring Cloud-Diensts angezeigt. Für die Anzeige können dann Metriken für eine App oder Instanz gefiltert werden.  Klicken Sie auf **Filter hinzufügen**, legen Sie die Eigenschaft auf **App** fest, und wählen Sie im Textfeld **Werte** die zu überwachende Zielanwendung aus. 

Sie können zwei Arten von Filtern (Eigenschaften) verwenden:
* App: Nach App-Namen filtern
* Instanz: Nach App-Instanz filtern

![Filter für Metriken](media/metrics/metrics-7.png)

Sie können auch die Option **Apply splitting** (Aufteilung anwenden) verwenden. Dabei werden mehrere Linien für eine App gezeichnet:

![Aufteilung von Metriken](media/metrics/metrics-8.png)

>[!TIP]
> Sie können auf der Seite „Metriken“ Ihre eigenen Diagramme erstellen und an das **Dashboard** anheften. Geben Sie dem Diagramm zunächst einen Namen.  Wählen Sie anschließend oben rechts **An Dashboard anheften** aus. Sie können nun Ihre Anwendung auf dem **Dashboard** im Portal überprüfen.

## <a name="user-metrics-options"></a>Benutzermetrikoptionen

Die verfügbaren Metriken und ihre Details sind in den nachfolgenden Tabellen aufgeführt.

### <a name="error"></a>Fehler
>[!div class="mx-tdCol2BreakAll"]
>| Name | Name der Metrik für den Azure Spring Cloud-Aktuator | Einheit | Details |
>|----|----|----|------------|
>| Tomcat Global Error (Globaler Tomcat-Fehler) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt)  | tomcat.global.error | Anzahl | Anzahl der bei der Verarbeitung von Anforderungen aufgetretenen Fehler |
>| tomcat.global.error | tomcat.global.error | Anzahl | Anzahl der bei der Verarbeitung von Anforderungen aufgetretenen Fehler |

### <a name="performance"></a>Leistung
>[!div class="mx-tdCol2BreakAll"]
>| Name | Name der Metrik für den Azure Spring Cloud-Aktuator | Einheit | Details |
>|----|----|----|------------|
>| System CPU Usage Percentage (Prozentualer Anteil der System-CPU-Auslastung) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | system.cpu.usage | Percent | Aktuelle CPU-Auslastung für das gesamte System Dieser Wert ist im [0.0,1.0]-Intervall ein Double. Ein Wert von 0.0 bedeutet, dass sich alle CPUs während des aktuell beobachteten Zeitraums im Leerlauf befanden. Ein Wert von 1.0 bedeutet, dass alle CPUs während 100 Prozent der Zeit des aktuell beobachteten Zeitraums aktiv ausgeführt wurden.|
>| system.cpu.usage | system.cpu.usage | Percent | Aktuelle CPU-Auslastung für das gesamte System Dieser Wert ist im [0.0,1.0]-Intervall ein Double. Ein Wert von 0.0 bedeutet, dass sich alle CPUs während des aktuell beobachteten Zeitraums im Leerlauf befanden. Ein Wert von 1.0 bedeutet, dass alle CPUs während 100 Prozent der Zeit des aktuell beobachteten Zeitraums aktiv ausgeführt wurden.|
>| App CPU Usage Percentage (Prozentualer Anteil der App-CPU-Auslastung) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | Prozentualer Anteil der App-CPU-Auslastung | Percent | Aktuelle CPU-Auslastung für den Java Virtual Machine-Prozess. Dieser Wert ist im [0.0,1.0]-Intervall ein Double. Ein Wert von 0.0 bedeutet, dass keine der CPUs während des aktuell beobachteten Zeitraums Threads des JVM-Prozesses ausgeführt hat. Ein Wert von 1.0 bedeutet, dass alle CPUs während 100 Prozent der Zeit des aktuell beobachteten Zeitraums aktiv JVM-Threads ausgeführt haben. Zu den JVM-Threads gehören Anwendungsthreads sowie interne JVM-Threads.|
>| process.cpu.usage | Prozentualer Anteil der App-CPU-Auslastung | Percent | Aktuelle CPU-Auslastung für den Java Virtual Machine-Prozess. Dieser Wert ist im [0.0,1.0]-Intervall ein Double. Ein Wert von 0.0 bedeutet, dass keine der CPUs während des aktuell beobachteten Zeitraums Threads des JVM-Prozesses ausgeführt hat. Ein Wert von 1.0 bedeutet, dass alle CPUs während 100 Prozent der Zeit des aktuell beobachteten Zeitraums aktiv JVM-Threads ausgeführt haben. Zu den JVM-Threads gehören Anwendungsthreads sowie interne JVM-Threads.|
>| App Memory Assigned (Zugewiesener App-Arbeitsspeicher) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.memory.committed | Byte | Die Menge des Arbeitsspeichers, die garantiert für die Verwendung durch die JVM zur Verfügung steht. Die JVM gibt möglicherweise Arbeitsspeicher für das System frei, und zugesicherter Arbeitsspeicher kann ggf. niedriger als anfänglicher Arbeitsspeicher sein. Zugesicherter Arbeitsspeicher ist immer größer oder gleich verwendetem Arbeitsspeicher. |
>| jvm.memory.committed | jvm.memory.committed | Byte | Die Menge des Arbeitsspeichers, die garantiert für die Verwendung durch die JVM zur Verfügung steht. Die JVM gibt möglicherweise Arbeitsspeicher für das System frei, und „Zugesichert“ könnte niedriger als „Init“ sein. „Zugesichert“ ist immer größer oder gleich „Verwendet“. |
>| App Memory Used (Verwendeter App-Arbeitsspeicher) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.memory.used | Byte | Die derzeit verwendete Arbeitsspeichermenge in Byte. |
>| jvm.memory.used | jvm.memory.used | Byte | Die derzeit verwendete Arbeitsspeichermenge in Byte. |
>| App Memory Max (Maximaler App-Arbeitsspeicher) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.memory.max | Byte | Die maximale Speichermenge, die für die Arbeitsspeicherverwaltung verwendet werden kann. Die Menge des verwendeten und zugesicherten Arbeitsspeichers ist immer kleiner oder gleich dem maximalen Wert, wenn er definiert ist. Eine Speicherzuordnung kann jedoch fehlschlagen, wenn sie versucht, den verwendeten Arbeitsspeicher so zu vergrößern, dass „Verwendet“ > „Zugesichert“ ist, selbst wenn „Verwendet <= „Max“ weiterhin zutrifft, z. B. wenn dem System der virtuelle Arbeitsspeicher ausgeht. |
>| jvm.memory.max | jvm.memory.max | Byte | Die maximale Speichermenge, die für die Arbeitsspeicherverwaltung verwendet werden kann. Die Menge des verwendeten und zugesicherten Arbeitsspeichers ist immer kleiner oder gleich dem maximalen Wert, wenn er definiert ist. Eine Speicherzuordnung kann jedoch fehlschlagen, wenn sie versucht, den verwendeten Arbeitsspeicher so zu vergrößern, dass „Verwendet“ > „Zugesichert“ ist, selbst wenn „Verwendet <= „Max“ weiterhin zutrifft, z. B. wenn dem System der virtuelle Arbeitsspeicher ausgeht. |
>| Max Available Old Generation Data Size (Maximal verfügbare Datengröße für den Speicherpool der alten Generation) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.gc.max.data.size | Byte | Die maximale Arbeitsspeicherauslastung des Tenured-Speicherpools ab dem Zeitpunkt, an dem die Java-VM gestartet wurde. |
>| jvm.gc.max.data.size | jvm.gc.max.data.size | Byte | Die maximale Arbeitsspeicherauslastung des Tenured-Speicherpools ab dem Zeitpunkt, an dem die Java-VM gestartet wurde. |
>| Old Generation Data Size (Datengröße für den Speicherpool der alten Generation) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.gc.live.data.size | Byte | Größe des Tenured-Arbeitsspeicherpools nach einer vollständigen Garbage Collection. |
>| jvm.gc.live.data.size | jvm.gc.live.data.size | Byte | Größe des Tenured-Arbeitsspeicherpools nach einer vollständigen Garbage Collection. |
>| Old Generation Data Size (Datengröße für den Speicherpool der alten Generation) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.gc.memory.promoted | Byte | Anzahl der positiven Erhöhungen der Größe des Tenured-Speicherpools vor der Garbage Collection gegenüber nach der Garbage Collection. |
>| jvm.gc.memory.promoted | jvm.gc.memory.promoted | Byte | Anzahl der positiven Erhöhungen der Größe des Tenured-Speicherpools vor der Garbage Collection gegenüber nach der Garbage Collection. |
>| Promote to Young Generation Data Size (Hochstufung auf die Datengröße des Speicherpools der neuen Generation) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.gc.memory.allocated | Byte | Inkrementiert zur Erhöhung der Größe des Eden-Speicherpools nach einer Garbage Collection gegenüber dem Zeitpunkt vor der nächsten. |
>| jvm.gc.memory.allocated | jvm.gc.memory.allocated | Byte | Inkrementiert zur Erhöhung der Größe des Eden-Speicherpools nach einer Garbage Collection gegenüber dem Zeitpunkt vor der nächsten. |
>| GC Pause Count (Anzahl der GC-Pausen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.gc.pause (total-count) | Anzahl | Anzahl aller Garbage Collections nach dem Start dieser Java-VM, einschließlich Eden- und Tenured-Garbage Collections. |
>| jvm.gc.pause.total.count | jvm.gc.pause (total-count) | Anzahl | Anzahl aller Garbage Collections nach dem Start dieser Java-VM, einschließlich Eden- und Tenured-Garbage Collections. |
>| GC Pause Total Time (Gesamtzeit der GC-Pausen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | jvm.gc.pause (total-time) | Millisekunden | Gesamte für Garbage Collections verwendete Zeit nach dem Start dieser Java-VM, einschließlich Eden- und Tenured-Garbage Collections. |
>| jvm.gc.pause.total.time | jvm.gc.pause (total-time) | Millisekunden | Gesamte für Garbage Collections verwendete Zeit nach dem Start dieser Java-VM, einschließlich Eden- und Tenured-Garbage Collections. |

### <a name="request"></a>Anforderung
>[!div class="mx-tdCol2BreakAll"]
>| Name | Name der Metrik für den Azure Spring Cloud-Aktuator | Einheit | Details |
>|----|----|----|------------|
>| Tomcat Total Sent Bytes (Gesamtmenge der von Tomcat gesendeten Bytes) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.global.sent | Byte | Menge der Daten, die vom Tomcat-Webserver gesendet wurden |
>| tomcat.global.sent | tomcat.global.sent | Byte | Menge der Daten, die vom Tomcat-Webserver gesendet wurden |
>| Tomcat Total Received Bytes (Gesamtmenge der von Tomcat empfangenen Bytes) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.global.received | Byte | Menge der Daten, die vom Tomcat-Webserver empfangen wurden |
>| tomcat.global.received | tomcat.global.received | Byte | Menge der Daten, die vom Tomcat-Webserver empfangen wurden |
>| Tomcat Request Total Time (Gesamtzeit der Tomcat-Anforderungen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.global.request (total-time) | Millisekunden | Insgesamt vom Tomcat-Webserver in Anspruch genommene Zeit für die Verarbeitung von Anforderungen |
>| Tomcat Request Total Count (Gesamtanzahl der Tomcat-Anforderungen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.global.request (total-count) | Anzahl | Gesamtzahl aller vom Tomcat-Webserver verarbeiteten Anforderungen. |
>| tomcat.global.request.total.count | tomcat.global.request (total-count) | Anzahl | Gesamtzahl aller vom Tomcat-Webserver verarbeiteten Anforderungen. |
>| Tomcat Request Max Time (Maximale Zeit für Tomcat-Anforderungen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.global.request.max | Millisekunden | Maximale vom Tomcat-Webserver in Anspruch genommene Zeit für die Verarbeitung einer Anforderung. |
>| tomcat.global.request.max | tomcat.global.request.max | Millisekunden | Maximale vom Tomcat-Webserver in Anspruch genommene Zeit für die Verarbeitung einer Anforderung. |

### <a name="session"></a>Sitzung
>[!div class="mx-tdCol2BreakAll"]
>| Name | Name der Metrik für den Azure Spring Cloud-Aktuator | Einheit | Details |
>|----|----|----|------------|
>| Tomcat Session Max Active Count (Maximale Anzahl aktiver Tomcat-Sitzungen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.sessions.active.max | Anzahl | Die maximale Anzahl von Sitzungen, die zur selben Zeit aktiv waren. |
>| tomcat.sessions.active.max | tomcat.sessions.active.max | Anzahl | Die maximale Anzahl von Sitzungen, die zur selben Zeit aktiv waren. |
>| Tomcat Session Max Alive Time (Maximale aktive Zeit für Tomcat-Sitzungen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.sessions.alive.max | Millisekunden | Der längste Zeitraum (in Sekunden), in dem eine abgelaufene Sitzung aktiv war. |
>| tomcat.sessions.alive.max | tomcat.sessions.alive.max | Millisekunden | Der längste Zeitraum (in Sekunden), in dem eine abgelaufene Sitzung aktiv war. |
>| Tomcat Session Created Count (Anzahl erstellter Tomcat-Sitzungen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.sessions.created | Anzahl | Gesamtzahl aller erstellten Sitzungen. |
>| tomcat.sessions.created | tomcat.sessions.created | Anzahl | Gesamtzahl aller erstellten Sitzungen. |
>| Tomcat Session Expired Count (Anzahl abgelaufener Tomcat-Sitzungen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.sessions.expired | Anzahl | Anzahl der abgelaufenen Sitzungen. |
>| tomcat.sessions.expired | tomcat.sessions.expired | Anzahl | Anzahl der abgelaufenen Sitzungen. |
>| Tomcat Session Rejected Count (Anzahl abgewiesener Tomcat-Sitzungen) (veraltet)<br><br>(wird nach dem 1. Juli 2020 entfernt) | tomcat.sessions.rejected | Anzahl | Anzahl der Sitzungen, die nicht erstellt wurden, da die maximal mögliche Anzahl aktiver Sitzungen bereits erreicht war. |
>| tomcat.sessions.rejected | tomcat.sessions.rejected | Anzahl | Anzahl der Sitzungen, die nicht erstellt wurden, da die maximal mögliche Anzahl aktiver Sitzungen bereits erreicht war. |
>| tomcat.sessions.active.current | tomcat.sessions.active.current | Anzahl | Tomcat Session Active Count (Maximale Anzahl aktiver Tomcat-Sitzungen) |

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte mit dem Azure-Metrik-Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started)

* [Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)

## <a name="next-steps"></a>Nächste Schritte
* [Tutorial: Überwachen von Spring Cloud-Ressourcen mithilfe von Warnungen und Aktionsgruppen](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)

* [Kontingente und Servicepläne für Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quotas)


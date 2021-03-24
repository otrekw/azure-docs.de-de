---
title: Richtlinien für zuverlässige Sammlungen
description: Richtlinien und Empfehlungen für die Verwendung von zuverlässigen Sammlungen (Reliable Collections) in Service Fabric in einer Azure Service Fabric-Anwendung.
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: f12db76f324d07c178b49150d4e574476e7d9929
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98784324"
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Richtlinien und Empfehlungen für Reliable Collections in Azure Service Fabric
Dieser Abschnitt enthält Richtlinien für die Verwendung von Reliable State Manager und Reliable Collections. Er soll Benutzern helfen, häufige Fehlerquellen zu vermeiden.

Die *Richtlinien*,*werden* als *einfache* Empfehlungen *präsentiert*.

* Ändern Sie kein benutzerdefiniertes Objekt, das von Lesevorgängen (z.B. `TryPeekAsync` oder `TryGetValueAsync`) zurückgegeben wurde. Zuverlässige Auflistungen geben ebenso wie gleichzeitige Auflistungen anstelle einer Kopie einen Verweis auf die Objekte zurück.
* Tiefenkopieren Sie zurückgegebene benutzerdefinierte Objekte, bevor Sie diese ändern. Da bei Strukturen und integrierten Typen eine Wertübergabe erfolgt, ist für sie nur dann eine Tiefenkopie erforderlich, wenn sie Verweistypfelder oder Eigenschaften enthalten, die Sie ändern möchten.
* Verwenden Sie `TimeSpan.MaxValue` nicht für Timeouts. Timeouts sollten verwendet werden, um Deadlocks zu erkennen.
* Verwenden Sie keine Transaktion, nachdem für sie ein Commit ausgeführt bzw. sie verworfen oder abgebrochen wurde.
* Verwenden Sie eine Enumeration nicht außerhalb des Transaktionsbereichs, in dem sie erstellt wurde.
* Erstellen Sie keine Transaktion innerhalb der `using`-Anweisung einer anderen Transaktion, da dies zu Deadlocks führen kann.
* Erstellen Sie keinen zuverlässigen Zustand mit `IReliableStateManager.GetOrAddAsync`, und verwenden Sie den zuverlässigen Zustand in derselben Transaktion. Dies führt zu einer „InvalidOperationException“.
* Stellen Sie sicher, dass Ihre `IComparable<TKey>` -Implementierung richtig ist. `IComparable<TKey>` ist erforderlich, damit das System Prüfpunkte und Zeilen zusammenfügen kann.
* Verwenden Sie Aktualisierungssperren beim Lesen eines Elements, das aktualisiert werden soll, um eine bestimmte Klasse von Deadlocks zu vermeiden.
* Erwägen Sie, die Anzahl der zuverlässigen Sammlungen pro Partition auf weniger als 1000 zu begrenzen. Bevorzugen Sie zuverlässige Sammlungen mit mehr Elementen vor zuverlässigen Sammlungen mit weniger Elementen.
* Sie sollten auch erwägen, Ihre Elemente (z.B. TKey + TValue für das Reliable Dictionary) unter 80 KB zu halten: je kleiner, desto besser. Damit verringern Sie die Nutzung großer Objektheaps sowie die Anforderungen an Datenträger und Netzwerk-E/A. Häufig wird damit auch die Replikation doppelter Daten reduziert, wenn nur ein kleiner Teil des Werts aktualisiert wird. Um dies im zuverlässigen Wörterbuch zu erreichen, werden häufig einzelne Zeilen in mehrere Zeilen aufgeteilt.
* Sie sollten zwecks Notfallwiederherstellung die Verwendung der Funktionen „Backup“ und „Wiederherstellung“ in Betracht ziehen.
* Verwenden Sie Vorgänge mit einer einzigen Entität und Vorgänge mit mehreren Entitäten (z.B. `GetCountAsync` und `CreateEnumerableAsync`) aufgrund der unterschiedlichen Isolationsstufen nicht in der gleichen Transaktion.
* Behandeln Sie „InvalidOperationException“. Benutzertransaktionen können aus verschiedenen Gründen vom System abgebrochen werden, z.B. wenn der Reliable State Manager seine primäre Rolle ändert, oder wenn eine Transaktion mit langer Laufzeit das Abschneiden des Transaktionsprotokolls einschränkt. In solchen Fällen erhalten die Benutzer möglicherweise eine InvalidOperationException, die angibt, dass ihre Transaktion bereits beendet wurde. Vorausgesetzt, das Beenden der Transaktion wurde nicht durch den Benutzer angefordert, ist der beste Weg, diese Ausnahme zu behandeln, die Transaktion zu beenden und zu überprüfen, ob das Abbruchtoken signalisiert wurde (oder ob sich die Rolle des Replikats geändert hat). Falls dies nicht der Fall ist, erstellen Sie eine neue Transaktion und versuchen Sie es erneut.  

Hier folgen einige Punkte, die es zu beachten gilt:

* Das Standardtimeout beträgt 4 Sekunden für alle Reliable Collections-APIs. Die meisten Benutzer sollten das Standardtimeout verwenden.
* Das Standardabbruchtoken ist `CancellationToken.None` in allen APIs für zuverlässige Auflistungen.
* Der Schlüsseltyp-Parameter (*TKey*) für Reliable Dictionary muss `GetHashCode()` und `Equals()` ordnungsgemäß implementieren. Schlüssel müssen unveränderlich sein.
* Zum Erreichen der Hochverfügbarkeit der zuverlässigen Auflistungen sollte jeder Dienst mindestens ein Ziel und eine Mindestgröße von 3 bei der Replikatgruppe haben.
* Lesevorgänge auf dem sekundären Replikat dürfen Versionen lesen, die nicht im Quorum committet wurden.
  Dies bedeutet, dass Datenversionen, die von einem einzelnen sekundären Replikat gelesen werden, falsch weiterverarbeitet werden können.
  Da Lesevorgänge von primären Replikaten immer stabil sind, können hier nie fehlerhafte Versionen auftreten.
* Die Vorgehensweise in Bezug auf die Sicherheit bzw. den Datenschutz der Daten, die von Ihrer Anwendung in einer zuverlässigen Sammlung aufbewahrt werden, ist Ihre Entscheidung und unterliegt den Schutzmechanismen Ihrer Speicherverwaltung. Beispielsweise kann die Verschlüsselung von Betriebssystem-Datenträgern genutzt werden, um Ihre ruhenden Daten zu schützen.
* Die `ReliableDictionary`-Enumeration verwendet eine sortierte Datenstruktur, deren Reihenfolge von einem Schlüssel bestimmt wird. Damit die Enumeration effizient ist, werden Commits einer temporären Hashtabelle hinzugefügt und später nach dem Prüfpunkt in die sortierte Datenhauptstruktur verschoben. Hinzufügungen/Aktualisierungen/Löschungen haben bei Überprüfungen im Idealfall eine Laufzeit von O(1) und im schlechtesten Fall eine Laufzeit von O(log n), wenn der Schlüssel vorliegt. Abrufvorgänge können O(1) oder O(log n) sein, je nachdem, ob Sie aus einem vor Kurzem durchgeführten Commitvorgang oder aus einem älteren Commitvorgang lesen.

## <a name="volatile-reliable-collections"></a>Flüchtige zuverlässige Sammlungen
Beachten Sie Folgendes, wenn Sie sich für die Verwendung von flüchtigen zuverlässigen Sammlungen entscheiden:

* Für ```ReliableDictionary``` gilt eine flüchtige Unterstützung.
* Für ```ReliableQueue``` gilt eine flüchtige Unterstützung.
* Für ```ReliableConcurrentQueue``` gilt KEINE flüchtige Unterstützung.
* Persistente Dienste können NICHT in flüchtige Dienste geändert werden. Beim Ändern des Flags ```HasPersistedState``` in ```false``` muss der gesamte Dienst von Grund auf neu erstellt werden.
* Flüchtige Dienste können NICHT in persistente Dienste geändert werden. Beim Ändern des Flags ```HasPersistedState``` in ```true``` muss der gesamte Dienst von Grund auf neu erstellt werden.
* ```HasPersistedState``` ist eine Dienstebenenkonfiguration. Dies bedeutet, dass **ALLE** Sammlungen entweder persistent oder flüchtig sind. Flüchtige und persistente Sammlungen können nicht gemischt werden.
* Der Quorumverlust einer flüchtigen Partition führt zu einem vollständigen Datenverlust.
* Die Sicherung und Wiederherstellung ist für flüchtige Dienste NICHT verfügbar.

## <a name="next-steps"></a>Nächste Schritte
* [Arbeiten mit Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transaktionen und Sperren](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Verwalten von Daten
  * [Sichern und Wiederherstellen](service-fabric-reliable-services-backup-restore.md)
  * [Benachrichtigungen](service-fabric-reliable-services-notifications.md)
  * [Serialisierung und Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Konfigurieren des Reliable State Managers](service-fabric-reliable-services-configuration.md)
* Andere
  * [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start.md)
  * [Entwicklerreferenz für zuverlässige Auflistungen](/dotnet/api/microsoft.servicefabric.data.collections#microsoft_servicefabric_data_collections)

---
title: Lesen und Aktualisieren einer lokalen Sicherung zuverlässiger Sammlungen
description: Mit dem Sicherungs-Explorer von Azure Service Fabric können Sie eine lokale Sicherung zuverlässiger Sammlungen lesen und aktualisieren.
author: athinanthny
ms.topic: conceptual
ms.date: 07/01/2020
ms.author: atsenthi
ms.openlocfilehash: 1da70c37c8a6ed93e7abe1b5d329e808c592e43a
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86034750"
---
# <a name="read-and-update-a-reliable-collections-backup-by-using-backup-explorer"></a>Lesen und Aktualisieren einer Sicherung zuverlässiger Sammlungen mit dem Sicherungs-Explorer

Der Sicherungs-Explorer von Azure Service Fabric unterstützt Sie bei der Datenkorrektur, wenn Daten in zuverlässigen Sammlungen von Service Fabric beschädigt sind. Der aktuelle Datenzustand kann durch einen beliebigen in eine Anwendung eingeführten Fehler oder durch falsche Eingaben in ein Livecluster beeinträchtigt werden.

Mithilfe des Sicherungs-Explorers können Sie die folgenden Aufgaben ausführen:
-   Abfragen der Metadaten für die Sammlung
-   Anzeigen des aktuellen Zustands und seiner Einträge in der Sammlung der Sicherung, die geladen wird
-   Auflisten der seit dem letzten Prüfpunkt erfolgten Transaktionen
-   Aktualisieren der Sammlung durch Hinzufügen, Aktualisieren oder Löschen von Einträgen in der Sammlung
-   Erstellen einer neuen Sicherung unter Verwendung des aktualisierten Zustands

> [!NOTE]
> Der Sicherungs-Explorer von Service Fabric unterstützt derzeit nur das Anzeigen und Bearbeiten zuverlässiger Sammlungen in der Sicherung.
>

## <a name="access-the-backup"></a>Zugreifen auf die Sicherung

Der Sicherungs-Explorer von Service Fabric kann auf eine der folgenden Weisen genutzt werden, um zuverlässige Sammlungen in der Sicherung anzuzeigen oder zu aktualisieren:
-   **Binär**: Mit einem NuGet-Paket können Sie zuverlässige Sammlungen anzeigen und ändern.
-   **HTTP/REST**: Mit einem HTTP-basierten REST-Server können Sie zuverlässige Sammlungen anzeigen und ändern.
-   **bkpctl**: Mithilfe der Befehlszeilenschnittstelle des Sicherungs-Explorers von Service Fabric können Sie eine Sicherung zuverlässige Sammlungen anzeigen und ändern.

Der Sicherungs-Explorer bietet für fortgeschrittene Benutzer eine C#-Bibliothek. Sie können die Bibliothek direkt in der Anwendung verwenden, um mit zuverlässigen Sammlungen zu arbeiten, ähnlich wie Kunden mit dem Zustands-Manager in ihren vorhandenen zustandsbehafteten Diensten arbeiten. Für normale Benutzer und in einem einfachen Anwendungsfall bietet der Explorer auch einen eigenständigen REST-Server, der APIs zur Überprüfung der Sicherungen verfügbar macht. Das CLI-Tool bkpctl setzt auf den REST-APIs auf und basiert auf Python. Sie können mit dem CLI-Tool Sicherungen lesen und aktualisieren und neue Sicherungen über die Befehlszeile erstellen.

Weitere Informationen finden Sie im GitHub-Repository [Service Fabric Backup Explorer](https://github.com/microsoft/service-fabric-backup-explorer). Das Repository enthält Quell- und Versionsinformationen sowie Installationsanweisungen.

Sie können das Repository auch lokal erstellen und an Sicherungen arbeiten.
 
Der Sicherungs-Explorer von NuGet (Microsoft.ServiceFabric.ReliableCollectionBackup.Parser) ist auf [nuget.org](https://www.nuget.org/) verfügbar. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr zu [zuverlässigen Sammlungen in zustandsbehafteten Azure Service Fabric-Diensten](service-fabric-reliable-services-reliable-collections.md).
* Machen Sie sich mit den [bewährten Methoden für Azure Service Fabric](service-fabric-best-practices-overview.md) vertraut.

---
title: Dateien und Verzeichnisse in Azure Batch
description: Erfahren Sie mehr über Dateien und Verzeichnisse und deren Verwendung in einem Azure Batch-Workflow aus Entwicklersicht.
ms.topic: conceptual
ms.date: 08/03/2020
ms.openlocfilehash: eafea6c234c3b261521f8a791b7a03e25388f02a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87552634"
---
# <a name="files-and-directories-in-azure-batch"></a>Dateien und Verzeichnisse in Azure Batch

In Azure Batch umfasst jeder Task ein Arbeitsverzeichnis, in dem Dateien und Verzeichnisse erstellt werden können. Dieses Arbeitsverzeichnis kann für das Speichern des von einem Task ausgeführten Programms, der dabei verarbeiteten Daten und der Ausgabe der Verarbeitung verwendet werden. Alle Dateien und Verzeichnisse eines Tasks befinden sich im Besitz des Taskbenutzers.

Der Batch-Dienst stellt einen Teil des Dateisystems auf einem Knoten als *Stammverzeichnis* bereit. Dieses Stammverzeichnis befindet sich auf dem temporären Speicherlaufwerk der VM, nicht direkt auf dem Laufwerk des Betriebssystems.

Tasks können auf das Stammverzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_ROOT_DIR` verweisen. Weitere Informationen zur Verwendung von Umgebungsvariablen finden Sie unter [Umgebungseinstellungen für Tasks](jobs-and-tasks.md#environment-settings-for-tasks).

## <a name="root-directory-structure"></a>Struktur des Stammverzeichnisses

Das Stammverzeichnis enthält die folgenden Verzeichnisstruktur:

![Screenshot der Computeknoten-Verzeichnisstruktur.](media\files-and-directories\node-folder-structure.png)

- **applications**: Enthält Informationen zu den Details von Anwendungspaketen, die auf dem Computeknoten installiert sind. Tasks können auf dieses Verzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_APP_PACKAGE` verweisen.

- **fsmounts**: Das Verzeichnis enthält alle Dateisysteme, die auf einem Computeknoten bereitgestellt werden. Tasks können auf dieses Verzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_MOUNTS_DIR` verweisen. Weitere Informationen finden Sie unter [Einbinden eines virtuellen Dateisystems in einen Batch-Pool](virtual-file-mount.md).

- **shared**: Dieses Verzeichnis bietet Lese-/Schreibzugriff auf *alle* Tasks, die auf einem Knoten ausgeführt werden. Mit jedem Task, der auf dem Knoten ausgeführt wird, können Dateien in diesem Verzeichnis erstellt, gelesen, aktualisiert und gelöscht werden. Tasks können auf dieses Verzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_SHARED_DIR` verweisen.

- **startup**: Dieses Verzeichnis wird von einem Starttask als Arbeitsverzeichnis verwendet. Alle Dateien, die vom Starttask auf den Knoten heruntergeladen werden, werden hier gespeichert. Der Starttask kann Dateien unter diesem Verzeichnis erstellen, lesen, aktualisieren und löschen. Tasks können auf dieses Verzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_STARTUP_DIR` verweisen.

- **volatile**: Dieses Verzeichnis dient internen Zwecken. Es gibt keine Garantie, dass Dateien in diesem Verzeichnis oder das Verzeichnis selbst in Zukunft vorhanden sein werden.

- **workitems**: Dieses Verzeichnis enthält die Verzeichnisse für Aufträge und deren Tasks auf dem Computeknoten.

    Im Verzeichnis **workitems** wird für jeden Task, der auf dem Knoten ausgeführt wird, ein Verzeichnis **Tasks** erstellt. Auf dieses Verzeichnis kann zugegriffen werden, indem sie auf die Umgebungsvariable `AZ_BATCH_TASK_DIR` verweisen.

    In jedem Verzeichnis **Tasks** erstellt der Batch-Dienst ein Arbeitsverzeichnis (`wd`), dessen eindeutiger Pfad durch die `AZ_BATCH_TASK_WORKING_DIR`-Umgebungsvariable angegeben wird. Dieses Verzeichnis bietet Lese-/Schreibzugriff auf den Task. Die Aufgabe kann Dateien unter diesem Verzeichnis erstellen, lesen, aktualisieren und löschen. Dieses Verzeichnis wird auf der Grundlage der für den Vorgang angegebenen Einschränkung *RetentionTime* beibehalten.

    Die Dateien `stdout.txt` und `stderr.txt` werden während der Ausführung des Tasks in den Ordner **Tasks** geschrieben.

> [!IMPORTANT]
> Wenn ein Knoten aus einem Pool entfernt wird, werden alle auf dem Knoten gespeicherten Dateien entfernt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Fehlerbehandlung und -erkennung](error-handling.md) in Azure Batch.
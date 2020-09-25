---
title: Sichern und Wiederherstellen von Azure Database for PostgreSQL Hyperscale-Servergruppen
description: Sichern und Wiederherstellen von Azure Database for PostgreSQL Hyperscale-Servergruppen
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: d300f3e02d2a1a83410d5b7d981298a4743fb223
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931549"
---
# <a name="backup-and-restore-for-azure-arc-enabled-postgresql-hyperscale-server-groups"></a>Sichern und Wiederherstellen von Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen

Sie können eine vollständige Sicherung/Wiederherstellung Ihrer Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe durchführen. In diesem Fall werden alle Datenbanken auf allen Knoten Ihrer Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe gesichert und/oder wiederhergestellt.
Sie müssen sicherstellen, dass eine Sicherungsspeicherklasse für Ihre Servergruppe konfiguriert ist, um eine Sicherung zu erstellen und diese wiederherzustellen. Zunächst müssen Sie eine Sicherungsspeicherklasse für den Zeitpunkt angeben, zu dem Sie die Servergruppe erstellen. Es ist noch nicht möglich, Ihre Servergruppe zur Verwendung einer Sicherungsspeicherklasse zu konfigurieren, nachdem sie erstellt wurde.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

> [!CAUTION]
> Die Vorschau unterstützt keine Sicherung/Wiederherstellung für Version 11 der Postgres-Engine. Es wird nur die Sicherung/Wiederherstellung für die Postgres-Version 12 unterstützt.

## <a name="verify-configuration"></a>Überprüfen der Konfiguration

Überprüfen Sie zunächst, ob Ihre vorhandene Servergruppe zur Verwendung von Sicherungsspeicherklassen konfiguriert wurde.

Führen Sie den folgenden Befehl aus, nachdem Sie den Namen Ihrer Servergruppe festgelegt haben:
```console
 azdata arc postgres server show -n postgres01
```
Sehen Sie sich den Speicherabschnitt der Ausgabe an:
```console
...
"storage": {
      "backups": {
        "className": "local-storage"
      },
      "data": {
        "className": "local-storage",
        "size": "5Gi"
      },
      "logs": {
        "className": "local-storage",
        "size": "5Gi"
      }
    }
...
```
Wenn Ihren der Abschnitt „Sicherungen“ angezeigt wird, bedeutet dies, dass Ihre Servergruppe zur Verwendung einer Sicherungsspeicherklasse konfiguriert wurde und bereit dafür ist, dass Sie Sicherungen erstellen und Wiederherstellungen durchführen. Wenn Ihnen der Abschnitt „Sicherungen“ nicht angezeigt wird, müssen Sie Ihre Servergruppe löschen und neu erstellen, um die Sicherungsspeicherklasse zu konfigurieren. Es ist derzeit nicht möglich, eine Sicherungsspeicherklasse zu konfigurieren, nachdem die Servergruppe erstellt wurde.

>[!IMPORTANT]
>Wenn Ihre Servergruppe bereits zur Verwendung einer Sicherungsspeicherklasse konfiguriert ist, überspringen Sie den nächsten Schritt, und fahren Sie direkt mit dem Schritt „Manuelles Erstellen einer vollständigen Sicherung“ fort.

## <a name="create-a-server-group"></a>Servergruppe erstellen 

Als Nächstes erstellen Sie eine Servergruppe, die für Sicherungen/Wiederherstellungen konfiguriert ist.

Damit Sie Sicherungen erstellen und mit diesen Wiederherstellungen durchführen können, müssen Sie einen Server erstellen, der mit einer Speicherklasse konfiguriert ist.

Führen Sie den folgenden Befehl aus, um eine Liste der Speicherklassen abzurufen, die für Ihren Kubernetes-Cluster verfügbar sind:

```console
kubectl get sc
```

<!--The general format of create server group command is documented [here](create-postgresql-instances.md)-->

```console
azdata arc postgres server create -n <name> --workers 2 --storage-class-backups <storage class name> [--storage-class-data <storage class name>] [--storage-class-logs <storage class name>]
```

Wenn Sie beispielsweise eine einfache auf kubeadm basierende Umgebung erstellt haben:
```console
azdata arc postgres server create -n postgres01 --workers 2 --storage-class-backups local-storage
```

## <a name="take-manual-full-backup"></a>Manuelles Erstellen einer vollständigen Sicherung

Als nächstes erstellen Sie eine vollständige Sicherung manuell.

Führen Sie den folgenden Befehl aus, um eine vollständige Sicherung der gesamten Daten und Protokollordner Ihrer Servergruppe zu erstellen:

```console
azdata arc postgres backup create [--name <backup name>] --server-name <server group name> [--no-wait] 
```
Hierbei gilt:
- __name__ gibt den Namen einer Sicherung an.
- __server-name__ gibt eine Servergruppe an.
- __no-wait__ gibt an, dass die Befehlszeile nicht auf den Abschluss der Sicherung wartet, sodass Sie mit der Verwendung des Befehlszeilenfensters fortfahren können.

>**Hinweis**: Der Befehl zum Auflisten der zur Wiederherstellung verfügbaren Sicherungen zeigt noch nicht an, zu welchem Datum und welcher Uhrzeit die Sicherung erstellt wurde. Daher empfiehlt es sich, der Sicherung einen Namen zu geben (mithilfe des Parameters „--name“), der Informationen zum Datum und zur Uhrzeit enthält.

Dieser Befehl koordiniert eine verteilte vollständige Sicherung für alle Knoten, aus denen Ihre Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe besteht. Das bedeutet, alle Daten in Ihren Coordinator- und Workerknoten werden gesichert.

Zum Beispiel:
```console
azdata arc postgres backup create --name MyBackup_Aug31_0730amPST --server-name postgres01
```

Wenn die Sicherung fertiggestellt wird, werden die ID, der Name und der Zustand der Sicherung zurückgegeben. Zum Beispiel:
```console
{
  "ID": "d134f51aa87f4044b5fb07cf95cf797f",
  "name": "MyBackup_Aug31_0730amPS",
  "state": "Done"
}
```

> [!NOTE]
> Folgendes ist noch nicht möglich:
> - Planen von automatischen Backups
> - Anzeigen des Fortschritts der Sicherungserstellung

## <a name="list-backups"></a>Auflisten der Sicherungen

Führen Sie die Sicherungen auf, die zur Wiederherstellung verfügbar sind.

Führen Sie den folgenden Befehl aus, um die Sicherungen aufzulisten, die zur Wiederherstellung verfügbar sind:

```console
azdata arc postgres backup list --server-name <servergroup name>
```

Zum Beispiel:
```console
azdata arc postgres backup list --server-name postgres01
```

Dabei wird eine Ausgabe wie die folgende zurückgegeben:
```console
ID                                Name                      State
--------------------------------  ------------------------  -------
d134f51aa87f4044b5fb07cf95cf797f  MyBackup_Aug31_0730amPST  Done
```

## <a name="restore-a-backup"></a>Wiederherstellen einer Sicherung

Führen Sie den folgenden Befehl aus, um die Sicherung einer gesamten Servergruppe wiederherzustellen:

```console
azdata arc postgres backup restore --server-name <server group name> --backup-id <backup id>
```

Hierbei gilt:
- __backup-id__ entspricht der ID der Sicherung, die mit dem Befehl zum Auflisten der Sicherungen angezeigt wird (siehe Schritt 3).
Hiermit wird eine verteilte vollständige Wiederherstellung für alle Knoten koordiniert, aus denen Ihre Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe besteht. Das bedeutet, alle Daten in Ihren Coordinator- und Workerknoten werden wiederhergestellt.

Zum Beispiel:
```console
azdata arc postgres backup restore --server-name postgres01 --backup-id d134f51aa87f4044b5fb07cf95cf797f
```

Wenn der Wiederherstellungsvorgang abgeschlossen ist, wird eine Ausgabe ähnlich der folgenden in der Befehlszeile zurückgegeben:
```console
{
  "ID": "d134f51aa87f4044b5fb07cf95cf797f",
  "state": "Done"
}
```
> [!NOTE]
> Folgendes ist noch nicht möglich:
> - Wiederherstellen einer Sicherung durch Angabe ihres Namens
> - Wiederherstellen einer Servergruppe unter einem anderen Namen oder einer anderen Servergruppe
> - Anzeigen des Fortschritts eines Wiederherstellungsvorgangs

## <a name="delete-backups"></a>Löschen von Sicherungen
Die Aufbewahrungsdauer für Sicherungen kann in der Vorschau nicht festgelegt werden. Sie können jedoch nicht benötigte Sicherungen manuell löschen.
Der allgemeine Befehl zum Löschen von Sicherungen lautet:
```console
azdata arc postgres backup delete  [--server-name, -sn] {[--name, -n], -id}
```
Dabei gilt Folgendes:
- `--server-name` ist der Name der Servergruppe, aus der der Benutzer eine Sicherung löschen möchte.
- `--name` ist der Name der Sicherung, die gelöscht werden soll.
- `-id` ist die ID der Sicherung, die gelöscht werden soll.

> [!NOTE]
> `--name` und `-id` schließen sich gegenseitig aus.

Sie können den Namen und die ID der Sicherungen abrufen, indem Sie den Befehl „list backup“ ausführen, wie im vorherigen Absatz erläutert.

Angenommen, die folgenden Sicherungen sind aufgelistet:
```console
azdata arc postgres backup list -sn postgres01
ID                                Name                    State
--------------------------------  ----------------------  -------
5b0481dfc1c94b4cac79dd56a1bb21f4  MyBackup091720200110am  Done
0cf39f1e92344e6db4cfa285d36c7b14  MyBackup091720200111am  Done
```
Wenn Sie die erste Sicherung löschen möchten, würden Sie den folgenden Befehl ausführen:
```console
azdata arc postgres backup delete -sn postgres01 -n MyBackup091720200110am
{
  "ID": "5b0481dfc1c94b4cac79dd56a1bb21f4",
  "name": "MyBackup091720200110am",
  "state": "Done"
}
```
Wenn Sie die Sicherungen zu diesem Zeitpunkt auflisten müssten, würden Sie die folgende Ausgabe erhalten:
```console
azdata arc postgres backup list -sn postgres01
ID                                Name                    State
--------------------------------  ----------------------  -------
0cf39f1e92344e6db4cfa285d36c7b14  MyBackup091720200111am  Done
```

Um weitere Informationen zum Löschbefehl zu erhalten, führen Sie Folgendes aus:
```console
azdata arc postgres backup delete --help
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über das [Aufskalieren](scale-out-postgresql-hyperscale-server-group.md) der Servergruppe (Hinzufügen von Workerknoten).
- Erfahren Sie mehr über das [zentrale Hoch- oder Herunterskalieren](scale-up-down-postgresql-hyperscale-server-group-using-cli.md) der Servergruppe (Vergrößern/Verkleinern von Arbeitsspeicher/virtuellen Kernen).

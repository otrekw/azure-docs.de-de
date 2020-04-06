---
title: Kopieren von Anwendungen und Daten auf Poolknoten
description: Erfahren Sie, wie Sie Anwendungen und Daten auf Poolknoten kopieren.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/17/2020
ms.author: labrenne
ms.openlocfilehash: 226a0d69ac387142ecf580537e35f8754ac848a6
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385355"
---
# <a name="copying-applications-and-data-to-pool-nodes"></a>Kopieren von Anwendungen und Daten auf Poolknoten

Azure Batch unterstützt verschiedene Möglichkeiten, um Daten und Anwendungen auf Computeknoten zu überführen, damit die Daten und Anwendungen für die Verwendung durch Aufgaben verfügbar sind. Möglicherweise sind die Daten und Anwendungen erforderlich, um den gesamten Auftrag auszuführen, sodass sie auf jedem Knoten installiert werden müssen. Einige sind möglicherweise nur für eine bestimmte Aufgabe erforderlich oder müssen für den Auftrag installiert werden, müssen jedoch nicht auf jedem Knoten installiert sein. Batch verfügt über Tools für jedes dieser Szenarien.

- **Zusammenlegen von Starttask-Ressourcendateien**: Für Anwendungen oder Daten, die auf jedem Knoten im Pool installiert werden müssen. Verwenden Sie diese Methode zusammen mit einem Anwendungspaket oder der Ressourcendateisammlung des Starttasks, um einen Installationsbefehl auszuführen.  

Beispiele: 
- Verwenden Sie die Starttask-Befehlszeile, um Anwendungen zu verschieben oder zu installieren.

- Geben Sie eine Liste bestimmter Dateien oder Container in einem Azure Storage-Konto an. Weitere Informationen finden Sie unter [add#resourcefile in der REST-Dokumentation](https://docs.microsoft.com/rest/api/batchservice/pool/add#resourcefile).

- Jeder Auftrag, der im Pool ausgeführt wird, führt „MyApplication.exe“ aus. Diese ausführbare Datei muss zunächst mit „MyApplication.msi“ installiert werden. Wenn Sie diesen Mechanismus verwenden, müssen Sie die Eigenschaft **Erfolg abwarten** des Starttasks auf **true** festlegen. Weitere Informationen finden Sie unter [add#starttask in der REST-Dokumentation](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask).

- **Anwendungspaketverweise** im Pool: Für Anwendungen oder Daten, die auf jedem Knoten im Pool installiert werden müssen. Mit einem Anwendungspaket ist kein Installationsbefehl verknüpft, Sie können jedoch einen Starttask verwenden, um einen beliebigen Installationsbefehl auszuführen. Wenn Ihre Anwendung keine Installation erfordert oder eine große Anzahl von Dateien umfasst, können Sie diese Methode verwenden. Anwendungspakete eignen sich gut für eine große Anzahl von Dateien, da sie eine große Anzahl von Dateiverweisen in einer geringen Nutzlast kombinieren. Wenn Sie versuchen, mehr als 100 separate Ressourcendateien in einen Task aufzunehmen, können im Batch-Dienst für einen einzelnen Task interne Systemeinschränkungen auftreten. Verwenden Sie außerdem Anwendungspakete, wenn strenge Versionsverwaltungsanforderungen gelten, sodass Sie möglicherweise viele verschiedene Versionen derselben Anwendung verwenden und zwischen ihnen wählen müssen. Weitere Informationen finden Sie unter [Bereitstellen von Anwendungen auf Computeknoten mit Batch-Anwendungspaketen](https://docs.microsoft.com/azure/batch/batch-application-packages).

- **Ressourcendateien für den Auftragsvorbereitungstask**: Für Anwendungen oder Daten, die installiert werden müssen, damit der Auftrag ausgeführt werden kann, jedoch nicht im gesamten Pool installiert werden müssen. Beispiel: Wenn Ihr Pool viele verschiedene Typen von Aufträgen aufweist und nur für einen Auftragstyp „MyApplication.msi“ ausgeführt werden muss, ist es sinnvoll, den Installationsschritt in eine Auftragsvorbereitungstask einzufügen. Weitere Informationen zu Auftragsvorbereitungstasks finden Sie unter [Ausführen von Tasks zum Vorbereiten und Freigeben von Aufträgen auf Azure Batch-Computeknoten](https://azure.microsoft.com/documentation/articles/batch-job-prep-release/).

- **Taskressourcendateien**: Für den Fall, dass eine Anwendung oder Daten nur für einen einzelnen Task relevant sind. Beispiel: Sie haben fünf Tasks, von denen jeder eine andere Datei verarbeitet und dann die Ausgabe in den Blobspeicher schreibt.  In diesem Fall sollte die Eingabedatei in der Sammlung der **Taskressourcendateien** angegeben werden, da jeder Task über eine eigene Eingabedatei verfügt.

## <a name="determine-the-scope-required-of-a-file"></a>Bestimmen des für eine Datei erforderlichen Bereichs

Sie müssen den Bereich einer Datei bestimmen, d. h. ob die Datei für einen Pool, einen Auftrag oder einen Task erforderlich ist. Dateien, deren Bereich auf den Pool festgelegt ist, sollten Poolanwendungspakete oder einen Starttask verwenden. Dateien, deren Bereich auf den Auftrag festgelegt ist, sollten einen Auftragsvorbereitungstask verwenden. Anwendungen sind ein gutes Beispiel für Dateien, deren Bereich auf Pool- oder Auftragsebene festgelegt ist. Dateien, deren Bereich auf den Task festgelegt ist, sollten Taskressourcendateien verwenden.

### <a name="other-ways-to-get-data-onto-batch-compute-nodes"></a>Weitere Möglichkeiten, Daten auf Batch-Computeknoten zu überführen

Es gibt weitere Möglichkeiten, Daten auf Batch-Computeknoten zu überführen, die nicht offiziell in die Batch-REST-API integriert sind. Da Sie die Kontrolle über Azure Batch-Knoten haben und benutzerdefinierte ausführbare Dateien ausführen können, können Sie Daten aus einer beliebigen Anzahl von benutzerdefinierten Quellen abrufen. Voraussetzung ist lediglich, dass der Batch-Knoten mit dem Ziel verbunden ist und Sie über die Anmeldeinformationen für diese Quelle auf dem Azure Batch-Knoten verfügen. Hier einige typische Beispiele:

- Herunterladen von Daten aus SQL
- Herunterladen von Daten aus anderen Webdiensten/benutzerdefinierten Speicherorten
- Zuordnen einer Netzwerkfreigabe

### <a name="azure-storage"></a>Azure-Speicher

Der Blobspeicher verfügt über Downloadskalierbarkeitsziele. Die Skalierbarkeitsziele der Azure Storage-Dateifreigabe sind identisch mit denen für ein einzelnes Blob. Die Größe wirkt sich auf die Anzahl der erforderlichen Knoten und Pools aus.


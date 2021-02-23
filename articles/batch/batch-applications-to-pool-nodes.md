---
title: Kopieren von Anwendungen und Daten auf Poolknoten
description: Erfahren Sie, wie Sie Anwendungen und Daten auf Poolknoten kopieren.
ms.topic: how-to
ms.date: 02/10/2021
ms.openlocfilehash: a5933a1c52e2848b6b414f1750bb24515fb9f28a
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100378501"
---
# <a name="copy-applications-and-data-to-pool-nodes"></a>Kopieren von Anwendungen und Daten auf Poolknoten

Azure Batch unterstützt verschiedene Möglichkeiten, um Daten und Anwendungen auf Computeknoten zu überführen, damit sie für die Verwendung durch Tasks verfügbar sind.

Welche Methode Sie wählen, kann vom Umfang Ihrer Datei oder Anwendung abhängen. Möglicherweise sind die Daten und Anwendungen erforderlich, um den gesamten Auftrag auszuführen, sodass sie auf jedem Knoten installiert werden müssen. Einige Dateien oder Anwendungen werden möglicherweise nur für einen bestimmten Task benötigt. Andere müssen möglicherweise für den Auftrag installiert werden, müssen aber nicht auf jedem Knoten vorhanden sein. Batch verfügt über Tools für jedes dieser Szenarien.

## <a name="determine-the-scope-required-of-a-file"></a>Bestimmen des für eine Datei erforderlichen Bereichs

Sie müssen den Bereich einer Datei bestimmen, d. h. ob die Datei für einen Pool, einen Auftrag oder einen Task erforderlich ist. Dateien, deren Bereich auf den Pool festgelegt ist, sollten Poolanwendungspakete oder einen Starttask verwenden. Dateien, deren Bereich auf den Auftrag festgelegt ist, sollten einen Auftragsvorbereitungstask verwenden. Anwendungen sind ein gutes Beispiel für Dateien, deren Bereich auf Pool- oder Auftragsebene festgelegt ist. Dateien, deren Bereich auf den Task festgelegt ist, sollten Taskressourcendateien verwenden.

## <a name="pool-start-task-resource-files"></a>Pool-Starttask-Ressourcendateien

Für Anwendungen oder Daten, die auf jedem Knoten im Pool installiert werden müssen, verwenden Sie Starttask-Ressourcendateien des Pools. Verwenden Sie diese Methode zusammen mit einem [Anwendungspaket](batch-application-packages.md) oder der Ressourcendateisammlung des Starttasks, um einen Installationsbefehl auszuführen.  

Sie können beispielsweise die Starttask-Befehlszeile verwenden, um Anwendungen zu verschieben oder zu installieren. Oder Sie können eine Liste von Dateien oder Containern in einem Azure Storage-Konto angeben. Weitere Informationen finden Sie unter [Add#ResourceFile in der REST-Dokumentation](/rest/api/batchservice/pool/add#resourcefile).

Wenn jeder Auftrag, der auf dem Pool ausgeführt wird, eine Anwendung (.exe) ausführt, die zuerst mit einer MSI-Datei installiert werden muss, müssen Sie die Eigenschaft **Erfolg abwarten** des Starttasks auf **true** festlegen. Weitere Informationen finden Sie unter [Erfolg abwarten in der REST-Dokumentation](/rest/api/batchservice/pool/add#starttask).

## <a name="application-package-references"></a>Anwendungspaketverweise

Für Anwendungen oder Daten, die auf jedem Knoten im Pool installiert werden müssen, ziehen Sie die Verwendung von [Anwendungspaketen](batch-application-packages.md) in Betracht. Mit einem Anwendungspaket ist kein Installationsbefehl verknüpft, Sie können jedoch einen Starttask verwenden, um einen beliebigen Installationsbefehl auszuführen. Wenn Ihre Anwendung keine Installation erfordert oder eine große Anzahl von Dateien umfasst, können Sie diese Methode verwenden.

Anwendungspakete eignen sich gut, wenn Sie über eine große Anzahl von Dateien verfügen, da sie viele Dateiverweise in einer geringen Nutzlast kombinieren. Wenn Sie versuchen, mehr als 100 separate Ressourcendateien in einen Task aufzunehmen, können im Batch-Dienst für einen einzelnen Task interne Systemeinschränkungen auftreten. Anwendungspakete sind auch dann nützlich, wenn Sie über viele verschiedene Versionen derselben Anwendung verfügen und zwischen ihnen auswählen müssen.

## <a name="extensions"></a>Erweiterungen

[Erweiterungen](create-pool-extensions.md) sind kleine Anwendungen, die Konfiguration und Setup nach der Bereitstellung auf Azure Batch-Computeknoten erleichtern. Wenn Sie einen Pool erstellen, können Sie eine unterstützte Erweiterung auswählen und auf den Computeknoten installieren, wenn diese bereitgestellt werden. Danach kann die Erweiterung ihre vorgesehene Aufgabe erfüllen.

## <a name="job-preparation-task-resource-files"></a>Ressourcendateien für den Auftragsvorbereitungstask

Für Anwendungen oder Daten, die installiert werden müssen, damit der Auftrag ausgeführt werden kann, jedoch nicht im gesamten Pool installiert werden müssen, ziehen Sie die Verwendung von [Ressourcendateien für den Auftragsvorbereitungstask](./batch-job-prep-release.md) in Betracht.

Beispiel: Wenn Ihr Pool viele verschiedene Typen von Aufträgen aufweist und nur ein Auftragstyp eine MSI-Datei für die Ausführung benötigt, ist es sinnvoll, den Installationsschritt in eine Auftragsvorbereitungstask einzufügen.

## <a name="task-resource-files"></a>Taskressourcendateien

Taskressourcendateien sind geeignet, wenn Ihre Anwendungen oder Daten nur für einen einzelnen Task relevant sind.

Angenommen, Sie verfügen über fünf Tasks, von denen jeder eine andere Datei verarbeitet und dann die Ausgabe in den Blobspeicher schreibt. In diesem Fall sollte die Eingabedatei in den Taskressourcendateien aufgeführt sein, da jeder Task über eine eigene Eingabedatei verfügt.

## <a name="additional-ways-to-get-data-onto-nodes"></a>Weitere Möglichkeiten zum Abrufen von Daten auf Knoten

Da Sie die Kontrolle über Azure Batch-Knoten haben und benutzerdefinierte ausführbare Dateien ausführen können, können Sie Daten aus beliebig vielen benutzerdefinierten Quellen abrufen. Stellen Sie sicher, dass der Azure Batch-Knoten über eine Verbindung mit dem Ziel verfügt und dass Sie über Anmeldeinformationen für diese Quelle auf dem Knoten verfügen.

Es folgen einige Beispiele für die Übertragung von Daten auf Azure Batch-Knoten:

- Herunterladen von Daten aus SQL
- Herunterladen von Daten aus anderen Webdiensten/benutzerdefinierten Speicherorten
- Zuordnen einer Netzwerkfreigabe

## <a name="azure-storage"></a>Azure-Speicher

Bedenken Sie, dass der Blobspeicher über Downloadskalierbarkeitsziele verfügt. Die Skalierbarkeitsziele der Azure Storage-Dateifreigabe sind identisch mit denen für ein einzelnes Blob. Die Größe wirkt sich auf die Anzahl der erforderlichen Knoten und Pools aus.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die Verwendung von [Anwendungspaketen mit Azure Batch](batch-application-packages.md).
- Erfahren Sie mehr über das Arbeiten mit [Knoten und Pools](nodes-and-pools.md).

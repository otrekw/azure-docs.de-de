---
title: Workflow und Ressourcen des Batch-Diensts
description: Hier lernen Sie die Features des Batch-Diensts und seinen allgemeinen Workflow aus der Sicht eines Entwicklers kennen.
ms.topic: conceptual
ms.date: 05/12/2020
ms.openlocfilehash: 76a0b140f3bea4b07a6de632abbcbc3fd26e582d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85965211"
---
# <a name="batch-service-workflow-and-resources"></a>Workflow und Ressourcen des Batch-Diensts

In dieser Übersicht über die Hauptkomponenten des Azure Batch-Diensts werden der allgemeine Workflow, den Batch-Entwickler zum Erstellen paralleler Computelösungen in großem Umfang verwenden können, sowie die wichtigsten verwendeten Dienstressourcen beschrieben.

Sie werden viele der hier beschriebenen Ressourcen und Features nutzen. Dies gilt unabhängig davon, ob Sie eine verteilte Computeanwendung oder einen Computedienst mit direkter Ausgabe von [REST-API](/rest/api/batchservice/)-Aufrufen entwickeln oder eines der [Batch SDKs](batch-apis-tools.md#batch-service-apis) verwenden.

> [!TIP]
> Eine allgemeiner gehaltene Einführung in den Batch-Dienst finden Sie unter [Grundlagen von Azure Batch](batch-technical-overview.md). Siehe auch die aktuellen [Batchdienstupdates](https://azure.microsoft.com/updates/?product=batch).

## <a name="basic-workflow"></a>Grundlegender Workflow

Der folgende allgemeine Workflow ist typisch für fast alle Anwendungen und Dienste, die den Batch-Dienst für die Verarbeitung von parallelen Workloads verwenden:

1. Laden Sie die **Datendateien**, die verarbeitet werden sollen, in ein [Azure-Speicherkonto](../storage/index.yml) hoch. Batch verfügt über integrierte Unterstützung für den Zugriff auf Azure Blob Storage, und Ihre Tasks können diese Dateien auf [Computeknoten](nodes-and-pools.md#nodes) herunterladen, wenn die Tasks ausgeführt werden.
2. Laden Sie die **Anwendungsdateien** hoch, die von Ihren Tasks ausgeführt werden sollen. Bei diesen Dateien kann es sich um Binärdateien oder Skripts und ihre Abhängigkeiten handeln, und sie werden von den Tasks in Ihren Aufträgen ausgeführt. Diese Dateien können von Ihren Tasks aus Ihrem Storage-Konto heruntergeladen werden. Alternativ können Sie das Batch-Feature [Anwendungspakete](nodes-and-pools.md#application-packages) für die Anwendungsverwaltung und -bereitstellung verwenden.
3. Erstellen Sie einen [Pool](nodes-and-pools.md#pools) mit Computeknoten. Beim Erstellen eines Pools geben Sie die Anzahl von Computeknoten für den Pool, deren Größe und das Betriebssystem an. Wenn die einzelnen Tasks des Auftrags ausgeführt werden, erfolgt die Zuweisung zur Ausführung auf einem der Knoten im Pool.
4. Erstellen Sie einen [Auftrag](jobs-and-tasks.md#jobs). Ein Auftrag verwaltet eine Sammlung von Tasks. Jeder Auftrag wird einem bestimmten Pool zugeordnet, in dem die Tasks des Auftrags ausgeführt werden.
5. Fügen Sie dem Auftrag [Aufgaben](jobs-and-tasks.md#tasks) hinzu. Jeder Task führt die hochgeladene Anwendung bzw. das Skript aus, um die Datendateien zu verarbeiten, die aus Ihrem Speicherkonto heruntergeladen werden. Nachdem ein Task abgeschlossen ist, kann die Ausgabe jeweils in Azure Storage hochgeladen werden.
6. Überwachen Sie den Auftragsstatus, und rufen Sie die Taskausgabe aus Azure Storage ab.

> [!NOTE]
> Zur Verwendung des Batch-Diensts wird ein [Batch-Konto](accounts.md) benötigt. Die meisten Batch-Lösungen verwenden außerdem ein zugeordnetes[ Azure-Speicherkonto](../storage/index.yml) zum Speichern und Abrufen von Dateien.

## <a name="batch-service-resources"></a>Ressourcen für den Batch-Dienst

In den folgenden Themen werden die Ressourcen von Batch besprochen, die Ihre verteilten Computingszenarien ermöglichen:

- [Batch-Konten und Azure Storage-Konten](accounts.md)
- [Knoten und Pools in Azure Batch](nodes-and-pools.md)
- [Aufträge und Aufgaben in Azure Batch](jobs-and-tasks.md)
- [Dateien und Verzeichnisse in Azure Batch](files-and-directories.md)

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Batch-APIs und Tools](batch-apis-tools.md), die für die Erstellung von Batch-Lösungen verfügbar sind.
- Informieren Sie sich über die Grundlagen der Entwicklung einer Batch-fähigen Anwendung mit der [Batch-.NET-Clientbibliothek](quick-run-dotnet.md) oder mit [Python](quick-run-python.md). In diesen Schnellstarts werden Sie durch eine Beispielanwendung geführt, die den Batch-Dienst zum Ausführen einer Workload auf mehreren Computeknoten verwendet und Azure Storage zum Bereitstellen und Abrufen von Workloaddateien nutzt.
- Laden Sie [Batch Explorer](https://azure.github.io/BatchExplorer/) für die Verwendung während der Entwicklung Ihrer Batch-Lösungen herunter, und installieren Sie ihn. Mit Batch Explorer können Sie Azure Batch-Anwendungen erstellen, debuggen und überwachen.
- Weitere Informationen finden Sie in den Communityressourcen, einschließlich [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-batch), dem [Repository der Batch Community](https://github.com/Azure/Batch) und dem [Azure Batch-Forum](/answers/topics/azure-batch.html).

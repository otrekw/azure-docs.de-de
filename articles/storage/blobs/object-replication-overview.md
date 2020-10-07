---
title: Übersicht über die Objektreplikation
titleSuffix: Azure Storage
description: Die Objektreplikation kopiert Blockblobs asynchron zwischen einem Quellspeicherkonto und einem Zielkonto. Mithilfe der Objektreplikation können Sie die Wartezeit bei Leseanforderungen minimieren, die Effizienz für Computeworkloads erhöhen, die Datenverteilung optimieren und die Kosten minimieren.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 4105698198e6fb7f4e3d3526ff9590ebca4898f1
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2020
ms.locfileid: "91612165"
---
# <a name="object-replication-for-block-blobs"></a>Objektreplikation für Blockblobs

Die Objektreplikation kopiert Blockblobs asynchron zwischen einem Quellspeicherkonto und einem Zielkonto. Folgende Szenarien werden unter anderem von der Objektreplikation unterstützt:

- **Minimieren der Wartezeit.** Die Objektreplikation kann die Wartezeit für Leseanforderungen verringern, indem Clients die Nutzung von Daten aus einer Region ermöglicht wird, die sich in größerer physischer Nähe befindet.
- **Erhöhen der Effizienz für Computeworkloads.** Bei der Objektreplikation können Computeworkloads dieselben Sätze von Blockblobs in verschiedenen Regionen verarbeiten.
- **Optimieren der Datenverteilung.** Sie können Daten an einem einzigen Standort verarbeiten bzw. analysieren und dann nur die Ergebnisse in zusätzliche Regionen replizieren.
- **Optimieren der Kosten.** Nachdem Ihre Daten repliziert wurden, können Sie die Kosten senken, indem Sie sie mithilfe von Lebenszyklus-Verwaltungsrichtlinien auf die Archivebene verschieben.

Das folgende Diagramm zeigt, wie die Objektreplikation Blockblobs aus einem Quellspeicherkonto in einer Region in Zielkonten in zwei verschiedenen Regionen repliziert.

:::image type="content" source="media/object-replication-overview/object-replication-diagram.svg" alt-text="Diagramm zur Veranschaulichung der Funktionsweise der Objektreplikation":::

Informationen zum Konfigurieren der Objektreplikation finden Sie unter [Konfigurieren der Objektreplikation](object-replication-configure.md).

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="prerequisites-for-object-replication"></a>Voraussetzungen für die Objektreplikation

Die Objektreplikation erfordert, dass außerdem die folgenden Azure Storage-Funktionen aktiviert sind:

- [Änderungsfeed](storage-blob-change-feed.md): Muss für das Quellkonto aktiviert sein. Informationen zum Aktivieren des Änderungsfeeds finden Sie unter [Aktivieren und Deaktivieren des Änderungsfeeds](storage-blob-change-feed.md#enable-and-disable-the-change-feed).
- [Blobversionsverwaltung](versioning-overview.md): Muss für das Quell- und das Zielkonto aktiviert sein. Informationen zum Aktivieren der Versionsverwaltung finden Sie unter [Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md).

Durch das Aktivieren des Änderungsfeeds und der Blobversionsverwaltung können zusätzliche Kosten entstehen. Weitere Informationen finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="object-replication-policies-and-rules"></a>Objektreplikationsrichtlinien und -regeln

Wenn Sie die Objektreplikation konfigurieren, erstellen Sie eine Replikationsrichtlinie, in der das Quellspeicherkonto und das Zielkonto angegeben werden. Eine Replikationsrichtlinie enthält mindestens eine Regel, die einen Quellcontainer und einen Zielcontainer angibt sowie anzeigt, welche Blockblobs im Quellcontainer repliziert werden.

Nachdem Sie die Objektreplikation konfiguriert haben, überprüft Azure Storage den Änderungsfeed für das Quellkonto regelmäßig und repliziert asynchron alle Schreib- oder Löschvorgänge in das Zielkonto. Die Replikationswartezeit hängt von der Größe des Blockblobs ab, das repliziert wird.

> [!IMPORTANT]
> Da Blockblobdaten asynchron repliziert werden, sind das Quell- und das Zielkonto nicht sofort synchron. Zurzeit gibt es keine SLA darüber, wie lange es dauert, Daten in das Zielkonto zu replizieren.

### <a name="replication-policies"></a>Replikationsrichtlinien

Wenn Sie die Objektreplikation konfigurieren, wird über den Azure Storage-Ressourcenanbieter eine Replikationsrichtlinie sowohl für das Quell- als auch für das Zielkonto erstellt. Die Replikationsrichtlinie wird durch eine Richtlinien-ID identifiziert. Die Richtlinie für das Quell- und das Zielkonto muss dieselbe Richtlinien-ID aufweisen, damit die Replikation stattfinden kann.

Ein Speicherkonto kann als Quellkonto für bis zu zwei Zielkonten fungieren. Die Quell- und Zielkonten dürfen sich in derselben oder in unterschiedlichen Regionen befinden. Sie können sich auch in unterschiedlichen Abonnements und in verschiedenen Azure Active Directory-Mandanten (Azure AD) befinden. Für jedes Paar aus Quell- und Zielkonto darf nur eine Replikationsrichtlinie erstellt werden.

### <a name="replication-rules"></a>Replikationsregeln

Replikationsregeln geben an, wie Azure Storage Blobs aus einem Quellcontainer in einen Zielcontainer repliziert. Sie können bis zu 10 Replikationsregeln für jede Replikationsrichtlinie angeben. Jede Replikationsregel definiert einen einzelnen Quell- und Zielcontainer, und jeder Quell- und Zielcontainer kann nur in einer Regel verwendet werden.

Beim Erstellen einer Replikationsregel werden standardmäßig nur neue Blockblobs kopiert, die anschließend dem Quellcontainer hinzugefügt werden. Sie können angeben, dass sowohl neue als auch vorhandene Blockblobs kopiert werden, oder Sie können einen benutzerdefinierten Kopierumfang definieren, der Blockblobs kopiert, die ab einem bestimmten Zeitpunkt erstellt wurden.

Sie können ferner einen oder mehrere Filter als Teil einer Replikationsregel angeben, um Blockblobs anhand eines Präfixes zu filtern. Wenn Sie ein Präfix angeben, werden nur Blobs in den Zielcontainer kopiert, die mit diesem Präfix im Quellcontainer übereinstimmen.

Die Quell- und Zielcontainer müssen beide vorhanden sein, bevor Sie sie in einer Regel angeben können. Nachdem Sie die Replikationsrichtlinie erstellt haben, wird der Zielcontainer schreibgeschützt. Alle Versuche, in den Zielcontainer zu schreiben, schlagen mit dem Fehlercode 409 (Konflikt) fehl. Sie können jedoch den Vorgang [Blobebene festlegen](/rest/api/storageservices/set-blob-tier) für einen Blob im Zielcontainer aufrufen, um ihn auf die Archivebene zu verschieben. Weitere Informationen zur Archivebene finden Sie unter [Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md#archive-access-tier).

## <a name="billing"></a>Abrechnung

Die Objektreplikation verursacht zusätzliche Kosten für Lese- und Schreibtransaktionen in den Quell- und Zielkonten sowie Gebühren für ausgehenden Datenverkehr für die Replikation von Daten aus dem Quellkonto in das Zielkonto und Lesegebühren für den Prozessänderungsfeed.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren der Objektreplikation](object-replication-configure.md)
- [Unterstützung für Änderungsfeeds in Azure Blob Storage](storage-blob-change-feed.md)
- [Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md)

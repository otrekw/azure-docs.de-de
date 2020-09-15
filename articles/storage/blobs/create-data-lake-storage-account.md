---
title: Erstellen eines Speicherkontos für Azure Data Lake Storage Gen2
description: Erfahren Sie, wie Sie ein Speicherkonto für die Verwendung mit Azure Data Lake Storage Gen2 erstellen.
author: normesta
ms.topic: how-to
ms.author: normesta
ms.date: 08/31/2020
ms.service: storage
ms.reviewer: stewu
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 6b63933fc625079bb490942cf3a32232a484fe38
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89269885"
---
# <a name="create-a-storage-account-to-use-with-azure-data-lake-storage-gen2"></a>Erstellen eines Speicherkontos für die Verwendung mit Azure Data Lake Storage Gen2

Um die Funktionen von Data Lake Storage Gen2 zu verwenden, erstellen Sie ein Speicherkonto mit einem hierarchischen Namespace.

## <a name="choose-a-storage-account-type"></a>Auswählen eines Speicherkontotyps

Die Funktionen von Data Lake Storage werden von den folgenden Typen von Speicherkonten unterstützt:

- General Purpose v2
- BlockBlobStorage

Informationen zum Auswählen zwischen diesen Optionen finden Sie unter [Übersicht über Azure Storage-Konten](../common/storage-account-overview.md).

## <a name="create-a-storage-account-with-a-hierarchical-namespace"></a>Erstellen eines Speicherkonto mit einem hierarchischen Namespace

Erstellen Sie ein Konto vom Typ [Universell V2](../common/storage-account-create.md) oder vom Typ [BlockBlobStorage](storage-blob-create-account-block-blob.md) mit aktivierter Einstellung **Hierarchischer Namespace**.

Sie aktivieren Data Lake Storage-Funktionen beim Erstellen des Kontos, indem Sie die Einstellung **Hierarchischer Namespace** auf der Registerkarte **Erweitert** auf der Seite **Speicherkonto erstellen** aktivieren. Diese Einstellung müssen Sie beim Erstellen des Kontos aktivieren. Später können Sie sie nicht mehr aktivieren.

In der folgenden Abbildung wird diese Einstellung auf der Seite **Speicherkonto erstellen** gezeigt.

> [!div class="mx-imgBorder"]
> ![Einstellung „Hierarchischer Namespace“](./media/create-data-lake-storage-account/hierarchical-namespace-feature.png)

Wenn Sie ein vorhandenes Speicherkonto mit Data Lake Storage verwenden möchten und die Einstellung „Hierarchischer Namespace“ deaktiviert ist, müssen Sie die Daten zu einem neuen Speicherkonto mit aktivierter Einstellung migrieren.

## <a name="next-steps"></a>Nächste Schritte

- [Speicherkontoübersicht](../common/storage-account-overview.md)
- [Verwenden von Azure Data Lake Storage Gen2 für Big Data-Anforderungen](data-lake-storage-data-scenarios.md)
- [Zugriffssteuerung in Azure Data Lake Storage Gen2](data-lake-storage-access-control.md)
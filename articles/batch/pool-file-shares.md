---
title: Azure-Dateifreigabe für Azure Batch-Pools
description: Hier erfahren Sie, wie Sie eine Azure Files-Freigabe von Computeknoten in einen Linux- oder Windows-Pool in Azure Batch einbinden.
ms.topic: how-to
ms.date: 05/24/2018
ms.openlocfilehash: e5682e7ba853973592c3a650a06ce72615cec7b6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98735493"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Verwenden einer Azure-Dateifreigabe mit einem Batch-Pool

[Azure Files](../storage/files/storage-files-introduction.md) bietet vollständig verwaltete Dateifreigaben in der Cloud, auf die über das Protokoll Server Message Block (SMB) zugegriffen werden kann. Dieser Artikel enthält Informationen und Codebeispiele für das Einbinden und Verwenden einer Azure-Dateifreigabe auf Computeknoten in einem Pool.

## <a name="considerations-for-use-with-batch"></a>Überlegungen für die Verwendung mit Batch

* Wenn Sie über Pools verfügen, die eine verhältnismäßig geringe Anzahl von parallelen Tasks ausführen, sollten Sie bei Verwendung einer Nicht-Premium-Instanz von Azure Files den Einsatz einer Azure-Dateifreigabe in Erwägung ziehen. Sehen Sie sich die [Skalierbarkeits- und Leistungsziele](../storage/files/storage-files-scale-targets.md) an, um anhand der erwarteten Poolgröße und der erwarteten Anzahl von Ressourcendateien zu ermitteln, ob Sie Azure Files verwenden sollten (wofür ein Azure Storage-Konto benötigt wird). 

* Azure-Dateifreigaben sind [kostengünstig](https://azure.microsoft.com/pricing/details/storage/files/). Für die Freigaben kann eine Datenreplikation in eine andere Region konfiguriert werden, sodass sie global redundant sind. 

* Sie können eine Azure-Dateifreigabe parallel von einem lokalen Computer aus einbinden. Stellen Sie jedoch sicher, dass Sie insbesondere bei Verwendung von REST-APIs die [Auswirkungen auf die Parallelität](../storage/blobs/concurrency-manage.md) verstehen.

* Sehen Sie sich auch die allgemeinen [Planungsüberlegungen](../storage/files/storage-files-planning.md) für Azure-Dateifreigaben an.


## <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe

[Erstellen Sie eine Dateifreigabe](../storage/files/storage-how-to-create-file-share.md) in einem Speicherkonto, das mit Ihrem Batch-Konto verknüpft ist, oder in einem separaten Speicherkonto.

## <a name="mount-an-azure-file-share-on-a-batch-pool"></a>Einbinden einer Azure-Dateifreigabe in einen Batch-Pool

Informationen zum [Einbinden eines virtuellen Dateisystems in einen Batch-Pool](virtual-file-mount.md) finden Sie in der Dokumentation.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Optionen zum Lesen und Schreiben von Daten in Batch finden Sie unter [Persistente Aufträge und Aufgabenausgabe](batch-task-output.md).
* Weitere Informationen finden Sie auch im [Batch Shipyard](https://github.com/Azure/batch-shipyard)-Toolkit, das [Shipyard-Rezepte](https://github.com/Azure/batch-shipyard/tree/master/recipes) enthält, mit denen Dateisysteme für Batch-Containerworkloads bereitgestellt werden können.
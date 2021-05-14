---
title: Grundlegendes zur Anwendung von Reservierungsrabatten auf Azure-Speicherdienste | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Rabatte für reservierte Kapazität auf Azure Blob Storage-, Azure Files- und Azure Data Lake Storage Gen2-Ressourcen angewendet werden.
author: tamram
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 03/08/2021
ms.author: tamram
ms.openlocfilehash: f18097f0da658b3a485b79f5279e9ecc54a41ea0
ms.sourcegitcommit: a8ff4f9f69332eef9c75093fd56a9aae2fe65122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "105024029"
---
# <a name="understand-how-reservation-discounts-are-applied-to-azure-storage-services"></a>Grundlegendes zur Anwendung von Reservierungsrabatten auf Azure Files-Dienste 
Mit Azure-Speicherdiensten können Sie durch Reservieren von Kapazität bei den Speicherkosten sparen. Azure Blob Storage, Azure Files und Azure Data Lake Storage Gen2 unterstützen reservierte Instanzen. Nach dem Erwerb von reservierter Kapazität wird der Reservierungsrabatt automatisch auf die Speicherressourcen angewendet, die den Reservierungsbedingungen entsprechen. Der Reservierungsrabatt gilt nur für Speicherkapazität. Bandbreite und Anforderungsrate werden zu den Sätzen für die nutzungsbasierte Bezahlung abgerechnet.

Weitere Informationen zu reservierter Azure Blob Storage- und Azure Data Lake Storage Gen2-Kapazität finden Sie unter [Optimieren der Kosten für Blobspeicher mit reservierter Kapazität](../../storage/blobs/storage-blob-reserved-capacity.md). Weitere Informationen zu reservierter Azure Files-Kapazität finden Sie unter [Optimieren der Kosten für Azure Files mit reservierter Kapazität](../../storage/files/files-reserve-capacity.md).

Informationen zu den Preisen für Azure Blob Storage-Reservierungen finden Sie in der [Preisübersicht für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/) und in der [Preisübersicht für Azure Data Lake Storage Gen2](https://azure.microsoft.com/pricing/details/storage/data-lake/). Informationen zu den Preisen für die Azure Files-Speicherreservierung finden Sie unter [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files).

## <a name="how-the-reservation-discount-is-applied"></a>Anwendung des Reservierungsrabatts
Der Rabatt für reservierte Kapazität wird auf unterstützte Speicherressourcen, die auf Stundenbasis abgerechnet werden, angewendet.

Nicht in Anspruch genommener Rabatt für reservierte Kapazität geht verloren. Wenn Sie also für eine Stunde über keine Blockblobs, Azure-Dateifreigaben oder Azure Data Lake Storage Gen2-Ressourcen verfügen, die den Reservierungsbedingungen entsprechen, verlieren Sie eine Reservierungsmenge für diese Stunde. Sie können ungenutzte reservierte Stunden nicht übertragen.

Wenn Sie eine Ressource löschen, gilt der Reservierungsrabatt automatisch für eine andere entsprechende Ressource im angegebenen Reservierungsumfang. Werden im angegebenen Reservierungsumfang keine entsprechenden Ressourcen gefunden, gehen die reservierten Stunden verloren.

## <a name="discount-examples"></a>Beispiele für die Anwendung des Rabatts
Die folgenden Beispiele zeigen, wie der Rabatt für die reservierte Kapazität abhängig von den jeweiligen Bereitstellungen angewendet wird.

Angenommen, Sie haben für eine Laufzeit von einem Jahr eine reservierte Kapazität von 100 TiB in der Region „USA, Westen 2“ erworben. Ihre Reservierung gilt für lokal redundanten Blobspeicher (LRS) der heißen Zugriffsebene.

Die Kosten dieser Beispielreservierung liegen bei 18 540 USD. Sie können entweder den gesamten Betrag vorab bezahlen oder sich für die Zahlung von 12 festen Monatsraten in Höhe von jeweils 1.545 USD entscheiden.

Für diese Beispiele gehen wir davon aus, dass Sie sich für einen monatlichen Zahlungsplan entschieden haben. In den folgenden Szenarien erfahren Sie, was passiert, wenn Sie Ihre reservierte Kapazität unter- oder überschreiten.

### <a name="underusing-your-capacity"></a>Unterschreitung Ihrer Kapazität
Angenommen, Sie haben in einer Stunde innerhalb des Reservierungszeitraums nur 80 TiB Ihrer reservierten Kapazität (100 TiB) beansprucht. Die verbleibenden 20 TiB werden für diese Stunde nicht angewendet und nicht übertragen.

### <a name="overusing-your-capacity"></a>Überschreitung Ihrer Kapazität
Angenommen, Sie haben in einer Stunde innerhalb des Reservierungszeitraums eine Speicherkapazität von 101 TiB beansprucht. Der Reservierungsrabatt gilt für 100 TiB Ihrer Daten, und die verbleibende Menge von 1 TiB wird für diese Stunde zu den Sätzen für die nutzungsbasierte Bezahlung abgerechnet. Wenn Ihre Nutzung in der nächsten Stunde 100 TiB beträgt, ist die gesamte Nutzung durch die Reservierung abgedeckt.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt
Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte
- [Optimieren der Kosten für Blobspeicher mit reservierter Kapazität](../../storage/blobs/storage-blob-reserved-capacity.md)
- [Optimieren der Kosten für Azure Files mit reservierter Kapazität](../../storage/files/files-reserve-capacity.md)
- [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md)

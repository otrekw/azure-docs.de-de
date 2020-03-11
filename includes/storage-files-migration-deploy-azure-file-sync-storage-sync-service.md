---
title: Bereitstellen eines Speichersynchronisierungsdiensts
description: Bereitstellen der Cloudressource für die Azure-Dateisynchronisierung Ein Speichersynchronisierungsdienst. Ein gemeinsamer Textblock der Migrationsdokumentation.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 4468d8c6b57b38bdbaaa01208cfc4915f734f6bb
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209289"
---
Für diesen Schritt benötigen Sie die Anmeldeinformationen für Ihr Azure-Abonnement. Möglicherweise ist das von Ihnen verwendete Azure-Abonnement nicht dasselbe Abonnement, das Sie für StorSimple verwenden.

Die wichtigste Ressource für die Konfiguration der Azure-Dateisynchronisierung ist der Speichersynchronisierungsdienst.
Es wird empfohlen, nur einen solchen Dienst für alle Server innerhalb des Unternehmens bereitzustellen, der für die Synchronisierung aller aktuellen und zukünftigen Dateien verwendet wird. Wenn Sie über mehrere StorSimple-Appliances verfügen, können Sie eine Speichersynchronisierungsdienst-Ressource für jede dieser Appliances erstellen. Sie sollten jedoch nur dann mehrere Speichersynchronisierungsdienste erstellen, wenn Sie über separate Server verfügen, zwischen denen niemals ein Datenaustausch stattfinden darf. Anderenfalls ist ein einziger Speichersynchronisierungsdienst die beste Vorgehensweise.

Wählen Sie eine Azure-Region für Ihren Speichersynchronisierungsdienst aus, die sich in der Nähe Ihrer Niederlassung befindet. Alle anderen Cloudressourcen müssen innerhalb derselben Region bereitgestellt werden.
Für eine vereinfachte Verwaltung wird empfohlen, in Ihrem Abonnement eine neue Ressourcengruppe für Synchronisierungs- und Speicherressourcen zu erstellen.

Im folgenden Artikel wird beschrieben, wie Sie einen Speichersynchronisierungsdienst bereitstellen. Befolgen Sie nur die Schritte in diesem Teil der Dokumentation. Spätere Schritte enthalten Links zu anderen Unterabschnitten dieser Dokumentation.

[Erfahren Sie, wie Sie einen Speichersynchronisierungsdienst bereitstellen.](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service)

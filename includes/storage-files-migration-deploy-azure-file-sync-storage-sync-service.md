---
title: Bereitstellen eines Speichersynchronisierungsdiensts
description: Bereitstellen der Cloudressource für die Azure-Dateisynchronisierung Ein Speichersynchronisierungsdienst. Ein gemeinsamer Textblock der Migrationsdokumentation.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: fe74bae95784dcb63a80ee7e280c02de2eadeb5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124708"
---
Für diesen Schritt benötigen Sie die Anmeldeinformationen für Ihr Azure-Abonnement.

Die wichtigste Ressource für die Konfiguration der Azure-Dateisynchronisierung ist der Speichersynchronisierungsdienst.
Es wird empfohlen, nur einen solchen Dienst für alle Server innerhalb des Unternehmens bereitzustellen, der für die Synchronisierung aller aktuellen und zukünftigen Dateien verwendet wird. Erstellen Sie mehrere Speichersynchronisierungsdienste nur dann, wenn Sie über separate Server verfügen, zwischen denen niemals ein Datenaustausch stattfinden darf (z. B. Synchronisierung derselben Azure-Dateifreigabe). Anderenfalls ist ein einziger Speichersynchronisierungsdienst die beste Vorgehensweise.

Wählen Sie eine Azure-Region für Ihren Speichersynchronisierungsdienst aus, die sich in der Nähe Ihrer Niederlassung befindet. Alle anderen Cloudressourcen müssen innerhalb derselben Region bereitgestellt werden.
Für eine vereinfachte Verwaltung erstellen Sie in Ihrem Abonnement eine neue Ressourcengruppe für Synchronisierungs- und Speicherressourcen.

Im folgenden Artikel wird beschrieben, wie Sie einen Speichersynchronisierungsdienst bereitstellen. Befolgen Sie nur die Schritte in diesem Teil der Dokumentation. Spätere Schritte enthalten Links zu anderen Unterabschnitten dieser Dokumentation.

[Erfahren Sie, wie Sie einen Speichersynchronisierungsdienst bereitstellen.](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service)

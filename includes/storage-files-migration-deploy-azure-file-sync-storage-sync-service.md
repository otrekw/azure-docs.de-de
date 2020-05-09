---
title: Bereitstellen eines Speichersynchronisierungsdiensts
description: Bereitstellen der Cloudressource für die Azure-Dateisynchronisierung, eines Speichersynchronisierungsdiensts. Ein gemeinsamer Textblock der Migrationsdokumentation.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 248b61530b80a00aa10272dcb3d28b85c6ee04e5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82143605"
---
Für diesen Schritt benötigen Sie die Anmeldeinformationen für Ihr Azure-Abonnement.

Die wichtigste Ressource, die für die Azure-Dateisynchronisierung konfiguriert werden muss, ist der *Speichersynchronisierungsdienst*. Es wird empfohlen, nur einen solchen Dienst für alle Server bereitzustellen, die aktuell oder in Zukunft für die Synchronisierung der gleichen Dateien verwendet werden. Erstellen Sie nur dann mehrere Speichersynchronisierungsdienste, wenn Sie über separate Gruppen von Servern verfügen, zwischen denen niemals ein Datenaustausch stattfinden darf (z. B. Synchronisierung derselben Azure-Dateifreigabe). Andernfalls ist die Best Practice ein einzelner Speichersynchronisierungsdienst.

Wählen Sie eine Azure-Region für Ihren Speichersynchronisierungsdienst aus, die sich in der Nähe Ihres Standorts befindet. Alle anderen Cloudressourcen müssen innerhalb derselben Region bereitgestellt werden.
Für eine vereinfachte Verwaltung erstellen Sie in Ihrem Abonnement eine neue Ressourcengruppe für Synchronisierungs- und Speicherressourcen.

Weitere Informationen finden Sie im [Abschnitt über das Bereitstellen des Speichersynchronisierungsdiensts](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service) im Artikel über das Bereitstellen der Azure-Dateisynchronisierung. Führen Sie nur die Schritte in diesem Teil des Artikels aus. Spätere Schritte enthalten Links zu anderen Abschnitten dieses Artikels.
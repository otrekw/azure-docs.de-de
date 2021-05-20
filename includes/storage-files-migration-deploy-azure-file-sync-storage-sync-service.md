---
title: include file
description: include file
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 2/20/2020
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: 180b615869579752f9a14aa2dcdd34f1676b577b
ms.sourcegitcommit: ba8f0365b192f6f708eb8ce7aadb134ef8eda326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2021
ms.locfileid: "109645081"
---
Sie benötigen die Anmeldeinformationen für Ihr Azure-Abonnement, um diesen Schritt abschließen zu können.

Die wichtigste Ressource, die für die Azure-Dateisynchronisierung konfiguriert werden muss, ist der *Speichersynchronisierungsdienst*. Es wird empfohlen, nur einen solchen Dienst für alle Server bereitzustellen, die aktuell oder in Zukunft für die Synchronisierung der gleichen Dateien verwendet werden. Erstellen Sie mehrere Speichersynchronisierungsdienste nur dann, wenn Sie über separate Server verfügen, zwischen denen niemals ein Datenaustausch stattfinden darf. Sie könnten z. B. über Server verfügen, die niemals dieselbe Azure-Dateifreigabe synchronisieren dürfen. Andernfalls besteht eine bewährte Methode darin, einen einzelnen Speichersynchronisierungsdienst zu verwenden.

Wählen Sie eine Azure-Region für Ihren Speichersynchronisierungsdienst aus, die sich in der Nähe Ihres Standorts befindet. Alle anderen Cloudressourcen müssen innerhalb derselben Region bereitgestellt werden. Für eine vereinfachte Verwaltung erstellen Sie in Ihrem Abonnement eine neue Ressourcengruppe für Synchronisierungs- und Speicherressourcen.

Weitere Informationen finden Sie im [Abschnitt über das Bereitstellen des Speichersynchronisierungsdiensts](../articles/storage/file-sync/file-sync-deployment-guide.md#deploy-the-storage-sync-service) im Artikel über das Bereitstellen der Azure-Dateisynchronisierung. Führen Sie nur die Schritte in diesem Abschnitt des Artikels aus. Spätere Schritte enthalten Links zu anderen Abschnitten dieses Artikels.
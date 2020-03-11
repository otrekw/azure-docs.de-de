---
title: Konfigurieren der Azure-Dateisynchronisierung
description: Konfigurieren Sie die Azure-Dateisynchronisierung. Ein gemeinsamer Textblock der Migrationsdokumentation.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 53d7af6b43ff24ab12501570385162759f4c7633
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209329"
---
In diesem Schritt werden alle Ressourcen und Ordner miteinander verknüpft, die Sie im vorherigen Schritt auf Ihrer Windows Server-Instanz eingerichtet haben.

* Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
* Wechseln Sie zu Ihrer Speichersynchronisierungsdienst-Ressource.
* Erstellen Sie innerhalb des Speichersynchronisierungsdiensts für jede Azure-Dateifreigabe eine neue *Synchronisierungsgruppe*. Im Kontext der Azure-Dateisynchronisierung wird die Azure-Dateifreigabe zu einem *Cloudendpunkt* in der Synchronisierungstopologie, die Sie durch das Erstellen einer Synchronisierungsgruppe beschreiben. Weisen Sie beim Erstellen der Synchronisierungsgruppe einen vertrauten Namen zu, anhand dessen Sie erkennen, welche Dateien synchronisiert werden. Stellen Sie sicher, dass Sie mit einem übereinstimmenden Namen auf die Azure-Dateifreigabe verweisen.
* Nach dem Erstellen der Synchronisierungsgruppe wird sie in der Liste der Synchronisierungsgruppen in einer eigenen Zeile angezeigt. Klicken Sie auf den Namen (einen Link), um die Inhalte der Synchronisierungsgruppe anzuzeigen. Ihre Azure-Dateifreigabe wird unter „Cloudendpunkte“ aufgeführt.
* Wechseln Sie zum Button *+ Serverendpunkt hinzufügen*. Der Ordner auf Ihrem lokalen Server, den Sie bereitgestellt haben, wird als Pfad für diesen *Serverendpunkt* verwendet.

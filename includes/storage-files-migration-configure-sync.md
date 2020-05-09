---
title: Konfigurieren der Azure-Dateisynchronisierung
description: Konfigurieren Sie die Azure-Dateisynchronisierung. Ein gemeinsamer Textblock der Migrationsdokumentation.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 562099c65273cad50596268b89055ec78d92d18a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82143632"
---
In diesem Schritt werden alle Ressourcen und Ordner miteinander verknüpft, die Sie im vorherigen Schritt auf Ihrer Windows Server-Instanz eingerichtet haben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wechseln Sie zu Ihrer Speichersynchronisierungsdienst-Ressource.
1. Erstellen Sie innerhalb des Speichersynchronisierungsdiensts für jede Azure-Dateifreigabe eine neue *Synchronisierungsgruppe*. Im Kontext der Azure-Dateisynchronisierung wird die Azure-Dateifreigabe zu einem *Cloudendpunkt* in der Synchronisierungstopologie, die Sie durch das Erstellen einer Synchronisierungsgruppe beschreiben. Weisen Sie beim Erstellen der Synchronisierungsgruppe einen vertrauten Namen zu, damit Sie erkennen können, welche Dateien synchronisiert werden. Stellen Sie sicher, dass Sie mit einem übereinstimmenden Namen auf die Azure-Dateifreigabe verweisen.
1. Nach dem Erstellen der Synchronisierungsgruppe wird in der Liste der Synchronisierungsgruppen eine eigene Zeile für diese Gruppe angezeigt. Wählen Sie den Namen (einen Link) aus, um die Inhalte der Synchronisierungsgruppe anzuzeigen. Ihre Azure-Dateifreigabe wird unter **Cloudendpunkte** aufgeführt.
1. Suchen Sie die Schaltfläche **+ Serverendpunkt hinzufügen**. Der Ordner auf dem lokalen Server, den Sie bereitgestellt haben, wird als Pfad für diesen *Serverendpunkt* verwendet.

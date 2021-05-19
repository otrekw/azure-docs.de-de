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
ms.openlocfilehash: f4baa89757d4ae93af8c1067cf6ef4617ce95e5a
ms.sourcegitcommit: ba8f0365b192f6f708eb8ce7aadb134ef8eda326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2021
ms.locfileid: "109645287"
---
In diesem Schritt werden alle Ressourcen und Ordner miteinander verknüpft, die Sie im vorherigen Schritt auf Ihrer Windows Server-Instanz eingerichtet haben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wechseln Sie zu Ihrer Speichersynchronisierungsdienst-Ressource.
1. Erstellen Sie innerhalb des Speichersynchronisierungsdiensts für jede Azure-Dateifreigabe eine neue *Synchronisierungsgruppe*. Im Kontext der Azure-Dateisynchronisierung wird die Azure-Dateifreigabe zu einem *Cloudendpunkt* in der Synchronisierungstopologie, die Sie durch das Erstellen einer Synchronisierungsgruppe beschreiben. Weisen Sie beim Erstellen der Synchronisierungsgruppe einen aussagekräftigen Namen zu, damit Sie erkennen können, welche Dateien synchronisiert werden. Stellen Sie sicher, dass Sie mit einem übereinstimmenden Namen auf die Azure-Dateifreigabe verweisen.
1. Nach dem Erstellen der Synchronisierungsgruppe wird in der Liste der Synchronisierungsgruppen eine eigene Zeile für diese Gruppe angezeigt. Wählen Sie den Namen (einen Link) aus, um die Inhalte der Synchronisierungsgruppe anzuzeigen. Ihre Azure-Dateifreigabe wird unter **Cloudendpunkte** aufgeführt.
1. Suchen Sie die Schaltfläche **Serverendpunkt hinzufügen**. Der Ordner auf dem lokalen Server, den Sie bereitgestellt haben, wird als Pfad für diesen *Serverendpunkt* verwendet.

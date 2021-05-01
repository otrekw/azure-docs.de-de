---
title: storage-files-create-storage-account-portal
description: Erstellen Sie ein Speicherkonto für Azure Files.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 04/15/2021
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 33159ab6dd014a153c8fd317fd291aeca033d6e9
ms.sourcegitcommit: 79c9c95e8a267abc677c8f3272cb9d7f9673a3d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107717833"
---
Ein Speicherkonto ist ein gemeinsam genutzter Pool mit Speicherplatz, in dem Sie eine Azure-Dateifreigabe oder andere Speicherressourcen wie Blobs oder Warteschlangen bereitstellen können. Ein Speicherkonto kann eine unbegrenzte Anzahl von Freigaben enthalten. Auf einer Freigabe kann eine unbegrenzte Anzahl von Dateien gespeichert werden, bis die Kapazitätsgrenzen des Speicherkontos erreicht sind.

So erstellen Sie ein Speicherkonto:

1. Klicken Sie im Menü auf der linken Seite auf **+** , um eine Ressource zu erstellen.
1. Wählen Sie **Speicherkonto** aus, um ein Speicherkonto zu erstellen.

    :::image type="content" source="../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png" alt-text="Screenshot: Speicherkontooption auf dem Blatt „Ressource erstellen“" lightbox="../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png":::

1. Geben Sie unter **Name** den Namen *mystorageacct* sowie einige zufällig gewählte Zahlen ein, bis durch ein grünes Häkchen signalisiert wird, dass der Name eindeutig ist. Der Name des Speicherkontos darf nur Kleinbuchstaben enthalten und muss global eindeutig sein. Notieren Sie sich den Speicherkontonamen. Er wird später noch benötigt. 
1. Übernehmen Sie unter **Leistung** den Standardwert **Standard**.
1. Wählen Sie unter **Replikation** die Option **Lokal redundanter Speicher (LRS)** aus.
1. Wählen Sie unter **Abonnement** das Abonnement aus, mit dem das Speicherkonto erstellt wurde. Falls Sie nur über ein einzelnes Abonnement verfügen, sollte es das Standardabonnement sein.
1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus. Geben Sie als Name *myResourceGroup* ein.
1. Wählen Sie unter **Standort** die Option **USA, Osten**.
1. Klicken Sie abschließend auf **Erstellen**, um die Bereitstellung zu starten.
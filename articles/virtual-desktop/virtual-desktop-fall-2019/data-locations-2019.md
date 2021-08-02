---
title: Datenstandorte für Azure Virtual Desktop (klassisch) – Azure
description: Eine kurze Übersicht der Standorte, an denen die Daten und Metadaten von Azure Virtual Desktop (klassisch) gespeichert werden
author: Heidilohr
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 9ddfc6cd8a18e9bdbee071d0c5fd64a0c14b2e95
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111754473"
---
# <a name="data-locations-for-azure-virtual-desktop-classic"></a>Datenstandorte für Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../data-locations.md) weiter.

Azure Virtual Desktop ist zurzeit für alle geografischen Standorte verfügbar. Anfänglich können Dienstmetadaten nur in den Regionen in den USA gespeichert werden. Administratoren können den Standort zum Speichern von Benutzerdaten auswählen, wenn sie die virtuellen Computer des Hostpools und die zugehörigen Dienste wie Dateiserver erstellen. Weitere Informationen zu Azure-Regionen erhalten Sie in der [Karte der Azure-Rechenzentren](https://azuredatacentermap.azurewebsites.net/).

>[!NOTE]
>Microsoft kontrolliert oder beschränkt nicht die Regionen, in denen Sie oder Ihre Benutzer auf Ihre benutzer- und App-spezifischen Daten zugreifen können.

>[!IMPORTANT]
>Azure Virtual Desktop speichert globale Metadateninformationen wie Mandantennamen, Hostpoolnamen, App-Gruppennamen und Benutzerprinzipalnamen in einem Rechenzentrum in den USA. Die gespeicherten Metadaten werden im Ruhezustand verschlüsselt, und georedundante Spiegel werden innerhalb der USA verwaltet. Alle Kundendaten wie App-Einstellungen und Benutzerdaten befinden sich an dem vom Kunden ausgewählten Standort und werden nicht vom Dienst verwaltet.

Dienstmetadaten werden für die Notfallwiederherstellung in die USA repliziert.
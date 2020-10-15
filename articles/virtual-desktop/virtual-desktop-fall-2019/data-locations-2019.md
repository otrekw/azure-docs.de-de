---
title: Datenstandorte für Windows Virtual Desktop (klassisch) – Azure
description: Eine kurze Übersicht der Standorte, an denen die Daten und Metadaten von Windows Virtual Desktop (klassisch) gespeichert werden
author: Heidilohr
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 869defde657c9cb4c8bea6bbacebb9458e5a2b96
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88008813"
---
# <a name="data-locations-for-windows-virtual-desktop-classic"></a>Datenstandorte für Windows Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für den Windows Virtual Desktop-Dienst (klassisch), der keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../data-locations.md) weiter.

Windows Virtual Desktop ist zurzeit für alle geografischen Standorte verfügbar. Anfänglich können Dienstmetadaten nur in den Regionen in den USA gespeichert werden. Administratoren können den Standort zum Speichern von Benutzerdaten auswählen, wenn sie die virtuellen Computer des Hostpools und die zugehörigen Dienste wie Dateiserver erstellen. Weitere Informationen zu Azure-Regionen erhalten Sie in der [Karte der Azure-Rechenzentren](https://azuredatacentermap.azurewebsites.net/).

>[!NOTE]
>Microsoft kontrolliert oder beschränkt nicht die Regionen, in denen Sie oder Ihre Benutzer auf Ihre benutzer- und App-spezifischen Daten zugreifen können.

>[!IMPORTANT]
>Windows Virtual Desktop speichert globale Metadateninformationen wie Mandantennamen, Hostpoolnamen, App-Gruppennamen und Benutzerprinzipalnamen in einem Rechenzentrum in den USA. Die gespeicherten Metadaten werden im Ruhezustand verschlüsselt, und georedundante Spiegel werden innerhalb der USA verwaltet. Alle Kundendaten wie App-Einstellungen und Benutzerdaten befinden sich an dem vom Kunden ausgewählten Standort und werden nicht vom Dienst verwaltet.

Dienstmetadaten werden für die Notfallwiederherstellung in die USA repliziert.
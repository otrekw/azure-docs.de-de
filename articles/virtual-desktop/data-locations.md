---
title: Datenstandorte für Windows Virtual Desktop – Azure
description: Eine kurze Übersicht der Standorte, an denen die Daten und Metadaten von Windows Virtual Desktop gespeichert werden.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 853f5766c4fd6344eecfd7be1d7911163133a8a5
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611533"
---
# <a name="data-locations-for-windows-virtual-desktop"></a>Datenstandorte für Windows Virtual Desktop

>[!IMPORTANT]
>Dieser Inhalt gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/data-locations-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows Virtual Desktop ist zurzeit für alle geografischen Standorte verfügbar. Administratoren können den Standort zum Speichern von Benutzerdaten auswählen, wenn sie die virtuellen Computer des Hostpools und die zugehörigen Dienste wie Dateiserver erstellen. Weitere Informationen zu Azure-Regionen erhalten Sie in der [Karte der Azure-Rechenzentren](https://azuredatacentermap.azurewebsites.net/).

>[!NOTE]
>Microsoft kontrolliert oder beschränkt nicht die Regionen, in denen Sie oder Ihre Benutzer auf Ihre benutzer- und App-spezifischen Daten zugreifen können.

>[!IMPORTANT]
>Windows Virtual Desktop speichert globale Metadateninformationen wie Mandantennamen, Hostpoolnamen, App-Gruppennamen und Benutzerprinzipalnamen in einem Rechenzentrum. Wenn ein Kunde ein Dienstobjekt erstellt, muss er einen Standort für das Dienstobjekt eingeben. Der eingegebene Standort bestimmt, wo die Metadaten für das Objekt gespeichert werden. Der Kunde wählt eine Azure-Region aus, und die Metadaten werden in der zugehörigen Geografie gespeichert. Eine Liste aller Azure-Regionen und zugehöriger Geografien finden Sie unter [Azure-Geografien](https://azure.microsoft.com/global-infrastructure/geographies/).

Derzeit wird nur das Speichern von Metadaten in der Azure-Geografie „Vereinigte Staaten, US“ unterstützt. Die gespeicherten Metadaten werden im Ruhezustand verschlüsselt, und georedundante Spiegelungen werden innerhalb der Geografie verwaltet. Alle Kundendaten wie App-Einstellungen und Benutzerdaten befinden sich an dem vom Kunden ausgewählten Standort und werden nicht vom Dienst verwaltet. Mit einer zunehmenden Verbreitung des Diensts werden weitere Geografien verfügbar.

Dienstmetadaten werden innerhalb der Azure-Geografie für die Notfallwiederherstellung repliziert.
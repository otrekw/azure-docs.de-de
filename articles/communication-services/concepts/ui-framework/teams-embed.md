---
title: 'Benutzeroberflächen-Framework: Teams-Einbettung'
titleSuffix: An Azure Communication Services quickstart
description: In diesem Dokument erfahren Sie, wie Sie mithilfe der Teams-Einbettung des Benutzeroberflächen-Frameworks von Azure Communication Services sofort einsatzbereite Telefonieumgebungen erstellen.
author: tophpalmer
ms.author: chpalm
ms.date: 11/16/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: e55cfb1a4dff7bfda2323e68777d6f50514b1608
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105930233"
---
# <a name="teams-embed"></a>Teams-Einbettung

[!INCLUDE [Public Preview Notice](../../includes/private-preview-include.md)]


Bei der Teams-Einbettung handelt es sich um eine Azure Communication Services-Funktion für allgemeine Telefonieinteraktionen zwischen Unternehmen und Kunden sowie zwischen Unternehmen. Den Kern des Teams-Einbettungssystems bilden [Video- und Sprachanrufe](../voice-video-calling/calling-sdk-features.md), das System nutzt allerdings die Anrufprimitiven von Azure, um eine umfassende Benutzerumgebung auf der Grundlage von Microsoft Teams-Besprechungen bereitzustellen.

Bei den Teams Embed SDKs handelt es sich um Closed-Source-Komponenten, die diese Funktionen in einem sofort einsatzbereiten, kombinierten Format verfügbar machen. Wenn Sie die Teams-Einbettung der Canvas Ihrer App hinzufügen, wird durch das SDK eine vollständige Benutzerumgebung generiert. Da sich diese Benutzerumgebung stark an Microsoft Teams-Besprechungen orientiert, profitieren Sie von Folgendem:

- Geringere Entwicklungsdauer und technische Komplexität
- Vertrautheit von Endbenutzern mit Teams
- Möglichkeit zur Wiederverwendung von [Endbenutzer-Schulungsinhalten für Teams](https://support.microsoft.com/office/meetings-in-teams-e0b0ae21-53ee-4462-a50d-ca9b9e217b67)

Die Teams-Einbettung bietet einen Großteil der Features, die auch in Teams-Besprechungen zur Verfügung stehen:

- Möglichkeit für Benutzer, vor der Besprechung die eigenen Audio- und Videogeräte zu konfigurieren
- Möglichkeit für Benutzer, Audio- und Videogeräte während der Besprechung zu konfigurieren
- [Videohintergründe:](https://support.microsoft.com/office/change-your-background-for-a-teams-meeting-f77a2381-443a-499d-825e-509a140f4780) Möglichkeit für Teilnehmer, den eigenen Hintergrund undeutlich zu machen oder zu ersetzen
- [Mehrere Optionen für die Videogalerie:](https://support.microsoft.com/office/using-video-in-microsoft-teams-3647fc29-7b92-4c26-8c2d-8a596904cdae) Umfangreiche Galerie, Together-Modus, Fokus, Fixieren und Spotlight
- [Inhaltsfreigabe:](https://support.microsoft.comoffice/share-content-in-a-meeting-in-teams-fcc2bf59-aecd-4481-8f99-ce55dd836ce8#ID0EABAAA=Mobile) Möglichkeit für Teilnehmer, den eigenen Bildschirm freizugeben

Weitere Informationen zu dieser Benutzeroberfläche im Vergleich zu anderen Azure-Kommunikations-SDKs finden Sie unter [Azure Communication Services: Benutzeroberflächen-Framework](ui-sdk-overview.md). 

---
title: Konfigurieren von Details zu VM-Angebotsauflistungen im Azure Marketplace
description: Erfahren Sie, wie Sie Details zu VM-Angebotsauflistungen im Azure Marketplace konfigurieren.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: mingshen
ms.date: 10/19/2020
ms.openlocfilehash: 2af96c0c2e850d8e1be67c23e71aa677773c1e03
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283210"
---
# <a name="how-to-configure-virtual-machine-offer-listing-details"></a>Konfigurieren von Details zu VM-Angebotsauflistungen

Auf der Seite **Angebotsliste** können Sie Angebotsdetails wie Name, Beschreibung, Links und Kontakte definieren.

> [!NOTE]
> Die Inhalte der Angebotsliste wie Beschreibung, Dokumente, Screenshots und Nutzungsbedingungen müssen nicht in englischer Sprache vorliegen, sofern die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur auf \<non-English language> verfügbar“. Sie können auch eine URL als Link zu einer Website bereitstellen, die Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

## <a name="marketplace-details"></a>Marketplace-Details

### <a name="name"></a>Name

Der hier eingegebene Name wird Kunden als Titel Ihres Angebots angezeigt. In dieses Feld wird automatisch der Name eingetragen, den Sie beim Erstellen des Angebots im Feld **Angebotsalias** eingegeben haben. Sie können diesen Namen später ändern. Der Name:

- Kann markenrechtlich geschützt sein. Sie können Marken- und Copyrightsymbole einschließen.
- Darf nicht mehr als 50 Zeichen umfassen.
- Darf keine Emojis enthalten.

### <a name="search-results-summary"></a>Zusammenfassung der Suchergebnisse

Geben Sie eine kurze Beschreibung Ihres Angebots ein, die in den Azure Marketplace-Suchergebnissen angezeigt werden soll. Sie darf bis zu 100 Zeichen enthalten.

### <a name="long-summary"></a>Lange Zusammenfassung

Geben Sie eine längere Beschreibung Ihres Angebots ein, die in den Azure Marketplace-Suchergebnissen angezeigt werden soll. Sie darf bis zu 256 Zeichen enthalten.

### <a name="description"></a>BESCHREIBUNG

[!INCLUDE [Long description-1](includes/long-description-1.md)]

[!INCLUDE [Long description-2](includes/long-description-2.md)]

Verwenden Sie HTML-Tags, um Ihre Beschreibung zu formatieren und ansprechender zu gestalten. Eine Liste der zulässigen Tags finden Sie unter [Unterstützte HTML-Tags](supported-html-tags.md).

### <a name="privacy-policy-link"></a>Link zur Datenschutzrichtlinie

Geben Sie die Webadresse (URL) der Datenschutzrichtlinie Ihrer Organisation ein. Stellen Sie sicher, dass Ihr Angebot den Datenschutzbestimmungen und -gesetzen entspricht. Sie müssen auch eine gültige Datenschutzrichtlinie auf Ihrer Website veröffentlichen.

## <a name="useful-links"></a>Nützliche Links

Stellen Sie ergänzende Onlinedokumente zu Ihrem Angebot bereit. Um einen Link hinzuzufügen, wählen Sie **Link hinzufügen** aus, und füllen Sie dann die folgenden Felder aus:

- **Name** : Kunden wird der Name auf der Seite mit den Details angezeigt.
- **Link (URL):** Geben Sie einen Link ein, über den Kunden Ihr Onlinedokument anzeigen können.

## <a name="customer-support-links"></a>Kundensupportlinks

Geben Sie die Supportwebsite an, auf der Kunden Ihr Supportteam erreichen können.

- Azure Global-Supportwebsite
- Azure Government-Supportwebsite

## <a name="partner-support-contact"></a>Partnersupportkontakt

Geben Sie Kontaktinformationen für Microsoft-Partner an, die sie verwenden können, wenn Ihre Kunden ein Supportticket öffnen. Diese Informationen werden nicht im Azure Marketplace aufgeführt.

- Name
- Email
- Phone

## <a name="engineering-contact"></a>Engineering-Kontakt

Geben Sie Kontaktinformationen an, die Microsoft verwenden soll, wenn Probleme (einschließlich Zertifizierungsprobleme) bei Ihrem Angebot auftreten. Diese Informationen werden nicht im Azure Marketplace aufgeführt.

- Name
- Email
- Phone

## <a name="azure-marketplace-media"></a>Azure Marketplace-Medien

Stellen Sie Logos und Bilder zur Verwendung mit Ihrem Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen. Bei unscharfen Bildern wird Ihre Übermittlung abgelehnt.

[!INCLUDE [logo tips](includes/graphics-suggestions.md)]

>[!NOTE]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

### <a name="azure-marketplace-logos"></a>Azure Marketplace-Logos

Geben Sie eine PNG-Datei für das **große** Logo an. Dies wird in Partner Center verwendet, um ein **kleines** und ein **mittleres** Logo zu erstellen. Sie können diese Logos später durch andere Bilder ersetzen.

- **Groß** (von 216 × 216 bis 350 × 350 Pixel, erforderlich)
- **Mittel** (90 × 90 Pixel, optional)
- **Klein** (48 × 48 Pixel, optional)

Diese Logos werden an unterschiedlichen Stellen in der Auflistung verwendet:

[!INCLUDE [logos-azure-marketplace-only](includes/logos-azure-marketplace-only.md)]

[!INCLUDE [Logo tips](includes/graphics-suggestions.md)]

### <a name="screenshots"></a>Screenshots

Fügen Sie bis zu fünf Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Jeder Screenshot muss 1.280 x 720 Pixel groß sein und im PNG-Format vorliegen. Jeder Screenshot muss einen Titel enthalten.

### <a name="videos"></a>Videos

Fügen Sie optional bis zu fünf Videos hinzu, die Ihr Angebot vorstellen. Die Videos sollten bei einem externen Videodienst gehostet werden. Geben Sie den Namen jedes Videos, die Webadresse und ein Miniaturbild des Videos im PNG-Format mit einer Größe von 1280 × 720 Pixel ein.

Weitere Ressourcen für Marketplace-Listen finden Sie unter [Bewährte Methoden für Angebotslistung](gtm-offer-listing-best-practices.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen von Plänen](azure-vm-create-plans.md)

---
title: Konfigurieren von Details eines Power BI-App-Angebotseintrags in Microsoft AppSource (Azure Marketplace)
description: Konfigurieren Sie Details eines Power BI-App-Angebotseintrags in Microsoft AppSource (Azure Marketplace).
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: keferna
ms.author: keferna
ms.date: 05/26/2021
ms.openlocfilehash: bc827cb42246129a507ee0894251b65d5c53a89c
ms.sourcegitcommit: 6323442dbe8effb3cbfc76ffdd6db417eab0cef7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110631579"
---
# <a name="configure-power-bi-app-offer-listing-details"></a>Konfigurieren von Details eines Power BI-App-Angebotseintrags

Über diese Seite können Sie die Angebotsdetails wie Angebotsname, Beschreibung, Links, Kontakte, Logos und Screenshots festlegen, die in Microsoft AppSource angezeigt werden.

> [!NOTE]
> Geben Sie die Details eines Angebotseintrags nur in einer Sprache an. Englisch ist nicht notwendig, sofern die Angebotsbeschreibung mit dem Satz beginnt: „Diese Anwendung ist nur verfügbar in [nicht-englische Sprache].“ Es ist auch zulässig, eine *URL mit einem nützlichen Link* bereitzustellen, um Inhalte in einer anderen Sprache als der im Angebotseintrag verwendeten anzubieten.

Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen in Microsoft AppSource angezeigt werden (die aufgelisteten Preise dienen lediglich als Beispiele und stellen keine tatsächlichen Kosten dar):

:::image type="content" source="media/power-bi/example-power-bi-app.png" alt-text="Veranschaulicht, wie dieses Angebot in Microsoft AppSource angezeigt wird.":::

##### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Logo
1. Produkte
1. Kategorien
1. Branchen
1. Supportadresse (Link)
1. Nutzungsbedingungen
1. Datenschutzrichtlinie
1. Angebotsname
1. Zusammenfassung der Suchergebnisse
1. BESCHREIBUNG
1. Screenshots/Videos

## <a name="marketplace-details"></a>Marketplace-Details

Der hier von Ihnen angegebene **Name** wird den Kunden als Titel des Angebots angezeigt. Dieses Feld ist bereits mit dem Namen ausgefüllt, den Sie bei der Erstellung des Angebots für **Angebotsalias** eingegeben haben; Sie können diesen jedoch ändern. Der Name:

- Kann Marken- und Copyrightsymbole enthalten.
- Darf höchstens 50 Zeichen umfassen.
- Darf keine Emojis enthalten.

Geben Sie eine kurze Beschreibung Ihres Angebots für die **Zusammenfassung der Suchergebnisse** (bis zu 100 Zeichen) an. Diese Beschreibung kann in Marketplace-Suchergebnissen verwendet werden.

[!INCLUDE [Long description-1](includes/long-description-1.md)]

[!INCLUDE [Long description-2](includes/long-description-2.md)]

Verwenden Sie HTML-Tags, um Ihre Beschreibung zu formatieren und ansprechender zu gestalten. Eine Liste der zulässigen Tags finden Sie unter [Unterstützte HTML-Tags](supported-html-tags.md).

#### <a name="helpprivacy-urls"></a>Hilfe-/Datenschutz-URLs

Geben Sie den **Hilfelink für Ihre App** (URL) ein, unter dem Kunden mehr über Ihr Angebot erfahren können. Ihre Hilfe-URL darf nicht mit der Support-URL identisch sein.

Geben Sie den **Link zur Datenschutzrichtlinie** (URL) Ihrer Organisation ein. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.

### <a name="contact-information"></a>Kontaktinformationen

Geben Sie den Namen, die E-Mail-Adresse und die Telefonnummer eines **Supportkontakts** und eines **technischen Ansprechpartners** an. Diese Informationen werden Kunden nicht angezeigt, sind aber für Microsoft verfügbar und können auch an CSP-Partner weitergegeben werden.

Geben Sie im Abschnitt **Supportkontakt** die **Support-URL** an, unter der CSP-Partner Support für Ihr Angebot erhalten. Ihre Support-URL darf nicht mit der Hilfe-URL identisch sein.

## <a name="supporting-documents"></a>Unterstützende Dokumente

Geben Sie hier bis zu drei zugehörige PDF-Marketingdokumente an, z. B. Whitepaper, Broschüren, Checklisten oder Präsentationen (optional).

## <a name="marketplace-media"></a>Marketplace-Medien

Stellen Sie Logos und Bilder zur Verwendung mit Ihrem Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen. Bei unscharfen Bildern wird Ihre Übermittlung abgelehnt.

>[!NOTE]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

### <a name="logos"></a>Logos

Geben Sie eine PNG-Datei für das **große** Logo an. Dies wird in Partner Center verwendet, um die anderen erforderlichen Größen zu erstellen. Später können Sie dieses Logo optional durch ein anderes Bild ersetzen.

Diese Logos werden an unterschiedlichen Stellen in der Auflistung verwendet:

[!INCLUDE [logos-appsource-only](includes/logos-appsource-only.md)]

[!INCLUDE [Logo tips](includes/graphics-suggestions.md)]

### <a name="screenshots"></a>Screenshots

Fügen Sie mindestens einen (und bis zu fünf) Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Alle Screenshots müssen im PNG-Format in der Größe 1.280 × 720 Pixel vorliegen. Fügen Sie jedem Screenshot einen Titel hinzu.

### <a name="videos"></a>Videos

Fügen Sie bis zu vier optionale Videos hinzu, in denen Ihr Angebot vorgestellt wird. Diese sollten auf einem externen Videodienst gehostet werden. Geben Sie den Namen jedes Videos, die Webadresse und ein Miniaturbild des Videos im PNG-Format mit einer Größe von 1280 × 720 Pixel ein.

Weitere Ressourcen für Marketplace-Listen finden Sie unter [Bewährte Methoden für Angebotslistung](gtm-offer-listing-best-practices.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie mit der nächsten Registerkarte im linken Navigationsmenü (**Technische Konfiguration**) fortfahren.

## <a name="next-steps"></a>Nächste Schritte

- [Festlegen der technischen Konfiguration des Angebots](power-bi-app-technical-configuration.md)

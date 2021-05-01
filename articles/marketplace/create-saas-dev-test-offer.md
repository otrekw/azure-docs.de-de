---
title: Erstellen eines Testangebots
description: Erfahren Sie, wie Sie ein separates Entwicklungsangebot zum Testen Ihres Angebots für die Produktion im kommerziellen Marketplace-Programm im Microsoft Partner Center erstellen.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: 893d38d7dcf2ef0910bc46d3e9bfd168c2a89162
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105543362"
---
# <a name="create-a-test-offer"></a>Erstellen eines Testangebots

Um in einer von Ihrem Produktionsangebot getrennten Umgebung zu entwickeln, erstellen Sie ein separates Angebot für Test- und Entwicklung (DEV) und ein separates Produktionsangebot (PROD). Informationen zu den Vorteilen der Verwendung eines separaten DEV-Angebots finden Sie unter [Planen eines SaaS-Angebots](plan-saas-offer.md#test-offer).

Sie konfigurieren die meisten Einstellungen in den DEV- und PROD-Angeboten identisch. Beispielsweise sollten offizielle Marketingsprache und -ressourcen wie Screenshots und Logos identisch sein. In den Fällen, in denen die Konfiguration identisch ist, können Sie Felder aus den Plänen im DEV-Angebot kopieren und in die Pläne im PROD-Angebot einfügen.

In den folgenden Abschnitten werden die Konfigurationsunterschiede zwischen DEV- und PROD-Angeboten beschrieben.

## <a name="offer-setup-page"></a>Seite „Angebotseinrichtung“

Sie sollten im Feld **Alias** beider Angebote denselben Alias verwenden, aber „_test“ an den Alias des DEV-Angebots anfügen. Wenn der Alias Ihres PROD-Angebots z. B. „contososolution“ ist, sollte der Alias des DEV-Angebots „contososolution_test“ lauten. Auf diese Weise können Sie Ihr DEV-Angebot leicht von Ihrem PROD-Angebot unterscheiden.

Verwenden Sie im Abschnitt **Kundenleads** eine Azure-Tabelle oder eine Test-CRM-Umgebung für das DEV-Angebot. Verwenden Sie das vorgesehene Leadverwaltungssystem für das PROD-Angebot.

## <a name="properties-page"></a>Eigenschaftenseite

Konfigurieren Sie diese Seite in DEV- und PROD-Angebot gleich.

## <a name="offer-listing-page"></a>Seite „Angebotsliste“

Konfigurieren Sie diese Seite in DEV- und PROD-Angebot gleich.

## <a name="preview-audience"></a>Vorschauzielgruppe

Fügen Sie im DEV-Angebot die Azure Active Directory-Benutzerprinzipalnamen (AAD) oder Microsoft-Konto-E-Mail-Adressen (MSA) von Entwicklern und Testern sowie sich selbst ein. Der Benutzerprinzipalname eines Benutzers in AAD kann sich von der E-Mail-Adresse des Benutzers unterscheiden. So funktioniert jane.doe@contoso.com beispielsweise nicht, janedoe@contoso.com hingegen schon. Die Benutzer, die Sie festlegen, haben Zugriff auf das DEV-Angebot, wenn Sie den **Vorschau**-Link während der Entwicklungs- und Testphase freigeben.

Fügen Sie im PROD-Angebot den Azure AD-Benutzerprinzipalnamen oder die Microsoft-Konto-E-Mail-Adresse der Benutzer ein, die das Angebot überprüfen, bevor sie die **„Live schalten“- Schaltfläche** auswählen.

## <a name="technical-configuration-page"></a>Seite „Technische Konfiguration“

In dieser Tabelle werden die Unterschiede zwischen den Einstellungen für DEV- und PROD-Angebote beschrieben.

***Tabelle 1: Unterschiede bei der technischen Konfiguration***

| Einstellung | DEV-Angebot | PROD-Angebot |
| ------------ | ------------- | ------------- |
| URL der Angebotsseite | Geben Sie Ihren Dev/Test-Endpunkt ein. | Geben Sie Ihren Produktionsendpunkt ein. |
| Verbindungswebhook | Geben Sie Ihren Dev/Test-Endpunkt ein. | Geben Sie Ihren Produktionsendpunkt ein. |
| Azure Active Directory-Mandanten-ID | Geben Sie Ihre Mandanten-ID für die Test-App-Registrierung (AAD-Verzeichnis-ID) ein. | Geben Sie Ihre Mandanten-ID für die Produktions-App-Registrierung ein. |
| ID der Azure Active Directory-Anwendung | Geben Sie Ihre Anwendungs-ID für die Test-App-Registrierung (Client-ID) ein. | Geben Sie Ihre Anwendungs-ID für die Produktions-App-Registrierung ein. |
||||

## <a name="plan-overview-page"></a>Planübersichtsseite

Wenn Sie Ihre Pläne erstellen, sollten Sie in DEV- und Prod-Angebot dieselbe _Plan-ID_ und denselben _Plannamen_ verwenden, mit der Ausnahme, dass Sie der Plan-ID im DEV-Angebot **_Test** anfügen. Wenn die Plan-ID im PROD-Angebot beispielsweise „enterprise“ lautet, sollte die Plan-ID im DEV-Angebot „enterprise_test“ lauten. Auf diese Weise können Sie Ihr DEV-Angebot leicht von Ihrem PROD-Angebot unterscheiden. Im PROD-Angebot erstellen Sie Pläne mit den Preismodellen und Preisen, die Sie für Ihr Angebot als ideal betrachten.

### <a name="plan-listing"></a>Planlisting

Geben Sie auf der Registerkarte **Planübersicht** > **Planlisten** in DEV- und PROD-Plan dieselbe Planbeschreibung ein.

### <a name="pricing-and-availability-page"></a>Seite „Preise und Verfügbarkeit“

Dieser Abschnitt enthält Anleitungen zur Fertigstellung der Seite **Planübersicht** > **Preise und Verfügbarkeit**.

#### <a name="markets"></a>Märkte

Wählen Sie dieselben Märkte für DEV- und PROD-Angebot aus.

#### <a name="pricing"></a>Preise

Verwenden Sie das DEV-Angebot, um mit Preismodellen zu experimentieren. Nachdem Sie überprüft haben, welches Preismodell oder welche Modelle am besten funktionieren, erstellen Sie im PROD-Angebot die Pläne mit den gewünschten Preismodellen und Preisen.

In den Plänen des DEV-Angebots sollten als Preis null (0) oder niedrige Preise enthalten sein. Das PROD-Angebot enthält die Preise, die Sie Kunden berechnen möchten.

> [!IMPORTANT]
> In der Vorschau vorgenommene Käufe werden sowohl für das DEV- als auch das PROD-Angebot verarbeitet. Wenn ein Angebot einen Monatspreis von 100 USD hat, werden Ihrem Unternehmen 100 USD in Rechnung gestellt. Wenn dies der Fall ist, können Sie ein [Supportticket](support.md) öffnen, und wir zahlen Ihnen den gesamten Betrag aus (und erheben keine Store Service-Gebühr).

#### <a name="pricing-model"></a>Preismodell

Verwenden Sie in den Plänen des DEV- und PROD-Angebots dasselbe Preismodell. Wenn der Plan beispielsweise im PROD-Angebot ein Pauschalpreis mit monatlicher Abrechnung ist, konfigurieren Sie den Plan im DEV-Angebot mit demselben Modell.

Um die Kosten für das Testen der Preismodelle einschließlich der benutzerdefinierten Marketplace-Verbrauchseinheiten zu reduzieren, sollten Sie den **Preis**-Abschnitt auf der Registerkarte **Preise und Verfügbarkeit** im DEV-Angebot mit niedrigeren Preisen konfigurieren als im PROD-Angebot. Hier finden Sie einige Richtlinien, die Sie beim Festlegen der Preise für Pläne im DEV-Angebot befolgen können.

***Tabelle 2: Preisrichtlinien***

| Preis | Kommentar |
| ------------ | ------------- |
| 0,00 € | Legen Sie Gesamttransaktionskosten von 0 (null) fest, die keine finanziellen Auswirkungen haben. Verwenden Sie diesen Preis für Aufrufe von Messungs-APIs oder Testen von Kaufplänen in Ihrem Angebot, während Sie Ihre Lösung entwickeln. |
| 0,01 - 49,99 USD | Verwenden Sie diesen Preisbereich, um Analysen, Berichte und den Kaufvorgang zu testen. |
| 50,00 USD und höher | Verwenden Sie diesen Preisbereich, um die Auszahlung zu testen. Weitere Informationen zum Zahlungsplan finden Sie unter [Auszahlungspläne und -prozesse](/partner-center/payout-policy-details). |
|||

Um zu vermeiden, dass Ihnen bei Ihrem Test eine Store Service-Gebühr in Rechnung gestellt wird, öffnen Sie ein [Support-Ticket](support.md).

#### <a name="free-trial"></a>Kostenlose Testversion

Aktivieren Sie keine kostenlose Testversion für das DEV-Angebot.

## <a name="co-sell-with-microsoft-page"></a>Seite „Co-Selling mit Microsoft“

Konfigurieren Sie die Registerkarte **Co-Selling mit Microsoft** des DEV-Angebots nicht.

## <a name="resell-through-csps"></a>Verkaufen über CSPs

Konfigurieren Sie die Registerkarte **Verkauf über CSPs** des DEV-Angebots nicht.

## <a name="next-steps"></a>Nächste Schritte

- [Testen und Veröffentlichen eines SaaS-Angebots](test-publish-saas-offer.md)

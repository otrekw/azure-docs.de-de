---
title: Seitenlayoutversionen
titleSuffix: Azure AD B2C
description: Versionsgeschichte des Seitenlayouts für die Anpassung der Benutzeroberfläche in benutzerdefinierten Richtlinien
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 08/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 868d99a82009dc8545fc24ad1cfa1da3959da131
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88852070"
---
# <a name="page-layout-versions"></a>Seitenlayoutversionen

Seitenlayoutpakete werden regelmäßig aktualisiert, um Korrekturen und Verbesserungen in ihre Seitenelemente aufzunehmen. Das folgende Änderungsprotokoll gibt die in den einzelnen Versionen eingeführten Änderungen an.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="self-asserted-page-selfasserted"></a>Selbstbestätigte Seite (selbstbestätigt)

**2.1.0**

- Korrekturen für Lokalisierung und Barrierefreiheit.

**2.0.0**

- Unterstützung für [Anzeigesteuerelemente](display-controls.md) in benutzerdefinierten Richtlinien hinzugefügt.

**1.2.0**

- Die Felder für Benutzername/E-Mail-Adresse und Kennwort verwenden jetzt das HTML-Element `form`, damit Microsoft Edge und Internet Explorer (IE) diese Informationen ordnungsgemäß speichern können.
- Konfigurierbare Verzögerung bei Validierung von Benutzereingaben für verbesserte Benutzerfreundlichkeit hinzugefügt.
- Korrekturen zur Barrierefreiheit
- Sie können nun [in den HTML-Tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern.
  - Laden Sie verknüpfte CSS-Dateien zur gleichen Zeit wie Ihre HTML-Vorlage, damit zwischen dem Laden der Dateien kein „Flackern“ auftritt.
  - Sie steuern damit die Reihenfolge, in der die `script`-Tags vor dem Laden der Seite abgerufen und ausgeführt werden.
- Als E-Mail-Feld wird jetzt `type=email` verwendet, und bei Tastaturen von Mobilgeräten werden jetzt die richtigen Vorschläge angezeigt
- Unterstützung für den Google Chrome-Übersetzer

**1.1.0**

- Abbruchwarnung wurde entfernt.
- CSS-Klasse für Fehlerelemente
- Anzeigen/Ausblenden der Fehlerlogik wurde verbessert.
- Standard-CSS wurde entfernt.

**1.0.0**

- Erste Veröffentlichung

## <a name="unified-sign-in-sign-up-page-with-password-reset-link-unifiedssp"></a>Vereinheitlichte Anmeldungs- und Registrierungsseite mit Link zur Kennwortzurücksetzung (unifiedssp)

**2.1.0**

- Unterstützung für mehrere Registrierungslinks hinzugefügt.
- Unterstützung für die Validierung von Benutzereingaben gemäß den in der Richtlinie definierten Prädikatsregeln hinzugefügt.

**1.2.0**

- Die Felder für Benutzername/E-Mail-Adresse und Kennwort verwenden jetzt das HTML-Element `form`, damit Microsoft Edge und Internet Explorer (IE) diese Informationen ordnungsgemäß speichern können.
- Korrekturen zur Barrierefreiheit
- Sie können nun [in den HTML-Tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern.
  - Laden Sie verknüpfte CSS-Dateien zur gleichen Zeit wie Ihre HTML-Vorlage, damit zwischen dem Laden der Dateien kein „Flackern“ auftritt.
  - Sie steuern damit die Reihenfolge, in der die `script`-Tags vor dem Laden der Seite abgerufen und ausgeführt werden.
- Als E-Mail-Feld wird jetzt `type=email` verwendet, und bei Tastaturen von Mobilgeräten werden jetzt die richtigen Vorschläge angezeigt
- Unterstützung für den Google Chrome-Übersetzer

**1.1.0**

- „Angemeldet bleiben“-Steuerelement (Keep Me Signed In, KMSI) wurde hinzugefügt.

**1.0.0**

- Erste Veröffentlichung

## <a name="mfa-page-multifactor"></a>MFA-Seite (Multi-Factor)

**1.2.1**

- Korrekturen zur Barrierefreiheit in Standardvorlagen

**1.2.0**

- Korrekturen zur Barrierefreiheit
- Sie können nun [in den HTML-Tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern.
  - Laden Sie verknüpfte CSS-Dateien zur gleichen Zeit wie Ihre HTML-Vorlage, damit zwischen dem Laden der Dateien kein „Flackern“ auftritt.
  - Sie steuern damit die Reihenfolge, in der die `script`-Tags vor dem Laden der Seite abgerufen und ausgeführt werden.
- Als E-Mail-Feld wird jetzt `type=email` verwendet, und bei Tastaturen von Mobilgeräten werden jetzt die richtigen Vorschläge angezeigt
- Unterstützung für den Google Chrome-Übersetzer

**1.1.0**

- Schaltfläche zur Codebestätigung wurde entfernt.
- Das Codeeingabefeld nimmt jetzt nur noch Eingaben mit bis zu sechs (6) Zeichen entgegen.
- Die Seite versucht automatisch, den eingegebenen Code zu überprüfen, wenn ein 6-stelliger Code eingegeben wird, ohne dass auf eine Schaltfläche geklickt werden muss.
- Wenn der Code falsch ist, wird das Eingabefeld automatisch gelöscht.
- Nach drei (3) Versuchen mit einem falschen Code sendet B2C eine Fehlermeldung an die vertrauende Seite.
- Korrekturen zur Barrierefreiheit
- Standard-CSS wurde entfernt.

**1.0.0**

- Erste Veröffentlichung

## <a name="exception-page-globalexception"></a>Seite mit Ausnahmen (globalexception)

**1.2.0**

- Korrekturen zur Barrierefreiheit
- Sie können nun [in den HTML-Tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern.
  - Laden Sie verknüpfte CSS-Dateien zur gleichen Zeit wie Ihre HTML-Vorlage, damit zwischen dem Laden der Dateien kein „Flackern“ auftritt.
  - Sie steuern damit die Reihenfolge, in der die `script`-Tags vor dem Laden der Seite abgerufen und ausgeführt werden.
- Als E-Mail-Feld wird jetzt `type=email` verwendet, und bei Tastaturen von Mobilgeräten werden jetzt die richtigen Vorschläge angezeigt
- Unterstützung für den Google Chrome-Übersetzer

**1.1.0**

- Korrektur zur Barrierefreiheit
- Standardmeldung bei fehlendem Kontakt wurde aus der Richtlinie entfernt.
- Standard-CSS wurde entfernt.

**1.0.0**

- Erste Veröffentlichung

## <a name="other-pages-providerselection-claimsconsent-unifiedssd"></a>Andere Seiten (ProviderSelection, ClaimsConsent, UnifiedSSD)

**1.2.0**

- Korrekturen zur Barrierefreiheit
- Sie können nun [in den HTML-Tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern.
  - Laden Sie verknüpfte CSS-Dateien zur gleichen Zeit wie Ihre HTML-Vorlage, damit zwischen dem Laden der Dateien kein „Flackern“ auftritt.
  - Sie steuern damit die Reihenfolge, in der die `script`-Tags vor dem Laden der Seite abgerufen und ausgeführt werden.
- Als E-Mail-Feld wird jetzt `type=email` verwendet, und bei Tastaturen von Mobilgeräten werden jetzt die richtigen Vorschläge angezeigt
- Unterstützung für den Google Chrome-Übersetzer

**1.0.0**

- Erste Veröffentlichung

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie die Benutzeroberfläche Ihrer Anwendungen in benutzerdefinierten Richtlinien anpassen können, finden Sie unter [Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie](custom-policy-ui-customization.md).

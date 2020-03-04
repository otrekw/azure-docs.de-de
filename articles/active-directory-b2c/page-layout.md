---
title: Seitenlayoutversionen
titleSuffix: Azure AD B2C
description: Versionsgeschichte des Seitenlayouts für die Anpassung der Benutzeroberfläche in benutzerdefinierten Richtlinien
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/26/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: b24e8cc5f6bb6213bf021362c5654afd4d5266fb
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77620613"
---
# <a name="page-layout-versions"></a>Seitenlayoutversionen

Seitenlayoutpakete werden regelmäßig aktualisiert, um Korrekturen und Verbesserungen in ihre Seitenelemente aufzunehmen. Das folgende Änderungsprotokoll gibt die in den einzelnen Versionen eingeführten Änderungen an.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="200"></a>2.0.0

- Selbstbestätigte Seite (`selfasserted`)
  - Unterstützung für [Anzeigesteuerelemente](display-controls.md) in benutzerdefinierten Richtlinien hinzugefügt.

## <a name="120"></a>1.2.0

- Alle Seiten
  - Korrekturen zur Barrierefreiheit
  - Sie können nun [in den HTML-Tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern.
    - Laden Sie verknüpfte CSS-Dateien zur gleichen Zeit wie Ihre HTML-Vorlage, damit zwischen dem Laden der Dateien kein „Flackern“ auftritt.
    - Sie steuern damit die Reihenfolge, in der die `script`-Tags vor dem Laden der Seite abgerufen und ausgeführt werden.
  - Als E-Mail-Feld wird jetzt `type=email` verwendet, und bei Tastaturen von Mobilgeräten werden jetzt die richtigen Vorschläge angezeigt
  - Unterstützung für den Google Chrome-Übersetzer
- Einheitliche und selbstbestätigte Seiten
  - Die Felder für Benutzername/E-Mail-Adresse und Kennwort verwenden jetzt das HTML-Element `form`, damit Microsoft Edge und Internet Explorer (IE) diese Informationen ordnungsgemäß speichern können.

## <a name="110"></a>1.1.0

- Seite mit Ausnahmen (globalexception)
  - Korrektur zur Barrierefreiheit
  - Standardmeldung bei fehlendem Kontakt wurde aus der Richtlinie entfernt.
  - Standard-CSS wurde entfernt.
- MFA-Seite (Multi-Factor)
  - Schaltfläche zur Codebestätigung wurde entfernt.
  - Das Codeeingabefeld nimmt jetzt nur noch Eingaben mit bis zu sechs (6) Zeichen entgegen.
  - Die Seite versucht automatisch, den eingegebenen Code zu überprüfen, wenn ein 6-stelliger Code eingegeben wird, ohne dass auf eine Schaltfläche geklickt werden muss.
  - Wenn der Code falsch ist, wird das Eingabefeld automatisch gelöscht.
  - Nach drei (3) Versuchen mit einem falschen Code sendet B2C eine Fehlermeldung an die vertrauende Seite.
  - Korrekturen zur Barrierefreiheit
  - Standard-CSS wurde entfernt.
- Selbstbestätigte Seite (selbstbestätigt)
  - Abbruchwarnung wurde entfernt.
  - CSS-Klasse für Fehlerelemente
  - Anzeigen/Ausblenden der Fehlerlogik wurde verbessert.
  - Standard-CSS wurde entfernt.
- SSP vereinheitlicht (unifiedssp)
  - „Angemeldet bleiben“-Steuerelement (Keep Me Signed In, KMSI) wurde hinzugefügt.

## <a name="100"></a>1.0.0

- Erste Veröffentlichung

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie die Benutzeroberfläche Ihrer Anwendungen in benutzerdefinierten Richtlinien anpassen können, finden Sie unter [Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie](custom-policy-ui-customization.md).

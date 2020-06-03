---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: 587a4bfed00415499bcca6d6054d4ab25cddf0b8
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84298918"
---
#### <a name="app-registrations-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Wählen Sie unter **Verwalten** die Option **API-Berechtigungen**.
1. Wählen Sie unter **Konfigurierte Berechtigungen** die Option **Berechtigung hinzufügen** aus.
1. Wählen Sie die Registerkarte **Microsoft-APIs** und dann **Microsoft Graph** aus.
1. Wählen Sie **Anwendungsberechtigungen**.
1. Erweitern Sie die entsprechende Berechtigungsgruppe, und aktivieren Sie das Kontrollkästchen neben der Berechtigung, die Sie Ihrer Verwaltungsanwendung gewähren möchten. Beispiel:
    * **AuditLog** > **AuditLog.Read.All**: Zum Lesen der Überwachungsprotokolle des Verzeichnisses.
    * **Directory** > **Directory.ReadWrite.All**: Für Benutzermigrations- oder Benutzerverwaltungsszenarien.
    * **Policy** > **Policy.ReadWrite.TrustFramework**: Für CI-/CD-Szenarien (Continuous Integration/Continuous Delivery). Zum Beispiel für die Bereitstellung benutzerdefinierter Richtlinien mit Azure Pipelines.
1. Wählen Sie **Berechtigungen hinzufügen** aus. Warten Sie einige Minuten, bevor Sie mit dem nächsten Schritt fortfahren.
1. Wählen Sie **Administratorzustimmung für (Name Ihres Mandanten) erteilen** aus.
1. Wählen Sie das derzeit angemeldete Administratorkonto aus, oder melden Sie sich mit einem Konto bei Ihrem Azure AD B2C-Mandanten an, dem mindestens die Rolle *Cloudanwendungsadministrator* zugewiesen wurde.
1. Wählen Sie **Akzeptieren** aus.
1. Wählen Sie **Aktualisieren** aus, und überprüfen Sie dann, ob „Gewährt für...“ unter **Status** angezeigt wird. Es kann einige Minuten dauern, bis die Berechtigungen weitergegeben wurden.

#### <a name="applications"></a>[Anwendungen](#tab/applications/)

1. Wählen Sie auf der Übersichtsseite **Registrierte App** die Option **Einstellungen** aus.
1. Wählen Sie unter **API-Zugriff** die Option **Erforderliche Berechtigungen** aus.
1. Wählen Sie **Microsoft Graph**.
1. Aktivieren Sie unter **Anwendungsberechtigungen** das Kontrollkästchen neben der Berechtigung, die Sie Ihrer Verwaltungsanwendung gewähren möchten. Beispiel:
    * **Alle Überwachungsprotokolldaten lesen**: Wählen Sie diese Berechtigung zum Lesen der Überwachungsprotokolle des Verzeichnisses aus.
    * **Verzeichnisdaten lesen und schreiben**: Wählen Sie diese Berechtigung für Benutzermigrations- oder Benutzerverwaltungsszenarien aus.
    * **Richtlinien des Vertrauensframeworks Ihrer Organisation lesen und schreiben**: Wählen Sie diese Berechtigung für CI-/CD-Szenarien (Continuous Integration/Continuous Delivery) aus. Zum Beispiel für die Bereitstellung benutzerdefinierter Richtlinien mit Azure Pipelines.
1. Wählen Sie **Speichern** aus.
1. Wählen Sie **Berechtigungen erteilen** und dann **Ja** aus. Es kann einige Minuten dauern, bis die Berechtigungen vollständig verteilt sind.

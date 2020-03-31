---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: bf8b9370abc58902d9c751505f633dee852210a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184345"
---
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

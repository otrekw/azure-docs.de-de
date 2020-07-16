---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: f8c972bdb9195008c2983d3993e8d9369749b284
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85200174"
---
#### <a name="app-registrations"></a>[App-Registrierungen](#tab/app-reg-ga/) 

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
1. Wenn Sie derzeit nicht als globaler Administrator angemeldet sind, melden Sie sich mit einem Konto bei Ihrem Azure AD B2C-Mandanten an, dem mindestens die Rolle *Cloudanwendungsadministrator* zugewiesen wurde, und wählen Sie **Administratorzustimmung für (Name Ihres Mandanten) erteilen** aus.
1. Wählen Sie **Aktualisieren** aus, und überprüfen Sie dann, ob „Gewährt für...“ unter **Status** angezeigt wird. Es kann einige Minuten dauern, bis die Berechtigungen weitergegeben wurden.

#### <a name="applications-legacy"></a>[Anwendungen (Legacy)](#tab/applications-legacy/)

1. Wählen Sie auf der Übersichtsseite **Registrierte App** die Option **Einstellungen** aus.
1. Wählen Sie unter **API-Zugriff** die Option **Erforderliche Berechtigungen** aus.
1. Wählen Sie **Microsoft Graph**.
1. Aktivieren Sie unter **Anwendungsberechtigungen** das Kontrollkästchen neben der Berechtigung, die Sie Ihrer Verwaltungsanwendung gewähren möchten. Beispiel:
    * **Alle Überwachungsprotokolldaten lesen**: Wählen Sie diese Berechtigung zum Lesen der Überwachungsprotokolle des Verzeichnisses aus.
    * **Verzeichnisdaten lesen und schreiben**: Wählen Sie diese Berechtigung für Benutzermigrations- oder Benutzerverwaltungsszenarien aus.
    * **Richtlinien des Vertrauensframeworks Ihrer Organisation lesen und schreiben**: Wählen Sie diese Berechtigung für CI-/CD-Szenarien (Continuous Integration/Continuous Delivery) aus. Zum Beispiel für die Bereitstellung benutzerdefinierter Richtlinien mit Azure Pipelines.
1. Wählen Sie **Speichern** aus.
1. Wählen Sie **Berechtigungen erteilen** und dann **Ja** aus. Es kann einige Minuten dauern, bis die Berechtigungen vollständig verteilt sind.

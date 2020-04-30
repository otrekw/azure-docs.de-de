---
title: 'Bedingter Zugriff: Kombinierte Sicherheitsinformationen – Azure Active Directory'
description: Erstellen einer benutzerdefinierten Richtlinie für bedingten Zugriff zur Registrierung von Sicherheitsinformationen
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c8081bb8145a6654c168fb2d664e1666b32dc18
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457908"
---
# <a name="conditional-access-securing-security-info-registration"></a>Bedingter Zugriff: Sichere Registrierung von Sicherheitsinformationen

Mit Benutzeraktionen in der Richtlinie für bedingten Zugriff kann jetzt sichergestellt werden, wann und wie sich Benutzer für Azure Multi-Factor Authentication registrieren, und auch die Self-Service-Kennwortzurücksetzung ist möglich. Diese Previewfunktion ist für Organisationen verfügbar, die die [kombinierte Registrierung (Vorschauversion)](../authentication/concept-registration-mfa-sspr-combined.md) aktiviert haben. Diese Funktionalität kann in Organisationen aktiviert werden, bei denen Bedingungen, z. B. ein vertrauenswürdiger Netzwerkstandort, verwendet werden sollen, um den Zugriff auf die Registrierung für Azure Multi-Factor Authentication und die Self-Service-Kennwortzurücksetzung (SSPR) zu beschränken. Weitere Informationen zu anwendbaren Bedingungen finden Sie im Artikel [Bedingter Zugriff: Bedingungen](concept-conditional-access-conditions.md).

## <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Erstellen einer Richtlinie zum Erzwingen der Registrierung von einem vertrauenswürdigen Standort

Die folgende Richtlinie gilt für alle ausgewählten Benutzer, die versuchen, sich über die Benutzeroberfläche für die kombinierte Registrierung zu registrieren. Der Zugriff wird blockiert, sofern die Verbindung nicht von einem Standort aus hergestellt wird, der als ein vertrauenswürdiges Netzwerk gekennzeichnet ist.

1. Navigieren Sie im **Azure-Portal** zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Geben Sie unter „Name“ einen Namen für diese Richtlinie ein. Beispiel: **Kombinierte Registrierung mit Sicherheitsinformationen in vertrauenswürdigen Netzwerken**.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** und dann die Benutzer und Gruppen aus, auf die Sie diese Richtlinie anwenden möchten.

   > [!WARNING]
   > Benutzer müssen für die [kombinierte Registrierung](../authentication/howto-registration-mfa-sspr-combined.md) aktiviert sein.

1. Wählen Sie unter **Cloud-Apps oder -aktionen** die Option **Benutzeraktionen** aus, und aktivieren Sie **Sicherheitsinformationen registrieren**.
1. Unter **Bedingungen** > **Standorte**:
   1. Konfigurieren Sie **Ja**.
   1. Schließen Sie **Alle Standorte** ein.
   1. Schließen Sie **Alle vertrauenswürdigen Standorte** aus.
   1. Wählen Sie auf dem Blatt „Standorte“ die Option **Fertig** aus.
   1. Wählen Sie auf dem Blatt „Bedingungen“ die Option **Fertig** aus.
1. Legen Sie unter **Bedingungen** > **Client-Apps (Vorschau)** die Option **Konfigurieren** auf **Ja** fest, und wählen Sie **Fertig** aus.
1. Unter **Zugriffssteuerungen** > **Erteilen**:
   1. Wählen Sie **Zugriff blockieren** aus.
   1. Klicken Sie dann auf **Auswählen**.
1. Legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Klicken Sie dann auf **Speichern**.

In Schritt 6 dieser Richtlinie können Organisationen unterschiedliche Auswahlmöglichkeiten festlegen. In der Richtlinie oben muss die Registrierung über einen vertrauenswürdigen Netzwerkstandort erfolgen. Organisationen können anstelle von **Standorte** beliebige verfügbare Bedingungen verwenden. Beachten Sie, dass diese Richtlinie eine Sperrrichtlinie ist, d. h., alle enthaltenen Elemente sind gesperrt, und alle nicht mit den enthaltenen Elementen übereinstimmenden Elemente sind zulässig. 

Einige Organisationen wählen möglicherweise anstelle des in Schritt 6 oben ausgewählten Standorts den Gerätestatus aus:

6. Gehen Sie unter **Bedingungen** > **Gerätestatus (Vorschau)** wie folgt vor.
   1. Konfigurieren Sie **Ja**.
   1. Schließen Sie **Alle Gerätezustände** ein.
   1. Schließen Sie **Gerät in Hybrid-Azure AD eingebunden** oder **Gerät als konform markiert** aus.
   1. Wählen Sie auf dem Blatt „Standorte“ die Option **Fertig** aus.
   1. Wählen Sie auf dem Blatt „Bedingungen“ die Option **Fertig** aus.

> [!WARNING]
> Wenn Sie den Gerätestatus als Bedingung in Ihrer Richtlinie verwenden, kann dies Auswirkungen auf Gastbenutzer im Verzeichnis haben. Die Auswirkungen von Richtlinienoptionen können im [reinen Berichtsmodus](concept-conditional-access-report-only.md) ermittelt werden.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-report-only.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

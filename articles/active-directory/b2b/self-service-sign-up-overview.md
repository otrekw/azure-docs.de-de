---
title: 'Self-Service-Registrierung für externe Identitäten: Azure AD'
description: Hier erfahren Sie, wie Sie es durch Aktivierung der Self-Service-Registrierung externen Benutzern ermöglichen, sich selbst für Ihre Anwendungen zu registrieren. Erstellen Sie eine personalisierte Registrierungsumgebung, indem Sie den Benutzerflow für die Self-Service-Registrierung anpassen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e0325b43b6726f04d5994b60404f218ac58122d
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594829"
---
# <a name="self-service-sign-up-preview"></a>Self-Service-Registrierung (Vorschauversion)
|     |
| --- |
| Die Self-Service-Registrierung ist eine öffentliche Previewfunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Wenn Sie Anwendungen für externe Benutzer freigeben, wissen Sie möglicherweise nicht immer im Voraus, wer Zugriff auf eine Anwendung benötigen wird. Statt Einladungen direkt an Einzelpersonen zu senden, können Sie es externen Benutzern ermöglichen, sich selbst für bestimmte Anwendungen zu registrieren, indem Sie die Self-Service-Registrierung aktivieren. Sie können eine personalisierte Registrierungsumgebung erstellen, indem Sie den Benutzerflow für die Self-Service-Registrierung anpassen. Beispielsweise können Sie Optionen für Azure AD oder soziale Netzwerke als Identitätsanbieter bereitstellen und Informationen über den Benutzer sammeln.

## <a name="user-flow-for-self-service-sign-up"></a>Benutzerflow für die Self-Service-Registrierung

Ein Benutzerflow für die Self-Service-Registrierung generiert über die freizugebende Anwendung eine Registrierungsumgebung für die externen Benutzer. Der Benutzerflow kann mehreren Anwendungen zugeordnet werden. Zunächst aktivieren Sie die Self-Service-Registrierung für Ihren Mandanten und richten einen Verbund mit allen Identitätsanbietern ein, die externe Benutzer für die Anmeldung verwenden dürfen. Anschließend erstellen Sie den Benutzerflow für die Registrierung, passen ihn an und weisen ihm Ihre Anwendungen zu.
Sie können Benutzerfloweinstellungen konfigurieren, um zu steuern, wie sich Benutzer für die Anwendung registriert:

- Für die Anmeldung verwendete Kontotypen (z. B. Konten für soziale Netzwerke wie Facebook) oder Azure AD-Konten
- Attribute, die vom Benutzer, der sich registriert, erfasst werden, z. B. Vorname, Postleitzahl oder Land des Wohnsitzes

Wenn sich ein Benutzer bei Ihrer Anwendung anmelden möchte, bei der es sich gleichermaßen um eine Web-, Mobil-, Desktop- oder Single-Page-Webanwendung (SPA) handeln kann, initiiert die Anwendung eine Autorisierungsanforderung an den vom Benutzerflow bereitgestellten Endpunkt. Der Benutzerflow definiert und steuert die Funktionalität für die Benutzer. Wenn Benutzer einen Benutzerflow für die Registrierung ausführen, generiert Azure AD ein Token und leitet sie dann wieder zurück zur Anwendung um. Mehrere Anwendungen können denselben Benutzerflow verwenden.

## <a name="example-of-self-service-sign-up"></a>Beispiel für die Self-Service-Registrierung

Im folgenden Beispiel wird veranschaulicht, wie Sie soziale Netzwerke als Identitätsanbieter in Azure AD mit Self-Service-Registrierungsfunktionen für Gastbenutzer einbinden.  
Ein Partner von Woodgrove öffnet die Woodgrove-App. Er möchte sich für ein Lieferantenkonto registrieren und wählt daher die Option „Request your supplier account“ (Lieferantenkonto anfordern) aus. Dadurch wird der Flow für die Self-Service-Registrierung initiiert.

![Beispiel einer Startseite für die Self-Service-Registrierung](media/self-service-sign-up-overview/example-start-sign-up-flow.png)

Der Benutzer verwendet für die Registrierung eine E-Mail-Adresse seiner Wahl.

![Beispiel für die Auswahl von Facebook für die Anmeldung](media/self-service-sign-up-overview/example-sign-in-with-facebook.png)

Azure AD erstellt mithilfe des Facebook-Kontos des Partners eine Verknüpfung mit Woodgrove und erstellt ein neues Konto.

Woodgrove möchte mehr über den Benutzer erfahren, etwa Name, Unternehmensname, Firmenregistrierungscode und Telefonnummer.

![Beispiel für die Benutzerregistrierungsattribute](media/self-service-sign-up-overview/example-enter-user-attributes.png)

Der Benutzer gibt die Informationen ein, setzt den Registrierungsflow fort und erhält Zugriff auf die benötigten Ressourcen.

![Beispiel für den angemeldeten Benutzer](media/self-service-sign-up-overview/example-signed-in.png)

## <a name="next-steps"></a>Nächste Schritte

 Lesen Sie die ausführlichen Informationen zum [Hinzufügen der Self-Service-Registrierung zu einer App](self-service-sign-up-user-flow.md).
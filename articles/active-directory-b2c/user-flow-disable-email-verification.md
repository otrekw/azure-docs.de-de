---
title: Deaktivieren der E-Mail-Überprüfung während der Kundenregistrierung
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie die E-Mail-Überprüfung bei der Benutzerregistrierung in Azure Active Directory B2C deaktivieren.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 10613bd2d6219272248f882e45ae1c33d2cc9d61
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85384208"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>Deaktivieren der E-Mail-Überprüfung bei der Benutzerregistrierung in Azure Active Directory B2C

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

Führen Sie die folgenden Schritte aus, um die E-Mail-Überprüfung zu deaktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Wählen Sie im Hauptmenü über den Filter **Verzeichnis + Abonnement** das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **Benutzerflows** aus.
1. Wählen Sie den Benutzerflow aus, dessen E-Mail-Überprüfung deaktiviert werden soll, beispielsweise *B2C_1_signinsignup*.
1. Wählen Sie **Seitenlayouts** aus.
1. Wählen Sie **Registrierungsseite für lokales Konto** aus.
1. Wählen Sie unter **Benutzerattribute** die Option **E-Mail-Adresse** aus.
1. Wählen Sie im Dropdownmenü **VERIFIZIERUNG ERFORDERLICH** die Option **Nein** aus.
1. Wählen Sie **Speichern** aus. Die E-Mail-Überprüfung ist jetzt für diesen Benutzerflow deaktiviert.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Anpassen der Benutzeroberfläche in Azure Active Directory B2C](customize-ui-overview.md).


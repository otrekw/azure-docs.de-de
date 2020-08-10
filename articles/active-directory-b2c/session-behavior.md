---
title: Konfigurieren des Sitzungsverhaltens – Azure Active Directory B2C | Microsoft-Dokumentation
description: Konfigurieren des Sitzungsverhaltens in Azure Active Directory B2C
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c47b0249824a3683b8f24cc01cc5eea5ccf32585
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87482667"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Konfigurieren des Sitzungsverhaltens in Azure Active Directory B2C

Mithilfe der [Sitzungsverwaltung für einmaliges Anmelden](session-overview.md) (Single Sign-On, SSO) in Azure Active Directory B2C (Azure AD B2C) kann ein Administrator die Interaktion mit einem Benutzer steuern, nachdem dieser bereits authentifiziert wurde. Der Administrator kann z. B. steuern, ob die Auswahl von Identitätsanbietern angezeigt wird oder ob Kontodetails erneut eingegeben werden müssen. In diesem Artikel wird das Konfigurieren der Einstellungen für einmaliges Anmelden für Azure AD B2C beschrieben.

## <a name="session-behavior-properties"></a>Eigenschaften des Sitzungsverhaltens

Sie können die folgenden Eigenschaften zum Verwalten von Webanwendungssitzungen verwenden:

- **Lebensdauer der Web-App-Sitzung (Minuten)** : Die Gültigkeitsdauer von Azure AD B2C-Sitzungscookies, die nach erfolgreicher Authentifizierung des Benutzers im Browser gespeichert werden
    - Standardwert: 1.440 Minuten.
    - Minimum (inklusive): 15 Minuten.
    - Maximum (inklusive): 1.440 Minuten.
- **Timeout für Web-App-Sitzung**: [Sitzungsablauftyp](session-overview.md#session-expiry-type), *Parallel* oder *Absolut*. 
- **SSO-Konfiguration**: [Sitzungsbereich](session-overview.md#session-scope) des SSO-Verhaltens über verschiedene Apps und Benutzerflows in Ihrem Azure AD B2C-Mandanten hinweg.


## <a name="configure-the-properties"></a>Konfigurieren der Eigenschaften

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü den Filter **Verzeichnis und Abonnement** und dann das Verzeichnis auswählen, das Ihren Azure AD B2C-Mandanten enthält.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Benutzerflows** aus.
5. Öffnen Sie den Benutzerflow, den Sie zuvor erstellt haben.
6. Wählen Sie **Eigenschaften** aus.
7. Konfigurieren Sie **Lebensdauer der Web-App-Sitzung (Minuten)** , **Timeout für Web-App-Sitzung**, **Konfiguration des einmaligen Anmeldens** und **ID-Token in Abmeldeanforderungen erforderlich** nach Bedarf.

    ![Eigenschafteneinstellungen für das Sitzungsverhalten im Azure-Portal](./media/session-behavior/session-behavior.png)

8. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Azure AD B2C-Sitzung](session-overview.md).
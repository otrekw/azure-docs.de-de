---
title: Voraussetzungen für den programmgesteuerten Zugriff auf Analysedaten
description: Informieren Sie sich über die Anforderungen, die Sie erfüllen müssen, bevor Sie programmgesteuert auf Analysedaten für den kommerziellen Marketplace zugreifen können.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: b61608c0cb53ab808c5d3d789ec5ddc318c6923d
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107106801"
---
# <a name="prerequisites-to-programmatically-access-analytics-data"></a>Voraussetzungen für den programmgesteuerten Zugriff auf Analysedaten

Bevor Sie programmgesteuert auf Analysedaten für den kommerziellen Marketplace zugreifen können, müssen Sie die folgenden Anforderungen erfüllen.

## <a name="commercial-marketplace-enrollment"></a>Registrierung beim kommerziellen Marketplace

Um programmgesteuert auf Analysedaten für den kommerziellen Marketplace zugreifen zu können, müssen Sie im Programm „Kommerzieller Marketplace“ registriert sein und über ein Partner Center-Konto verfügen. Informationen dazu finden Sie unter [Erstellen eines Kontos im kommerziellen Marketplace in Partner Center](create-account.md).

## <a name="create-azure-active-directory-application"></a>Erstellen der Azure Active Directory-Anwendung

Für den programmgesteuerten Zugriff auf Analysedaten für den kommerziellen Marketplace können keine normalen Benutzeranmeldeinformationen verwendet werden. Es muss eine Azure AD-Anwendung (Azure Active Directory) zusammen mit einem geheimen Schlüssel für den Zugriff auf die Analyse-APIs erstellt werden. Informationen zum Erstellen einer Azure AD-Anwendung und eines geheimen Schlüssels finden Sie unter [Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform](../active-directory/develop/quickstart-register-app.md).

## <a name="associate-the-azure-ad-application-to-the-partner-center-tenant"></a>Zuordnen der Azure AD-Anwendung zum Partner Center-Mandanten

Die Azure AD-Anwendung, die Sie im Azure-Portal erstellt haben, muss mit Ihrem Partner Center-Konto verknüpft werden. Die Schritte lauten wie folgt:

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard) an.
1. Wählen Sie in der rechten oberen Ecke das Zahnradsymbol und dann **Kontoeinstellungen** aus.
1. Wählen Sie im Menü **Kontoeinstellungen** die Option **Benutzerverwaltung** aus.
1. Wählen Sie **Azure AD-Anwendungen** und dann **+ Azure AD-Anwendung erstellen** aus.
1. Wählen Sie die Azure AD-Anwendung aus, die Sie im Azure-Portal erstellt haben, und klicken Sie auf **Weiter**.
1. Aktivieren Sie das Kontrollkästchen **Manager (Windows)** , und wählen Sie dann **Hinzufügen** aus.

    :::image type="content" source="./media/analytics-programmatic-access/azure-ad-roles.png" alt-text="Abbildung der Seite „Azure AD-Anwendung erstellen“ mit den Kontrollkästchen zum Auswählen von Rollen":::

## <a name="generate-an-azure-ad-token"></a>Generieren eines Azure AD-Tokens

Sie müssen ein Azure AD-Token mithilfe der Anwendungs-ID (Client-ID) generieren. Mit dieser ID können Sie Ihre Clientanwendung in Microsoft Identity Platform und den geheimen Clientschlüssel aus dem vorherigen Schritt eindeutig identifizieren. Die Schritte zum Generieren eines Azure AD-Tokens finden Sie unter [Dienst-zu-Dienst-Aufrufe mit Clientanmeldeinformationen (freigegebenes Geheimnis oder Zertifikat)](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md).

> [!NOTE]
> Das Token ist für eine Stunde gültig.

## <a name="next-steps"></a>Nächste Schritte

- [Paradigma für den programmgesteuerten Zugriff](analytics-programmatic-access.md)
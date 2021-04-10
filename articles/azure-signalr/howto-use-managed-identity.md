---
title: Verwaltete Identitäten in Azure SignalR Service
description: Erfahren Sie, wie verwaltete Identitäten in Azure SignalR Service funktionieren und wie verwaltete Identitäten in serverlosen Szenarien verwendet werden können.
author: chenyl
ms.service: signalr
ms.topic: article
ms.date: 06/8/2020
ms.author: chenyl
ms.openlocfilehash: dee15977318eda7bcd0b1950286bb33f621221dd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98731583"
---
# <a name="managed-identities-for-azure-signalr-service"></a>Verwaltete Identitäten für Azure SignalR Service

In diesem Artikel erfahren Sie, wie Sie eine verwaltete Identität für den Azure SignalR Service erstellen, und wie Sie diese in serverlosen Szenarien verwenden können.

> [!Important] 
> Azure SignalR Service kann nur eine verwaltete Identität unterstützen. Das bedeutet, dass Sie entweder eine systemseitig oder benutzerseitig zugewiesene Identität hinzufügen können. 

## <a name="add-a-system-assigned-identity"></a>Hinzufügen einer systemseitig zugewiesenen Identität

Erstellen Sie zum Einrichten einer verwalteten Identität im Azure-Portal zunächst eine Azure SignalR Service-Instanz, und aktivieren Sie dann das Feature.

1. Erstellen Sie wie gewohnt eine Azure-SignalR-Service-Instanz im Portal. Navigieren Sie im Portal zu dieser Instanz.

2. Wählen Sie **Identität** aus.

4. Ändern Sie auf der Registerkarte **Systemseitig zugewiesen** den **Status** in **Ein**. Wählen Sie **Speichern** aus.

    :::image type="content" source="media/signalr-howto-use-managed-identity/system-identity-portal.png" alt-text="Hinzufügen einer systemseitig zugewiesenen Identität im Portal":::

## <a name="add-a-user-assigned-identity"></a>Hinzufügen einer benutzerseitig zugewiesenen Identität

Für das Erstellen einer Azure SignalR Service-Instanz mit einer benutzerseitig zugewiesenen Identität müssen Sie die Identität erstellen und dann den Ressourcenbezeichner der Identität zu Ihrem Dienst hinzufügen.

1. Folgen Sie zum Erstellen einer benutzerseitig verwalteten Identitätsressource [diesen Anweisungen](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

2. Erstellen Sie wie gewohnt eine Azure-SignalR-Service-Instanz im Portal. Navigieren Sie im Portal zu dieser Instanz.

3. Wählen Sie **Identität** aus.

4. Wählen Sie auf der Registerkarte **Benutzerseitig zugewiesen** die Option **Hinzufügen** aus.

5. Suchen Sie nach der zuvor erstellten Identität, und wählen Sie sie aus. Wählen Sie **Hinzufügen**.

    :::image type="content" source="media/signalr-howto-use-managed-identity/user-identity-portal.png" alt-text="Hinzufügen einer benutzerseitig zugewiesenen Identität im Portal":::

## <a name="use-a-managed-identity-in-serverless-scenarios"></a>Verwenden einer verwalteten Identität in serverlosen Szenarien

Azure SignalR Service ist ein vollständig verwalteter Dienst, sodass Sie keine verwaltete Identität verwenden können, um Token manuell abzurufen. Stattdessen verwendet Azure SignalR Service die von Ihnen festgelegte verwaltete Identität, um ein Zugriffstoken abzurufen. Der Dienst legt dann in serverlosen Szenarien das Zugriffstoken in einem `Authorization`-Header in einer Upstreamanforderung fest.

### <a name="enable-managed-identity-authentication-in-upstream-settings"></a>Aktivieren der verwalteten Identitätsauthentifizierung in Upstreameinstellungen

1. Fügen Sie eine systemseitig oder benutzerseitig zugewiesene Identität hinzu.

2. Fügen Sie eine Upstreameinstellung hinzu, und klicken Sie auf ein beliebiges Sternchen, um eine Detailseite wie unten dargestellt zu öffnen.
    :::image type="content" source="media/signalr-howto-use-managed-identity/pre-msi-settings.png" alt-text="pre-msi-setting":::
    
    :::image type="content" source="media/signalr-howto-use-managed-identity/msi-settings.png" alt-text="msi-setting":::

3. In den Einstellungen für die Authentifizierung der verwalteten Identität können Sie für **Ressource** die Zielressource angeben. Die Ressource wird zu einem `aud`-Anspruch in dem erhaltenen Zugriffstoken, der als Teil der Validierung in Ihren Upstreamendpunkten verwendet werden kann. Für die Ressource kann eine der folgenden Optionen zutreffen:
    - Leer
    - Anwendungs-ID (Client) des Dienstprinzipals
    - URI der Anwendungs-ID des Dienstprinzipals
    - [Ressourcen-ID eines Azure-Diensts](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)

    > [!NOTE]
    > Wenn Sie in Ihrem Dienst ein Zugriffstoken selbst überprüfen, können Sie eines der Ressourcenformate auswählen. Stellen Sie lediglich sicher, dass der **Ressource**-Wert in den **Auth**-Einstellungen und die Validierung konsistent sind. Wenn Sie die rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) für eine Datenebene verwenden, müssen Sie die Ressource verwenden, die der Dienstanbieter anfordert.

### <a name="validate-access-tokens"></a>Überprüfen von Zugriffstoken

Das Token im `Authorization`-Header ist ein [Microsoft Identity Platform-Zugriffstoken](../active-directory/develop/access-tokens.md#validating-tokens).

Um Zugriffstoken zu überprüfen, sollte Ihre App auch die Zielgruppe und die signierenden Token validieren. Diese müssen anhand der Werte im OpenID Discovery-Dokument überprüft werden. Ein Beispiel finden Sie in der [mandantenunabhängigen Version des Dokuments](https://login.microsoftonline.com/common/.well-known/openid-configuration).

Die Azure Active Directory-Middleware (Azure AD) verfügt über integrierte Funktionen zur Validierung von Zugriffstoken. Sie können unsere [Beispiele](../active-directory/develop/sample-v2-code.md) durchsuchen, um eines in der Sprache Ihrer Wahl zu finden.

Wir bieten Bibliotheken und Codebeispiele, die zeigen, wie Sie die Tokenüberprüfung ausführen können. Für die JSON Web Token-Validierung (JWT) stehen auch mehrere Open-Source-Partnerbibliotheken zur Verfügung. Es gibt dort mindestens eine Option für nahezu jede Plattform und Sprache. Weitere Informationen zu Azure AD-Authentifizierungsbibliotheken sowie Codebeispiele finden Sie unter [Microsoft Identity Platform-Authentifizierungsbibliotheken](../active-directory/develop/reference-v2-libraries.md).

#### <a name="authentication-in-function-app"></a>Authentifizierung in Funktions-Apps

Das Festlegen der Zugriffstokenüberprüfung in Funktions-Apps ist einfach und effizient und erfordert keinen Code.

1. Schalten Sie auf der Seite **Authentifizierung/Autorisierung** die Option **App Service-Authentifizierung** auf **Ein** um.

2. Wählen Sie unter **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** die Option **Mit Azure Active Directory anmelden** aus.

3. Klicken Sie unter „Authentifizierungsanbieter“ auf **Azure Active Directory**.

4. Führen Sie auf der neuen Seite Folgendes aus. Wählen Sie **Express** und **Neue AD-App erstellen** aus, und klicken Sie dann auf **OK**. :::image type="content" source="media/signalr-howto-use-managed-identity/function-aad.png" alt-text="Funktion – AAD":::

5. Navigieren Sie zu SignalR Service, und befolgen Sie die [Schritte](howto-use-managed-identity.md#add-a-system-assigned-identity) zum Hinzufügen einer vom System zugewiesenen Identität oder einer vom Benutzer zugewiesenen Identität.

6. Navigieren Sie zu den **Upstreameinstellungen** in SignalR Service, und wählen Sie **Verwaltete Identität verwenden** und **Vorhandene AD-App auswählen** aus. Wählen Sie die zuvor erstellte Anwendung aus.

Nach diesen Einstellungen lehnt die Funktions-App Anforderungen ohne Zugriffstoken im Header ab.

## <a name="use-a-managed-identity-for-key-vault-reference"></a>Verwenden einer verwalteten Identität als Key Vault-Verweis

SignalR Service kann auf Key Vault zugreifen, um ein Geheimnis mithilfe der verwalteten Identität abzurufen.

1. Fügen Sie eine vom System oder vom Benutzer zugewiesene Identität für Azure SignalR Service hinzu.

2. Erteilen Sie der verwalteten Identität in den Zugriffsrichtlinien im Key Vault Leseberechtigung. Weitere Informationen finden Sie unter [Zuweisen einer Key Vault-Zugriffsrichtlinie über das Azure-Portal](../key-vault/general/assign-access-policy-portal.md).

Derzeit kann dieses Feature in folgenden Szenarien verwendet werden:

- [Verweisen auf ein Geheimnis im Upstream-URL-Muster](./concept-upstream.md#key-vault-secret-reference-in-url-template-settings)


## <a name="next-steps"></a>Nächste Schritte

- [Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service](signalr-concept-serverless-development-config.md)
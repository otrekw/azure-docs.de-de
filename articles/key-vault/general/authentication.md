---
title: Authentifizieren bei Azure Key Vault
description: Es wird beschrieben, wie Sie sich bei Azure Key Vault authentifizieren.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 03/31/2021
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.openlocfilehash: 08ed6fc4c4a18a699a8a27b2436f5eb92252fa7e
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108755211"
---
# <a name="authentication-in-azure-key-vault"></a>Authentifizierung in Azure Key Vault

Die Authentifizierung mit Key Vault funktioniert in Verbindung mit [Azure Active Directory (Azure AD)](../../active-directory/fundamentals/active-directory-whatis.md), was für die Authentifizierung der Identität eines bestimmten **Sicherheitsprinzipals** zuständig ist.

Ein Sicherheitsprinzipal ist ein Objekt, das Benutzer, Gruppen, Dienste oder Anwendungen darstellt, die Zugriff auf Azure-Ressourcen anfordern. Azure weist jedem Sicherheitsprinzipal eine eindeutige **Objekt-ID** zu.

* Ein **Benutzer** als Sicherheitsprinzipal identifiziert eine Person, die über ein Profil in Azure Active Directory verfügt.

* Eine **Gruppe** als Sicherheitsprinzipal identifiziert eine Gruppe von Benutzern, die in Azure Active Directory erstellt wurden. Alle Rollen oder Berechtigungen, die der Gruppe zugewiesen werden, werden allen Benutzern in der Gruppe erteilt.

* Ein **Dienstprinzipal** ist ein Typ des Sicherheitsprinzipals, der für eine Anwendung oder einen Dienst steht und sozusagen ein Codeabschnitt anstelle eines Benutzers oder einer Gruppe ist. Die Objekt-ID eines Dienstprinzipals wird als dessen **Client-ID** bezeichnet und verhält sich wie der Benutzername. Der **geheime Clientschlüssel** des Dienstprinzipals verhält sich wie sein Kennwort.

Für Anwendungen gibt es zwei Möglichkeiten, einen Dienstprinzipal abzurufen:

* Empfohlen: Aktivieren einer systemseitig zugewiesenen **verwalteten Identität** für die Anwendung.

    Mit der verwalteten Identität verwaltet Azure intern den Dienstprinzipal der Anwendung und authentifiziert die Anwendung automatisch mit anderen Azure-Diensten. Die verwaltete Identität ist für Anwendungen verfügbar, die für eine Vielzahl von Diensten bereitgestellt werden.

    Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../active-directory/managed-identities-azure-resources/overview.md). Weitere Informationen finden Sie unter [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), wo Sie Links zu Artikeln finden, in denen beschrieben wird, wie die verwaltete Identität für bestimmte Dienste (z. B. App Service, Azure Functions, Virtual Machines usw.) aktiviert wird.

* Wenn Sie die verwaltete Identität nicht verwenden können, **registrieren** Sie die Anwendung stattdessen bei Ihrem Azure AD-Mandanten gemäß Beschreibung in [Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform](../../active-directory/develop/quickstart-register-app.md). Bei der Registrierung wird auch ein zweites Anwendungsobjekt erstellt, das die App für alle Mandanten identifiziert.

## <a name="configure-the-key-vault-firewall"></a>Konfigurieren der Key Vault-Firewall

Standardmäßig ermöglicht Key Vault den Zugriff auf Ressourcen über öffentliche IP-Adressen. Der höheren Sicherheit willen können Sie auch den Zugriff auf bestimmte IP-Adressbereiche, Dienstendpunkte, virtuelle Netzwerke oder private Endpunkte einschränken.

Weitere Informationen finden Sie unter [Zugreifen auf Azure Key Vault hinter einer Firewall](./access-behind-firewall.md).

## <a name="the-key-vault-request-operation-flow-with-authentication"></a>Ablauf des Key Vault-Anforderungsvorgangs mit Authentifizierung

Die Key Vault-Authentifizierung ist Teil jedes Anforderungsvorgang für Key Vault. Das abgerufene Token kann bei nachfolgenden Aufrufen wiederverwendet werden. Exemplarischer Ablauf der Authentifizierung:

1. Von einem Token wird die Authentifizierung bei Azure AD angefordert. Beispiele:
    * Eine Azure-Ressource (beispielsweise ein virtueller Computer oder eine App Service-Anwendung mit einer verwalteten Identität) kontaktiert den REST-Endpunkt, um ein Zugriffstoken abzurufen.
    * Ein Benutzer meldet sich beim Azure-Portal mit einem Benutzernamen und einem Kennwort an.

1. Wenn die Authentifizierung bei Azure AD erfolgreich ist, wird dem Sicherheitsprinzipal ein OAuth-Token gewährt.

1. Die Key Vault-REST-API wird über den Endpunkt (URI) der Key Vault-Instanz aufgerufen.

1. Die Key Vault-Firewall überprüft die folgenden Kriterien. Wenn ein Kriterium erfüllt ist, ist der Aufruf zulässig. Andernfalls wird der Aufruf blockiert, und eine unzulässige Antwort wird zurückgegeben.

    * Die Firewall wird deaktiviert, und der öffentliche Key Vault-Endpunkt (URI) ist über das öffentliche Internet erreichbar.
    * Bei der aufrufenden Funktion handelt es sich um einen [vertrauenswürdigen Key Vault-Dienst](./overview-vnet-service-endpoints.md#trusted-services), der die Firewall umgehen darf.
    * Die aufrufende Funktion ist in der Firewall nach IP-Adresse, virtuellem Netzwerk oder Dienstendpunkt aufgelistet.
    * Die aufrufende Funktion kann Key Vault über eine konfigurierte Private Link-Verbindung erreichen.    

1. Wird der Aufruf durch die Firewall zugelassen, ruft Key Vault Azure AD auf, um das Zugriffstoken des Sicherheitsprinzipals zu überprüfen.

1. Von Key Vault wird überprüft, ob der Sicherheitsprinzipal über die erforderliche Berechtigung für den angeforderten Vorgang verfügt. Wenn dies nicht der Fall ist, gibt Key Vault eine unzulässige Antwort zurück.

1. Key Vault führt den angeforderten Vorgang aus und gibt das Ergebnis zurück.

Das folgende Diagramm veranschaulicht den Prozess für eine Anwendung, die eine „Geheimnis abrufen“-API von Key Vault aufruft:

![Der Azure Key Vault-Authentifizierungsfluss](../media/authentication/authentication-flow.png)

> [!NOTE]
> Von Key Vault SDK-Clients für Geheimnisse, Zertifikate und Schlüssel wird ein zusätzlicher Aufruf ohne Zugriffstoken an Key Vault gesendet. Dies hat eine 401-Antwort zum Abrufen von Mandanteninformationen zur Folge. Weitere Informationen finden Sie unter [Authentifizierung, Anforderungen und Antworten](authentication-requests-and-responses.md).

## <a name="authentication-to-key-vault-in-application-code"></a>Authentifizierung bei Key Vault in Anwendungscode

Das Key Vault SDK verwendet die Azure Identity-Clientbibliothek. Dies ermöglicht eine nahtlose, umgebungsübergreifende Authentifizierung bei Key Vault mit dem gleichen Code.

**Azure Identity-Clientbibliotheken**

| .NET | Python | Java | JavaScript |
|--|--|--|--|
|[Azure Identity SDK .NET](/dotnet/api/overview/azure/identity-readme)|[Azure Identity SDK Python](/python/api/overview/azure/identity-readme)|[Azure Identity SDK Java](/java/api/overview/azure/identity-readme)|[Azure Identity SDK JavaScript](/javascript/api/overview/azure/identity-readme)|   

Weitere Informationen zu bewährten Methoden sowie Entwicklerbeispiele finden Sie unter [Authentifizieren bei Key Vault mit Code](developers-guide.md#authenticate-to-key-vault-in-code).

## <a name="next-steps"></a>Nächste Schritte

- [Entwicklerhandbuch für Key Vault](developers-guide.md)
- [Zuweisen einer Key Vault-Zugriffsrichtlinie über das Azure-Portal](assign-access-policy-portal.md)
- [Zuweisen einer Azure RBAC-Rolle zu Key Vault](rbac-guide.md)
- [Behandeln von Problemen mit Schlüsseltresor-Zugriffsrichtlinien](troubleshooting-access-issues.md)
- [Azure Key Vault: REST-API-Fehlercodes](rest-error-codes.md)

- [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md)

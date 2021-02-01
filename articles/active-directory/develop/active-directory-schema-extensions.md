---
title: Verwenden von Azure AD-Schemaerweiterungsattributen in Ansprüchen
titleSuffix: Microsoft identity platform
description: In diesem Artikel wird beschrieben, wie Sie mit Verzeichnisschema-Erweiterungsattributen in Tokenansprüchen Benutzerdaten an Anwendungen senden.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: how-to
ms.date: 07/29/2020
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, jeedes, luleon
ms.openlocfilehash: 0127c8d796126d1e99b1fa38a9506df477c7eb49
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98755729"
---
# <a name="using-directory-schema-extension-attributes-in-claims"></a>Verwenden von Verzeichnisschema-Erweiterungsattributen in Ansprüchen

Verzeichnisschema-Erweiterungsattribute bieten eine Möglichkeit, zusätzliche Daten in Azure Active Directory für Benutzer- und andere Verzeichnisobjekte (z. B. Gruppen, Mandantendetails, Dienstprinzipale) zu speichern.  Nur Erweiterungsattribute für Benutzerobjekte können zum Ausgeben von Ansprüchen an Anwendungen verwendet werden. In diesem Artikel wird beschrieben, wie Sie mit den Verzeichnisschema-Erweiterungsattributen in Tokenansprüchen Benutzerdaten an Anwendungen senden.

> [!NOTE]
> Microsoft Graph bietet zwei weitere Erweiterungsmechanismen zum Anpassen von Graph-Objekten. Diese werden als offene Microsoft Graph-Erweiterungen und Microsoft Graph-Schemaerweiterungen bezeichnet. Ausführliche Informationen finden Sie in der [Dokumentation zu Microsoft Graph](/graph/extensibility-overview). Daten, die mit diesen Funktionen in Microsoft Graph-Objekten gespeichert werden, sind nicht als Quellen für Ansprüche in Token verfügbar.

Verzeichnisschema-Erweiterungsattribute sind immer mit einer Anwendung im Mandanten verknüpft, und die *ApplicationId* der Anwendung verweist in ihrem Namen auf diese Attribute.

Der Bezeichner für ein Verzeichnisschema-Erweiterungsattribut hat die Form *Extension_xxxxxxxxx_AttributeName*.  Dabei entspricht *xxxxxxxxx* der *applicationId* der Anwendung, für welche die Erweiterung definiert wurde.

## <a name="registering-and-using-directory-schema-extensions"></a>Registrieren und Verwenden von Verzeichnisschemaerweiterungen
Verzeichnisschema-Erweiterungsattribute können auf zweierlei Art und Weise registriert und aufgefüllt werden:

- Konfigurieren von AD Connect zum Erstellen der Attribute und zum Synchronisieren der enthaltenen Daten aus dem lokalem Active Directory. Informationen hierzu finden Sie unter [Azure AD Connect Sync: Verzeichniserweiterungen](../hybrid/how-to-connect-sync-feature-directory-extensions.md).
- Verwenden von Microsoft Graph für die Registrierung, Festlegen der Werte und Lesen aus [Schemaerweiterungen](/graph/extensibility-overview). [PowerShell-Cmdlets](/powershell/azure/active-directory/using-extension-attributes-sample) sind ebenfalls verfügbar.

### <a name="emitting-claims-with-data-from-directory-schema-extension-attributes-created-with-ad-connect"></a>Ausgeben von Ansprüchen mit Daten aus den mit AD Connect erstellten Verzeichnisschema-Erweiterungsattributen
Mit AD Connect erstellte und synchronisierte Verzeichnisschema-Erweiterungsattribute sind immer der Anwendungs-ID zugeordnet, die von AD Connect verwendet wird. Sie können diese Attribute als Quelle für Ansprüche verwenden, indem Sie sie in der Portalumgebung für SAML-Anwendungen (in der Konfigurationsumgebung der Katalog- oder Nicht-Katalog-Anwendung unter **Unternehmensanwendungen** registriert) bei der Konfiguration der **Unternehmensanwendungen** als Ansprüche konfigurieren, oder indem Sie eine Richtlinie zum Zuordnen von Ansprüchen für Anwendungen verwenden, die in der Anwendungsregistrierungsumgebung registriert wurden.  Sobald sich ein über AD Connect erstelltes Verzeichniserweiterungsattribut im Verzeichnis befindet, wird es auf der Benutzeroberfläche für die SAML-SSO-Anspruchskonfiguration angezeigt.

### <a name="emitting-claims-with-data-from-directory-schema-extension-attributes-created-for-an-application-using-graph-or-powershell"></a>Ausgeben von Ansprüchen mit Daten aus Verzeichnisschema-Erweiterungsattributen, die mit Microsoft Graph oder PowerShell für eine Anwendung erstellt wurden
Wenn ein Verzeichnisschema-Erweiterungsattribut mit Microsoft Graph oder PowerShell für eine Anwendung registriert wird (z. B. bei der ersten Einrichtung der Anwendung oder bei einem Bereitstellungsschritt), kann die gleiche Anwendung in Azure Active Directory so konfiguriert werden, dass sie bei der Anmeldung des Benutzers die Daten in diesem Attribut aus einem Benutzerobjekt in einem Anspruch empfängt.  Die Anwendung kann so konfiguriert werden, dass sie Daten in Verzeichnisschemaerweiterungen empfängt, die mithilfe [optionaler Ansprüche](active-directory-optional-claims.md#configuring-directory-extension-optional-claims) für dieselbe Anwendung registriert wurden.  Diese können im Anwendungsmanifest festgelegt werden.  Dadurch kann eine mehrinstanzenfähige Anwendung Verzeichnisschema-Erweiterungsattribute für die Eigenverwendung registrieren. Wenn die Anwendung in einem Mandanten bereitgestellt wird, werden die zugehörigen Verzeichnisschemaerweiterungen verfügbar und können für Benutzer in diesem Mandanten festgelegt und verwendet werden.  Wenn die Anwendung im Mandanten konfiguriert wurde und die Zustimmung erteilt ist, kann sie zum Speichern und Abrufen von Daten über Microsoft Graph und zum Zuordnen von Ansprüchen in Token verwendet werden, die Microsoft Identity Platform für Anwendungen ausgibt.

Verzeichnisschema-Erweiterungsattribute können für jede Anwendung registriert und aufgefüllt werden.

Wenn eine Anwendung Ansprüche mit Daten aus einem für eine andere Anwendung registrierten Erweiterungsattribut senden muss, müssen Sie eine [Richtlinie zum Zuordnen von Ansprüchen](active-directory-claims-mapping.md) verwenden, um das Erweiterungsattribut dem Anspruch zuzuordnen.  Ein gängiges Muster für das Verwalten von Verzeichnisschema-Erweiterungsattributen besteht darin, eine Anwendung zu erstellen, die eigens als Registrierungspunkt für alle benötigten Schemaerweiterungen fungieren soll.  Dabei muss es sich nicht um eine reale Anwendung handeln. Diese Technik bedeutet, dass alle Namen von Erweiterungen die gleiche Anwendungs-ID enthalten.

Nachfolgend finden Sie beispielsweise eine Richtlinie für die Anspruchszuordnung, mit der ein einzelner Anspruch aus einem Verzeichnisschema-Erweiterungsattribut in einem OAuth/OIDC-Token ausgegeben wird:

```json
{
    "ClaimsMappingPolicy": {
        "Version": 1,
        "IncludeBasicClaimSet": "false",
        "ClaimsSchema": [{
                "Source": "User",
                "ExtensionID": "extension_xxxxxxx_test",
                "JWTClaimType": "http://schemas.contoso.com/identity/claims/exampleclaim"
            },
        ]
    }
}
```

Dabei ist *xxxxxxx* die Anwendungs-ID, mit der die Erweiterung registriert wurde.

> [!TIP]
> Beim Festlegen von Verzeichniserweiterungsattributen für Objekte ist die Groß-/Kleinschreibung zu beachten.  Bei der Einrichtung der Erweiterungsattribute spielt die Groß-/Kleinschreibung in den Namen keine Rolle. Sie wird jedoch vom Tokendienst beim Lesen aus dem Verzeichnis beachtet.  Wenn ein Erweiterungsattribut für ein Benutzerobjekt mit dem Namen „LegacyId“ und für ein anderes Benutzerobjekt namens „legacyid“ festgelegt wird und das Attribut einem Anspruch mit dem Namen „LegacyId" zugeordnet wird, werden die Daten (und der Anspruch im Token für den ersten Benutzer, jedoch nicht für den zweiten) erfolgreich abgerufen.
>
> Der Parameter „Id“ im Anspruchsschema, das für integrierte Verzeichnisattribute verwendet wird, lautet „ExtensionID“ für Verzeichniserweiterungsattribute.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum [Hinzufügen benutzerdefinierter oder zusätzlicher Ansprüche zu den SAML 2.0- und JWT-Token (JSON Web Token)](active-directory-optional-claims.md).
- Informationen zum [Anpassen von Ansprüchen, die in Token für eine bestimmte App ausgegeben werden](active-directory-claims-mapping.md).
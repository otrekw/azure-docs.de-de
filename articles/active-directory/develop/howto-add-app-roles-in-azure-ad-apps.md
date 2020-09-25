---
title: Hinzufügen von App-Rollen und Abrufen dieser Rollen aus einem Token | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie App-Rollen zu einer bei Azure Active Directory registrierten Anwendung hinzufügen, diesen Rollen Benutzer und Gruppen zuweisen und sie im Rollenanspruch im Token empfangen.
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 09/15/2020
ms.author: kkrishna
ms.reviewer: kkrishna, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 02fe3c1ebc893dea259abcda3ea1d13ab96d68d5
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90706012"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Gewusst wie: Hinzufügen von App-Rollen in Ihrer Anwendung und Empfangen der Rollen im Token

Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) ist eine beliebte Methode zum Erzwingen der Autorisierung in Anwendungen. Bei der Verwendung der RBAC gewährt ein Administrator Berechtigungen für Rollen und nicht für einzelne Benutzer oder Gruppen. Der Administrator kann anschließend verschiedenen Benutzern und Gruppen Rollen zuweisen, um zu steuern, wer auf welche Inhalte und Funktionen zugreifen kann.

Entwickler können die RBAC mit Anwendungsrollen und Rollenansprüche verwenden und dadurch mit weniger Aufwand sicher die Autorisierung in ihren Apps erzwingen.

Ein weiterer Ansatz ist die Verwendung von Azure AD-Gruppen und Gruppenansprüchen wie im [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet)-Codebeispiel auf GitHub gezeigt. Azure AD-Gruppen und Anwendungsrollen schließen sich nicht gegenseitig aus. Sie können zusammen verwendet werden, um eine noch differenziertere Zugriffssteuerung zu ermöglichen.

## <a name="declare-roles-for-an-application"></a>Deklarieren von Rollen für eine Anwendung

Anwendungsrollen werden im [Azure-Portal](https://portal.azure.com) definiert.  Wenn sich ein Benutzer bei der Anwendung anmeldet, gibt Azure AD für den Benutzer einen `roles`-Anspruch für jede einzeln zugewiesene Rolle und für die Gruppenmitgliedschaft aus.  Die Zuweisung von Benutzern und Gruppen zu Rollen kann über die Benutzeroberfläche des Portals oder programmgesteuert mit [Microsoft Graph](/graph/azuread-identity-access-management-concept-overview) erfolgen.

So erstellen Sie eine App-Rolle

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie den Filter **Verzeichnis + Abonnement** im oberen Menü und dann den Azure Active Directory-Mandanten aus, der die App-Registrierung enthält, zu der Sie eine App-Rolle hinzufügen möchten.
1. Suchen Sie nach **Azure Active Directory**, und wählen Sie diese Option aus.
1. Wählen Sie unter **Verwalten** die Option **App-Registrierungen** und dann die Anwendung aus, in der Sie App-Rollen definieren möchten.
1. Wählen Sie **App-Rollen | Vorschau** und dann **App-Rolle erstellen** aus.

   :::image type="content" source="media/howto-add-app-roles-in-azure-ad-apps/app-roles-overview-pane.png" alt-text="Bereich „App-Rollen“ einer App-Registrierung im Azure-Portal":::
1. Geben Sie im Bereich **App-Rolle erstellen** die Einstellungen für die Rolle ein. Die Tabelle im Anschluss an das Bild beschreibt die einzelnen Einstellungen und ihre Parameter.

    :::image type="content" source="media/howto-add-app-roles-in-azure-ad-apps/app-roles-create-context-pane.png" alt-text="Bereich „App-Rollen, Kontext erstellen“ einer App-Registrierung im Azure-Portal":::

    | Feld | Beschreibung | Beispiel |
    |-------|-------------|---------|
    | **Anzeigename** | Anzeigename für die App-Rolle, die auf den Administratorbildschirmen für Zustimmung und App-Zuweisung angezeigt wird. Dieser Wert kann Leerzeichen enthalten.  | `Survey Writer` |
    | **Zulässige Mitgliedstypen** | Gibt an, ob diese App-Rolle Benutzern, Anwendungen oder beiden zugeordnet werden kann. | `Users/Groups` |
    | **Wert** | Gibt den Wert des Rollenanspruchs an, den die Anwendung in den Token erwarten sollte. Der Wert sollte genau mit der Zeichenfolge übereinstimmen, auf die im Code der Anwendung verwiesen wird. Der Wert darf keine Leerzeichen enthalten. | `Survey.Create` |
    | **Beschreibung** | Eine ausführlichere Beschreibung der App-Rolle, die dem Administrator auf den App-Zuweisungs- und -Zustimmungsoberflächen angezeigt wird. | `Writers can create surveys.` |
    | **Möchten Sie diese App-Rolle aktivieren?** | Gibt an, ob die App-Rolle aktiviert ist. Um eine App-Rolle zu löschen, deaktivieren Sie dieses Kontrollkästchen, und wenden Sie die Änderung an, bevor Sie den Löschvorgang starten. | *Überprüft* |

1. Wählen Sie **Übernehmen** aus, um die Änderungen zu speichern.

> [!NOTE]
> Die Anzahl der Rollen, die Sie hinzufügen, wird auf die für ein Anwendungsmanifest definierten Grenzwerte angerechnet. Informationen zu diesen Grenzwerten finden Sie im Abschnitt [Grenzwerte für das Manifest](./reference-app-manifest.md#manifest-limits) der [Azure Active Directory-App-Manifestreferenz](reference-app-manifest.md).

## <a name="assign-users-and-groups-to-roles"></a>Zuweisen von Benutzern und Gruppen zu Rollen

Nachdem Sie App-Rollen in Ihrer Anwendung hinzugefügt haben, können Sie den Rollen Benutzer und Gruppen zuweisen.

1. Wählen Sie in **Azure Active Directory** im Azure-Portal die Option **Enterprise-Anwendungen** im linken Navigationsmenü aus.
1. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

     Falls die Anwendung nicht in der Liste angezeigt wird, können Sie oben in der Liste **Alle Anwendungen** die Filter verwenden, um den Inhalt der Liste einzugrenzen. Sie können auch in der Liste nach unten scrollen, um nach Ihrer Anwendung zu suchen.

1. Wählen Sie die Anwendung aus, in der Sie Benutzer oder eine Sicherheitsgruppe zu Rollen zuweisen möchten.
1. Klicken Sie unter **Verwalten** auf **Benutzer und Gruppen**.
1. Wählen Sie **Benutzer hinzufügen** aus, um den Bereich **Zuweisung hinzufügen** zu öffnen.
1. Wählen Sie im Bereich **Zuweisung hinzufügen** den Selektor **Benutzer und Gruppen** aus.

     Eine Liste von Benutzern und Sicherheitsgruppen wird angezeigt. Sie können sowohl nach einem bestimmten Benutzer oder einer bestimmten Gruppe suchen als auch mehrere in der Liste angezeigte Benutzer und Gruppen auswählen.

1. Nachdem Sie Benutzer und Gruppen ausgewählt haben, wählen Sie die Schaltfläche **Auswählen** aus, um fortzufahren.
1. Wählen Sie im Bereich **Zuweisung hinzufügen** die Option **Rolle auswählen** aus. Alle Rollen, die Sie für die Anwendung definiert haben, werden angezeigt.
1. Wählen Sie eine Rolle und dann die Schaltfläche **Auswählen** aus.
1. Wählen Sie die Schaltfläche **Zuweisen** aus, um die Zuweisungen von Benutzern und Gruppen zur App abzuschließen.
1. Vergewissern Sie sich, dass die hinzugefügten Benutzer und Gruppen in der Liste **Benutzer und Gruppen** angezeigt werden.

## <a name="receive-roles-in-tokens"></a>Empfangen von Rollen in Token

Wenn sich Benutzer, denen verschiedene App-Rollen zugewiesen sind, bei der Anwendung anmelden, enthalten deren Token die zugewiesenen Rollen im `roles`-Anspruch.

## <a name="web-api-application-permissions"></a>Web-API-Anwendungsberechtigungen

Sie können App-Rollen definieren, die auf **Benutzer/Gruppen**, **Anwendungen** oder **beide** abzielen. Wenn Sie **Anwendungen** oder **Beide** auswählen, wird die App-Rolle als Anwendungsberechtigung in den **API-Berechtigungen** einer Client-App angezeigt.

So wählen Sie die Anwendungsberechtigung in einer Client-App-Registrierung aus, nachdem Sie eine Rolle definiert haben, die auf **Anwendungen** oder **beide** abzielt:

1. Wählen Sie **Azure Active Directory** im Azure-Portal, dann **App-Registrierungen** und anschließend die Registrierung der Client-App aus.
1. Wählen Sie unter **Verwalten** die Option **API-Berechtigungen**.
1. Wählen Sie **Berechtigung hinzufügen** > **Meine APIs** aus.
1. Wählen Sie die App aus, zu der Sie die Anwendungsrolle hinzugefügt haben, und wählen Sie dann **Anwendungsberechtigungen** aus.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Ressourcen erfahren Sie mehr über App-Rollen:

- Codebeispiel: [Add authorization using app roles & roles claims to an ASP.NET Core web app](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles) (Hinzufügen von Autorisierung zu einer ASP.NET Core-Web-App mit App-Rollen und Rollenansprüchen) (GitHub)
- Video: [Implement authorization in your applications with Microsoft identity platform ](https://www.youtube.com/watch?v=LRoc-na27l0) (Implementieren der Autorisierung in Ihren Anwendungen mit Microsoft Identity Platform) (1:01:15)
- [Azure Active Directory-App-Manifest](./reference-app-manifest.md)
- [ Azure AD-AD-Zugriffstoken](access-tokens.md)
- [Azure AD `id_tokens`](id-tokens.md)

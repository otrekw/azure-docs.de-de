---
title: Übersicht über das Azure API Management-Entwicklerportal
titleSuffix: Azure API Management
description: Erfahren Sie mehr über das Entwicklerportal in API Management.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/22/2019
ms.author: apimpm
ms.openlocfilehash: a06184cb45280d323a050085e94945a730d6c72c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75442594"
---
# <a name="azure-api-management-developer-portal-overview"></a>Azure API Management-Entwicklerportal: Übersicht

Das Entwicklerportal ist eine automatisch generierte, vollständig anpassbare Website mit der Dokumentation Ihrer APIs. Dort können API-Consumer ihre APIs ermitteln, erfahren, wie sie verwendet werden, Zugriff anfordern und die APIs ausprobieren.

In diesem Artikel werden die Unterschiede zwischen selbstgehosteten und verwalteten Versionen des Entwicklerportals in API Management beschrieben. Außerdem wird die Architektur erläutert, und es werden Antworten auf häufig gestellte Fragen bereitgestellt.

> [!WARNING]
>
> Erfahren Sie, wie Sie von der Vorschauversion zur allgemein verfügbaren Version des neuen Entwicklerportals [migrieren](#preview-to-ga) können.

![Entwicklerportal für API Management](media/api-management-howto-developer-portal/cover.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="managed-vs-self-hosted"></a> Verwaltete und selbstgehostete Versionen

Sie können Ihr Entwicklerportal auf zwei Arten erstellen:

- **Verwaltete Version**: Indem Sie das in Ihre API Management-Instanz integrierte Portal bearbeiten und anpassen, auf das Sie über die URL `<your-api-management-instance-name>.developer.azure-api.net` zugreifen können. Lesen Sie [diesen Dokumentationsartikel](api-management-howto-developer-portal-customize.md), um zu erfahren, wie Sie auf das verwaltete Portal zugreifen und es anpassen können.
- **Selbst gehostete Version**: Indem Sie Ihr Portal außerhalb einer API Management-Instanz bereitstellen und selbst hosten. Dieser Ansatz ermöglicht es Ihnen, die Codebasis des Portals zu bearbeiten und die bereitgestellten Kernfunktionen zu erweitern. Sie müssen auch ein Upgrade des Portals auf die neueste Version ausführen. Details und Anweisungen finden Sie im [GitHub-Repository mit dem Quellcode des Portals][1]. Das [Tutorial für die verwaltete Version](api-management-howto-developer-portal-customize.md) führt Sie schrittweise durch den Verwaltungsbereich des Portals, der auch in der selbstgehosteten Version enthalten ist.

## <a name="portal-architectural-concepts"></a>Konzepte der Portarchitektur

Die Portalkomponenten können logisch in zwei Kategorien unterteilt werden: *Code* und *Inhalt*.

*Code* wird im [GitHub-Repository][1] verwaltet und umfasst Folgendes:

- Widgets, die visuelle Elemente darstellen und HTML, JavaScript, Formatierungsfunktionen, Einstellungen und Inhaltszuordnung kombinieren. Beispiele hierfür sind ein Bild, ein Textabsatz, ein Formular, eine Liste mit APIs usw.
- Formatierungsdefinitionen, die angeben, wie Widgets formatiert werden können.
- Die Engine, die statische Webseiten aus Portalinhalten generiert und in JavaScript geschrieben ist.
- Einen visuellen Editor, der das Anpassen und Erstellen im Browser ermöglicht.

*Inhalt* wird in zwei Unterkategorien unterteilt: *Portalinhalt* und *API Management-Inhalt*.

*Portalinhalt* ist für das Portal spezifisch und umfasst Folgendes:

- Seiten: etwa Landing Page, API-Tutorials, Blogbeiträge
- Medien: Bilder, Animationen und andere dateibasierte Inhalte
- Layouts: Vorlagen, die mit einer URL abgeglichen werden und definieren, wie Seiten angezeigt werden
- Formatvorlagen: Werte für Formatierungsdefinitionen, z.B. Schriftarten, Farben, Rahmen
- Einstellungen: Konfiguration, z.B. Favicon, Websitemetadaten

*Portalinhalte* (mit Ausnahme von Medien) werden als JSON-Dokumente ausgedrückt.

*API Management-Inhalte* umfassen Entitäten wie APIs, Vorgänge, Produkte und Abonnements.

Das Portal basiert auf einem angepassten Fork des [Paperbits-Frameworks](https://paperbits.io/). Die ursprüngliche Paperbits-Funktionalität wurde erweitert, um für API Management spezifische Widgets (etwa eine Liste mit APIs oder Produkten) und einen API Management-Dienstconnector zum Speichern und Abrufen von Inhalten bereitzustellen.

## <a name="faq"></a> Häufig gestellte Fragen

In diesem Abschnitt beantworten wir häufig gestellte Fragen zum neuen Entwicklerportal, die allgemeiner Natur sind. Fragen, die sich speziell auf die selbstgehostete Version beziehen, finden Sie im [GitHub-Repository im Abschnitt „Wiki“](https://github.com/Azure/api-management-developer-portal/wiki).

### <a name="a-idpreview-to-ga-how-can-i-migrate-from-the-preview-version-of-the-portal"></a><a id="preview-to-ga"/> Wie kann ich von der Vorschauversion des Portals migrieren?

Mit der Vorschauversion des Entwicklerportals haben Sie den Vorschauinhalt in Ihrem API Management-Dienst bereitgestellt. Der Standardinhalt wurde in der allgemein verfügbaren Version zur Verbesserung der Benutzerfreundlichkeit erheblich geändert. Außerdem sind neue Widgets enthalten.

Wenn Sie die verwaltete Version verwenden, setzen Sie den Inhalt des Portals zurück, indem Sie im Abschnitt **Vorgänge** des Menüs auf **Inhalt zurücksetzen** klicken. Wenn Sie diesen Vorgang bestätigen, wird der gesamte Inhalt des Portals entfernt, und die neuen Standardinhalte werden bereitgestellt. Die Engine des Portals wurde automatisch in Ihrem API Management-Dienst aktualisiert.

![Zurücksetzen des Portalinhalts](media/api-management-howto-developer-portal/reset-content.png)

Wenn Sie die selbstgehostete Version verwenden, verwenden Sie die Dateien `scripts/cleanup.bat` und `scripts/generate.bat` aus dem GitHub-Repository, um vorhandene Inhalte zu entfernen und neue Inhalte bereitzustellen. Stellen Sie sicher, dass Sie den Code Ihres Portals im Vorfeld im GitHub-Repository auf die neueste Version aktualisieren.

Wenn Sie den Inhalt des Portals nicht zurücksetzen möchten, ziehen Sie ggf. in Erwägung, neu verfügbare Widgets auf Ihren Seiten zu verwenden. Vorhandene Widgets wurden automatisch auf die neuesten Versionen aktualisiert.

Wenn Ihr Portal nach der Ankündigung der allgemeinen Verfügbarkeit bereitgestellt wurde, sollte es bereits über die neuen Standardinhalte verfügen. Von Ihrer Seite ist keine Aktion erforderlich.

### <a name="how-can-i-migrate-from-the-old-developer-portal-to-the-new-developer-portal"></a>Wie kann ich aus dem alten Entwicklerportal zum neuen Portal migrieren?

Die Portale sind nicht kompatibel, und Sie müssen den Inhalt manuell migrieren.

### <a name="does-the-new-portal-have-all-the-features-of-the-old-portal"></a>Verfügt das neue Portal über alle Features des alten Portals?

Das neue Entwicklerportal unterstützt keine *Anwendungen* und *Issues*. Wenn Sie *Issues* im alten Portal verwendet haben und diese im neuen Portal benötigen, posten Sie einen Kommentar in [einem dedizierten GitHub-Issue](https://github.com/Azure/api-management-developer-portal/issues/122).

Die Authentifizierung mit OAuth in der interaktiven Entwicklerkonsole wird noch nicht unterstützt. Sie können den Fortschritt anhand des [GitHub-Problems](https://github.com/Azure/api-management-developer-portal/issues/208) verfolgen.

### <a name="has-the-old-portal-been-deprecated"></a>Wurde das alte Portal eingestellt?

Die alten Entwickler- und Herausgeberportale sind nun *Legacyfeatures*. Sie erhalten nur noch Sicherheitsupdates. Neue Features werden nur im neuen Entwicklerportal implementiert.

Die Einstellung veralteter Portale wird separat angekündigt. Wenn Sie Fragen, Bedenken oder Kommentare haben, posten Sie diese in [einem dedizierten GitHub-Issue](https://github.com/Azure/api-management-developer-portal/issues/121).

### <a name="how-can-i-automate-portal-deployments"></a>Wie kann ich Portalbereitstellungen automatisieren?

Sie können über die REST-API programmgesteuert auf die Inhalte des Entwicklerportals zugreifen und diese verwalten, und zwar unabhängig davon, ob Sie eine verwaltete oder eine selbstgehostete Version verwenden.

Die APIs sind im [Wiki-Abschnitt des GitHub-Repositorys][2] dokumentiert. Diese Dokumentation kann auch für die Automatisierung von Migrationen von Portalinhalten zwischen Umgebungen verwendet werden (z.B. aus einer Testumgebung in die Produktionsumgebung). Weitere Informationen zu diesem Vorgang finden Sie [in diesem Dokumentationsartikel](https://aka.ms/apimdocs/migrateportal) auf GitHub.

### <a name="does-the-portal-support-azure-resource-manager-templates-andor-is-it-compatible-with-api-management-devops-resource-kit"></a>Unterstützt das Portal Azure Resource Manager-Vorlagen und/oder ist es mit dem API Management DevOps Resource Kit kompatibel?

Nein.

### <a name="do-i-need-to-enable-additional-vnet-connectivity-for-the-new-managed-portal-dependencies"></a>Benötige ich zusätzliche VNET-Konnektivität für die neuen Abhängigkeiten des verwalteten Portals?

In den meisten Fällen nicht.

Wenn sich Ihr API Management-Dienst in einem internen VNET befindet, kann nur innerhalb des Netzwerks auf Ihr Entwicklerportal zugegriffen werden. Der Hostname des Verwaltungsendpunkts muss über den Computer, mit dem Sie auf die Verwaltungsschnittstelle des Portals zugreifen, zur internen VIP des Diensts aufgelöst werden. Vergewissern Sie sich, dass der Verwaltungsendpunkt im DNS registriert ist. Ist die Konfiguration fehlerhaft, wird ein Fehler angezeigt: `Unable to start the portal. See if settings are specified correctly in the configuration (...)`.

### <a name="i-have-assigned-a-custom-api-management-domain-and-the-published-portal-doesnt-work"></a>Ich habe eine benutzerdefinierte API Management-Domäne zugewiesen, und das veröffentlichte Portal funktioniert nicht.

Nachdem Sie die Domäne aktualisiert haben, müssen Sie [das Portal erneut veröffentlichen](api-management-howto-developer-portal-customize.md#publish), damit die Änderungen wirksam werden.

### <a name="i-have-added-an-identity-provider-and-i-cant-see-it-in-the-portal"></a>Ich habe einen Identitätsanbieter hinzugefügt, er wird im Portal aber nicht angezeigt.

Nachdem Sie einen Identitätsanbieter (beispielsweise AAD oder AAD B2C) konfiguriert haben, müssen Sie [das Portal erneut veröffentlichen](api-management-howto-developer-portal-customize.md#publish), damit die Änderungen wirksam werden.

### <a name="i-have-set-up-delegation-and-the-portal-doesnt-use-it"></a>Ich habe die Delegierung eingerichtet, sie wird vom Portal aber nicht verwendet.

Nachdem Sie die Delegierung eingerichtet haben, müssen Sie [das Portal erneut veröffentlichen](api-management-howto-developer-portal-customize.md#publish), damit die Änderungen wirksam werden.

### <a name="my-other-api-management-configuration-changes-havent-been-propagated-in-the-developer-portal"></a>Meine anderen API Management-Konfigurationsänderungen wurden nicht an das Entwicklerportal weitergegeben.

Bei den meisten Konfigurationsänderungen (beispielsweise für das VNET, für die Anmeldung und für Produktbedingungen) ist eine [erneute Veröffentlichung des Portals](api-management-howto-developer-portal-customize.md#publish) erforderlich.

### <a name="im-getting-a-cors-error-when-using-the-interactive-console"></a>Beim Verwenden der interaktiven Konsole erhalte ich einen CORS-Fehler.

Die interaktive Konsole sendet eine clientseitige API-Anforderung über den Browser. Sie können das CORS-Problem beheben, indem Sie [eine CORS-Richtlinie](api-management-cross-domain-policies.md#CORS) für Ihre API(s) hinzufügen. Sie können alle Parameter manuell angeben oder Platzhalterwerte (`*`) verwenden. Beispiel:

```XML
<cors>
    <allowed-origins>
        <origin>*</origin>
    </allowed-origins>
    <allowed-methods>
        <method>GET</method>
        <method>POST</method>
        <method>PUT</method>
        <method>DELETE</method>
        <method>HEAD</method>
        <method>OPTIONS</method>
        <method>PATCH</method>
        <method>TRACE</method>
    </allowed-methods>
    <allowed-headers>
        <header>*</header>
    </allowed-headers>
    <expose-headers>
        <header>*</header>
    </expose-headers>
</cors>
```

> [!NOTE]
> 
> Wenn Sie die CORS-Richtlinie nicht im API-Bereich, sondern im Produktbereich anwenden und Ihre API die Abonnementschlüsselauthentifizierung über einen Header verwendet, funktioniert Ihre Konsole nicht.
>
> Der Browser gibt automatisch eine HTTP-Anforderung vom Typ „OPTIONS“ aus, die keinen Header mit dem Abonnementschlüssel enthält. Aufgrund des fehlenden Abonnementschlüssels kann API Management den OPTIONS-Aufruf keinem Produkt zuordnen und somit die CORS-Richtlinie nicht anwenden.
>
> Zur Umgehung dieses Problems können Sie den Abonnementschlüssel in einem Abfrageparameter übergeben.

### <a name="what-permissions-do-i-need-to-edit-the-developer-portal"></a>Welche Berechtigungen sind zum Bearbeiten des Entwicklerportals erforderlich?

Sollte beim Öffnen des Portals im Verwaltungsmodus der Fehler `Oops. Something went wrong. Please try again later.` angezeigt werden, verfügen Sie möglicherweise nicht über die erforderlichen Berechtigungen (RBAC).

Bei den Legacyportalen war die Berechtigung `Microsoft.ApiManagement/service/getssotoken/action` im Dienstbereich (`/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>`) erforderlich, um dem Benutzer Administratorzugriff auf die Portale zu gewähren. Für das neue Portal ist die Berechtigung `Microsoft.ApiManagement/service/users/token/action` im Bereich `/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>/users/1` erforderlich.

Sie können das folgende PowerShell-Skript verwenden, um eine Rolle mit der erforderlichen Berechtigung zu erstellen. Vergessen Sie nicht, den Parameter `<subscription-id>` zu ändern. 

```PowerShell
#New Portals Admin Role 
Import-Module Az 
Connect-AzAccount 
$contributorRole = Get-AzRoleDefinition "API Management Service Contributor" 
$customRole = $contributorRole 
$customRole.Id = $null
$customRole.Name = "APIM New Portal Admin" 
$customRole.Description = "This role gives the user ability to log in to the new Developer portal as administrator" 
$customRole.Actions = "Microsoft.ApiManagement/service/users/token/action" 
$customRole.IsCustom = $true 
$customRole.AssignableScopes.Clear() 
$customRole.AssignableScopes.Add('/subscriptions/<subscription-id>') 
New-AzRoleDefinition -Role $customRole 
```
 
Nachdem die Rolle erstellt wurde, kann sie im Azure-Portal im Abschnitt **Zugriffssteuerung (IAM)** einem beliebigen Benutzer zugewiesen werden. Wenn Sie diese Rolle einem Benutzer zuweisen, wird die Berechtigung im Dienstbereich zugewiesen. Der Benutzer kann SAS-Token im Namen *jedes beliebigen* Benutzers im Dienst generieren. Diese Rolle muss mindestens dem Administrator des Diensts zugewiesen werden. Der folgende PowerShell-Befehl zeigt, wie Sie die Rolle dem Benutzer `user1` im kleinstmöglichen Bereich zuweisen, um dem Benutzer keine unnötigen Berechtigungen zu erteilen: 

```PowerShell
New-AzRoleAssignment -SignInName "user1@contoso.com" -RoleDefinitionName "APIM New Portal Admin" -Scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>/users/1" 
```

Nach Erteilung der Berechtigungen muss sich der betreffende Benutzer abmelden und erneut beim Azure-Portal anmelden, damit die neuen Berechtigungen wirksam werden.

### <a name="im-seeing-the-unable-to-start-the-portal-see-if-settings-are-specified-correctly--error"></a>Der Fehler `Unable to start the portal. See if settings are specified correctly (...)` wird angezeigt.

Dieser Fehler wird angezeigt, wenn ein Aufruf vom Typ `GET` für `https://<management-endpoint-hostname>/subscriptions/xxx/resourceGroups/xxx/providers/Microsoft.ApiManagement/service/xxx/contentTypes/document/contentItems/configuration?api-version=2018-06-01-preview` nicht erfolgreich war. Der Aufruf wird vom Browser über die Verwaltungsschnittstelle des Portals ausgegeben.

Wenn sich Ihr API Management-Dienst in einem VNET befindet, lesen Sie weiter oben die Frage zur VNET-Konnektivität.

Der Aufruffehler kann auch auf ein SSL-Zertifikat zurückzuführen sein, das einer benutzerdefinierten Domäne zugewiesen ist und dem der Browser nicht vertraut. Sie können die benutzerdefinierte Domäne des Verwaltungsendpunkts entfernen. API Management greift daraufhin auf den Standardendpunkt mit einem vertrauenswürdigen Zertifikat zurück.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum neuen Entwicklerportal:

- [Zugreifen auf das neue Entwicklerportal und Verwalten des Portals](api-management-howto-developer-portal-customize.md)
- [Einrichten einer selbstgehosteten Version des Portals][2]

Weitere Ressourcen:

- [GitHub-Repository mit dem Quellcode][1]
- [Öffentliche Roadmap des Projekts][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects
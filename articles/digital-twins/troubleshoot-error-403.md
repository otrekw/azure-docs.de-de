---
title: 'Fehler bei Azure Digital Twins-Anforderung mit Status: 403 (Unzulässig)'
description: 'Ursachen und Behebungen für „Fehler bei Service Request. Status: 403 (Unzulässig)“ bei Azure Digital Twins.'
ms.service: digital-twins
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.date: 7/20/2020
ms.openlocfilehash: d1c3ad9aa034e6eace5323dd80c5275699a6e728
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331497"
---
# <a name="service-request-failed-status-403-forbidden"></a>Fehler bei Service Request. Status: 403 (Unzulässig)

In diesem Artikel werden die Ursachen und Schritte zur Behebung beim Empfang eines 403-Fehlers bei Service Requests an Azure Digital Twins beschrieben. 

## <a name="symptoms"></a>Symptome

Dieser Fehler kann bei vielen Service Request-Typen auftreten, für die eine Authentifizierung erforderlich ist. Die Auswirkung ist ein Fehler bei der API-Anforderung mit Rückgabe des Fehlerstatus `403 (Forbidden)`.

## <a name="causes"></a>Ursachen

### <a name="cause-1"></a>Ursache 1

In den meisten Fällen zeigt dieser Fehler an, dass Ihre Azure RBAC-Berechtigungen (Role-Based Access Control, rollenbasierte Zugriffssteuerung in Azure) für den Dienst nicht ordnungsgemäß eingerichtet wurden. Viele Aktionen für eine Azure Digital Twins-Instanz erfordern die Rolle *Azure Digital Twins-Besitzer (Vorschau)* **für die Instanz, die Sie verwalten möchten**. 

### <a name="cause-2"></a>Ursache 2

Wenn Sie für die Kommunikation mit Azure Digital Twins eine Client-App verwenden, die sich mit einer [App-Registrierung](how-to-create-app-registration.md) authentifiziert, kann dieser Fehler auftreten, da Ihre App-Registrierung keine eingerichteten Berechtigungen für den Azure Digital Twins-Dienst aufweist.

Die App-Registrierung muss über konfigurierte Zugriffsberechtigungen für die Azure Digital Twins-APIs verfügen. Wenn Ihre Client-App dann bei der App-Registrierung authentifiziert wird, erhält sie die für die App-Registrierung konfigurierten Berechtigungen.

## <a name="solutions"></a>Lösungen

### <a name="solution-1"></a>Lösung 1

Die erste Lösung besteht darin sicherzustellen, dass Ihr Azure-Benutzer über die Rolle _**Azure Digital Twins-Besitzer (Vorschau)**_ für die zu verwaltende Instanz verfügt. Wenn Sie nicht über diese Rolle verfügen, richten Sie sie ein.

Beachten Sie, dass sich diese Rolle von folgenden Rollen unterscheidet:
* Rolle *Besitzer* für das gesamte Azure-Abonnement. *Azure Digital Twins-Besitzer (Vorschau)* ist eine Rolle in Azure Digital Twins und bezieht sich auf diese einzelne Azure Digital Twins-Instanz.
* Rolle *Besitzer* in Azure Digital Twins. Es handelt sich um zwei verschiedene Verwaltungsrollen für Azure Digital Twins, und *Azure Digital Twins-Besitzer (Vorschau)* ist die Rolle, die während der Vorschauphase für die Verwaltung verwendet werden sollte.

#### <a name="check-current-setup"></a>Überprüfen des aktuellen Setups

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

#### <a name="fix-issues"></a>Beheben von Problemen 

Wenn Sie nicht über diese Rollenzuweisung verfügen, sollte eine Person mit der Rolle „Besitzer“ in Ihrem **Azure-Abonnement** den folgenden Befehl ausführen, um Ihrem Azure-Benutzer die Rolle *Azure Digital Twins-Besitzer (Vorschau)* für die **Azure Digital Twins-Instanz** zuzuweisen. 

Wenn Sie ein Besitzer des Abonnements sind, können Sie diesen Befehl selbst ausführen. Bitten Sie andernfalls einen Besitzer, diesen Befehl für Sie auszuführen.

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<your-Azure-AD-email>" --role "Azure Digital Twins Owner (Preview)"
```

Weitere Informationen zu dieser Rollenanforderung und zum Zuweisungsvorgang finden Sie im [Abschnitt *Einrichten der Zugriffsberechtigungen für Ihren Benutzer*](how-to-set-up-instance-CLI.md#set-up-user-access-permissions) unter *Gewusst wie: Einrichten einer Instanz und der Authentifizierung (CLI oder Portal)* .

Wenn Sie bereits über diese Rollenzuweisung verfügen *und* eine Azure AD-App-Registrierung zur Authentifizierung einer Client-App verwenden, können Sie mit der nächsten Lösung fortfahren, falls diese Lösung das 403-Problem nicht gelöst hat.

### <a name="solution-2"></a>Lösung 2

Wenn Sie eine Azure AD-App-Registrierung verwenden, um eine Client-App zu authentifizieren, besteht die zweite mögliche Lösung darin, zu überprüfen, ob die App-Registrierung über Berechtigungen verfügt, die für den Azure Digital Twins-Dienst konfiguriert sind. Wenn diese nicht konfiguriert sind, richten Sie sie ein.

#### <a name="check-current-setup"></a>Überprüfen des aktuellen Setups

Wenn Sie überprüfen möchten, ob die Berechtigungen ordnungsgemäß konfiguriert wurden, navigieren Sie im Azure-Portal zur [Übersichtsseite der Azure AD-App-Registrierung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps). Sie können selbst auf diese Seite gelangen, indem Sie über die Portalsuchleiste nach *App-Registrierungen* suchen.

Wechseln Sie zur Registerkarte *Alle Anwendungen*, um alle App-Registrierungen anzuzeigen, die in Ihrem Abonnement erstellt wurden.

Die soeben erstellte App-Registrierung sollte in der Liste angezeigt werden. Wählen Sie sie aus, um die Details zu öffnen.

:::image type="content" source="media/troubleshoot-error-403/app-registrations.png" alt-text="Seite „App-Registrierungen“ im Azure-Portal":::

Vergewissern Sie sich zunächst, dass die Azure Digital Twins-Berechtigungseinstellungen für die Registrierung ordnungsgemäß festgelegt wurden. Wählen Sie hierzu auf der Menüleiste *Manifest* aus, um den Manifestcode der App-Registrierung anzuzeigen. Scrollen Sie zum unteren Bereich des Codefensters, und suchen Sie nach diesen Feldern unter `requiredResourceAccess`. Die Werte sollten den Werten im nachstehenden Screenshot entsprechen:

:::image type="content" source="media/troubleshoot-error-403/verify-manifest.png" alt-text="Seite „App-Registrierungen“ im Azure-Portal":::

Wählen Sie als Nächstes auf der Menüleiste *API-Berechtigungen* aus, um sich zu vergewissern, dass diese App-Registrierung Lese-/Schreibberechtigungen für Azure Digital Twins enthält. Es sollte ein Eintrag wie der folgende angezeigt werden:

:::image type="content" source="media/troubleshoot-error-403/verify-api-permissions.png" alt-text="Seite „App-Registrierungen“ im Azure-Portal":::

#### <a name="fix-issues"></a>Beheben von Problemen

Wenn etwas anders als beschrieben aussieht, befolgen Sie die Anweisungen zum Einrichten einer App-Registrierung unter [*Vorgehensweise: Erstellen einer App-Registrierung*](how-to-create-app-registration.md).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Einrichtungsschritte zum Erstellen und Authentifizieren einer neuen Azure Digital Twins-Instanz:
* [*Verwenden Einrichten einer Instanz und der Authentifizierung (CLI)* ](how-to-set-up-instance-cli.md)

Informieren Sie sich weiter über die Sicherheit und Berechtigungen in Azure Digital Twins:
* [*Konzepte: Sicherheit für Azure Digital Twins-Lösungen*](concepts-security.md)
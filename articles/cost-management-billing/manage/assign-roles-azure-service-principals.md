---
title: Zuweisen von Rollen zu Azure Enterprise Agreement-Dienstprinzipalnamen
description: Dieser Artikel unterstützt Sie beim Zuweisen von Rollen zu Dienstprinzipalnamen mithilfe von PowerShell und Rest-APIs.
author: bandersmsft
ms.reviewer: ruturajd
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 03/07/2021
ms.author: banders
ms.openlocfilehash: e7f5370e1e387947d196959fef31043ea8f4d3bd
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102508519"
---
# <a name="assign-roles-to-azure-enterprise-agreement-service-principal-names"></a>Zuweisen von Rollen zu Azure Enterprise Agreement-Dienstprinzipalnamen

Sie können Ihre EA-Registrierung (Enterprise Agreement) im [Azure Enterprise Portal](https://ea.azure.com/) verwalten. Sie können verschiedene Rollen erstellen, um Ihre Organisation zu verwalten, Kosten anzuzeigen und Abonnements zu erstellen. Dieser Artikel unterstützt Sie bei der Automatisierung einiger dieser Aufgaben durch die Verwendung von Azure PowerShell und Rest-APIs mit Azure-Dienstprinzipalnamen (Service Prinzipal Names, SPNs).

Bevor Sie beginnen, stellen Sie sicher, dass Sie den Inhalt der folgenden Artikel kennen:

- [Verwalten von Azure Enterprise Agreement-Rollen](understand-ea-roles.md)
- [Anmelden mit Azure PowerShell](/powershell/azure/authenticate-azureps)
- [How to call Azure REST APIs with Postman](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman) (Aufrufen von Azure-REST-APIs mit Postman, in englischer Sprache)

## <a name="create-and-authenticate-your-service-principal"></a>Erstellen und Authentifizieren des Dienstprinzipals

Zum Automatisieren von EA-Aktionen mit einem SPN müssen Sie eine Azure Active Directory-Anwendung (Azure AD) erstellen. Die Authentifizierung kann automatisiert erfolgen. Lesen Sie die folgenden Artikel, und befolgen Sie die Schritte zum Erstellen und Authentifizieren des Dienstprinzipals.

1. [Erstellen eines Dienstprinzipals](../../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal)
2. [Abrufen der Werte für Mandanten-ID und App-ID für die Anmeldung](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)

Der folgende Screenshot zeigt ein Beispiel für die Anwendungsregistrierung.

:::image type="content" source="./media/assign-roles-azure-service-principals/register-an-application.png" alt-text="Screenshot: Registrieren einer Anwendung" lightbox="./media/assign-roles-azure-service-principals/register-an-application.png" :::

### <a name="find-your-spn-and-tenant-id"></a>Suchen des SPN und der Mandanten-ID

Außerdem benötigen Sie die Objekt-ID des SPN und die Mandanten-ID der Anwendung. Sie benötigen die Informationen für Berechtigungszuweisungen in späteren Abschnitten.

Sie finden die Mandanten-ID der Azure AD-Anwendung auf der Übersichtsseite für die Anwendung. Navigieren Sie im Azure-Portal zu Azure Active Directory, und wählen Sie **Unternehmensanwendungen** aus. Suchen Sie die Anwendung.

:::image type="content" source="./media/assign-roles-azure-service-principals/enterprise-application.png" alt-text="Screenshot: Beispiel für eine Unternehmensanwendung" lightbox="./media/assign-roles-azure-service-principals/enterprise-application.png" :::

Wählen Sie die App aus. Im folgenden Beispiel werden die Anwendungs-ID und die Objekt-ID angezeigt.

:::image type="content" source="./media/assign-roles-azure-service-principals/application-id-object-id.png" alt-text="Screenshot: Eine Anwendungs-ID und Objekt-ID für eine Unternehmensanwendung" lightbox="./media/assign-roles-azure-service-principals/application-id-object-id.png" :::

Die Mandanten-ID finden Sie auf der Übersichtsseite von Microsoft Azure AD.

:::image type="content" source="./media/assign-roles-azure-service-principals/tenant-id.png" alt-text="Screenshot, der zeigt, wo Sie die Mandanten-ID anzeigen können" lightbox="./media/assign-roles-azure-service-principals/tenant-id.png" :::

Die Mandanten-ID des Prinzipals wird an verschiedenen Stellen auch als Prinzipal-ID, SPN oder Objekt-ID bezeichnet. Der Wert der Azure AD-Mandanten-ID ähnelt einer GUID mit dem folgenden Format: `11111111-1111-1111-1111-111111111111`.

## <a name="permissions-that-can-be-assigned-to-the-spn"></a>Berechtigungen, die dem SPN zugewiesen werden können

In den nächsten Schritten erteilen Sie der Azure AD-Anwendung die Berechtigung, Aktionen mithilfe einer EA-Rolle auszuführen. Sie können dem SPN nur die folgenden Rollen zuweisen. Die Rollendefinitions-ID wird später in den Zuweisungsschritten genau wie hier angegeben verwendet.

| Rolle | Zulässige Aktionen | Rollendefinitions-ID |
| --- | --- | --- |
| EnrollmentReader | Kann die Nutzung und die Gebühren für alle Konten und Abonnements anzeigen. Kann den Saldo der Azure-Vorauszahlung (früher als Mindestverbrauch bezeichnet) für die Registrierung anzeigen. | 24f8edb6-1668-4659-b5e2-40bb5f3a7d7e |
| DepartmentReader | Herunterladen der Nutzungsdetails für die verwaltete Abteilung. Kann die Nutzung und die Gebühren für die Abteilung anzeigen. | db609904-a47f-4794-9be8-9bd86fbffd8a |
| SubscriptionCreator | Erstellen neuer Abonnements im angegebenen Kontobereich. | a0bcee42-bf30-4d1b-926a-48d21664ef71 |

- Ein EnrollmentReader kann einem SPN nur von einem Benutzer mit der Rolle „EnrollmentWriter“ zugewiesen werden.
- Ein DepartmentReader kann einem SPN nur von einem Benutzer zugewiesen werden, der über die Rolle „EnrollmentWriter“ oder „DepartmentWriter“ verfügt.
- Die Rolle „SubscriptionCreator“ kann einem SPN nur von einem Benutzer zugewiesen werden, der der Kontobesitzer des Registrierungskontos ist.

## <a name="assign-enrollment-account-role-permission-to-the-spn"></a>Zuweisen der Registrierungskonto-Rollenberechtigung zum SPN

Lesen Sie den REST-API-Artikel [Rollenzuweisungen – PUT](/rest/api/billing/2019-10-01-preview/roleassignments/put).

Llicken Sie im Artikel auf **Jetzt testen**, um mit der Verwendung des SPN zu beginnen.

:::image type="content" source="./media/assign-roles-azure-service-principals/put-try-it.png" alt-text="Screenshot der Option „Jetzt testen“ im Artikel „Rollenzuweisungen – PUT“" lightbox="./media/assign-roles-azure-service-principals/put-try-it.png" :::

Melden Sie sich mit Ihrem Konto bei dem Mandanten an, der Zugriff auf die Registrierung hat, der Sie Zugriff zuweisen möchten.

Geben Sie in der API-Anforderung die folgenden Parameter an.

**billingAccountName**

Der Parameter ist die Abrechnungskonto-ID. Sie finden ihn im Azure-Portal auf der Übersichtsseite „Kostenverwaltung + Abrechnung“.

:::image type="content" source="./media/assign-roles-azure-service-principals/billing-account-id.png" alt-text="Screenshot der Abrechnungskonto-ID" lightbox="./media/assign-roles-azure-service-principals/billing-account-id.png" :::

**billingRoleAssignmentName**

Der Parameter ist eine eindeutige GUID, die Sie angeben müssen. Eine GUID kann mit dem PowerShell-Befehl [New-Guid](/powershell/module/microsoft.powershell.utility/new-guid) erstellt werden.

Sie können zum Generieren einer eindeutigen GUID auch die Website [Online GUID / UUID Generator](https://guidgenerator.com/) (in englischer Sprache) verwenden.

**api-version**

Verwenden Sie die Version **2019-10-01-preview**.

Der Anforderungstext enthält JSON-Code, den Sie verwenden müssen.

Verwenden Sie den Beispielanforderungstext in [Rollenzuweisungen – PUT – Beispiele](/rest/api/billing/2019-10-01-preview/roleassignments/put#examples).

Es gibt drei Parameter, die Sie im JSON-Code verwenden müssen.

| Parameter | Ort |
| --- | --- |
| properties.principalId | Siehe [Suchen des SPN und der Mandanten-ID](#find-your-spn-and-tenant-id). |
| properties.principalTenantId | Siehe [Suchen des SPN und der Mandanten-ID](#find-your-spn-and-tenant-id). |
| properties.roleDefinitionId | „/providers/Microsoft.Billing/billingAccounts/{BillingAccountName}/billingRoleDefinitions/24f8edb6-1668-4659-b5e2-40bb5f3a7d7e“ |

Der Name des Abrechnungskontos ist der Parameter, den Sie in den API-Parametern verwendet haben. Dabei handelt es sich um die Registrierungs-ID, die im EA-Portal und Azure-Portal angezeigt wird.

Beachten Sie, dass `24f8edb6-1668-4659-b5e2-40bb5f3a7d7e` die ID der Abrechnungsrollendefinition für einen EnrollmentReader ist.

Klicken Sie auf **Ausführen**, um den Befehl zu starten.

:::image type="content" source="./media/assign-roles-azure-service-principals/roleassignments-put-try-it-run.png" alt-text="Screenshot: Beispiel für eine Rollenzuweisung mit „PUT“ nach Klicken auf „Jetzt testen“ mit Beispielinformationen zum Ausführen des Befehls" lightbox="./media/assign-roles-azure-service-principals/roleassignments-put-try-it-run.png" :::

Die Antwort `200 OK` bedeutet, dass der SPN erfolgreich hinzugefügt wurde.

Jetzt können Sie den SPN (Azure AD-Anwendung mit der Objekt-ID) verwenden, um automatisch auf EA-APIs zuzugreifen. Der SPN verfügt über die Rolle „EnrollmentReader“.

## <a name="assign-the-department-reader-role-to-the-spn"></a>Zuweisen der Rolle „DepartmentReader“ zum SPN

Lesen Sie zunächst den REST-API-Artikel [Zuweisen von Registrierungsrollen für Abteilungen – PUT](/rest/api/billing/2019-10-01-preview/enrollmentdepartmentroleassignments/put).

Klicken Sie im Artikel auf **Jetzt testen**.

:::image type="content" source="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it.png" alt-text="Screenshot: Die Option „Jetzt testen“ im Artikel „Zuweisen von Registrierungsrollen für Abteilungen – PUT“" lightbox="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it.png" :::

Melden Sie sich mit Ihrem Konto bei dem Mandanten an, der Zugriff auf die Registrierung hat, der Sie Zugriff zuweisen möchten.

Geben Sie in der API-Anforderung die folgenden Parameter an.

**billingAccountName**

Dies ist die ID des Abrechnungskontos. Sie finden sie im Azure-Portal auf der Übersichtsseite „Kostenverwaltung + Abrechnung“.

:::image type="content" source="./media/assign-roles-azure-service-principals/billing-account-id.png" alt-text="Screenshot der ID des Abrechnungskontos" lightbox="./media/assign-roles-azure-service-principals/billing-account-id.png" :::

**billingRoleAssignmentName**

Der Parameter ist eine eindeutige GUID, die Sie angeben müssen. Eine GUID kann mit dem PowerShell-Befehl [New-Guid](/powershell/module/microsoft.powershell.utility/new-guid) erstellt werden.

Sie können zum Generieren einer eindeutigen GUID auch die Website [Online GUID / UUID Generator](https://guidgenerator.com/) (in englischer Sprache) verwenden.

**departmentName**

Dies ist die Abteilungs-ID. Die Abteilungs-IDs können im Azure-Portal angezeigt werden. Navigieren Sie zu „Kostenverwaltung + Abrechnung“ > **Abteilungen**.

In diesem Beispiel wurde die Abteilung „ACE“ verwendet. Die ID im Beispiel lautet `84819`.

:::image type="content" source="./media/assign-roles-azure-service-principals/department-id.png" alt-text="Screenshot: Beispiel für eine Abteilungs-ID" lightbox="./media/assign-roles-azure-service-principals/department-id.png" :::

**api-version**

Verwenden Sie die Version **2019-10-01-preview**.

Der Anforderungstext enthält JSON-Code, den Sie verwenden müssen.

Verwenden Sie das Beispiel in [Zuweisen von Registrierungsrollen für Abteilungen – PUT](/billing/2019-10-01-preview/enrollmentdepartmentroleassignments/put). Es gibt drei Parameter, die Sie im JSON-Code verwenden müssen.

| Parameter | Ort |
| --- | --- |
| properties.principalId | Siehe [Suchen des SPN und der Mandanten-ID](#find-your-spn-and-tenant-id). |
| properties.principalTenantId | Siehe [Suchen des SPN und der Mandanten-ID](#find-your-spn-and-tenant-id). |
| properties.roleDefinitionId | „/providers/Microsoft.Billing/billingAccounts/{BillingAccountName}/billingRoleDefinitions/db609904-a47f-4794-9be8-9bd86fbffd8a“ |

Der Name des Abrechnungskontos ist der Parameter, den Sie in den API-Parametern verwendet haben. Dabei handelt es sich um die Registrierungs-ID, die im EA-Portal und Azure-Portal angezeigt wird.

Die Definitions-ID `db609904-a47f-4794-9be8-9bd86fbffd8a` der Abrechnungsrolle gilt für einen DepartmentReader.

Klicken Sie auf **Ausführen**, um den Befehl zu starten.

:::image type="content" source="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it-run.png" alt-text="Screenshot: Beispiel für die Rollenzuweisung mit „PUT“ nach Klicken auf „Jetzt testen“ im Artikel zum Zuweisen von Registrierungsrollen für Abteilungen mit Beispielinformationen zum Ausführen des Befehls" lightbox="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it-run.png" :::

Die Antwort `200 OK` bedeutet, dass der SPN erfolgreich hinzugefügt wurde.

Jetzt können Sie den SPN (Azure AD-Anwendung mit der Objekt-ID) verwenden, um automatisch auf EA-APIs zuzugreifen. Der SPN verfügt über die Rolle „DepartmentReader“.

## <a name="assign-the-subscription-creator-role-to-the-spn"></a>Zuweisen der Rolle „SubscriptionCreator“ zum SPN

Lesen Sie den Artikel [Zuweisen von Registrierungsrollenzuweisungen für Konten – PUT](/rest/api/billing/2019-10-01-preview/enrollmentaccountroleassignments/put).

Klicken Sie im Artikel auf **Jetzt testen**, um dem SPN die Rolle „SubscriptionCreator“ zuzuweisen.

:::image type="content" source="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it.png" alt-text="Screenshot: Die Option „Jetzt testen“ im Artikel „Zuweisen von Registrierungsrollen für Konten – PUT“" lightbox="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it.png" :::

Melden Sie sich mit Ihrem Konto bei dem Mandanten an, der Zugriff auf die Registrierung hat, der Sie Zugriff zuweisen möchten.

Geben Sie in der API-Anforderung die folgenden Parameter an. Lesen Sie den Artikel [Zuweisen von Registrierungsrollen für Konten – PUT – URI-Parameter](/rest/api/billing/2019-10-01-preview/enrollmentaccountroleassignments/put#uri-parameters).

**billingAccountName**

Der Parameter ist die Abrechnungskonto-ID. Sie finden ihn im Azure-Portal auf der Übersichtsseite „Kostenverwaltung + Abrechnung“.

:::image type="content" source="./media/assign-roles-azure-service-principals/billing-account-id.png" alt-text="Screenshot der ID des Abrechnungskontos" lightbox="./media/assign-roles-azure-service-principals/billing-account-id.png" :::

**billingRoleAssignmentName**

Der Parameter ist eine eindeutige GUID, die Sie angeben müssen. Eine GUID kann mit dem PowerShell-Befehl [New-Guid](/powershell/module/microsoft.powershell.utility/new-guid) erstellt werden.

Sie können zum Generieren einer eindeutigen GUID auch die Website [Online GUID / UUID Generator](https://guidgenerator.com/) (in englischer Sprache) verwenden.
**enrollmentAccountName**

Der Parameter ist die Konto-ID. Suchen Sie im Azure-Portal unter „Cost Management + Abrechnung“ im Bereich „Registrierung“ und „Abteilung“ die Konto-ID für den Kontonamen.

In diesem Beispiel wurde ein Konto mit dem Namen „GTM Test Account“ verwendet. Die ID lautet `196987`.

:::image type="content" source="./media/assign-roles-azure-service-principals/account-id.png" alt-text="Screenshot der Konto-ID" lightbox="./media/assign-roles-azure-service-principals/account-id.png" :::

**api-version**

Verwenden Sie die Version **2019-10-01-preview**.

Der Anforderungstext enthält JSON-Code, den Sie verwenden müssen.

Verwenden Sie das Beispiel in [Zuweisen von Registrierungsrollen für Abteilungen – PUT – Beispiele](/rest/api/billing/2019-10-01-preview/enrollmentdepartmentroleassignments/put#putenrollmentdepartmentadministratorroleassignment).

Es gibt drei Parameter, die Sie im JSON-Code verwenden müssen.

| Parameter | Ort |
| --- | --- |
| properties.principalId | Siehe [Suchen des SPN und der Mandanten-ID](#find-your-spn-and-tenant-id). |
| properties.principalTenantId | Siehe [Suchen des SPN und der Mandanten-ID](#find-your-spn-and-tenant-id). |
| properties.roleDefinitionId | „/providers/Microsoft.Billing/billingAccounts/{BillingAccountID}/enrollmentAccounts/196987/billingRoleDefinitions/a0bcee42-bf30-4d1b-926a-48d21664ef71“ |

Der Name des Abrechnungskontos ist der Parameter, den Sie in den API-Parametern verwendet haben. Dabei handelt es sich um die Registrierungs-ID, die im EA-Portal und Azure-Portal angezeigt wird.

Die Definitions-ID `a0bcee42-bf30-4d1b-926a-48d21664ef71` der Abrechnungsrolle gilt für die Rolle „SubscriptionCreator“.

Klicken Sie auf **Ausführen**, um den Befehl zu starten.

:::image type="content" source="./media/assign-roles-azure-service-principals/enrollment-account-role-assignments-put-try-it.png" alt-text="Screenshot: Die Option „Jetzt testen“ im Artikel „Zuweisen von Registrierungsrollen für Konten – PUT“" lightbox="./media/assign-roles-azure-service-principals/enrollment-account-role-assignments-put-try-it.png" :::

Die Antwort `200 OK` bedeutet, dass der SPN erfolgreich hinzugefügt wurde.

Jetzt können Sie den SPN (Azure AD-Anwendung mit der Objekt-ID) verwenden, um automatisch auf EA-APIs zuzugreifen. Der SPN verfügt über die Rolle „SubscriptionCreator“.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Verwaltung im Azure EA-Portal](ea-portal-administration.md).
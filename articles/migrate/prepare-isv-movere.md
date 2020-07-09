---
title: Vorbereiten von Azure Migrate für die Arbeit mit einem ISV-Tool/Movere
description: In diesem Artikel wird beschrieben, wie Sie Azure Migrate für die Arbeit mit einem ISV-Tool oder Movere vorbereiten, sowie die ersten Schritte bei der Verwendung des Tools.
ms.topic: how-to
ms.date: 05/07/2020
ms.openlocfilehash: 9513e783d4f9d7be83f1434d4dd9011844af8993
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682649"
---
# <a name="prepare-to-work-with-an-isv-tool-or-movere"></a>Vorbereiten der Verwendung eines ISV-Tools oder von Movere

Wenn Sie ein [ISV-Tool](migrate-services-overview.md#isv-integration) oder Movere einem Azure Migrate-Projekt hinzugefügt haben, gibt es ein paar durchzuführende Schritte, bevor Sie das Tool verknüpfen und Daten an Azure Migrate senden. 

## <a name="check-azure-ad-permissions"></a>Überprüfen der Azure AD-Berechtigungen

Ihr Azure-Benutzerkonto benötigt die folgenden Berechtigungen:

- Berechtigung zum Registrieren einer Azure Active Directory-App (Azure AD) bei Ihrem Azure-Mandanten
- Berechtigung zum Zuordnen einer Rolle zur Azure AD-App auf Abonnementebene


### <a name="set-permissions-to-register-an-azure-ad-app"></a>Festlegen von Berechtigungen zum Registrieren einer Azure AD-App

1. Überprüfen Sie in Azure AD die Rolle Ihr Konto.
2. Wenn Sie die Benutzerrolle besitzen, wählen Sie im linken Bereich **Benutzereinstellungen** aus, und überprüfen Sie, ob Benutzer Anwendungen registrieren können. Wenn die Einstellung auf **Ja** festgelegt ist, können aller Benutzer im Azure AD-Mandanten eine App registrieren. Wenn die Einstellung auf **Nein** festgelegt ist, können nur Administratorbenutzer Apps registrieren.   
3. Wenn Sie keine Berechtigungen besitzen, kann ein Administratorbenutzer Ihrem Benutzerkonto die Rolle [Anwendungsadministrator](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-administrator) zuweisen, damit Sie die App registrieren können.
4. Nachdem das Tool mit Azure Migrate verknüpft wurde, kann der Administrator die Rolle aus Ihrem Konto entfernen.

### <a name="set-permissions-to-assign-a-role-to-an-azure-ad-app"></a>Festlegen von Berechtigungen zum Zuweisen einer Rolle zu einer Azure AD-App
 
In Ihrem Azure-Abonnement muss Ihr Konto über **Microsoft.Authorization/*/Write**-Zugriff verfügen, um einer Azure AD-App eine Rolle zuzuweisen. 

1. Öffnen Sie im Azure-Portal **Abonnements**.
2. Wählen Sie das relevante Abonnement aus. Wenn es nicht angezeigt wird, wählen Sie den Filter **globale Abonnements** aus. 
3. Wählen Sie **My permissions** (Meine Berechtigungen). Wählen Sie anschließend **Klicken Sie hier, um die vollständigen Zugangsdaten für dieses Abonnement anzuzeigen** aus.
4. Überprüfen Sie in **Rollenzuweisungen** > **Ansicht** die Berechtigungen. Wenn Ihr Konto keine Berechtigungen besitzt, bitten Sie den Abonnementadministrator, Sie der Rolle [Benutzerzugriffsadministrator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) oder [Besitzer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) hinzuzufügen.
 

## <a name="start-using-the-tool"></a>Beginnen mit der Verwendung des Tools

1. Wenn Sie noch keine Lizenz oder eine kostenlose Testversion für das Tool haben, wählen Sie im Tooleintrag in Azure Migrate in **Registrieren** die Option **Weitere Informationen** aus.
2. Befolgen Sie im Tool die Anweisungen, um eine Verknüpfung zwischen dem Tool und dem Azure Migrate-Projekt herzustellen und Daten an Azure Migrate zu senden.

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie die Anweisungen Ihres ISV oder von Movere, um Daten an Azure Migrate zu senden.

   

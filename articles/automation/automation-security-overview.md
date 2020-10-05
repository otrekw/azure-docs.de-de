---
title: Übersicht über die Azure Automation-Kontoauthentifizierung
description: Dieser Artikel enthält eine Übersicht über die Azure Automation-Kontoauthentifizierung.
keywords: Automation-Sicherheit, sicher Automation; Automation-Authentifizierung
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: 8068d6ebe67dee1408420441aacd83726a1986df
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89434264"
---
# <a name="automation-account-authentication-overview"></a>Übersicht über die Automation-Kontoauthentifizierung

Mit Azure Automation können Sie Aufgaben für Ressourcen in Azure, lokal und mit anderen Cloudanbietern, z.B. Amazon Web Services (AWS), automatisieren. Sie können Runbooks verwenden, um Ihre Aufgaben zu automatisieren, oder einen Hybrid Runbook Worker, wenn Sie Azure-fremde Aufgaben verwalten müssen. In beiden Umgebungen werden Berechtigungen für einen sicheren Ressourcenzugriff mit den minimalen Rechten benötigt, die innerhalb des Azure-Abonnements erforderlich sind.

In diesem Artikel werden von Azure Automation unterstützte Authentifizierungsszenarien beschrieben. Außerdem werden die ersten Schritte für die Umgebungen erläutert, die Sie verwalten müssen.

## <a name="automation-account"></a>Automation-Konto

Wenn Sie das erste Mal mit Azure Automation arbeiten, müssen Sie zunächst mindestens ein Automation-Konto erstellen. Mithilfe von Automation-Konten können Sie Ihre Automation-Ressourcen, Runbooks, Objekte und Konfigurationen von den Ressourcen anderer Konten isolieren. Sie können Automation-Konten dazu verwenden, um Ressourcen in separate logische Umgebungen zu trennen. Beispielsweise können Sie ein Konto für die Entwicklung, ein Konto für die Produktion und ein Konto für Ihre lokale Umgebung nutzen. Ein Azure Automation-Konto unterscheidet sich von einem Microsoft-Konto, das unter Ihrem Azure-Abonnement erstellt wird. Eine Einführung in die Erstellung eines Automation-Kontos finden Sie unter [Erstellen eines Azure Automation-Kontos](automation-quickstart-create-account.md).

## <a name="automation-resources"></a>Automation-Ressourcen

Die Automation-Ressourcen für jedes Automation-Konto sind zwar mit einer einzelnen Azure-Region verknüpft, mit dem Konto können jedoch alle Ressourcen in Ihrem Azure-Abonnement verwaltet werden. Automation-Konten werden in erster Linie dann in unterschiedlichen Regionen erstellt, wenn Sie über Richtlinien verfügen, die eine Isolierung von Daten und Ressourcen innerhalb einer spezifischen Region erfordern.

Alle Aufgaben, die Sie für Ressourcen mit Azure Resource Manager und den PowerShell-Cmdlets in Azure Automation erstellen, müssen gegenüber Azure mit Azure Active Directory (Azure AD) basierend auf den Anmeldeinformationen für die Organisationsidentität authentifiziert werden.

## <a name="run-as-account"></a>Ausführendes Konto

Ausführende Konten in Azure Automation sorgen für Authentifizierung, um Ressourcen in Azure mit PowerShell-Cmdlets verwalten zu können. Wenn Sie ein ausführendes Konto erstellen, wird in Azure AD ein neuer Dienstprinzipalbenutzer erstellt, und diesem Benutzer wird auf der Abonnementebene die Rolle „Mitwirkender“ zugewiesen. Für Runbooks, die Hybrid Runbook Worker auf virtuellen Azure-Computern nutzen, können Sie anstelle von ausführenden Konten die [Authentifizierung von Runbooks mit verwalteten Identitäten](automation-hrw-run-runbooks.md#runbook-auth-managed-identities) für die Authentifizierung bei Ihren Azure-Ressourcen verwenden.

## <a name="service-principal-for-run-as-account"></a>Dienstprinzipal für ausführendes Konto

Der Dienstprinzipal für ein ausführendes Konto verfügt standardmäßig über keine Leseberechtigungen für Azure AD. Wenn Sie Berechtigungen zum Lesen oder Verwalten von Azure AD hinzufügen möchten, müssen Sie dem Dienstprinzipal diese Berechtigung unter **API-Berechtigungen** erteilen. Weitere Informationen finden Sie unter [Hinzufügen von Berechtigungen für den Zugriff auf Ihre Web-API](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-your-web-api).

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Die rollenbasierte Zugriffssteuerung ist für Azure Resource Manager verfügbar, um einem Azure AD-Benutzerkonto und ausführenden Konto zulässige Aktionen zu gewähren und den Dienstprinzipal zu authentifizieren. Weitere Informationen zur Entwicklung Ihres Modells zum Verwalten von Automation-Berechtigungen finden Sie im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md) .

## <a name="runbook-authentication-with-hybrid-runbook-worker"></a>Runbookauthentifizierung mit Hybrid Runbook Worker

Für Runbooks, die in Ihrem Rechenzentrum über einen Hybrid Runbook Worker oder für Computingdienste in anderen Cloudumgebungen wie AWS ausgeführt werden, kann nicht das gleiche Verfahren verwendet werden, das normalerweise für die Authentifizierung von Runbooks bei Azure-Ressourcen genutzt wird. Der Grund: Diese Ressourcen werden außerhalb von Azure ausgeführt, weshalb für die Authentifizierung bei Ressourcen, auf die lokal zugegriffen wird, ihre in Automation definierten Sicherheitsanmeldeinformationen verwendet werden müssen. Weitere Informationen zur Runbookauthentifizierung mit Runbook Workern finden Sie unter [Ausführen von Runbooks auf einem Hybrid Runbook Worker](automation-hrw-run-runbooks.md).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen eines Automation-Kontos über das Azure-Portal finden Sie unter [Erstellen eines eigenständigen Azure Automation-Kontos](automation-create-standalone-account.md).
* Wenn Sie Ihr Konto lieber mithilfe einer Vorlage erstellen möchten, finden Sie weitere Informationen unter [Erstellen eines Automation-Kontos mithilfe einer Azure Resource Manager-Vorlage](quickstart-create-automation-account-template.md).
* Informationen zur Authentifizierung mithilfe von Amazon Web Services finden Sie unter [Authentifizieren von Azure Automation-Runbooks mit Amazon Web Services](automation-config-aws-account.md).
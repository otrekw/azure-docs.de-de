---
title: Einführung in die Authentifizierung in Azure Automation
description: Dieser Artikel enthält eine Übersicht über die Automation-Sicherheit und die unterschiedlichen Authentifizierungsmethoden, die für Automation-Konten in Azure Automation verfügbar sind.
keywords: Automation-Sicherheit, sicher Automation; Automation-Authentifizierung
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: 3d3dbaad18f6acbe1ddf17d81f54e4232c838dd7
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82787412"
---
# <a name="introduction-to-authentication-in-azure-automation"></a>Einführen in die Authentifizierung in Azure Automation

Mit Azure Automation können Sie Aufgaben für Ressourcen in Azure, lokal und mit anderen Cloudanbietern, z.B. Amazon Web Services (AWS), automatisieren. Damit ein Runbook die erforderlichen Aktionen ausführen kann, muss es über Berechtigungen für den sicheren Zugriff auf die Ressourcen mit den geringstmöglichen Rechten im Abonnement verfügen.

In diesem Artikel werden die unterschiedlichen Authentifizierungsszenarios beschrieben, die von Azure Automation unterstützt werden. Außerdem werden die ersten Schritte für die Umgebung(en) erläutert, die Sie verwalten müssen.  

## <a name="automation-account-overview"></a>Automation-Konto – Übersicht

Wenn Sie das erste Mal mit Azure Automation arbeiten, müssen Sie zunächst mindestens ein Automation-Konto erstellen. Mithilfe von Automation-Konten können Sie Ihre Automation-Ressourcen (Runbooks, Objekte, Konfigurationen) von den Ressourcen in anderen Automation-Konten isolieren. Sie können Automation-Konten dazu verwenden, um Ressourcen in separate logische Umgebungen zu trennen. Beispielsweise können Sie ein Konto für die Entwicklung, ein Konto für die Produktion und ein Konto für Ihre lokale Umgebung nutzen. Ein Azure Automation-Konto unterscheidet sich von einem Microsoft-Konto, das unter Ihrem Azure-Abonnement erstellt wird.

Die Automation-Ressourcen für jedes Automation-Konto sind mit einer einzelnen Azure-Region verknüpft, aber die Automation-Konten können alle Ressourcen in Ihrem Abonnement verwalten. Der Hauptgrund für das Erstellen von Automation-Konten in unterschiedlichen Regionen ist der, dass Richtlinien eine Isolierung von Daten und Ressourcen innerhalb einer spezifischen Region vorsehen.

Alle Aufgaben, die Sie für Ressourcen mit dem Azure Resource Manager und den Azure-Cmdlets in Azure Automation durchführen, müssen gegenüber Azure mit Azure Active Directory basierend auf den Anmeldeinformationen für die Organisationsidentität authentifiziert werden. In Azure Automation bieten ausführende Konten Authentifizierung für die Verwaltung von Ressourcen in Azure mit Azure-Cmdlets. Beim Erstellen eines ausführenden Kontos wird ein neuer Dienstprinzipalbenutzer in Azure Active Directory (AD) erstellt, und diesem Benutzer wird dann auf Abonnementebene die Rolle „Mitwirkender“ zugewiesen. Für Runbooks, die Hybrid Runbook Worker auf virtuellen Azure-Computern nutzen, können Sie anstelle von ausführenden Konten [Authentifizierung von Runbooks mit verwalteten Identitäten](automation-hrw-run-runbooks.md#runbook-auth-managed-identities) für die Authentifizierung bei Ihren Azure-Ressourcen verwenden.

Der Dienstprinzipal für ein ausführendes Konto hat standardmäßig keine Leseberechtigungen für Azure AD. Wenn Sie Berechtigungen zum Lesen oder Verwalten von Azure AD hinzufügen möchten, müssen Sie dem Dienstprinzipal diese Berechtigung unter **API-Berechtigungen** erteilen. Weitere Informationen finden Sie unter [Hinzufügen von Zugriffsberechtigungen für Web-APIs](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

Die rollenbasierte Zugriffssteuerung ist für Azure Resource Manager verfügbar, um einem Azure AD-Benutzerkonto und ausführenden Konto zulässige Aktionen zu gewähren und den Dienstprinzipal zu authentifizieren. Weitere Informationen zur Entwicklung Ihres Modells zum Verwalten von Automation-Berechtigungen finden Sie im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md) .  

Für Runbooks, die in Ihrem Rechenzentrum über einen Hybrid Runbook Worker oder für Computing-Dienste in anderen Cloudumgebungen wie AWS ausgeführt werden, kann nicht das gleiche Verfahren verwendet werden, das normalerweise für die Authentifizierung von Runbooks bei Azure-Ressourcen genutzt wird. Der Grund: Diese Ressourcen werden außerhalb von Azure ausgeführt, weshalb für die Authentifizierung bei Ressourcen, auf die lokal zugegriffen wird, ihre in Automation definierten Sicherheitsanmeldeinformationen verwendet werden müssen. Weitere Informationen zur Authentifizierung von Runbooks mit Runbookworkern finden Sie unter [Ausführen von Runbooks auf einem Hybrid Runbook Worker](automation-hrw-run-runbooks.md). 

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines eigenständigen Azure Automation-Kontos](automation-create-standalone-account.md)

* [Erstellen eines Automation-Kontos mithilfe einer Azure Resource Manager-Vorlage](automation-create-account-template.md)

* [Authentifizieren von Azure Automation-Runbooks mit Amazon Web Services](automation-config-aws-account.md)

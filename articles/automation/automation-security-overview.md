---
title: Übersicht über die Azure Automation-Kontoauthentifizierung
description: Dieser Artikel enthält eine Übersicht über die Azure Automation-Kontoauthentifizierung.
keywords: Automation-Sicherheit, sicher Automation; Automation-Authentifizierung
services: automation
ms.subservice: process-automation
ms.date: 09/28/2020
ms.topic: conceptual
ms.openlocfilehash: bcb5f61c93bd4c3ff7c0f81ae808807f7deb71df
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91766089"
---
# <a name="automation-account-authentication-overview"></a>Übersicht über die Automation-Kontoauthentifizierung

Mit Azure Automation können Sie Aufgaben für Ressourcen in Azure, lokal und mit anderen Cloudanbietern, z.B. Amazon Web Services (AWS), automatisieren. Mit Runbooks können Sie Ihre Aufgaben automatisieren. Wenn Sie Geschäfts- oder Betriebsprozesse außerhalb von Azure verwalten müssen, verwenden Sie einen Hybrid Runbook Worker. In beiden Umgebungen werden Berechtigungen benötigt, um mit minimalen Rechten sicher auf die Ressourcen zugreifen zu können.

In diesem Artikel werden von Azure Automation unterstützte Authentifizierungsszenarien beschrieben. Außerdem werden die ersten Schritte für die Umgebungen erläutert, die Sie verwalten müssen.

## <a name="automation-account"></a>Automation-Konto

Wenn Sie das erste Mal mit Azure Automation arbeiten, müssen Sie zunächst mindestens ein Automation-Konto erstellen. Mithilfe von Automation-Konten können Sie Ihre Automation-Ressourcen, Runbooks, Objekte und Konfigurationen von den Ressourcen anderer Konten isolieren. Sie können Automation-Konten dazu verwenden, um Ressourcen in separate logische Umgebungen zu trennen. Beispielsweise können Sie ein Konto für die Entwicklung, ein Konto für die Produktion und ein Konto für Ihre lokale Umgebung nutzen. Ein Azure Automation-Konto unterscheidet sich von einem Microsoft-Konto, das unter Ihrem Azure-Abonnement erstellt wird. Eine Einführung in die Erstellung eines Automation-Kontos finden Sie unter [Erstellen eines Azure Automation-Kontos](automation-quickstart-create-account.md).

## <a name="automation-resources"></a>Automation-Ressourcen

Die Automation-Ressourcen für jedes Automation-Konto sind zwar mit einer einzelnen Azure-Region verknüpft, mit dem Konto können jedoch alle Ressourcen in Ihrem Azure-Abonnement verwaltet werden. Automation-Konten werden in erster Linie dann in unterschiedlichen Regionen erstellt, wenn Sie über Richtlinien verfügen, die eine Isolierung von Daten und Ressourcen innerhalb einer spezifischen Region erfordern.

Alle Aufgaben, die Sie für Ressourcen mit Azure Resource Manager und den PowerShell-Cmdlets in Azure Automation erstellen, müssen gegenüber Azure mit Azure Active Directory (Azure AD) basierend auf den Anmeldeinformationen für die Organisationsidentität authentifiziert werden.

## <a name="run-as-accounts"></a>Ausführende Konten

Ausführende Konten in Azure Automation bieten Authentifizierung für die Verwaltung von Azure Resource Manager-Ressourcen oder von Ressourcen, die im klassischen Bereitstellungsmodell bereitgestellt wurden. In Azure Automation gibt es zwei Arten von ausführenden Konten:

* Ausführendes Azure-Konto
* Klassisches ausführendes Azure-Konto

Weitere Informationen zu diesen beiden Bereitstellungsmodellen finden Sie unter [Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/management/deployment-models.md).

>[!NOTE]
>Azure Cloud Solution Provider-Abonnements (Azure CSP) unterstützen nur das Azure Resource Manager-Modell. Dienste, die nicht auf Azure Resource Manager basieren, sind in diesem Programm nicht verfügbar. Wenn Sie ein CSP-Abonnement verwenden, wird kein klassisches ausführendes Azure-Konto erstellt, sondern das ausführende Azure-Konto. Weitere Informationen zu CSP-Abonnements finden Sie unter [Verfügbare Dienste in CSP-Abonnements](/azure/cloud-solution-provider/overview/azure-csp-available-services).

### <a name="run-as-account"></a>Ausführendes Konto

Das ausführende Azure-Konto verwaltet Azure-Ressourcen basierend auf dem Azure Resource Manager-Bereitstellungs- und -Verwaltungsdienst für Azure.

Ein ausführendes Azure-Konto wird für die folgenden Aufgaben verwendet:

* Erstellung einer Azure AD-Anwendung mit einem selbstsignierten Zertifikat, Erstellung eines Dienstprinzipalkonto für die Anwendung in Azure AD und Zuweisung der Rolle [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) für das Konto in Ihrem aktuellen Abonnement. Sie können die Zertifikateinstellung in „Besitzer“ oder eine beliebige andere Rolle ändern. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).

* Erstellt ein Automation-Zertifikatasset mit dem Namen `AzureRunAsCertificate` im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Schlüssel des Zertifikats, der von der Azure AD-Anwendung verwendet wird.

* Erstellt ein Automation-Verbindungsasset mit dem Namen `AzureRunAsConnection` im angegebenen Automation-Konto. Die Verbindungsressource enthält die Anwendungs-ID, die Mandanten-ID, die Abonnement-ID und den Zertifikatfingerabdruck.

### <a name="azure-classic-run-as-account"></a>Klassisches ausführendes Azure-Konto

Das klassische ausführende Azure-Konto verwaltet klassische Azure-Ressourcen basierend auf dem klassischen Bereitstellungsmodell. Sie müssen Co-Administrator für das Abonnement sein, um diese Art Konto zu erstellen oder zu erneuern.

Ein klassisches ausführendes Azure-Konto wird für die folgenden Aufgaben verwendet:

* Erstellen eines Verwaltungszertifikats im Abonnement.

* Erstellen eines Automation-Zertifikatassets mit dem Namen `AzureClassicRunAsCertificate` im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Zertifikatschlüssel, der vom Verwaltungszertifikat verwendet wird.

* Erstellen ein Automation-Verbindungsassets mit dem Namen `AzureClassicRunAsConnection` im angegebenen Automation-Konto. Das Verbindungsasset enthält den Abonnementnamen, die Abonnement-ID und den Namen des Zertifikatassets.

>[!NOTE]
>Ein klassisches ausführendes Azure-Konto wird nicht mehr standardmäßig erstellt, wenn Sie ein Automation-Konto erstellen. Dieses Konto wird einzeln anhand der im Artikel [Verwalten eines ausführenden Azure Automation-Kontos](manage-runas-account.md#create-a-run-as-account-in-azure-portal) beschriebenen Schritte erstellt.

## <a name="service-principal-for-run-as-account"></a>Dienstprinzipal für ausführendes Konto

Der Dienstprinzipal für ein ausführendes Konto verfügt standardmäßig über keine Leseberechtigungen für Azure AD. Wenn Sie Berechtigungen zum Lesen oder Verwalten von Azure AD hinzufügen möchten, müssen Sie dem Dienstprinzipal diese Berechtigung unter **API-Berechtigungen** erteilen. Weitere Informationen finden Sie unter [Hinzufügen von Berechtigungen für den Zugriff auf Ihre Web-API](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-your-web-api).

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Die rollenbasierte Zugriffssteuerung ist für Azure Resource Manager verfügbar, um einem Azure AD-Benutzerkonto und ausführenden Konto zulässige Aktionen zu gewähren und den Dienstprinzipal zu authentifizieren. Weitere Informationen zur Entwicklung Ihres Modells zum Verwalten von Automation-Berechtigungen finden Sie im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md) .

## <a name="runbook-authentication-with-hybrid-runbook-worker"></a>Runbookauthentifizierung mit Hybrid Runbook Worker

Für Runbooks, die in Ihrem Rechenzentrum über einen Hybrid Runbook Worker oder für Computingdienste in anderen Cloudumgebungen wie AWS ausgeführt werden, kann nicht das gleiche Verfahren verwendet werden, das normalerweise für die Authentifizierung von Runbooks bei Azure-Ressourcen genutzt wird. Der Grund: Diese Ressourcen werden außerhalb von Azure ausgeführt, weshalb für die Authentifizierung bei Ressourcen, auf die lokal zugegriffen wird, ihre in Automation definierten Sicherheitsanmeldeinformationen verwendet werden müssen. Weitere Informationen zur Runbookauthentifizierung mit Runbook Workern finden Sie unter [Ausführen von Runbooks auf einem Hybrid Runbook Worker](automation-hrw-run-runbooks.md).

Für Runbooks, die Hybrid Runbook Worker auf virtuellen Azure-Computern nutzen, können Sie anstelle von ausführenden Konten die [Authentifizierung von Runbooks mit verwalteten Identitäten](automation-hrw-run-runbooks.md#runbook-auth-managed-identities) für die Authentifizierung bei Ihren Azure-Ressourcen verwenden.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen eines Automation-Kontos über das Azure-Portal finden Sie unter [Erstellen eines eigenständigen Azure Automation-Kontos](automation-create-standalone-account.md).
* Wenn Sie Ihr Konto lieber mithilfe einer Vorlage erstellen möchten, finden Sie weitere Informationen unter [Erstellen eines Automation-Kontos mithilfe einer Azure Resource Manager-Vorlage](quickstart-create-automation-account-template.md).
* Informationen zur Authentifizierung mithilfe von Amazon Web Services finden Sie unter [Authentifizieren von Azure Automation-Runbooks mit Amazon Web Services](automation-config-aws-account.md).
---
title: Sicherheitsübersicht
description: Sicherheitsinformationen zu Azure Arc-fähigen Servern.
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: be79be3030af76425b54fd683784d0e216ac2cf5
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91329039"
---
# <a name="azure-arc-for-servers-security-overview"></a>Sicherheitsübersicht über Azure Arc für Server

In diesem Artikel werden die Sicherheitskonfiguration und entsprechenden Aspekte beschrieben, die Sie vor der Bereitstellung von Azure Arc-fähigen Servern in Ihrem Unternehmen auswerten sollten.

## <a name="identity-and-access-control"></a>Identität und Zugriffssteuerung

Jeder Azure Arc-fähige Server hat eine verwaltete Identität als Teil einer Ressourcengruppe innerhalb eines Azure-Abonnements, wobei diese Identität den Server darstellt, der lokal oder in einer anderen Cloudumgebung ausgeführt wird. Der Zugriff auf diese Ressource wird durch die standardmäßige [rollenbasierte Zugriffssteuerung von Azure](../../role-based-access-control/overview.md) gesteuert. Auf der Seite [**Zugriffssteuerung (IAM)** ](../../role-based-access-control/role-assignments-portal.md#access-control-iam) im Azure-Portal können Sie überprüfen, wer Zugriff auf Ihren Azure Arc-fähigen Server hat.

:::image type="content" source="./media/security-overview/access-control-page.png" alt-text="Zugriffssteuerung bei Azure Arc-fähigem Server" border="false" lightbox="./media/security-overview/access-control-page.png":::

Benutzer und Anwendungen, denen Zugriff der Rolle [Mitwirkender](../../role-based-access-control/built-in-roles.md#contributor) oder „Administrator“ auf die Ressource gewährt wurde, können Änderungen an der Ressource vornehmen, einschließlich der Bereitstellung oder des Löschens von [Erweiterungen](manage-vm-extensions.md) auf dem Computer. Erweiterungen können beliebige Skripts enthalten, die in einem privilegierten Kontext ausgeführt werden. Daher sollten alle Mitwirkenden an der Azure-Ressource als indirekter Administrator des Servers angesehen werden.

Die Rolle **Onboarding für Azure Connected Machine** ist für das Onboarding im großen Stil verfügbar und kann nur neue Arc-fähige Server in Azure lesen oder erstellen. Sie kann nicht zum Löschen von Servern verwendet werden, die bereits registriert sind, oder zum Verwalten von Erweiterungen. Als bewährte Methode empfehlen wir, diese Rolle nur dem Azure Active Directory-Dienstprinzipal (Azure AD) zuzuweisen, der zum Onboarding von Computern im großen Stil verwendet wird.

Benutzer als Mitglied der Rolle **Ressourcenadministrator für Azure Connected Machine** können Computer lesen, ändern, erneuten integrieren und löschen. Diese Rolle ist für die Unterstützung der Verwaltung von Arc-fähigen Servern konzipiert, jedoch für keine anderen Ressourcen in der Ressourcengruppe oder im Abonnement.

## <a name="agent-security-and-permissions"></a>Agent-Sicherheit und -Berechtigungen

Um den Azure Connected Machine-Agent (azcmagent) zu verwalten, muss Ihr Benutzerkonto unter Windows Mitglied der lokalen Gruppe „Administratoren“ sein. Unter Linux müssen Sie über root-Zugriffsberechtigungen verfügen.

Der Azure Connected Machine-Agent besteht aus drei Diensten, die auf Ihrem Computer ausgeführt werden.

* Der Hybrid Instance Metadata Service (himds) ist für alle Kernfunktionen von Arc verantwortlich. Dies umfasst das Senden von Heartbeats an Azure, das Verfügbarmachen eines lokalen Instanzmetadatendiensts für andere Apps, um mehr über die Azure-Ressourcen-ID des Computers zu erfahren, und das Abrufen von Azure AD-Tokens zum Authentifizieren bei anderen Azure-Diensten. Dieser Dienst wird als nicht privilegiertes virtuelles Dienstkonto unter Windows und als **himds**-Benutzer unter Linux ausgeführt.

* Der Gastkonfigurationsdienst (GCService) ist für das Auswerten von Azure Policy auf dem Computer verantwortlich.

* Der Gastkonfigurations-Erweiterungsdienst (ExtensionService) ist für das Installieren, Aktualisieren und Löschen von Erweiterungen (Agents, Skripts oder andere Software) auf dem Computer verantwortlich.

Die Gastkonfigurations- und Erweiterungsdienste werden unter Windows als „Lokales System“ und unter Linux als „root“ ausgeführt.

## <a name="using-a-managed-identity-with-arc-enabled-servers"></a>Verwenden einer verwalteten Identität mit Arc-fähigen Servern

Standardmäßig kann die von Arc verwendete systemseitig zugewiesene Azure Active Directory-Identität nur zum Aktualisieren des Status des Arc-fähigen Servers in Azure verwendet werden. Beispielsweise der *zuletzt angezeigte* Heartbeat-Status. Sie können der Identität optional weitere Rollen zuweisen, wenn eine Anwendung auf Ihrem Server die systemseitig zugewiesene Identität verwendet, um auf andere Azure-Dienste zuzugreifen.

Während jede Anwendung, die auf dem Computer ausgeführt wird, auf den Hybrid Instance Metadata Service zugreifen kann, können nur autorisierte Anwendungen ein Azure AD-Token für die systemseitig zugewiesene Identität anfordern. Beim ersten Zugriffsversuch auf den Token-URI generiert der Dienst ein zufällig generiertes kryptografisches Blob an einem Speicherort im Dateisystem, das nur vertrauenswürdige Aufrufer lesen können. Der Aufrufer muss dann die Datei lesen (die beweist, dass sie über die entsprechenden Berechtigungen verfügt) und die Anforderung mit dem Dateiinhalt im Autorisierungsheader wiederholen, um erfolgreich ein Azure AD-Token abzurufen.

* Unter Windows muss der Aufrufer Mitglied der lokalen Gruppe **Administratoren** oder der Gruppe **Hybrid-Agent-Erweiterungsanwendungen** sein, um das Blob lesen zu können.

* Unter Linux muss der Aufrufer Mitglied der **himds**-Gruppe sein, um das Blob lesen zu können.

## <a name="using-disk-encryption"></a>Verwenden der Datenträgerverschlüsselung

Der Azure Connected Machine-Agent verwendet die Authentifizierung mit öffentlichem Schlüssel für die Kommunikation mit dem Azure-Dienst. Nachdem Sie einen Server in Azure Arc integriert haben, wird ein privater Schlüssel auf dem Datenträger gespeichert und immer dann verwendet, wenn der Agent mit Azure kommuniziert. Bei Diebstahl kann der private Schlüssel auf einem anderen Server verwendet werden, um mit dem Dienst zu kommunizieren, und so agieren, als ob es sich um den ursprünglichen Server handelt. Dies umfasst das Erhalten des Zugriffs auf die systemseitig zugewiesene Identität sowie auf alle Ressourcen, auf die diese Identität Zugriff hat. Die Datei mit dem privaten Schlüssel ist so geschützt, dass Sie nur das **himds**-Konto zum Lesen darauf Zugriff hat. Um Offlineangriffe zu verhindern, wird dringend empfohlen, die vollständige Datenträgerverschlüsselung (z. B. BitLocker, dm-crypt usw.) auf dem Betriebssystemvolume Ihres Servers zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie Server mit Arc-Unterstützung auf mehreren Hybridcomputern auswerten oder aktivieren, lesen Sie [Übersicht über den Connected Machine-Agent](agent-overview.md), um die Anforderungen, technischen Details zum Agent und Bereitstellungsmethoden zu verstehen.

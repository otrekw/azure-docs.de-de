---
title: Verschieben von Azure App Service-Ressourcen
description: Verwenden Sie Azure Resource Manager, um App Service-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement zu verschieben.
ms.topic: conceptual
ms.date: 12/13/2019
ms.openlocfilehash: 338b0559a5de9468ff60024b88d0f676a9fc3e8b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75474928"
---
# <a name="move-guidance-for-app-service-resources"></a>Anleitung zum Verschieben von App Service-Ressourcen

In diesem Artikel werden die Schritte zum Verschieben von App Service-Ressourcen beschrieben. Für das Verschieben von App Service-Ressourcen in ein neues Abonnement gelten bestimmte Anforderungen.

## <a name="move-across-subscriptions"></a>Verschieben zwischen Abonnements

Beim Verschieben einer Web-App zwischen Abonnements gelten die folgenden Richtlinien:

- Die Zielressourcengruppe darf keine App Service-Ressourcen besitzen. Zu App Service-Ressourcen zählen:
    - Web-Apps
    - App Service-Pläne
    - Hochgeladene oder importierte SSL-Zertifikate
    - App Service-Umgebungen
- Alle App Service-Ressourcen in der Ressourcengruppe müssen zusammen verschoben werden. Beachten Sie, dass App Service-Umgebungen nicht in eine neue Ressourcengruppe oder in ein neues Abonnement verschoben werden können.
- Sie können ein Zertifikat, das an ein Web gebunden ist, ohne Löschen der SSL-Bindungen verschieben, solange das Zertifikat mit allen anderen Ressourcen in der Ressourcengruppe verschoben wird.
- App Service-Ressourcen können nur aus der Ressourcengruppe verschoben werden, in der sie ursprünglich erstellt wurden. Wenn eine App Service-Ressource nicht mehr in ihrer ursprünglichen Ressourcengruppe enthalten ist, verschieben Sie sie zurück zu ihrer ursprünglichen Ressourcengruppe. Verschieben Sie dann die Ressource zwischen Abonnements.

Wenn Sie sich nicht an die ursprüngliche Ressourcengruppe erinnern, können Sie diese mithilfe der Diagnosefunktion finden. Wählen Sie für Ihre Web-App **Diagnose und Problembehandlung** aus. Wählen Sie dann **Konfiguration und Verwaltung** aus.

![„Diagnose“ auswählen](./media/app-service-move-limitations/select-diagnostics.png)

Wählen Sie **Migrationsoptionen** aus.

![Migrationsoptionen auswählen](./media/app-service-move-limitations/select-migration.png)

Wählen Sie die Option für empfohlene Schritte, um die Web-App zu verschieben.

![„Empfohlene Schritte“ auswählen](./media/app-service-move-limitations/recommended-steps.png)

Sie sehen die empfohlenen Maßnahmen, die Sie ergreifen sollten, bevor Sie die Ressourcen verschieben. Die Informationen enthalten die ursprüngliche Ressourcengruppe für die Web-App.

![Empfehlungen](./media/app-service-move-limitations/recommendations.png)

## <a name="move-support"></a>Unterstützung für das Verschieben

Um zu bestimmen, welche App Service-Ressourcen verschoben werden können, sehen Sie sich den Supportstatus für Folgendes an:

- [Microsoft.AppService](../move-support-resources.md#microsoftappservice)
- [Microsoft.CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)
- [Microsoft.DomainRegistration](../move-support-resources.md#microsoftdomainregistration)
- [Microsoft.Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>Nächste Schritte

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../move-resource-group-and-subscription.md).

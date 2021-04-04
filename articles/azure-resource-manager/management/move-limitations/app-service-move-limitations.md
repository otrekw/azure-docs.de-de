---
title: Verschieben von Azure App Service-Ressourcen
description: Verwenden Sie Azure Resource Manager, um App Service-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement zu verschieben.
ms.topic: conceptual
ms.date: 08/10/2020
ms.openlocfilehash: 27555a4616befca41c7e970e947afa1cd1ff7248
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90531371"
---
# <a name="move-guidance-for-app-service-resources"></a>Anleitung zum Verschieben von App Service-Ressourcen

In diesem Artikel werden die Schritte zum Verschieben von App Service-Ressourcen beschrieben. Für das Verschieben von App Service-Ressourcen in ein neues Abonnement gelten bestimmte Anforderungen.

## <a name="move-across-subscriptions"></a>Verschieben zwischen Abonnements

Beim Verschieben einer Web-App zwischen Abonnements gelten die folgenden Richtlinien:

- Die Zielressourcengruppe darf keine App Service-Ressourcen besitzen. Zu App Service-Ressourcen zählen:
    - Web-Apps
    - App Service-Pläne
    - Hochgeladene oder importierte TLS/SSL-Zertifikate
    - App Service-Umgebungen
- Alle App Service-Ressourcen in der Ressourcengruppe müssen zusammen verschoben werden.
- App Service-Umgebungen können nicht in eine neue Ressourcengruppe oder ein neues Abonnement verschoben werden. Sie können jedoch eine Web-App und einen App Service-Plan in ein neues Abonnement verschieben, ohne die App Service-Umgebung zu verschieben. Nach dem Verschieben wird die Web-App nicht mehr in der App Service-Umgebung gehostet.
- Sie können ein Zertifikat, das an ein Web gebunden ist, ohne Löschen der TLS-Bindungen verschieben, solange das Zertifikat mit allen anderen Ressourcen in der Ressourcengruppe verschoben wird.
- App Service-Ressourcen können nur aus der Ressourcengruppe verschoben werden, in der sie ursprünglich erstellt wurden. Wenn eine App Service-Ressource nicht mehr in ihrer ursprünglichen Ressourcengruppe enthalten ist, verschieben Sie sie zurück zu ihrer ursprünglichen Ressourcengruppe. Verschieben Sie dann die Ressource zwischen Abonnements.

Wenn Sie sich nicht an die ursprüngliche Ressourcengruppe erinnern, können Sie diese mithilfe der Diagnosefunktion finden. Wählen Sie für Ihre Web-App **Diagnose und Problembehandlung** aus. Wählen Sie dann **Konfiguration und Verwaltung** aus.

![„Diagnose“ auswählen](./media/app-service-move-limitations/select-diagnostics.png)

Wählen Sie **Migrationsoptionen** aus.

![Migrationsoptionen auswählen](./media/app-service-move-limitations/select-migration.png)

Wählen Sie die Option für empfohlene Schritte, um die Web-App zu verschieben.

![„Empfohlene Schritte“ auswählen](./media/app-service-move-limitations/recommended-steps.png)

Sie sehen die empfohlenen Maßnahmen, die Sie ergreifen sollten, bevor Sie die Ressourcen verschieben. Die Informationen enthalten die ursprüngliche Ressourcengruppe für die Web-App.

![Screenshot zeigt empfohlene Schritte zum Verschieben von Microsoft-Punkt-Webressourcen.](./media/app-service-move-limitations/recommendations.png)

## <a name="move-support"></a>Unterstützung für das Verschieben

Um zu bestimmen, welche App Service-Ressourcen verschoben werden können, sehen Sie sich den Supportstatus für Folgendes an:

- [Microsoft.AppService](../move-support-resources.md#microsoftappservice)
- [Microsoft.CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)
- [Microsoft.DomainRegistration](../move-support-resources.md#microsoftdomainregistration)
- [Microsoft.Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>Nächste Schritte

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../move-resource-group-and-subscription.md).

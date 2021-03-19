---
title: Zugriff auf die Datenebene mit Azure AD RBAC
description: Zugreifen auf die Datenebene mit rollenbasierter Azure Active Directory-Zugriffssteuerung.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 02/04/2021
ms.custom: devx-track-java
ms.openlocfilehash: 2909d40fbc7cb5b310ea17f17a6e683ce47638ce
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102220094"
---
# <a name="access-the-data-plane-with-azure-active-directory-and-role-based-access-control"></a>Zugreifen auf die Datenebene mit rollenbasierter Azure Active Directory-Zugriffssteuerung.

In diesem Artikel wird der Zugriff auf den Azure Spring Cloud-Konfigurationsserver und die Endpunkte der Dienstregistrierung mithilfe der rollenbasierten Zugriffssteuerung (RBAC) von Azure Active Directory (AAD) erläutert.

## <a name="assign-role-to-aad-usergroup-msi-or-service-principal"></a>Zuweisen einer Rolle zu einem AAD-Benutzer bzw. einer -Gruppe, einem MSI oder Dienstprinzipal

Wenn Sie AAD und RBAC verwenden möchten, müssen Sie mithilfe des folgenden Verfahrens einem Benutzer, einer Gruppe oder einem Dienstprinzipal die Rolle *Azure Spring Cloud-Datenleser* zuweisen:

1. Navigieren Sie zur Dienstübersichtsseite Ihrer Dienstinstanz.

2. Klicken Sie auf **Zugriffssteuerung (IAM)** , um das Blatt „Zugriffssteuerung (IAM)“ zu öffnen.

3. Klicken Sie auf die Schaltfläche **Hinzufügen** und auf **Rollenzuweisungen hinzufügen** (zum Hinzufügen ist möglicherweise Autorisierung erforderlich).

4. Suchen Sie unter **Rolle** nach der Option *Azure Spring Cloud-Datenleser*, und wählen Sie sie aus.
5. Weisen Sie Zugriff auf `User, group, or service principal` oder `User assigned managed identity` entsprechend dem Benutzertyp zu. Suchen Sie nach dem Benutzer, und wählen Sie ihn aus.  
6. Klicken Sie auf `Save`.

   ![assign-role](media/access-data-plane-aad-rbac/assign-data-reader-role.png)

## <a name="access-data-plane"></a>Zugreifen auf die Datenebene

Nachdem einem AAD-Benutzer die Rolle *Azure Spring Cloud-Datenleser* zugewiesen wurde, können sich Kunden mit dem Benutzer, dem Dienstprinzipal oder der verwalteten Identität bei der Azure CLI anmelden.  Weitere Informationen zum Abrufen eines Zugriffstokens finden Sie unter [Authentifizieren der Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).  Verwenden Sie dann die folgenden Befehle:

```azurecli
az login
az account get-access-token
```

Derzeit unterstützt die CLI Standardendpunkte des Konfigurationsservers und der Dienstregistrierung. Weitere Informationen finden Sie unter [Produktionsbereite Endpunkte](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints). 

Kunden können auch eine vollständige Liste der unterstützten Endpunkte des Konfigurationsservers und der Dienstregistrierung abrufen, indem Sie auf Endpunkte zugreifen:
* *https://SERVICE_NAME.Root_Endpoint/eureka/actuator/*
* *https://SERVICE_NAME.Root_Endpoint/config/actuator/* 

Das Zugriffstoken im Header stellt die Autorisierung bereit. Nur die GET-Methode wird unterstützt.

Greifen Sie z. B. auf einen Endpunkt wie *https://SERVICE_NAME.Root_Endpoint/eureka/actuator/health* zu, um den Integritätsstatus von eureka anzuzeigen.

Verschiedene Stammendpunkte werden unten entsprechend den verschiedenen Cloudtypen gezeigt.

| Cloud          | Stammendpunkt              |
| -------------- | -------------------------- |
| Öffentlich         | svc.azuremicroservices.io  |
| Mooncake/China | svc.microservices.azure.cn |

Wenn die Antwort *401 Unauthorized* (401 Nicht autorisiert) ist, überprüfen Sie, ob die Rolle erfolgreich zugewiesen wurde.  Es dauert einige Minuten, bis die Rolle wirksam wird. Sie können auch überprüfen, ob das Zugriffstoken abgelaufen ist.

## <a name="next-steps"></a>Nächste Schritte
* [Authentifizieren der Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)
* [Produktionsbereite Endpunkte](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints)

## <a name="see-also"></a>Siehe auch
* [Erstellen von Rollen und Berechtigungen](spring-cloud-howto-permissions.md)
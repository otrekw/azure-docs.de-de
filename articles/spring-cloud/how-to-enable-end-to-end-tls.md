---
title: Aktivieren von End-to-End-TLS (Transport Layer Security)
titleSuffix: Azure Spring Cloud
description: Erfahren Sie, wie Sie End-to-End-TLS (Transport Layer Security) für eine Anwendung aktivieren.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 03/24/2021
ms.custom: devx-track-java
ms.openlocfilehash: 2fce1e619435a8f637619f0fa3ec3a619163c60b
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108135147"
---
# <a name="enable-end-to-end-tls-for-an-application"></a>Aktivieren von End-to-End-TLS für eine Anwendung

In diesem Thema erfahren Sie, wie Sie End-to-End-SSL/TLS aktivieren, um Datenverkehr von einem Eingangscontroller zu Anwendungen zu schützen, die HTTPS unterstützen. Nachdem Sie End-to-End-TLS aktiviert und ein Zertifikat aus dem Schlüsseltresor geladen haben, wird die gesamte Kommunikation innerhalb von Azure Spring Cloud durch TLS geschützt.

   ![Graph der durch TLS gesicherten Kommunikation.](media/enable-end-to-end-tls/secured-tls.png)

## <a name="prerequisites"></a>Voraussetzungen 

- Eine bereitgestellte Azure Spring Cloud-Instanz. Informationen zu den ersten Schritten finden Sie unter [Schnellstart: Starten einer Java Spring-Anwendung mit der Azure CLI](./quickstart.md).
- Wenn Sie mit End-to-End-TLS nicht vertraut sind, finden Sie weitere Informationen im [Beispiel für End-to-End-TLS](https://github.com/Azure-Samples/spring-boot-secure-communications-using-end-to-end-tls-ssl).
- Um die erforderlichen Zertifikate sicher in Spring Boot-Apps zu laden, können Sie [keyvault spring boot starter](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-keyvault-certificates) verwenden.


## <a name="enable-end-to-end-tls-on-an-existing-app"></a>Aktivieren von End-to-End-TLS für eine vorhandene App

Verwenden Sie den Befehl `az spring-cloud app update --enable-end-to-end-tls`, um End-to-End-TLS für eine App zu aktivieren oder zu deaktivieren.

```azurecli
az spring-cloud app update --enable-end-to-end-tls -n app_name -s service_name -g resource_group_name
az spring-cloud app update --enable-end-to-end-tls false -n app_name -s service_name -g resource_group_name
```

## <a name="enable-end-to-end-tls-when-you-bind-custom-domain"></a>Aktivieren von End-to-End-TLS beim Binden einer benutzerdefinierten Domäne

Verwenden Sie den Befehl `az spring-cloud app custom-domain update --enable-end-to-end-tls` oder`az spring-cloud app custom-domain bind --enable-end-to-end-tls`, um End-to-End-TLS für eine App zu aktivieren oder zu deaktivieren.

```azurecli
az spring-cloud app custom-domain update --enable-end-to-end-tls -n app_name -s service_name -g resource_group_name
az spring-cloud app custom-domain bind --enable-end-to-end-tls -n app_name -s service_name -g resource_group_name
```

## <a name="enable-end-to-end-tls-using-azure-portal"></a>End-to-End-TLS über das Azure-Portal aktivieren
Um End-to-End-TLS im [Azure-Portal](https://portal.azure.com/) zu aktivieren, richten Sie zunächst eine App ein und aktivieren dann die Funktion.

1. Erstellen Sie wie gewohnt eine App im Portal. Navigieren Sie im Portal zu dieser App.
2. Scrollen Sie im linken Navigationsbereich nach unten zur Gruppe **Einstellungen**.
3. Wählen Sie **End-to-End TLS**.
4. Schalten Sie **End-to-End TLS** auf *Ja*.

 ![Aktivieren Sie End-to-End-TLS im Portal](./media/enable-end-to-end-tls/enable-tls.png)


## <a name="verify-end-to-end-tls-status"></a>Überprüfen des Status von End-to-End-TLS

Verwenden Sie den Befehl `az spring-cloud app show`, um den Wert von `enableEndToEndTls` zu überprüfen.
```
az spring-cloud app show -n app_name -s service_name -g resource_group_name
```

## <a name="next-steps"></a>Nächste Schritte
* [Zugreifen auf den Konfigurationsserver und die Dienstregistrierung](how-to-access-data-plane-azure-ad-rbac.md)
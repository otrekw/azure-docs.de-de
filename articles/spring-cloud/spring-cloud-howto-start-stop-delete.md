---
title: Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: brendm
ms.openlocfilehash: daa549e248668add54530e90174134c4e0059b3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76276838"
---
# <a name="start-stop-and-delete-your-azure-spring-cloud-application"></a>Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung

In diesem Leitfaden wird erläutert, wie Sie den Zustand einer Anwendung in Azure Spring Cloud mithilfe des Azure-Portals oder der Azure CLI ändern.

## <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Nach dem Bereitstellen einer Anwendung können Sie sie über das Azure-Portal starten, beenden und löschen.

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Spring Cloud-Dienstinstanz.
1. Wählen Sie die Registerkarte **Anwendungsdashboard** aus.
1. Wählen Sie die Anwendung aus, deren Zustand Sie ändern möchten.
1. Auf der Seite **Übersicht** für diese Anwendung wählen Sie eine der Optionen **Starten/Beenden**, **Neu starten** oder **Löschen** aus.

## <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

> [!NOTE]
> Sie können optionale Parameter verwenden und Standardwerte mit der Azure CLI konfigurieren. Weitere Informationen zur Azure CLI finden Sie in der [Referenzdokumentation](spring-cloud-cli-reference.md).  

Installieren Sie mithilfe des folgenden Befehls zuerst die Azure Spring Cloud-Erweiterung für die Azure-Befehlszeilenschnittstelle:

```azurecli
az extension add --name spring-cloud
```

Wählen Sie dann einen der Azure CLI-Vorgänge aus:

* So starten Sie Ihre Anwendung:

    ```azurecli
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So beenden Sie Ihre Anwendung:

    ```azurecli
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So starten Sie Ihre Anwendung neu:

    ```azurecli
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So löschen Sie Ihre Anwendung:

    ```azurecli
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

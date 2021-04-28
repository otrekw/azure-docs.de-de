---
title: Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: f5fcd28f35260bf3b312e089e788f765c0219745
ms.sourcegitcommit: 2e123f00b9bbfebe1a3f6e42196f328b50233fc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108070909"
---
# <a name="start-stop-and-delete-your-azure-spring-cloud-application"></a>Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

In diesem Leitfaden wird erläutert, wie Sie den Zustand einer Anwendung in Azure Spring Cloud mithilfe des Azure-Portals oder der Azure CLI ändern.

## <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Nach dem Bereitstellen einer Anwendung können Sie sie über das Azure-Portal starten, beenden und löschen.

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Spring Cloud-Dienstinstanz.
1. Wählen Sie die Registerkarte **Anwendungsdashboard** aus.
1. Wählen Sie die Anwendung aus, deren Zustand Sie ändern möchten.
1. Auf der Seite **Übersicht** für diese Anwendung wählen Sie eine der Optionen **Starten/Beenden**, **Neu starten** oder **Löschen** aus.

## <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

> [!NOTE]
> Sie können optionale Parameter verwenden und Standardwerte mit der Azure CLI konfigurieren. Weitere Informationen zur Azure CLI finden Sie in der [Referenzdokumentation](/cli/azure/spring-cloud).  

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

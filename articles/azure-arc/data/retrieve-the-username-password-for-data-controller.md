---
title: Abrufen des Benutzernamens und des Kennworts für die Verbindung mit einem Arc-Datencontroller
description: Abrufen des Benutzernamens und des Kennworts für die Verbindung mit einem Arc-Datencontroller
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 8cabb48e3620f1a17d1bb9b87e1646ce2793143b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91761701"
---
# <a name="retrieve-the-user-name-and-password-to-connect-to-the-arc-data-controller"></a>Abrufen des Benutzernamens und des Kennworts für die Verbindung mit einem Arc-Datencontroller

Möglicherweise befinden Sie sich in einer Situation, in der Sie den Benutzernamen und das Kennwort für den Datencontroller abrufen müssen. Diese Befehle benötigen Sie bei der Ausführung, 

```console
azdata login
```

wenn Sie der Kubernetes-Administrator für den Cluster sind. Als solcher verfügen Sie über Berechtigungen zum Ausführen von Befehlen, um aus den Kubernetes-Geheimnisspeichern die Informationen abzurufen, die Azure Arc dort persistent speichert.

> [!NOTE]
>  Wenn Sie einen anderen Namen für den Namespace verwendet haben, in dem der Datencontroller erstellt wurde, stellen Sie sicher, dass Sie den Parameter `-n arc` in den folgenden Befehlen ändern, um den Namen des Namespace zu verwenden, in dem Sie den Datencontroller erstellt haben.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="linux"></a>Linux

Führen Sie den folgenden Befehl aus, um den Benutzernamen abzurufen:

```console
kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['username']}" | base64 -d
```

Führen Sie den folgenden Befehl aus, um das Kennwort abzurufen:

```console
kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['password']}" | base64 -d
```

## <a name="powershell"></a>PowerShell

Führen Sie den folgenden Befehl aus, um den Benutzernamen abzurufen:

```console
[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String((kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['username']}")))
```

Führen Sie den folgenden Befehl aus, um das Kennwort abzurufen:

```console
[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String((kubectl -n arc get secret controller-login-secret -o=jsonpath="{.data['password']}")))
```

## <a name="next-steps"></a>Nächste Schritte

Testen anderer [Szenarien](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/active-directory-domain-services/scenarios.md)

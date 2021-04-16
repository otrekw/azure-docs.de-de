---
title: Aktualisieren der Anmeldeinformationen für einen Dienstprinzipal
description: Aktualisieren der Anmeldeinformationen für einen Dienstprinzipal
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: dnethi
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 12/09/2020
ms.topic: how-to
ms.openlocfilehash: 1c7ccec6228a6dcfb457bda8f6ecd384775d4b27
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "104669541"
---
# <a name="update-service-principal-credentials"></a>Aktualisieren der Anmeldeinformationen für einen Dienstprinzipal

Wenn sich die Anmeldeinformationen des Dienstprinzipals ändern, müssen Sie die geheimen Schlüssel im Datencontroller aktualisieren.

Wenn Sie den Datencontroller z. B. mit bestimmten Werten für die Mandanten-ID, die Client-ID und den geheimen Clientschlüssel für den Dienstprinzipal bereitgestellt haben und dann einen oder mehrere dieser Werte ändern, müssen Sie die Geheimnisse im Datencontroller aktualisieren.  Im Folgenden finden Sie die Anweisungen zum Aktualisieren der Mandanten-ID, der Client-ID und des geheimen Clientschlüssels. 

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="background"></a>Hintergrund

Der Dienstprinzipal wurde unter [Erstellen des Dienstprinzipals](upload-metrics-and-logs-to-azure-monitor.md#create-service-principal) erstellt. 

## <a name="steps"></a>Schritte

1. Greifen Sie im Standard-Editor auf das Geheimnis des Dienstprinzipals zu.

   ```console
   kubectl edit secret/upload-service-principal-secret -n <name of namespace>
   ```

   Führen Sie beispielsweise den folgenden Befehl aus, um das Geheimnis des Dienstprinzipals für einen Datencontroller im `arc`-Namespace zu bearbeiten:

   ```console
   kubectl edit secret/upload-service-principal-secret -n arc
   ```

   Mit dem Befehl `kubecl edit` wird die YML-Datei mit den Anmeldeinformationen im Standard-Editor geöffnet. 


1. Bearbeiten Sie das Geheimnis des Dienstprinzipals. 

   Ersetzen Sie im Standard-Editor die Werte im Abschnitt „data“ durch die aktualisierten Anmeldeinformationen.

   Beispiel:

   ```console
   # Please edit the object below. Lines beginning with a '#' will be ignored,
   # and an empty file will abort the edit. If an error occurs while saving this file will be
   # reopened with the relevant failures.
   #
   apiVersion: v1
   data:
     authority: aHR0cHM6Ly9sb2dpbi5taWNyb3NvZnRvbmxpbmUuY29t
     clientId: NDNiNDcwYrFTGWYzOC00ODhkLTk0ZDYtNTc0MTdkN2YxM2Uw
     clientSecret: VFA2RH125XU2MF9+VVhXenZTZVdLdECXFlNKZi00Lm9NSw==
     tenantId: NzJmOTg4YmYtODZmMRFVBGTJLSATkxYWItMmQ3Y2QwMTFkYjQ3
   kind: Secret
   metadata:
     creationTimestamp: "2020-12-02T05:02:04Z"
     name: upload-service-principal-secret
     namespace: arc
     resourceVersion: "7235659"
     selfLink: /api/v1/namespaces/arc/secrets/upload-service-principal-secret
     uid: 7fb693ff-6caa-4a31-b83e-9bf22be4c112
   type: Opaque
   ```

   Bearbeiten Sie die Werte für `clientID`, `clientSecret` und/oder `tenantID` nach Bedarf. 

> [!NOTE]
>Die Werte müssen Base64-codiert sein. Bearbeiten Sie keine der anderen Eigenschaften.

Wenn für `clientId` `clientSecret` oder `tenantID` ein falscher Wert angegeben wird, finden Sie in den Pod-/Controllercontainerprotokollen `control-xxxx` eine Fehlermeldung wie die folgende:

```output
YYYY-MM-DD HH:MM:SS.mmmm | ERROR | [AzureUpload] Upload task exception: A configuration issue is preventing authentication - check the error message from the server for details.You can modify the configuration in the application registration portal. See https://aka.ms/msal-net-invalid-client for details.  Original exception: AADSTS7000215: Invalid client secret is provided.
```



## <a name="next-steps"></a>Nächste Schritte

[Abrufen des Benutzernamens und des Kennworts für die Verbindung mit einem Arc-Datencontroller](retrieve-the-username-password-for-data-controller.md)

[Erstellen eines Dienstprinzipals](upload-metrics-and-logs-to-azure-monitor.md#create-service-principal)

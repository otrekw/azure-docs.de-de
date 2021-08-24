---
title: Erstellen einer Azure SQL Managed Instance mithilfe von Azure Data Studio
description: Erstellen einer Azure SQL Managed Instance mithilfe von Azure Data Studio
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: dnethi
ms.author: dinethi
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
ms.openlocfilehash: 15d4ba669f736fd1322c137a16d8b79f40cea6d5
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355661"
---
# <a name="create-sql-managed-instance---azure-arc-using-azure-data-studio"></a>Erstellen einer verwalteten SQL-Instanz in Azure Arc mithilfe von Azure Data Studio

In diesem Artikel erhalten Sie Informationen zu den Schritten für das Installieren einer verwalteten Azure SQL-Instanz in Azure Arc mithilfe von Azure Data Studio.

[!INCLUDE [azure-arc-common-prerequisites](../../../includes/azure-arc-common-prerequisites.md)]

[!INCLUDE [use-insider-azure-data-studio](includes/use-insider-azure-data-studio.md)]

## <a name="create-azure-sql-managed-instance-on-azure-arc"></a>Erstellen einer Azure SQL Managed Instance in Azure Arc

- Starten von Azure Data Studio
- Klicken Sie auf der Registerkarte „Verbindungen“ auf die drei Punkte oben links, und wählen Sie dann „Neue Bereitstellung“ aus.
- Klicken Sie bei den Bereitstellungsoptionen auf **Azure SQL Managed Instance – Azure Arc**. 
  > [!NOTE]
  > Möglicherweise werden Sie hier dazu aufgefordert, die geeignete CLI zu installieren, wenn dies noch nicht getan wurde.
- Akzeptieren Sie die Datenschutz- und Lizenzbedingungen, und klicken Sie unten auf **Auswählen**.

- Geben Sie auf dem Blatt „Azure SQL Managed Instance bereitstellen – Azure Arc“ die folgenden Informationen ein:
  - Geben Sie einen Namen für die SQL Server-Instanz ein.
  - Geben Sie ein Kennwort für die SQL Server-Instanz ein, und bestätigen Sie dieses.
  - Wählen Sie die Speicherklasse aus, die sich für Daten eignet.
  - Wählen Sie die Speicherklasse aus, die sich für Protokolle eignet.

- Klicken Sie auf die Schaltfläche **Bereitstellen**.

- Dadurch sollte die Erstellung der Azure SQL Managed Instance – Azure Arc auf dem Datencontroller gestartet werden.

- Nach einigen Minuten sollte die Erstellung erfolgreich abgeschlossen werden.

## <a name="connect-to-azure-sql-managed-instance---azure-arc-from-azure-data-studio"></a>Stellen Sie in Azure Data Studio eine Verbindung zur Azure SQL Managed Instance – Azure Arc her.

- Zeigen Sie mithilfe folgender Befehle alle bereitgestellten Azure SQL Managed Instances an:

```azurecli
az sql mi-arc list --k8s-namespace <namespace> --use-k8s
```

Die Ausgabe sollte folgendermaßen aussehen. Kopieren Sie den ServerEndpoint-Wert (einschließlich der Portnummer) von dort.

```console

Name          Replicas    ServerEndpoint     State
------------  ----------  -----------------  -------
sqlinstance1  1/1         25.51.65.109:1433  Ready
```

- Klicken Sie in Azure Data Studio unter der Registerkarte **Verbindungen** in der Ansicht **Server** auf **Neue Verbindung**.
- Fügen Sie auf dem Blatt **Verbindung** den ServerEndpoint-Wert in das Textfeld „Server“ ein.
- Wählen Sie **SQL-Anmeldung** als Authentifizierungstyp aus.
- Geben Sie als Benutzernamen *sa* ein.
- Geben Sie das Kennwort für das Konto `sa` ein.
- Geben Sie optional den genauen Datenbanknamen ein, um eine Verbindung dazu herzustellen.
- Klicken Sie außerdem optional bei Bedarf auf die Option „Add new server group“ (Neue Servergruppe hinzufügen).
- Klicken Sie auf **Verbinden**, um eine Verbindung zur Azure SQL Managed Instance – Azure Arc herzustellen.

## <a name="next-steps"></a>Nächste Schritte

Als nächsten Schritt können Sie [Ihre SQL-Instanz überwachen](monitor-grafana-kibana.md).

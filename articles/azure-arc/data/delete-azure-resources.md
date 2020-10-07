---
title: Löschen von Azure-Ressourcen
description: Löschen von Azure-Ressourcen
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 46c1df8a175657e20dce984d0c8825ae99170246
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91761650"
---
# <a name="delete-resources-from-azure"></a>Löschen von Azure-Ressourcen

> [!NOTE]
>  Die in diesem Artikel beschriebenen Optionen zum Löschen von Ressourcen sind nicht rückgängig zu machen.

> [!NOTE]
>  Da der einzige Konnektivitätsmodus, der für Azure Arc-fähige Datendienste angeboten wird, derzeit der Indirect Connected-Modus ist, wird er durch das Löschen einer Kubernetes-Instanz nicht aus Azure entfernt. Umgekehrt wird er durch das Löschen einer Azure-Instanz nicht aus Kubernetes entfernt.  Das Löschen einer Ressource ist ein zweistufiger Prozess, der in Zukunft verbessert wird.  In Zukunft wird Kubernetes die allgemeingültige Datenquelle sein, und Azure wird auf der Grundlage dieser Daten aktualisiert.

In einigen Fällen müssen Sie möglicherweise Azure Arc-fähige Datendienstressourcen in Azure Resource Manager (ARM) manuell löschen.  Sie können die Ressourcen mit einer der folgenden Optionen löschen.

- [Löschen von Azure-Ressourcen](#delete-resources-from-azure)
  - [Löschen einer ganzen Ressourcengruppe](#delete-an-entire-resource-group)
  - [Löschen bestimmter Ressourcen in der Ressourcengruppe](#delete-specific-resources-in-the-resource-group)
  - [Löschen von Ressourcen über die Azure CLI](#delete-resources-using-the-azure-cli)
    - [Löschen von SQL Managed Instance-Ressourcen über die Azure CLI](#delete-sql-managed-instance-resources-using-the-azure-cli)
    - [Löschen von PostgreSQL Hyperscale-Servergruppenressourcen über die Azure CLI](#delete-postgresql-hyperscale-server-group-resources-using-the-azure-cli)
    - [Löschen von Azure Arc-Datencontrollerressourcen über die Azure CLI](#delete-azure-arc-data-controller-resources-using-the-azure-cli)
    - [Löschen einer Ressourcengruppe über die Azure CLI](#delete-a-resource-group-using-the-azure-cli)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="delete-an-entire-resource-group"></a>Löschen einer ganzen Ressourcengruppe
Wenn Sie eine bestimmte dedizierte Ressourcengruppe für Azure Arc-fähige Datendienste verwendet haben und Sie *alle* Elemente der Ressourcengruppe löschen möchten, können Sie die Ressourcengruppe samt Inhalt löschen.  

Führen Sie die folgenden Schritte aus, um eine Ressourcengruppe im Azure-Portal zu löschen:

- Navigieren Sie zu der Ressourcengruppe im Azure-Portal, in der die Azure Arc-fähigen Datendienstressourcen erstellt wurden.
- Klicken Sie auf die Schaltfläche **Ressourcengruppe löschen**.
- Bestätigen Sie den Löschvorgang, indem Sie den Ressourcengruppennamen eingeben und auf **Löschen** klicken.

## <a name="delete-specific-resources-in-the-resource-group"></a>Löschen bestimmter Ressourcen in der Ressourcengruppe

Sie können bestimmte Azure Arc-fähige Datendienstressourcen aus einer Ressourcengruppe im Azure-Portal löschen. Führen Sie dazu folgende Schritte aus:

- Navigieren Sie zu der Ressourcengruppe im Azure-Portal, in der die Azure Arc-fähigen Datendienstressourcen erstellt wurden.
- Wählen Sie alle zu löschenden Ressourcen aus.
- Klicken Sie auf die Schaltfläche „Löschen“.
- Bestätigen Sie den Löschvorgang, indem Sie „Ja“ eingeben und auf **Löschen** klicken.

## <a name="delete-resources-using-the-azure-cli"></a>Löschen von Ressourcen über die Azure CLI

Sie können bestimmte Azure Arc-fähige Datendienstressourcen über die Azure CLI löschen.

### <a name="delete-sql-managed-instance-resources-using-the-azure-cli"></a>Löschen von SQL Managed Instance-Ressourcen über die Azure CLI

Um SQL Managed Instance-Ressourcen über die Azure CLI aus Azure zu löschen, ersetzen Sie die Platzhalterwerte im folgenden Befehl und führen den Befehl dann aus.

```console
az resource delete --name <sql instance name> --resource-type Microsoft.AzureData/sqlManagedInstances --resource-group <resource group name>

#Example
#az resource delete --name sql1 --resource-type Microsoft.AzureData/sqlManagedInstances --resource-group rg1
```

### <a name="delete-postgresql-hyperscale-server-group-resources-using-the-azure-cli"></a>Löschen von PostgreSQL Hyperscale-Servergruppenressourcen über die Azure CLI

Um PostgreSQL Hyperscale-Servergruppenressourcen über die Azure CLI aus Azure zu löschen, ersetzen Sie die Platzhalterwerte im folgenden Befehl und führen den Befehl dann aus.

```console
az resource delete --name <postgresql instance name> --resource-type Microsoft.AzureData/postgresInstances --resource-group <resource group name>

#Example
#az resource delete --name pg1 --resource-type Microsoft.AzureData/postgresInstances --resource-group rg1
```

### <a name="delete-azure-arc-data-controller-resources-using-the-azure-cli"></a>Löschen von Azure Arc-Datencontrollerressourcen über die Azure CLI

> [!NOTE]
> Vor dem Löschen eines Azure Arc-Datencontrollers sollten Sie alle Datenbankinstanzressourcen löschen, die von diesem verwaltet werden.

Um Azure Arc-Datencontroller über die Azure CLI aus Azure zu löschen, ersetzen Sie die Platzhalterwerte im folgenden Befehl und führen den Befehl dann aus.

```console
az resource delete --name <data controller name> --resource-type Microsoft.AzureData/dataControllers --resource-group <resource group name>

#Example
#az resource delete --name dc1 --resource-type Microsoft.AzureData/dataControllers --resource-group rg1
```

### <a name="delete-a-resource-group-using-the-azure-cli"></a>Löschen einer Ressourcengruppe über die Azure CLI

Sie können die Azure CLI auch zum [Löschen einer Ressourcengruppe](/azure/azure-resource-manager/management/delete-resource-group) verwenden.

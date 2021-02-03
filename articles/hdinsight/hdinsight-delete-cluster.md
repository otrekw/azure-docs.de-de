---
title: 'Gewusst wie: Löschen eines HDInsight-Clusters – Azure'
description: Informationen zu den verschiedenen Möglichkeiten, einen Azure HDInsight-Cluster zu löschen
ms.service: hdinsight
ms.topic: how-to
ms.custom: H1Hack27Feb2017,hdinsightactive, devx-track-azurecli
ms.date: 11/29/2019
ms.openlocfilehash: 55c19ee9b23d43b0f9988363405d76fa16949ec9
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98946031"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Löschen eines HDInsight-Clusters mithilfe von Browser, PowerShell oder Azure-Befehlszeilenschnittstelle

Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden. In diesem Artikel erfahren Sie, wie Sie einen Cluster mithilfe des [Azure-Portals](https://portal.azure.com), des [Azure PowerShell Az-Modul](/powershell/azure/) und der [Azure CLI](/cli/azure/) löschen.

> [!IMPORTANT]  
> Das Löschen eines HDInsight-Clusters führt nicht zum Löschen der zugeordneten Azure Storage-Konten oder Data Lake Storage-Instanz. Sie können die in diesen Diensten gespeicherten Daten künftig wiederverwenden.

## <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie vom linken Menü zu **Alle Dienste** > **Analytics** > **HDInsight-Cluster**, und wählen Sie Ihren Cluster aus.

3. Wählen Sie in der Standardansicht das Symbol **Löschen** aus. Folgen Sie der Aufforderung, Ihren Cluster zu löschen.

    ![Schaltfläche zum Löschen des HDInsight-Clusters](./media/hdinsight-delete-cluster/hdinsight-delete-cluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Ersetzen Sie im folgenden Code `CLUSTERNAME` durch den Namen Ihres HDInsight-Clusters. Geben Sie über eine PowerShell-Eingabeaufforderung den folgenden Befehl ein, um den Cluster zu löschen:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Ersetzen Sie im folgenden Code `CLUSTERNAME` durch den Namen Ihres HDInsight-Clusters und `RESOURCEGROUP` durch den Namen Ihrer Ressourcengruppe.  Geben Sie über eine Eingabeaufforderung Folgendes ein, um den Cluster zu löschen:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```

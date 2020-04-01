---
title: 'Schnellstart: Erstellen einer Azure Notification Hubs-Instanz mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle eine Azure Notification Hubs-Instanz erstellen.
services: notification-hubs
author: dbradish-microsoft
manager: barbkess
editor: sethmanheim
ms.service: notification-hubs
ms.devlang: azurecli
ms.workload: mobile
ms.topic: quickstart
ms.date: 03/17/2020
ms.author: dbradish
ms.reviewer: sethm
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: 830fd33e19a10ec6472650e3d26fec677b82c3d7
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80069483"
---
# <a name="quickstart-create-an-azure-notification-hub-using-the-azure-cli"></a>Schnellstart: Erstellen einer Azure Notification Hubs-Instanz mithilfe der Azure-Befehlszeilenschnittstelle

Azure Notification Hubs bietet ein benutzerfreundliches, horizontal skalierbares Pushmodul, mit dem Sie von einem beliebigen Back-End (cloudbasiert oder lokal) Benachrichtigungen an sämtliche Plattformen (iOS, Android, Windows, Kindle, Baidu usw.) senden können. Weitere Informationen zu diesem Dienst finden Sie unter [Was ist Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

In dieser Schnellstartanleitung erstellen Sie einen Notification Hub mithilfe der Azure-Befehlszeilenschnittstelle. Im ersten Abschnitt erfahren Sie, wie Sie einen Notification Hub-Namespace erstellen und Zugriffsrichtlinieninformationen für diesen Namespace abfragen. Im zweiten Abschnitt erfahren Sie, wie Sie einen Notification Hub in einem bereits vorhandenen Namespace erstellen.  Außerdem erfahren Sie, wie Sie eine benutzerdefinierte Zugriffsrichtlinie erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Für Notification Hubs ist mindestens die Version 2.0.67 der Azure-Befehlszeilenschnittstelle erforderlich. Führen Sie `az --version` aus, um die installierte Version und die abhängigen Bibliotheken zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

1. Melden Sie sich an.

   Melden Sie sich mit dem Befehl [az login](/cli/azure/reference-index#az-login) an, falls Sie eine lokale Installation der Befehlszeilenschnittstelle verwenden.

    ```azurecli-interactive
    az login
    ```

    Führen Sie die in Ihrem Terminal angezeigten Schritte aus, um den Authentifizierungsprozess abzuschließen.

2. Installieren Sie die Erweiterung für die Azure-Befehlszeilenschnittstelle (Azure CLI).

   Installieren Sie die [Azure CLI-Erweiterung für Notification Hubs](/cli/azure/ext/notification-hub/notification-hub), um die Azure CLI-Befehle für Notification Hubs ausführen zu können.  

    ```azurecli-interactive
    az extension add --name notification-hub
   ```

3. Erstellen Sie eine Ressourcengruppe.

   Azure Notification Hubs muss genau wie alle anderen Azure-Ressourcen in einer Ressourcengruppe bereitgestellt werden. Mit Ressourcengruppen können verwandte Azure-Ressourcen organisiert und verwaltet werden.

   Erstellen Sie für diese Schnellstartanleitung mithilfe des folgenden Befehls vom Typ [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe namens *spnhubrg* am Standort *eastus*:

   ```azurecli-interactive
   az group create --name spnhubrg --location eastus
   ```

## <a name="create-a-notification-hub-namespace"></a>Erstellen eines Notification Hub-Namespace

1. Erstellen Sie einen Namespace für Ihre Notification Hubs.

   Ein Namespace enthält einen oder mehrere Hubs, und der Name muss über alle Azure-Abonnements hinweg eindeutig sein.  Verwenden Sie den Befehl [az notification-hub namespace check-availability](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-check-availability), um die Verfügbarkeit des angegebenen Dienstnamespace zu prüfen.  Führen Sie den Befehl [az notification-hub namespace create](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-create) aus, um einen Namespace zu erstellen.  

   ```azurecli-interactive
   #check availability
   az notification-hub namespace check-availability --name spnhubns

   #create the namespace
   az notification-hub namespace create --resource-group spnhubrg --name spnhubns  --location eastus --sku Free
   ```

2. Listen Sie die Schlüssel und Verbindungszeichenfolgen für Ihre Namespacezugriffsrichtlinie auf.

   Für einen neuen Namespace wird automatisch eine Zugriffsrichtlinie namens **RootManageSharedAccessKey** erstellt.  Jede Zugriffsrichtlinie verfügt über zwei Gruppen von Schlüsseln und Verbindungszeichenfolgen.  Führen Sie den Befehl [az notification-hub namespace authorization-rule list-keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) aus, um die Schlüssel und Verbindungszeichenfolgen für den Namespace aufzulisten.

   ```azurecli-interactive
   az notification-hub namespace authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --name RootManageSharedAccessKey
   ```

## <a name="create-notification-hubs"></a>Erstellen von Notification Hubs

1. Erstellen Sie Ihren ersten Notification Hub.

   In dem neuen Namespace kann nun ein Notification Hub erstellt werden.  Führen Sie den Befehl [az notification-hub create](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-create) aus, um einen Notification Hub zu erstellen.

   ```azurecli-interactive
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name spfcmtutorial1nhub --location eastus --sku Free
   ```

2. Erstellen Sie einen zweiten Notification Hub.

   In einem einzelnen Namespace können mehrere Notification Hubs erstellt werden.  Führen Sie erneut den Befehl `az notification-hub create` aus, um einen zweiten Notification Hub im gleichen Namespace zu erstellen.

   ```azurecli-interactive
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name mysecondnhub --location eastus --sku Free
   ```

## <a name="work-with-access-policies"></a>Verwenden von Zugriffsrichtlinien

1. Erstellen Sie eine neue Autorisierungsregel für einen Notification Hub.

   Für jeden neuen Notification Hub wird automatisch eine Zugriffsrichtlinie erstellt.  Verwenden Sie den Befehl [az notification-hub authorization-rule create](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-create), um eine eigene Zugriffsrichtlinie zu erstellen und anzupassen.

   ```azurecli-interactive
   az notification-hub authorization-rule create --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --rights Listen Send
   ```

2. Listen Sie die Zugriffsrichtlinien für einen Notification Hub auf.

   Verwenden Sie den Befehl [az notification-hub authorization-rule list](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list), um die vorhandenen Zugriffsrichtlinien für einen Notification Hub abzufragen.

   ```azurecli-interactive
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table
   ```

   > [!IMPORTANT]
   > Verwenden Sie in Ihrer Anwendung nicht die Richtlinie **DefaultFullSharedAccessSignature**. Diese ist nur für die Verwendung in Ihrem Back-End vorgesehen.  Verwenden Sie in Ihrer Clientanwendung nur Zugriffsrichtlinien vom Typ **Listen** (Lauschen).

3. Listen Sie die Schlüssel und Verbindungszeichenfolgen für eine Notification Hub-Zugriffsrichtlinie auf.

   Jede Zugriffsrichtlinie verfügt über zwei Gruppen von Schlüsseln und Verbindungszeichenfolgen.  Sie werden später für die Behandlung von Pushbenachrichtigungen benötigt.  Führen Sie den Befehl [az notification-hub authorization-rule list-keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) aus, um die Schlüssel und Verbindungszeichenfolgen für eine Notification Hub-Zugriffsrichtlinie aufzulisten.

   ```azurecli-interactive
   #query the keys and connection strings for DefaultListenSharedAccessSignature
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name DefaultListenSharedAccessSignature --output json

   #query the keys and connection strings for the custom policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --output table
   ```

   > [!NOTE]
   > Ein [Notification Hub-Namespace](/cli/azure/ext/notification-hub/notification-hub/namespace/authorization-rule#ext-notification-hub-az-notification-hub-namespace-authorization-rule-list-keys) und ein [Notification Hub](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) haben jeweils separate Zugriffsrichtlinien.  Achten Sie beim Abfragen von Schlüsseln und Verbindungszeichenfolgen auf die Verwendung der korrekten Azure CLI-Referenz.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, führen Sie den Befehl [az group delete](/cli/azure/group) aus, um die Ressourcengruppe und alle zugehörigen Ressourcen zu löschen.

```azurecli-interactive
az group delete --name spnhubrg
```

## <a name="see-also"></a>Weitere Informationen

Informieren Sie sich über alle Funktionen, die für die Verwaltung von Notification Hubs über die Azure-Befehlszeilenschnittstelle zur Verfügung stehen.

* [Notification Hubs: Vollständige Azure CLI-Referenzliste](/cli/azure/ext/notification-hub/notification-hub)
* [Notification Hubs: Azure CLI-Referenzliste für den Namespace](/cli/azure/ext/notification-hub/notification-hub/namespace)
* [Notification Hubs: Azure CLI-Referenzliste für die Autorisierungsregel](/cli/azure/ext/notification-hub/notification-hub/authorization-rule)
* [Notification Hubs: Azure CLI-Referenzliste für Anmeldeinformationen](/cli/azure/ext/notification-hub/notification-hub/credential)

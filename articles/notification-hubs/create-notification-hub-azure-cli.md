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
ms.date: 05/27/2020
ms.author: dbradish
ms.reviewer: sethm
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: 3014a66c633a4293de8cd6eb325e962366c103b9
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85208262"
---
# <a name="quickstart-create-an-azure-notification-hub-using-the-azure-cli"></a>Schnellstart: Erstellen einer Azure Notification Hubs-Instanz mithilfe der Azure-Befehlszeilenschnittstelle

Azure Notification Hubs bietet ein benutzerfreundliches, horizontal skalierbares Pushmodul, mit dem Sie von einem beliebigen Back-End (cloudbasiert oder lokal) Benachrichtigungen an sämtliche Plattformen (iOS, Android, Windows, Kindle, Baidu usw.) senden können. Weitere Informationen zu diesem Dienst finden Sie unter [Was ist Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

In dieser Schnellstartanleitung erstellen Sie einen Notification Hub mithilfe der Azure-Befehlszeilenschnittstelle. Im ersten Abschnitt erfahren Sie, wie Sie einen Namespace für Benachrichtigungs-Hubs erstellen.  Im zweiten Abschnitt erfahren Sie, wie Sie einen Notification Hub in einem bereits vorhandenen Namespace erstellen.  Außerdem erfahren Sie, wie Sie eine benutzerdefinierte Zugriffsrichtlinie erstellen.  

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Für Notification Hubs ist mindestens die Version 2.0.67 der Azure-Befehlszeilenschnittstelle erforderlich. Führen Sie `az --version` aus, um die installierte Version und die abhängigen Bibliotheken zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

1. Melden Sie sich an.

   Melden Sie sich mit dem Befehl [az login](/cli/azure/reference-index#az-login) an, falls Sie eine lokale Installation der Befehlszeilenschnittstelle verwenden.

    ```azurecli
    az login
    ```

    Führen Sie die in Ihrem Terminal angezeigten Schritte aus, um den Authentifizierungsprozess abzuschließen.

2. Installieren Sie die Erweiterung für die Azure-Befehlszeilenschnittstelle (Azure CLI).

   Bei der Verwendung von Erweiterungsverweisen für die Azure CLI müssen Sie die Erweiterung zunächst installieren.  Mit Azure CLI-Erweiterungen erhalten Sie Zugriff auf experimentelle Befehle und Vorabversionen von Befehlen, die noch nicht als Bestandteil der Kern-CLI bereitgestellt wurden.  Weitere Informationen zu Erweiterungen, u. a. zum Aktualisieren und Deinstallieren, finden Sie unter [Verwenden von Erweiterungen mit der Azure CLI](/cli/azure/azure-cli-extensions-overview).

   Installieren Sie die [Erweiterung für Notification Hubs](/cli/azure/ext/notification-hub/notification-hub) mithilfe des folgenden Befehls:

    ```azurecli
    az extension add --name notification-hub
   ```

3. Erstellen Sie eine Ressourcengruppe.

   Azure Notification Hubs muss genau wie alle anderen Azure-Ressourcen in einer Ressourcengruppe bereitgestellt werden. Mit Ressourcengruppen können verwandte Azure-Ressourcen organisiert und verwaltet werden.

   Erstellen Sie für diese Schnellstartanleitung mithilfe des folgenden Befehls vom Typ [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe namens _spnhubrg_ am Standort _eastus_:

   ```azurecli
   az group create --name spnhubrg --location eastus
   ```

## <a name="create-a-notification-hub-namespace"></a>Erstellen eines Notification Hub-Namespace

1. Erstellen Sie einen Namespace für Ihre Benachrichtigungs-Hubs.

   Ein Namespace enthält mindestens einen Hub, und **der Name muss über alle Azure-Abonnements hinweg eindeutig und mindestens sechs Zeichen lang sein**.  Verwenden Sie den Befehl [az notification-hub namespace check-availability](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-check-availability), um die Verfügbarkeit eines Namens zu prüfen.

   ```azurecli
   az notification-hub namespace check-availability --name spnhubns
   ```

   Die Azure CLI reagiert mit der folgenden Konsolenausgabe auf Ihre Verfügbarkeitsanfrage:

   ```output
   {
   "id": "/subscriptions/yourSubscriptionID/providers/Microsoft.NotificationHubs/checkNamespaceAvailability",
   "isAvailable": true,
   "location": null,
   "name": "spnhubns",
   "properties": false,
   "sku": null,
   "tags": null,
   "type": "Microsoft.NotificationHubs/namespaces/checkNamespaceAvailability"
   }
   ```

   Beachten Sie die zweite Zeile in der Antwort der Azure CLI: `"isAvailable": true`.  In dieser Zeile ist `false` angegeben, wenn der gewünschte Name, den Sie für den Namespace angegeben haben, verfügbar ist.  Nachdem Sie die Verfügbarkeit des Namens bestätigt haben, führen Sie den Befehl [az notification-hub namespace create](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-create) aus, um Ihren Namespace zu erstellen.  

   ```azurecli
   az notification-hub namespace create --resource-group spnhubrg --name spnhubns  --location eastus --sku Free
   ```

   Wenn der Wert für `--name`, den Sie für den Befehl `az notification-hub namespace create` angegeben haben, nicht verfügbar ist oder die [Benennungsregeln und -einschränkungen für Azure-Ressourcen](/azure/azure-resource-manager/management/resource-name-rules) nicht erfüllt, reagiert die Azure CLI mit der folgenden Konsolenausgabe:

   ```output
   #the name is not available
   The specified name is not available. For more information visit https://aka.ms/eventhubsarmexceptions.

   #the name is invalied
   The specified service namespace is invalid.
   ```

   Wenn der erste von Ihnen ausprobierte Name nicht erfolgreich ist, wählen Sie einen anderen Namen für Ihren neuen Namespace aus, und führen Sie den Befehl `az notification-hub namespace create` erneut aus.

   > [!NOTE]
   > Ab diesem Schritt müssen Sie den Wert des Parameters `--namespace` in jedem Azure CLI-Befehl ersetzen, den Sie aus dieser Schnellstartanleitung kopieren.

2. Rufen Sie eine Liste der Namespaces ab.

   Verwenden Sie den Befehl [az notification-hub namespace list](/cli/azure/ext/notification-hub/notification-hub/namespace?view=azure-cli-latest#ext-notification-hub-az-notification-hub-namespace-list), um die Details zum neuen Namespace anzuzeigen.  Der Parameter `--resource-group` ist optional, wenn Sie alle Namespaces für ein Abonnement anzeigen möchten.

   ```azurecli
   az notification-hub namespace list --resource-group spnhubrg
   ```

## <a name="create-notification-hubs"></a>Erstellen von Notification Hubs

1. Erstellen Sie Ihren ersten Notification Hub.

   In dem neuen Namespace können nun Benachrichtigungs-Hubs erstellt werden.  Führen Sie den Befehl [az notification-hub create](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-create) aus, um einen Notification Hub zu erstellen.

   ```azurecli
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name spfcmtutorial1nhub --location eastus --sku Free
   ```

2. Erstellen Sie einen zweiten Notification Hub.

   In einem einzelnen Namespace können mehrere Notification Hubs erstellt werden.  Führen Sie erneut den Befehl `az notification-hub create` aus, um einen zweiten Notification Hub im gleichen Namespace zu erstellen.

   ```azurecli
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name mysecondnhub --location eastus --sku Free
   ```

3. Rufen Sie eine Liste der Benachrichtigungs-Hubs ab.

   Die Azure CLI gibt mit jedem ausgeführten Befehl eine Erfolgsmeldung oder eine Fehlermeldung zurück. Sie haben jedoch auch die Möglichkeit, eine Liste der Benachrichtigungs-Hubs abzurufen.  Zu diesem Zweck wurde der Befehl [az notification-hub list](/cli/azure/ext/notification-hub/notification-hub?view=azure-cli-latest#ext-notification-hub-az-notification-hub-list) entworfen.

   ```azurecli
   az notification-hub list --resource-group spnhubrg --namespace-name spnhubns --output table
   ```

## <a name="work-with-notification-hub-access-policies"></a>Verwenden von Zugriffsrichtlinien für Benachrichtigungs-Hubs

1. Listen Sie die Zugriffsrichtlinien für einen Notification Hub auf.

   Azure Notification Hubs nutzt [SAS-Sicherheit (Shared Access Signature)](/azure/notification-hubs/notification-hubs-push-notification-security) durch die Verwendung von Zugriffsrichtlinien.  Zwei Richtlinien werden automatisch erstellt, wenn Sie einen Benachrichtigungs-Hub erstellen.  Die Verbindungszeichenfolgen aus diesen Richtlinien sind zum Konfigurieren von Pushbenachrichtigungen erforderlich.  Mit dem Befehl [az  notification-hub authorization-rule list](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list) wird eine Liste mit Richtliniennamen und den entsprechenden Ressourcengruppen bereitgestellt.

   ```azurecli
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table
   ```

   > [!IMPORTANT]
   > Verwenden Sie in Ihrer Anwendung nicht die Richtlinie _DefaultFullSharedAccessSignature_. Diese ist nur für die Verwendung in Ihrem Back-End vorgesehen.  Verwenden Sie in Ihrer Clientanwendung nur Zugriffsrichtlinien vom Typ `Listen`.

2. Erstellen Sie eine neue Autorisierungsregel für einen Notification Hub.

   Wenn Sie weitere Autorisierungsregeln mit aussagekräftigen Namen erstellen möchten, können Sie eigene Zugriffsrichtlinien erstellen und anpassen. Verwenden Sie dazu den Befehl [az notification-hub authorization-rule create](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-create).  Der Parameter `--rights` ist eine durch Leerzeichen getrennte Liste der Berechtigungen, die Sie zuweisen möchten.

   ```azurecli
   az notification-hub authorization-rule create --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --rights Listen Manage Send
   ```

3. Listen Sie die Schlüssel und Verbindungszeichenfolgen für eine Notification Hub-Zugriffsrichtlinie auf.

   Jede Zugriffsrichtlinie verfügt über zwei Gruppen von Schlüsseln und Verbindungszeichenfolgen.  Sie werden später zum [Konfigurieren eines Benachrichtigungs-Hubs](/azure/notification-hubs/configure-notification-hub-portal-pns-settings) benötigt.  Führen Sie den Befehl [az notification-hub authorization-rule list-keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) aus, um die Schlüssel und Verbindungszeichenfolgen für eine Notification Hub-Zugriffsrichtlinie aufzulisten.

   ```azurecli
   #query the keys and connection strings for DefaultListenSharedAccessSignature
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name DefaultListenSharedAccessSignature --output table
   ```

   ```azurecli
   #query the keys and connection strings for a custom policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --output table
   ```

   > [!NOTE]
   > Ein [Notification Hub-Namespace](/cli/azure/ext/notification-hub/notification-hub/namespace/authorization-rule#ext-notification-hub-az-notification-hub-namespace-authorization-rule-list-keys) und ein [Notification Hub](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) haben jeweils separate Zugriffsrichtlinien.  Achten Sie beim Abfragen von Schlüsseln und Verbindungszeichenfolgen auf die Verwendung der korrekten Azure CLI-Referenz.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, führen Sie den Befehl [az group delete](/cli/azure/group) aus, um die Ressourcengruppe und alle zugehörigen Ressourcen zu löschen.

```azurecli
az group delete --name spnhubrg
```

## <a name="next-steps"></a>Nächste Schritte

* In dieser Schnellstartanleitung haben Sie einen Notification Hub erstellt. Unter [Schnellstart: Einrichten von Pushbenachrichtigungen in einem Notification Hub](configure-notification-hub-portal-pns-settings.md) erfahren Sie, wie Sie den Hub mit PNS-Einstellungen (Platform Notification System, Plattformbenachrichtigungssystem) konfigurieren.

* Informieren Sie sich über die umfassenden Funktionen, die für die Verwaltung von Benachrichtigungs-Hubs über die Azure-Befehlszeilenschnittstelle zur Verfügung stehen.

  [Notification Hubs: Vollständige Referenzliste](/cli/azure/ext/notification-hub/notification-hub)

  [Notification Hubs: Namespacereferenzliste](/cli/azure/ext/notification-hub/notification-hub/namespace)

  [Notification Hubs: Referenzliste für die Autorisierungsregel](/cli/azure/ext/notification-hub/notification-hub/authorization-rule)

  [Notification Hubs: Referenzliste für Anmeldeinformationen](/cli/azure/ext/notification-hub/notification-hub/credential)

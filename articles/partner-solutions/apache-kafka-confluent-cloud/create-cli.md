---
title: 'Erstellen von Apache Kafka in Confluent Cloud über Azure CLI: Azure-Partnerlösungen'
description: In diesem Artikel wird beschrieben, wie Sie Azure CLI verwenden, um eine Instanz von Apache Kafka in Confluent Cloud zu erstellen.
ms.service: partner-services
ms.topic: quickstart
ms.date: 06/07/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: a9c49b1a19cbed080255492b90554ce0b138c3bd
ms.sourcegitcommit: 096e7972e2a1144348f8d648f7ae66154f0d4b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2021
ms.locfileid: "112534792"
---
# <a name="quickstart-get-started-with-apache-kafka-for-confluent-cloud---azure-cli"></a>Schnellstart: Erste Schritte mit Apache Kafka in Confluent Cloud - Azure CLI

In dieser Schnellstartanleitung verwenden Sie Azure Marketplace und Azure CLI, um eine Instanz von Apache Kafka in Confluent Cloud zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto. Falls Sie kein aktives Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.
- Sie müssen für Ihr Azure-Abonnement über die Rolle _Besitzer_ oder _Mitwirkender_ verfügen. Die Integration zwischen Azure und Confluent kann nur von Benutzern eingerichtet werden, für die Zugriff als _Besitzer_ oder _Mitwirkender_ gewährt wurde. [Vergewissern Sie sich zunächst, dass Sie über die entsprechende Zugriffsberechtigung verfügen](../../role-based-access-control/check-access.md).

## <a name="find-offer"></a>Suchen nach dem Angebot

Verwenden Sie das Azure-Portal, um nach der Anwendung „Apache Kafka in Confluent Cloud“ zu suchen.

1. Navigieren Sie in einem Webbrowser zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich an.

1. Falls Sie während einer der letzten Sitzungen den **Marketplace** besucht haben, können Sie das Symbol aus den verfügbaren Optionen auswählen. Suchen Sie andernfalls nach _Marketplace_.

    :::image type="content" source="media/marketplace.png" alt-text="Marketplace-Symbol":::

1. Auf der Seite **Marketplace** werden je nach gewünschtem Plan zwei Optionen angezeigt. Sie können sich für einen Plan mit nutzungsbasierter Bezahlung oder einen Vertragsplan registrieren. Die nutzungsbasierte Bezahlung ist öffentlich verfügbar. Der Vertragsplan ist für Kunden verfügbar, für die ein privates Angebot genehmigt wurde.

   - Suchen Sie für Kunden mit **nutzungsbasierter Bezahlung** nach _Apache Kafka in Confluent Cloud_. Wählen Sie das Angebot „Apache Kafka in Confluent Cloud“ aus.

     :::image type="content" source="media/search-pay-as-you-go.png" alt-text="Suchen nach dem Azure Marketplace-Angebot":::

   - Wählen Sie für Kunden mit **Vertragsplan** den Link zum **Anzeigen von privaten Angeboten** aus. Beim Vertragsplan müssen Sie sich bei der Registrierung zur Zahlung eines Mindestausgabenbetrags verpflichten. Verwenden Sie diese Option nur, wenn Sie wissen, dass Sie den Dienst längere Zeit benötigen.

     :::image type="content" source="media/view-private-offers.png" alt-text="Anzeigen von privaten Angeboten":::

     Suchen Sie nach _Apache Kafka in Confluent Cloud_.

     :::image type="content" source="media/select-from-private-offers.png" alt-text="Auswählen des privaten Angebots":::

## <a name="create-resource"></a>Erstellen von Ressourcen

Nachdem Sie das Angebot für Apache Kafka in Confluent Cloud ausgewählt haben, können Sie mit der Einrichtung der Anwendung beginnen.

Bereiten Sie zunächst Ihre Umgebung für die Azure-Befehlszeilenschnittstelle vor:

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../../includes/azure-cli-prepare-your-environment-no-header.md)]

Verwenden Sie nach der Anmeldung den Befehl [az confluent organization create](/cli/azure/confluent/organization#az_confluent_organization_create), um die neue Organisationsressource zu erstellen:

```azurecli
az confluent organization create --name "myOrganization" --resource-group "myResourceGroup" \
 --location "my location" \ 
 --offer-detail id="string" plan-id="string" plan-name="string" publisher-id="string" term-unit="string" \ 
 --user-detail email-address="contoso@microsoft.com" first-name="string" last-name="string" \ 
 --tags Environment="Dev" 
```

> [!NOTE]
> Wenn der Befehl Informationen zurückgeben soll, bevor der Erstellungsvorgang abgeschlossen ist, fügen Sie den optionalen Parameter `--no-wait` hinzu. Der Vorgang wird ausgeführt, bis die Erstellung der Confluent-Organisation abgeschlossen wurde.
 
Verwenden Sie den Befehl [az confluent organization wait](/cli/azure/confluent/organization#az_confluent_organization_wait) um die CLI-Ausführung anzuhalten, bis das spezifische Ereignis oder die spezifische Bedingung der Organisation auftritt. So warten Sie beispielsweise, bis eine Organisation erstellt wurde:

```azurecli
az confluent organization wait --name "myOrganization" --resource-group "myResourceGroup" --created
```

Um eine Liste vorhandener Organisationen anzuzeigen, verwenden Sie den Befehl [az confluent organization list](/cli/azure/confluent/organization#az_confluent_organization_list).

Sie können alle Organisationen in Ihrem Abonnement anzeigen:

```azurecli
az confluent organization list
```

Oder Sie zeigen die Organisationen in einer Ressourcengruppe an:

```azurecli
az confluent organization list --resource-group "myResourceGroup"
```

Verwenden Sie den Befehl [az confluent organization show](/cli/azure/confluent/organization#az_confluent_organization_show), um die Eigenschaften einer bestimmten Organisation anzuzeigen.

Sie können die Organisationen nach Namen anzeigen:

```azurecli
az confluent organization show --name "myOrganization" --resource-group "myResourceGroup"
```

Oder Sie zeigen die Organisationen nach Ressourcen-ID an:

```azurecli
az confluent organization show --ids "/subscriptions/{SubID}/resourceGroups/{myResourceGroup}/providers/Microsoft.Confluent/organizations/{myOrganization}"
```

Falls Sie eine Fehlermeldung erhalten, helfen Ihnen die Informationen unter [Problembehandlung: Apache Kafka in Confluent Cloud-Lösungen](troubleshoot.md) weiter.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten der Confluent Cloud-Ressource](manage.md)

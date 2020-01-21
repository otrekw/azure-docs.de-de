---
title: Hinzufügen von Artefakten zu Integrationsdienstumgebungen
description: Hinzufügen von Logik-Apps, Integrationskonten und benutzerdefinierten Connectors zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) für den Zugriff auf Azure-VNETs (virtual networks, virtuelle Netzwerke)
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: c597bc4430e4390f0e29e4fe8ae4014521e1ae74
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75732230"
---
# <a name="add-artifacts-to-your-integration-service-environment-ise-in-azure-logic-apps"></a>Hinzufügen von Artefakten zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps

Fügen Sie nach der Erstellung einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) Artefakte wie Logik-Apps, Integrationskonten und Connectors hinzu, damit diese auf die Ressourcen in Ihrem virtuellen Azure-Netzwerk zugreifen können. Beispielsweise werden verwaltete ISE-Connectors, die nach dem Erstellen der ISE verfügbar werden, nicht automatisch im Logik-App-Designer angezeigt. Bevor Sie diese ISE-Connectors verwenden können, müssen Sie [diese Connectors manuell Ihrer ISE hinzufügen und bereitstellen](#add-ise-connectors-environment), damit Sie im Logik-App-Designer angezeigt werden.

## <a name="prerequisites"></a>Voraussetzungen

* ein Azure-Abonnement Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die ISE, die Sie für die Ausführung Ihrer Logik-Apps erstellt haben. Besitzen Sie keine ISE, [erstellen Sie zuerst eine](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-apps"></a>Erstellen von Logik-Apps

Führen Sie die folgenden Schritte aus, um Logik-Apps zu erstellen, die in Ihrer Integrationsdienstumgebung (ISE) ausgeführt werden:

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Logik-Apps** > **Hinzufügen** aus.

   ![Hinzufügen einer neuen Logik-App zur ISE](./media/add-artifacts-integration-service-environment-ise/add-logic-app-to-ise.png)

   Oder

   Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Integration** > **Logik-App**.

1. Geben Sie den Namen, das Azure-Abonnement und die Azure-Ressourcengruppe (neu oder vorhanden) an, die Sie für Ihre Logik-App verwenden möchten.

1. Wählen Sie in der Liste **Speicherort** im Abschnitt **Integrationsdienstumgebungen** Ihre ISE aus, z. B.:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-logic-app-with-integration-service-environment.png)

   > [!IMPORTANT]
   > Falls Sie Ihre Logik-Apps mit einem Integrationskonto verwenden möchten, muss für diese Logik-Apps und das Integrationskonto dieselbe ISE verwendet werden.

1. Fahren Sie wie gewohnt mit der [Erstellung Ihrer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) fort.

   Informationen zu den Unterschieden, wie Trigger und Aktionen funktionieren und wie sie bezeichnet werden, wenn Sie eine Integrationsdienstumgebung verwenden, im Vergleich zum globalen Logic Apps-Dienst, finden Sie unter [„Isoliert“ gegenüber „Global“ in der Übersicht zur Integrationsdienstumgebung](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#difference).

1. Informationen zum Verwalten von Logik-Apps und API-Verbindungen in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-accounts"></a>Erstellen von Integrationskonten

Basierend auf der während der Erstellung gewählten [ISE-SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) umfasst Ihre ISE die Nutzung des jeweiligen Integrationskontos ohne weitere Kosten. Logik-Apps, die in einer Integrationsdienstumgebung (ISE) enthalten sind, können nur auf Integrationskonten verweisen, die sich in derselben ISE befinden. Damit ein Integrationskonto mit Logik-Apps in einer ISE funktioniert, muss sowohl für das Integrationskonto als auch für die Logik-Apps *dieselbe Umgebung* als Speicherort verwendet werden. Weitere Informationen zu Integrationskonten und ISEs finden Sie unter [Integrationskonten mit ISE](connect-virtual-network-vnet-isolated-environment-overview.md#create-integration-account-environment).

Führen Sie die folgenden Schritte aus, um ein Integrationskonto zu erstellen, das eine ISE verwendet:

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Integrationskonten** > **Hinzufügen** aus.

   ![Hinzufügen eines neuen Integrationskontos zur ISE](./media/add-artifacts-integration-service-environment-ise/add-integration-account-to-ise.png)

   Oder

   Wählen Sie im Hauptmenü von Azure die Optionen **Ressource erstellen** > **Integration** > **Integrationskonto** aus.

1. Geben Sie den Namen, das Azure-Abonnement, die Azure-Ressourcengruppe (neu oder vorhanden) und den Tarif für Ihr Integrationskonto an.

1. Wählen Sie in der Liste **Speicherort** im Abschnitt **Integrationsdienstumgebungen** dieselbe ISE aus, die von Ihren Logik-Apps verwendet wird, z. B.:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-integration-account-with-integration-service-environment.png)

1. [Verknüpfen Sie Ihre Logik-App wie gewohnt mit Ihrem Integrationskonto.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)

1. Fügen Sie Ihrem Integrationskonto anschließend Artefakte hinzu, etwa [Parteien](../logic-apps/logic-apps-enterprise-integration-partners.md) und [Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md).

1. Informationen zum Verwalten von Integrationskonten in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps.)

<a name="add-ise-connectors-environment"></a>

## <a name="add-ise-connectors"></a>Hinzufügen von ISE-Connectors

Von Microsoft verwaltete ISE-Connectors, die nach dem Erstellen der ISE verfügbar werden, werden nicht automatisch in der Connectorauswahl im Logik-App-Designer angezeigt. Bevor Sie diese ISE-Connectors verwenden können, müssen Sie diese Connectors manuell Ihrer ISE hinzufügen und bereitstellen, damit Sie im Logik-App-Designer angezeigt werden.

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Verwaltete Connectors** aus. Wählen Sie auf der Symbolleiste **Hinzufügen** aus.

   ![Anzeigen verwalteter Connectors](./media/add-artifacts-integration-service-environment-ise/ise-view-managed-connectors.png)

1. Öffnen Sie im Bereich **Neuen verwalteten Connector hinzufügen** die Liste **Connector suchen**. Wählen Sie den ISE-Connector aus, den Sie verwenden möchten, der aber noch nicht in Ihrer ISE bereitgestellt ist. Klicken Sie auf **Erstellen**.

   ![Wählen Sie den ISE-Connector aus, den Sie in Ihrer ISE bereitstellen möchten.](./media/add-artifacts-integration-service-environment-ise/add-managed-connector.png)

   Nur ISE-Connectors, die berechtigt, aber noch nicht in Ihrer ISE bereitgestellt sind, werden Ihnen zur Auswahl angezeigt. Connectors, die bereits in der ISE bereitgestellt wurden, sind für die Auswahl nicht verfügbar.

<a name="create-custom-connectors-environment"></a>

## <a name="create-custom-connectors"></a>Erstellen benutzerdefinierter Connectors

Wenn Sie benutzerdefinierte Connectors in ihrer ISE verwenden möchten, erstellen Sie diese direkt in der ISE.

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Benutzerdefinierte Connectors** > **Hinzufügen** aus.

   ![Erstellen eines benutzerdefinierten Connectors](./media/add-artifacts-integration-service-environment-ise/add-custom-connector-to-ise.png)

1. Geben Sie den Namen, das Azure-Abonnement und die Azure-Ressourcengruppe (neu oder vorhanden) an, die Sie für den benutzerdefinierten Connector verwenden möchten.

1. Wählen Sie in der Liste **Speicherort** im Abschnitt **Integrationsdienstumgebungen** dieselbe ISE aus, die von Ihren Logik-Apps verwendet wird, und wählen Sie **Erstellen** aus. Beispiel:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-custom-connector-with-integration-service-environment.png)

1. Wählen Sie den neuen benutzerdefinierten Connector und anschließend **Bearbeiten** aus. Beispiel:

   ![Auswählen und Bearbeiten eines benutzerdefinierten Connectors](./media/add-artifacts-integration-service-environment-ise/edit-custom-connectors.png)

1. Erstellen Sie anschließend den Connector wie gewohnt auf der Grundlage einer [OpenAPI-Definition](https://docs.microsoft.com/connectors/custom-connectors/define-openapi-definition#import-the-openapi-definition) oder eines [SOAP](https://docs.microsoft.com/connectors/custom-connectors/create-register-logic-apps-soap-connector#2-define-your-connector).

1. Informationen zum Verwalten von benutzerdefinierten Connectors in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps.)

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Integrationsdienstumgebungen](../logic-apps/ise-manage-integration-service-environment.md)

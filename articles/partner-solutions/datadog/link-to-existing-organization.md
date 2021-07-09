---
title: Verknüpfen mit vorhandener Datadog-Organisation – Azure-Partnerlösungen
description: In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals eine Verknüpfung mit einer vorhandenen Datadog-Instanz herstellen.
ms.service: partner-services
ms.topic: quickstart
ms.date: 05/28/2021
author: tfitzmac
ms.author: tomfitz
ms.custom: references_regions
ms.openlocfilehash: b142b7d6e34905fe02de2a7546f9265289016808
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110656896"
---
# <a name="quickstart-link-to-existing-datadog-organization"></a>Schnellstart: Verknüpfen mit vorhandener Datadog-Organisation

In dieser Schnellstartanleitung stellen Sie eine Verknüpfung mit einer vorhandenen Datadog-Organisation her. Sie können entweder [eine neue Datadog-Organisation erstellen](create.md) oder eine Verknüpfung mit einer vorhandenen Datadog-Organisation herstellen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Ihre erste Datadog-Instanz in Azure erstellen können, [müssen Sie zunächst Ihre Umgebung konfigurieren](prerequisites.md). Führen Sie diese Schritte aus, bevor Sie mit den nächsten Schritten in dieser Schnellstartanleitung fortfahren.

## <a name="find-offer"></a>Suchen nach dem Angebot

Suchen Sie im Azure-Portal nach Datadog.

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich an.

1. Falls Sie während einer der letzten Sitzungen den **Marketplace** besucht haben, können Sie das Symbol aus den verfügbaren Optionen auswählen. Suchen Sie andernfalls nach _Marketplace_.

    :::image type="content" source="media/link-to-existing-organization/marketplace.png" alt-text="Marketplace-Symbol":::

1. Suchen Sie im Marketplace nach **Datadog**.

1. Wählen Sie auf dem Bildschirm Planübersicht die Option **Einrichten und abonnieren** aus.

   :::image type="content" source="media/link-to-existing-organization/datadog-app-2.png" alt-text="Datadog-Anwendung in Azure Marketplace.":::

## <a name="link-to-existing-datadog-organization"></a>Link zu vorhandener Datadog-Organisation

Im Portal können Sie auswählen, ob Sie eine Datadog-Organisation erstellen oder ein Azure-Abonnement mit einer vorhandenen Datadog-Organisation verknüpfen möchten.

Wenn Sie eine Verknüpfung mit einer vorhandenen Datadog-Organisation herstellen, wählen Sie unter **Azure-Abonnement mit vorhandener Datadog-Organisation verknüpfen** die Option **Erstellen** aus.

:::image type="content" source="media/link-to-existing-organization/datadog-create-link-selection.png" alt-text="Erstellen oder Verknüpfen einer Datadog-Organisation" border="true":::

Sie können Ihre neue Datadog-Ressource in Azure mit einer vorhandenen Datadog-Organisation in **US3** verknüpfen.

Im Portal wird ein Formular zum Erstellen der Datadog-Ressource angezeigt.

:::image type="content" source="media/link-to-existing-organization/link-to-existing.png" alt-text="Verknüpfen mit vorhandener Datadog-Organisation." border="true":::

Geben Sie die folgenden Werte an:

|Eigenschaft | BESCHREIBUNG
|:-----------|:-------- |
| Subscription | Wählen Sie das Azure-Abonnement aus, das Sie zum Erstellen der Datadog-Ressource verwenden möchten. Sie müssen über Besitzerzugriff verfügen. |
| Resource group | Geben Sie an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden möchten. Eine [Ressourcengruppe](../../azure-resource-manager/management/overview.md#resource-groups) ist ein Container, der zusammengehörige Ressourcen für eine Azure-Lösung enthält. |
| Ressourcenname | Geben Sie einen Namen für die Datadog-Ressource an. Dieser Name ist der Name der neuen Datadog-Organisation, wenn eine neue Datadog-Organisation erstellt wird. |
| Standort | Wählen Sie USA, Westen 2 aus. Derzeit ist „USA, Westen 2“ die einzige unterstützte Region. |

Wählen Sie **Mit Datadog-Organisation verknüpfen** aus. Der Link öffnet ein Fenster für die Datadog-Authentifizierung. Melden Sie sich bei Datadog an.

Standardmäßig verknüpft Azure Ihre aktuelle Datadog-Organisation mit ihrer Datadog-Ressource. Wenn Sie einen Link zu einer anderen Organisation möchten, wählen Sie die entsprechende Organisation wie unten dargestellt im Authentifizierungsfenster aus.

:::image type="content" source="media/link-to-existing-organization/select-datadog-organization.png" alt-text="Wählen Sie die entsprechende Datadog-Organisation zum Verknüpfen aus" border="true":::

Wählen Sie **Weiter: Metriken und Protokolle** aus, um Metriken und Protokolle zu konfigurieren.

## <a name="configure-metrics-and-logs"></a>Konfigurieren von Metriken und Protokollen

Verwenden Sie Azure-Ressourcentags, um zu konfigurieren, welche Metriken und Protokolle an Datadog gesendet werden. Sie können Metriken und Protokolle für bestimmte Ressourcen einschließen oder ausschließen.

Tagregeln zum Senden von **Metriken** sind:

- Standardmäßig werden Metriken für alle Ressourcen erfasst, ausgenommen virtuelle Computer, VM-Skalierungsgruppen und App Service-Pläne.
- Virtuelle Computer, VM-Skalierungsgruppen und App Service-Pläne mit *Include*-Tags senden Metriken an Datadog.
- Virtuelle Computer, VM-Skalierungsgruppen und App Service-Pläne mit *Exclude*-Tags senden keine Metriken an Datadog.
- Wenn ein Konflikt zwischen Einschluss- und Ausschlussregeln vorliegt, hat der Ausschluss Priorität.

Tagregeln für das Senden von **Protokollen** sind:

- Standardmäßig werden Protokolle für alle Ressourcen gesammelt.
- Azure-Ressourcen mit *Include*-Tags senden Protokolle an Datadog.
- Azure-Ressourcen mit  *Exclude*-Tags senden keine Protokolle an Datadog.
- Wenn ein Konflikt zwischen Einschluss- und Ausschlussregeln vorliegt, hat der Ausschluss Priorität.

Der folgende Screenshot zeigt beispielsweise eine Tagregel, bei der nur diejenigen virtuellen Computer, VM-Skalierungsgruppen und App Service-Pläne, die als *Datadog = True* gekennzeichnet sind, Metriken an Datadog senden.

:::image type="content" source="media/link-to-existing-organization/config-metrics-logs.png" alt-text="Protokolle und Metriken konfigurieren." border="true":::

Es gibt zwei Arten von Protokollen, die von Azure an Datadog ausgegeben werden können.

* **Protokolle auf Abonnementebene**: Bieten Erkenntnisse zu Vorgängen in Bezug auf Ihre Ressourcen auf der [Steuerungsebene](../../azure-resource-manager/management/control-plane-and-data-plane.md). Updates für Dienstintegritätsereignisse sind ebenfalls enthalten. Verwenden Sie das Aktivitätsprotokoll, um das Was, Wer und Wann für Schreibvorgänge (Put, Post, DELETE) zu bestimmen. Es gibt jeweils ein Aktivitätsprotokoll für jedes Azure-Abonnement.

* **Azure-Ressourcenprotokolle**: bieten Einblicke in Vorgänge, die auf der [Datenebene](../../azure-resource-manager/management/control-plane-and-data-plane.md) für eine Azure-Ressource durchgeführt wurden. Das Abrufen eines Geheimnisses aus einem Key Vault ist beispielsweise ein Vorgang auf Datenebene. Eine Anforderung an eine Datenbank ist ebenfalls ein Vorgang auf Datenebene. Der Inhalt dieser Protokolle variiert je nach Azure-Dienst und -Ressourcentyp.

Um Protokolle auf Abonnementebene an Datadog zu senden, wählen Sie **Abonnement-Aktivitätsprotokolle senden** aus. Wenn diese Option nicht aktiviert ist, wird keines der Protokolle auf Abonnementebene an Datadog gesendet.

Um Azure-Ressourcenprotokolle an Datadog zu senden, wählen Sie **Azure-Ressourcenprotokolle für alle definierten Ressourcen senden** aus. Die Typen von Azure-Ressourcenprotokollen sind unter [Azure Monitor-Ressourcenprotokollkategorien](../../azure-monitor/essentials/resource-logs-categories.md)aufgeführt.  Verwenden Sie Azure-Ressourcentags, um den Satz von Azure-Ressourcen zu filtern, die Protokolle an Datadog senden.

Die an Datadog gesendeten Protokolle werden von Azure abgerechnet. Weitere Informationen finden Sie unter [Preisgestaltung von Plattformprotokollen](https://azure.microsoft.com/pricing/details/monitor/), die an Azure Marketplace-Partner gesendet werden.

Nachdem Sie die Konfiguration von Metriken und Protokollen abgeschlossen haben, wählen Sie **Weiter: einmaliges Anmelden** aus.

## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

Wenn Sie die Datadog-Ressource mit einer vorhandenen Datadog-Organisation verknüpfen, können Sie das einmalige Anmelden in diesem Schritt nicht einrichten. Stattdessen richten Sie das einmalige Anmelden ein, nachdem Sie die Datadog-Ressource erstellt haben. Weitere Informationen finden Sie unter [Neukonfigurieren des einmaligen Anmeldens](manage.md#reconfigure-single-sign-on).

:::image type="content" source="media/link-to-existing-organization/linking-sso.png" alt-text="Einmaliges Anmelden zum Verknüpfen mit einer vorhandenen Datadog-Organisation." border="true":::

Klicken Sie auf **Weiter: Tags**.

## <a name="add-custom-tags"></a>Benutzerdefinierte Tags hinzufügen

Sie können benutzerdefinierte Tags für die neue Datadog-Ressource angeben. Geben Sie Name-Wert-Paare für die Tags an, die auf die Datadog-Ressource angewendet werden sollen.

:::image type="content" source="media/link-to-existing-organization/tags.png" alt-text="Fügen Sie benutzerdefinierte Tags für die Datadog-Ressource hinzu." border="true":::

Wenn Sie das Hinzufügen von Tags abgeschlossen haben, wählen Sie **Weiter: überprüfen und erstellen** aus.

## <a name="review--create-datadog-resource"></a>Datadog-Ressource überprüfen und erstellen

Überprüfen Sie Ihre Auswahl und die Nutzungsbedingungen. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus.

:::image type="content" source="media/link-to-existing-organization/review-create.png" alt-text="Datadog-Ressource überprüfen und erstellen." border="true":::

Azure stellt die Datadog-Ressource bereit.

Wählen Sie nach Abschluss des Vorgangs die Option **Zur Ressource wechseln**, um die Datadog-Ressource anzuzeigen.

:::image type="content" source="media/link-to-existing-organization/go-to-resource.png" alt-text="Bereitstellung von Datadog-Ressourcen." border="true":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten der Datadog-Ressource](manage.md)

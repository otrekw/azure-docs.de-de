---
title: 'Erstellen von Datadog: Azure-Partner-Lösungen'
description: In diesem Artikel wird beschrieben, wie Sie das Azure-Portal verwenden, um eine Instanz von Datadog zu erstellen.
ms.service: partner-services
ms.topic: quickstart
ms.date: 02/19/2021
author: tfitzmac
ms.author: tomfitz
ms.custom: references_regions
ms.openlocfilehash: 7af8b82c5da6c60527b45b6e8e292b9f067016ec
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101747588"
---
# <a name="quickstart-get-started-with-datadog"></a>Schnellstart: Erste Schritte mit Datadog

In dieser Schnellstartanleitung erstellen Sie eine Instanz von Datadog. Sie können entweder eine neue Datadog-Organisation erstellen oder eine Verknüpfung zu einer vorhandenen Datadog-Organisation herstellen.

## <a name="pre-requisites"></a>Voraussetzungen

Zum Einrichten der Azure Datadog-Integration müssen Sie über **Besitzer**-Zugriff auf das Azure-Abonnement verfügen. Stellen Sie sicher, dass Sie über den richtigen Zugriff verfügen, bevor Sie mit der Einrichtung beginnen.

Wenn Sie die Funktion „Einmaliges Anmelden“ (SSO, Single-Sign-On) für Security Assertion Markup Language (SAML) in der Datadog-Ressource verwenden möchten, müssen Sie eine Unternehmensanwendung einrichten. Zum Hinzufügen einer Unternehmensanwendung benötigen Sie eine der folgenden Rollen: globaler Administrator, Cloudanwendungsadministrator, Anwendungsadministrator oder Besitzer des Dienstprinzipals.

Gehen Sie folgendermaßen vor, um die Unternehmensanwendung einzurichten:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Wählen Sie **Azure Active Directory** aus.
1. Wählen Sie im linken Bereich **Unternehmensanwendungen** aus.
1. Wählen Sie **Neue Anwendung**.
1. Suchen Sie unter **Aus Katalog hinzufügen** nach *datadog*. Wählen Sie das Suchergebnis und dann **Hinzufügen** aus.

   :::image type="content" source="media/create/datadog-azure-ad-app-gallery.png" alt-text="Datadog-Anwendung im AAD-Unternehmenskatalog." border="true":::

1. Nachdem die App erstellt wurde, wechseln Sie im Seitenbereich zu Eigenschaften. Legen Sie **Benutzerzuweisung erforderlich?** auf **Nein** fest, und wählen Sie **Speichern** aus.

   :::image type="content" source="media/create/user-assignment-required.png" alt-text="Festlegen von Eigenschaften für die Datadog-Anwendung" border="true":::

1. Wechseln Sie im Seitenbereich zu **einmaliges Anmelden**. Wählen Sie **SAML** aus.

   :::image type="content" source="media/create/saml-sso.png" alt-text="SAML-Authentifizierung." border="true":::

1. Wählen Sie **Ja**, wenn Sie aufgefordert werden, die **SSO-Einstellungen zu speichern**.

   :::image type="content" source="media/create/save-sso.png" alt-text="Einmaliges Anmelden für die Datadog-App speichern" border="true":::

1. Die Einrichtung des einmaligen Anmeldens ist nun abgeschlossen.

## <a name="find-offer"></a>Suchen nach dem Angebot

Suchen Sie im Azure-Portal nach Datadog.

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich an.

1. Falls Sie während einer der letzten Sitzungen den **Marketplace** besucht haben, können Sie das Symbol aus den verfügbaren Optionen auswählen. Suchen Sie andernfalls nach _Marketplace_.

    :::image type="content" source="media/create/marketplace.png" alt-text="Marketplace-Symbol":::

1. Suchen Sie im Marketplace nach **Datadog**.

1. Wählen Sie auf dem Bildschirm Planübersicht die Option **Einrichten und abonnieren** aus.

   :::image type="content" source="media/create/datadog-app.png" alt-text="Datadog-Anwendung in Azure Marketplace.":::

## <a name="create-a-datadog-resource-in-azure"></a>Erstellen einer Datadog-Ressource in Azure

Im Portal wird ein Formular zum Erstellen der Datadog-Ressource angezeigt.

:::image type="content" source="media/create/datadog-create-resource.png" alt-text="Datadog-Ressource erstellen" border="true":::

Geben Sie die folgenden Werte an:

|Eigenschaft | BESCHREIBUNG
|:-----------|:-------- |
| Subscription | Wählen Sie das Azure-Abonnement aus, das Sie zum Erstellen der Datadog-Ressource verwenden möchten. Sie müssen über Besitzerzugriff verfügen. |
| Resource group | Geben Sie an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden möchten. Eine [Ressourcengruppe](../../azure-resource-manager/management/overview.md#resource-groups) ist ein Container, der zusammengehörige Ressourcen für eine Azure-Lösung enthält. |
| Ressourcenname | Geben Sie einen Namen für die Datadog-Ressource an. Dieser Name ist der Name der neuen Datadog-Organisation, wenn eine neue Datadog-Organisation erstellt wird. |
| Standort | Wählen Sie USA, Westen 2 aus. Derzeit ist „USA, Westen 2“ die einzige unterstützte Region. |
| Datadog-Organisation | Wählen Sie **Neu** aus, um eine neue Datadog-Organisation zu erstellen. Um einen Link zu einer vorhandenen Datadog-Organisation zu erhalten, wählen Sie **Vorhandene** aus. |
| Tarif | Wenn Sie eine neue Organisation erstellen, wählen Sie aus der Liste der verfügbaren Datadog-Pläne aus. |
| Abrechnungszeitraum | Monatlich. |

Wenn Sie eine Verknüpfung mit einer vorhandenen Datadog-Organisation herstellen, finden Sie weitere Informationen im nächsten Abschnitt. Wählen Sie andernfalls **Weiter: Metriken und Protokolle** aus, und überspringen Sie den nächsten Abschnitt.

## <a name="link-to-existing-datadog-organization"></a>Link zu vorhandener Datadog-Organisation

Sie können Ihre neue Datadog-Ressource in Azure mit einer vorhandenen Datadog-Organisation verknüpfen.

Wählen Sie **Vorhandene** für Datenorganisation aus, und wählen Sie dann **mit Datadog-Org verknüpfen** aus.

:::image type="content" source="media/create/link-to-existing.png" alt-text="Verknüpfen mit vorhandener Datadog-Organisation." border="true":::

Der Link öffnet ein Fenster für die Datadog-Authentifizierung. Melden Sie sich bei Datadog an.

Standardmäßig verknüpft Azure Ihre aktuelle Datadog-Organisation mit ihrer Datadog-Ressource. Wenn Sie einen Link zu einer anderen Organisation möchten, wählen Sie die entsprechende Organisation wie unten dargestellt im Authentifizierungsfenster aus.

:::image type="content" source="media/create/select-datadog-organization.png" alt-text="Wählen Sie die entsprechende Datadog-Organisation zum Verknüpfen aus" border="true":::

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

:::image type="content" source="media/create/config-metrics-logs.png" alt-text="Protokolle und Metriken konfigurieren." border="true":::

Es gibt zwei Arten von Protokollen, die von Azure an Datadog ausgegeben werden können.

1. **Protokolle auf Abonnementebene**: Bieten Erkenntnisse zu Vorgängen in Bezug auf Ihre Ressourcen auf der [Steuerungsebene](../../azure-resource-manager/management/control-plane-and-data-plane.md). Updates für Dienstintegritätsereignisse sind ebenfalls enthalten. Verwenden Sie das Aktivitätsprotokoll, um das Was, Wer und Wann für Schreibvorgänge (Put, Post, DELETE) zu bestimmen. Es gibt jeweils ein Aktivitätsprotokoll für jedes Azure-Abonnement.

1. **Azure-Ressourcenprotokolle**: bieten Einblicke in Vorgänge, die auf der [Datenebene](../../azure-resource-manager/management/control-plane-and-data-plane.md) für eine Azure-Ressource durchgeführt wurden. Das Abrufen eines Geheimnisses aus einem Key Vault ist beispielsweise ein Vorgang auf Datenebene. Eine Anforderung an eine Datenbank ist ebenfalls ein Vorgang auf Datenebene. Der Inhalt dieser Protokolle variiert je nach Azure-Dienst und -Ressourcentyp.

Um Protokolle auf Abonnementebene an Datadog zu senden, wählen Sie **Abonnement-Aktivitätsprotokolle senden** aus. Wenn diese Option nicht aktiviert ist, wird keines der Protokolle auf Abonnementebene an Datadog gesendet.

Um Azure-Ressourcenprotokolle an Datadog zu senden, wählen Sie **Azure-Ressourcenprotokolle für alle definierten Ressourcen senden** aus. Die Typen von Azure-Ressourcenprotokollen sind unter [Azure Monitor-Ressourcenprotokollkategorien](../../azure-monitor/essentials/resource-logs-categories.md)aufgeführt.  Verwenden Sie Azure-Ressourcentags, um den Satz von Azure-Ressourcen zu filtern, die Protokolle an Datadog senden.  

Die an Datadog gesendeten Protokolle werden von Azure abgerechnet. Weitere Informationen finden Sie unter [Preisgestaltung von Plattformprotokollen](https://azure.microsoft.com/pricing/details/monitor/), die an Azure Marketplace-Partner gesendet werden.

Nachdem Sie die Konfiguration von Metriken und Protokollen abgeschlossen haben, wählen Sie **Weiter: einmaliges Anmelden** aus.

## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

Wenn Ihre Organisation Azure Active Directory als Identitätsanbieter verwendet, können Sie einmaliges Anmelden vom Azure-Portal zu Datadog einrichten. Wenn Ihre Organisation einen anderen Identitätsanbieter verwendet oder Sie das einmalige Anmelden zu diesem Zeitpunkt nicht einrichten möchten, können Sie diesen Abschnitt überspringen.

Wenn Sie die Datadog-Ressource mit einer vorhandenen Datadog-Organisation verknüpfen, können Sie das einmalige Anmelden in diesem Schritt nicht einrichten. Stattdessen richten Sie das einmalige Anmelden ein, nachdem Sie die Datadog-Ressource erstellt haben. Weitere Informationen finden Sie unter [Neukonfigurieren des einmaligen Anmeldens](manage.md#reconfigure-single-sign-on).

:::image type="content" source="media/create/linking-sso.png" alt-text="Einmaliges Anmelden zum Verknüpfen mit einer vorhandenen Datadog-Organisation." border="true":::

Aktivieren Sie das Kontrollkästchen **Einmaliges Anmelden über Azure Active Directory aktivieren**, um das einmalige Anmelden über Azure Active Directory einzurichten.

Der Azure-Portal ruft die entsprechende Datadog-Anwendung aus Azure Active Directory ab. Die Anwendung entspricht der Unternehmensanwendung, die Sie in einem vorherigen Schritt bereitgestellt haben.

Wählen Sie den Namen der Datadog-Anwendung aus.

:::image type="content" source="media/create/sso.png" alt-text="Aktivieren Sie das einmaliges Anmelden bei Datadog." border="true":::

Klicken Sie auf **Weiter: Tags**.

## <a name="add-custom-tags"></a>Benutzerdefinierte Tags hinzufügen

Sie können benutzerdefinierte Tags für die neue Datadog-Ressource angeben. Geben Sie Name-Wert-Paare für die Tags an, die auf die Datadog-Ressource angewendet werden sollen.

:::image type="content" source="media/create/tags.png" alt-text="Fügen Sie benutzerdefinierte Tags für die Datadog-Ressource hinzu." border="true":::

Wenn Sie das Hinzufügen von Tags abgeschlossen haben, wählen Sie **Weiter: überprüfen und erstellen** aus.

## <a name="review--create-datadog-resource"></a>Datadog-Ressource überprüfen und erstellen

Überprüfen Sie Ihre Auswahl und die Nutzungsbedingungen. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus.

:::image type="content" source="media/create/review-create.png" alt-text="Datadog-Ressource überprüfen und erstellen." border="true":::

Azure stellt die Datadog-Ressource bereit.

Wählen Sie nach Abschluss des Vorgangs die Option **Zur Ressource wechseln**, um die Datadog-Ressource anzuzeigen.

:::image type="content" source="media/create/go-to-resource.png" alt-text="Bereitstellung von Datadog-Ressourcen." border="true":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten der Datadog-Ressource](manage.md)

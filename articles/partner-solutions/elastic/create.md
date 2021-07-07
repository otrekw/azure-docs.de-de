---
title: 'Erstellen einer Elastic-Anwendung: Azure-Partnerlösungen'
description: In diesem Artikel wird beschrieben, wie Sie das Azure-Portal verwenden, um eine Instanz von Elastic zu erstellen.
ms.service: partner-services
ms.topic: quickstart
ms.date: 05/19/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: 456decb74534cfd3ca5bfbf966c57b4182814225
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111952603"
---
# <a name="quickstart-get-started-with-elastic"></a>Schnellstart: Erste Schritte mit Elastic

In dieser Schnellstartanleitung verwenden Sie das Azure-Portal, um eine Instanz von Elastic in Ihre Azure-Lösungen zu integrieren.

## <a name="prerequisites"></a>Voraussetzungen

- Abonnementbesitzer: Die Integration von Elastic in Azure kann nur von Benutzern erstellt werden, die über Zugriff vom Typ _Besitzer_ auf das Azure-Abonnement verfügen. [Vergewissern Sie sich, dass Sie über den richtigen Zugriff verfügen](../../role-based-access-control/check-access.md), bevor Sie mit der Einrichtung beginnen.
- App mit einmaligem Anmelden: Die automatische Navigation zwischen dem Azure-Portal und der Elastic Cloud wird per einmaligem Anmelden (Single Sign-On, SSO) ermöglicht. Diese Option ist automatisch für alle Azure-Benutzer aktiviert. 

## <a name="find-offer"></a>Suchen nach dem Angebot

Verwenden Sie das Azure-Portal, um nach der Elastic-Anwendung zu suchen.

1. Navigieren Sie in einem Webbrowser zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich an.

1. Falls Sie während einer der letzten Sitzungen den **Marketplace** besucht haben, können Sie das Symbol aus den verfügbaren Optionen auswählen. Suchen Sie andernfalls nach _Marketplace_.

    :::image type="content" source="media/create/marketplace.png" alt-text="Marketplace-Symbol":::

1. Suchen Sie nach _Elastic_, und wählen Sie aus den verfügbaren Ergebnissen dann **Elasticsearch (Elastic Cloud)** aus.

1. Wählen Sie **Einrichten und abonnieren** aus.

   :::image type="content" source="media/create/set-up.png" alt-text="Auswählen des Angebots":::

## <a name="create-resource"></a>Erstellen von Ressourcen

Nachdem Sie das Angebot für Elastic ausgewählt haben, ist alles zum Einrichten der Anwendung bereit.

1. Geben Sie auf der Seite **Create Elastic Resource** (Elastic-Ressource erstellen) mit den Grundeinstellungen die folgenden Werte an.

    :::image type="content" source="media/create/create-resource.png" alt-text="Formular zum Einrichten der Elastic-Ressource":::

    | Eigenschaft | BESCHREIBUNG |
    | ---- | ---- |
    | **Abonnement** | Wählen Sie in der Dropdownliste ein Azure-Abonnement aus, für das Sie über Zugriff als Besitzer verfügen. |
    | **Ressourcengruppe** | Geben Sie an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene verwenden möchten. Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Weitere Informationen finden Sie in der [Übersicht über den Azure Resource Manager](../../azure-resource-manager/management/overview.md). |
    | **Elastic account name** (Name des Elastic-Kontos) | Geben Sie den Namen des Elastic-Kontos ein, das Sie erstellen möchten. |
    | **Region** | Wählen Sie **USA, Westen 2** oder **Vereinigtes Königreich, Süden** aus. Während der Vorschauphase werden für Elastic nur diese Regionen unterstützt. |
    | **Pricing Plan** (Tarif) | Wählen Sie **Pay as you go** (Nutzungsbasierte Bezahlung) aus. |
    | **Preis** | Wird basierend auf dem ausgewählten Elastic-Plan angegeben. |

   Wählen Sie nach Abschluss der Eingabe die Option **Next: Logs and Metrics** (Weiter: Protokolle und Metriken) aus.

1. Geben Sie unter **Logs & metrics** (Protokolle und Metriken) die Protokolle an, die an Elastic gesendet werden sollen.

    :::image type="content" source="media/create/configure-logs.png" alt-text="Auswählen der zu sendenden Protokolle":::

   Es gibt zwei Arten von Protokollen, die von Azure an Elastic ausgegeben werden können.

   **Abonnementprotokolle**: Ermöglichen über die [Verwaltungsebene](../../azure-resource-manager/management/control-plane-and-data-plane.md) den Einblick in die Vorgänge auf den einzelnen Azure-Ressourcen des Abonnements. Die Protokolle enthalten auch Updates zu Service Health-Ereignissen. Über das Aktivitätsprotokoll können Sie für jeden Schreibvorgang (PUT, POST, DELETE), der für die Ressourcen Ihres Abonnements durchgeführt wurde, ermitteln, welcher Benutzer welche Aktion zu welchem Zeitpunkt durchgeführt hat. Es gibt jeweils ein Aktivitätsprotokoll für jedes Azure-Abonnement.

   **Azure-Ressourcenprotokolle**: Ermöglichen den Einblick in Vorgänge, die auf der [Datenebene](../../azure-resource-manager/management/control-plane-and-data-plane.md) ablaufen. Beispiele für Aktivitäten auf der Datenebene sind das Abrufen eines Geheimnisses aus einem Schlüsseltresor oder das Senden einer Anforderung an eine Datenbank. Der Inhalt dieser Protokolle variiert je nach Azure-Dienst und -Ressourcentyp. Die Typen von Azure-Ressourcenprotokollen sind unter [Azure Monitor-Ressourcenprotokollkategorien](../../azure-monitor/essentials/resource-logs-categories.md)aufgeführt.

   Verwenden Sie Ressourcentags zum Filtern der Azure-Ressourcen, die Protokolle an Elastic senden. Die Tagregeln für das Senden von Protokollen lauten:

   * Standardmäßig werden Protokolle für alle Ressourcen gesammelt. 
   * Ressourcen mit *Include*-Tags senden Protokolle an Elastic. 
   * Ressourcen mit *Exclude*-Tags senden keine Protokolle an Elastic. 
   * Wenn ein Konflikt zwischen Einschluss- und Ausschlussregeln vorliegt, hat der Ausschluss Priorität.
 
   Wählen Sie **Next: Tags** (Weiter: Tags) aus, um Tags für die neue Elastic-Ressource einzurichten.

1. Fügen Sie unter **Tags** benutzerdefinierte Tags für die neue Elastic-Ressource hinzu. Jedes Tag besteht aus einem Namen und einem Wert. Wählen Sie nach dem Hinzufügen der Tags die Option **Weiter: Überprüfen + erstellen** aus, um zum letzten Schritt für die Ressourcenerstellung zu navigieren. 

   :::image type="content" source="media/create/add-tags.png" alt-text="Hinzufügen von Tags zur Elastic-Ressource":::

1. Unter **Überprüfen + erstellen** wird Ihre Konfiguration überprüft. Sie können sich die Auswahl ansehen, die Sie in den vorherigen Formularen vorgenommen haben. Darüber hinaus können Sie auch die Bedingungen des Angebots lesen.

   :::image type="content" source="media/create/review-validation.png" alt-text="Auswahl für Überprüfung":::

   Wählen Sie die Option **Erstellen** aus, nachdem die Überprüfung erfolgreich abgeschlossen wurde und Sie die Bedingungen überprüft haben.

1. Azure startet die Bereitstellung.

   :::image type="content" source="media/create/deployment-in-progress.png" alt-text="Bereitstellungsstatus":::

1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressource wechseln** aus, um die bereitgestellte Ressource anzuzeigen.

    :::image type="content" source="media/create/deployment-complete.png" alt-text="Anzeigen des Bereitstellungsstatus":::


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten der Elastic-Ressource](manage.md)
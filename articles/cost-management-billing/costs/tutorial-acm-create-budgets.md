---
title: 'Tutorial: Erstellen und Verwalten von Azure-Budgets'
description: Dieses Tutorial hilft bei der Planung und Abrechnung der Kosten für Azure-Dienste, die Sie in Anspruch nehmen.
author: bandersmsft
ms.author: banders
ms.date: 04/26/2021
ms.topic: tutorial
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.custom: seodec18
ms.openlocfilehash: 60b7cfa744de244015f99a2e42070eb3430878c7
ms.sourcegitcommit: 2f322df43fb3854d07a69bcdf56c6b1f7e6f3333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108015661"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Tutorial: Erstellen und Verwalten von Azure-Budgets

Budgets in Cost Management helfen Ihnen, die organisatorische Verantwortlichkeit zu planen und zu steigern. Sie unterstützen Sie dabei, andere über ihre Ausgaben zu informieren, um die Kosten proaktiv zu steuern und die Entwicklung der Ausgaben im Laufe der Zeit zu überwachen. Sie können Warnungen basierend auf den tatsächlichen oder prognostizierten Kosten konfigurieren, um sicherzustellen, dass Ihre Ausgaben innerhalb des Ausgabenlimits Ihrer Organisation liegen. Wenn die erstellte Budgetschwellenwerte überschritten werden, werden nur Benachrichtigungen ausgelöst. Keines Ihrer Ressourcen wird beeinträchtigt, und die Nutzung wird nicht beendet. Sie können Budgets verwenden, um Ausgaben bei der Kostenanalyse zu vergleichen und zu verfolgen.

Kosten- und Nutzungsdaten sind in der Regel innerhalb von 8 bis 24 Stunden verfügbar, und Budgets werden alle 24 Stunden anhand dieser Kosten überprüft. Machen Sie sich mit der [Aktualisierung von Kosten- und Nutzungsdaten](./understand-cost-mgt-data.md#cost-and-usage-data-updates-and-retention) vertraut. Wenn ein Budgetschwellenwert erreicht wird, werden E-Mail-Benachrichtigungen normalerweise innerhalb einer Stunde nach der Auswertung gesendet.

Die Budgets werden am Ende eines Zeitraums (monatlich, vierteljährlich oder jährlich) auf den gleichen Budgetbetrag automatisch zurückgesetzt, wenn Sie ein zukünftiges Ablaufdatum auswählen. Da sie auf den gleichen Budgetbetrag zurückgesetzt werden, müssen Sie separate Budgets anlegen, wenn die budgetierten Währungsbeträge für zukünftige Zeiträume abweichen. Wenn ein Budget abläuft, wird es automatisch gelöscht.

Die Beispiele in diesem Tutorial führen Sie durch die Erstellung und Bearbeitung eines Budgets für ein Abonnement von Azure Enterprise Agreement (EA).

Sehen Sie sich das Video [Anwenden von Budgets auf Abonnements im Azure-Portal](https://www.youtube.com/watch?v=UrkHiUx19Po) an, um zu erfahren, wie Sie Budgets in Azure erstellen können, um Ausgaben zu überwachen. Weitere Videos finden Sie im [YouTube-Kanal zu Cost Management](https://www.youtube.com/c/AzureCostManagement).

>[!VIDEO https://www.youtube.com/embed/UrkHiUx19Po]

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Budgets im Azure-Portal
> * Erstellen und Bearbeiten von Budgets mit PowerShell
> * Erstellen eines Budgets mit einer Azure Resource Manager-Vorlage

## <a name="prerequisites"></a>Voraussetzungen

Budgets werden für die folgenden Arten von Azure-Kontentypen und -Bereichen unterstützt:

- Bereiche für rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)
    - Verwaltungsgruppen
    - Subscription
- Enterprise Agreement-Bereiche
    - Abrechnungskonto
    - Department
    - Registrierungskonto
- Individuelle Vereinbarungen
    - Abrechnungskonto
- Bereiche für Microsoft-Kundenvereinbarung
    - Abrechnungskonto
    - Abrechnungsprofil
    - Rechnungsabschnitt
    - Kunde
- AWS-Bereiche
    - Externes Konto
    - Externes Abonnement

Um Budgets anzeigen zu können, müssen Sie mindestens über Lesezugriff auf Ihr Azure-Konto verfügen.

Bei einem neuen Abonnement können Sie nicht sofort ein Budget erstellen oder andere Cost Management-Features nutzen. Es kann bis zu 48 Stunden dauern, bis Sie alle Cost Management-Features verwenden können.

Zum Anzeigen von Budgets für Azure EA-Abonnements müssen Sie über Lesezugriff verfügen. Zum Erstellen und Verwalten von Budgets müssen Sie über die Berechtigung „Mitwirkender“ verfügen.

Die folgenden Azure-Berechtigungen (oder Bereich) werden pro Abonnement für Budgets nach Benutzer und Gruppe unterstützt.

- Besitzer – kann Budgets für ein Abonnement erstellen, ändern oder löschen.
- Mitwirkender und Mitwirkender für Cost Management – kann eigene Budgets erstellen, ändern oder löschen. Kann den Betrag für von anderen Personen erstellten Budgets ändern.
- Leser und Leser für Cost Management – kann Budgets anzeigen, für die er die Berechtigung hat.

Weitere Informationen zu Bereichen finden Sie unter [Verstehen von und Arbeiten mit Bereichen](understand-work-scopes.md). Weitere Informationen zum Zuweisen der Berechtigung für Cost Management-Daten finden Sie unter [Zuweisen des Zugriffs auf Daten in Cost Management](./assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

- Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-budget-in-the-azure-portal"></a>Erstellen eines Budgets im Azure-Portal

Sie können ein Azure-Abonnementbudget für den Zeitraum eines Monats, Quartals oder Jahres erstellen.

Um ein Budget zu erstellen oder anzuzeigen, öffnen Sie einen Bereich im Azure-Portal, und wählen Sie im Menü **Budgets** aus. Navigieren Sie beispielsweise zu **Abonnements**, wählen Sie ein Abonnement in der Liste aus, und wählen Sie dann **Budgets** im Menü aus. Verwenden Sie die Pille **Bereich**, um zu einem anderen Bereich in „Budgets“ zu wechseln, z. B. zu einer Verwaltungsgruppe. Weitere Informationen zu Bereichen finden Sie unter [Verstehen von und Arbeiten mit Bereichen](understand-work-scopes.md).

Nachdem Sie Budgets erstellt haben, wird eine einfache Ansicht Ihrer aktuellen Ausgaben im Abgleich mit dem Budget angezeigt.

Wählen Sie **Hinzufügen**.

:::image type="content" source="./media/tutorial-acm-create-budgets/budgets-cost-management.png" alt-text="Screenshot: Liste von bereits erstellten Budgets" lightbox="./media/tutorial-acm-create-budgets/budgets-cost-management.png" :::

Vergewissern Sie sich, dass im Fenster **Budget erstellen** der richtige Bereich angezeigt wird. Wählen Sie alle Filter aus, die Sie hinzufügen möchten. Mit Filtern können Sie Budgets für bestimmte Kosten erstellen, z.B. für Ressourcengruppen in einem Abonnement oder einen Dienst wie virtuelle Computer. Filter, die Sie in der Kostenanalyse verwenden können, können auch auf ein Budget angewendet werden.

Nachdem Sie Ihren Bereich und die Filter identifiziert haben, geben Sie einen Budgetnamen ein. Wählen Sie dann einen Zurücksetzungszeitraum für ein Monats-, Quartals- oder Jahresbudget aus. Der Zurücksetzungszeitraum bestimmt das Zeitfenster, das durch das Budget analysiert wird. Die durch das Budget ausgewerteten Kosten beginnen am Anfang jedes neuen Zeitraums bei Null. Die Erstellung eines Quartalsbudgets entspricht der eines Monatsbudget. Der Unterschied besteht darin, dass der Budgetbetrag für das Quartal gleichmäßig auf die drei Monate des Quartals verteilt wird. Ein jährlicher Budgetbetrag wird gleichmäßig auf alle 12 Monate des Kalenderjahrs aufgeteilt.

Wenn Sie über ein Abonnement mit nutzungsbasierter Bezahlung, ein MSDN-Abonnement oder ein Visual Studio-Abonnement verfügen, stimmt der Abrechnungszeitraum für Ihre Rechnung möglicherweise nicht mit dem Kalendermonat überein. Bei diesen Abonnementarten und Ressourcengruppen können Sie ein Budget erstellen, das Ihrem Rechnungszeitraum oder Kalendermonaten entspricht. Zum Erstellen eines Budgets, das Ihrem Rechnungszeitraum entspricht, wählen Sie als Zurücksetzungszeitraum **Abrechnungsmonat**, **Abrechnungsquartal** oder **Abrechnungsjahr** aus. Wenn Sie ein Budget entsprechend dem Kalendermonat erstellen möchten, wählen Sie als Zurücksetzungszeitraum **Monatlich**, **Vierteljährlich** oder **Jährlich** aus.

Identifizieren Sie als Nächstes das Ablaufdatum, an dem das Budget ungültig wird und ihre Kosten nicht mehr auswertet.

Basierend auf den bis jetzt im Budget ausgewählten Feldern wird ein Diagramm angezeigt, das Sie bei der Auswahl eines Schwellenwerts für Ihr Budget unterstützen soll. Das vorgeschlagene Budget basiert auf den höchsten prognostizierten Kosten, die für Sie in zukünftigen Zeiträumen anfallen können. Sie können den Budgetbetrag ändern.

:::image type="content" source="./media/tutorial-acm-create-budgets/create-monthly-budget.png" alt-text="Screenshot: Erstellung eines Budgets mit monatlichen Kostendaten" lightbox="./media/tutorial-acm-create-budgets/create-monthly-budget.png" :::

Wählen Sie nach dem Konfigurieren des Budgetbetrags **Weiter** aus, um Budgetwarnungen für tatsächliche Kosten und für prognostizierte Kosten zu konfigurieren.

## <a name="configure-actual-costs-budget-alerts"></a>Konfigurieren von Budgetwarnungen für tatsächliche Kosten

Budgets erfordern mindestens einen Kostenschwellenwert (% des Budgets) und eine entsprechende E-Mail-Adresse. Sie können optional bis zu fünf Schwellenwerte und fünf E-Mail-Adressen in ein einzelnes Budget aufnehmen. Wenn ein Budgetschwellenwert erreicht wird, werden E-Mail-Benachrichtigungen normalerweise innerhalb einer Stunde nach der Auswertung gesendet. Budgetwarnungen für tatsächliche Kosten werden für die tatsächlichen Kosten generiert, die im Verhältnis zu den konfigurierten Budgetschwellenwerten angefallen sind.

## <a name="configure-forecasted-budget-alerts"></a>Konfigurieren von Budgetwarnungen für prognostizierte Kosten

Warnungen zu prognostizierten Kosten informieren Sie in einer erweiterten Benachrichtigung darüber, dass Ihre Ausgabentrends wahrscheinlich Ihr Budget überschreiten. Für die Warnungen werden [Vorhersagen der prognostizierten Kosten](quick-acm-cost-analysis.md#understand-forecast) verwendet. Warnungen werden generiert, wenn die Kostenprognose den festgelegten Schwellenwert überschreitet. Sie können einen prognostizierten Schwellenwert (Prozentsatz des Budgets) konfigurieren. Wenn ein prognostizierter Budgetschwellenwert erreicht wird, werden Benachrichtigungen normalerweise innerhalb einer Stunde nach der Auswertung gesendet.

Wenn Sie zwischen der Konfiguration einer Warnung für tatsächliche Kosten und der Warnung für prognostizierte Kosten wechseln möchten, verwenden Sie beim Konfigurieren der Warnung das Feld `Type`, wie in der nachfolgenden Abbildung gezeigt.

Wenn Sie E-Mails erhalten möchten, fügen Sie azure-noreply@microsoft.com der Liste mit genehmigten Absendern hinzu, damit E-Mails nicht im Junk-E-Mail-Ordner landen. Weitere Informationen zu Benachrichtigungen finden Sie unter [Verwenden von Kostenwarnungen](./cost-mgt-alerts-monitor-usage-spending.md).

Im nachstehenden Beispiel wird eine E-Mail-Warnung generiert, wenn 90 Prozent des Budgets erreicht sind. Wenn Sie ein Budget mit der API für Budgets erstellen, können Sie Personen auch Rollen zuweisen, damit sie Warnungen erhalten. Das Zuweisen von Rollen zu Personen wird im Azure-Portal nicht unterstützt. Weitere Informationen zur Budgets-API von Azure finden Sie unter [API für Budgets](/rest/api/consumption/budgets). Informationen zum Senden einer E-Mail-Warnung in einer anderen Sprache finden Sie unter [Unterstützte Gebietsschemas für Budgetwarnungs-E-Mails](manage-automation.md#supported-locales-for-budget-alert-emails).

Bei Warnungsgrenzwerten wird ein Bereich von 0,01 bis 1.000 Prozent des von Ihnen angegebenen Budgetschwellenwerts unterstützt.

:::image type="content" source="./media/tutorial-acm-create-budgets/budget-set-alert.png" alt-text="Screenshot: Warnungsbedingungen" lightbox="./media/tutorial-acm-create-budgets/budget-set-alert.png" :::

Nach der Erstellung eines Budgets wird es in der Kostenanalyse angezeigt. Die Betrachtung Ihres Budgets in Bezug auf Ihren Ausgabentrend ist einer der ersten Schritte bei der [Analyse Ihrer Kosten und Ausgaben](./quick-acm-cost-analysis.md).

:::image type="content" source="./media/tutorial-acm-create-budgets/cost-analysis.png" alt-text="Screenshot: Beispielbudget mit Ausgaben in der Kostenanalyse" lightbox="./media/tutorial-acm-create-budgets/cost-analysis.png" :::

Im obigen Beispiel haben Sie ein Budget für ein Abonnement erstellt. Sie können auch ein Budget für eine Ressourcengruppe erstellen. Wenn Sie ein Budget für eine Ressourcengruppe erstellen möchten, navigieren Sie zu **Cost Management + Abrechnung** &gt; **Abonnements** &gt; Abonnement auswählen > **Ressourcengruppen** > Ressourcengruppe auswählen > **Budgets** > und dann **Hinzufügen**, um ein Budget hinzuzufügen.

### <a name="create-a-budget-for-combined-azure-and-aws-costs"></a>Erstellen eines Budgets für kombinierte Azure- und AWS-Kosten

Sie können Ihre Azure- und AWS-Kosten gruppieren, indem Sie dem Connector eine Verwaltungsgruppe sowie die zugehörigen konsolidierten und verknüpften Konten zuweisen. Weisen Sie Ihre Azure-Abonnements derselben Verwaltungsgruppe zu. Erstellen Sie dann ein Budget für die kombinierten Kosten.

1. Wählen Sie in Cost Management die Option **Budgets** aus.
1. Wählen Sie **Hinzufügen**.
1. Wählen Sie **Bereich ändern** und dann die Verwaltungsgruppe aus.
1. Setzen Sie die Erstellung des Budgets bis zum Abschluss fort.

## <a name="costs-in-budget-evaluations"></a>Kosten in Budgetauswertungen

Die Kostenauswertungen für Budgets beinhalten jetzt reservierte Instanzen und Kaufdaten. Falls diese Gebühren für Sie relevant sind, erhalten Sie möglicherweise Warnungen, weil die Gebühren in Ihren Auswertungen berücksichtigt werden. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, um sicherzustellen, dass die Budgetschwellenwerte ordnungsgemäß konfiguriert sind, um die neuen Kosten zu berücksichtigen. An den abgerechneten Azure-Gebühren hat sich nichts geändert. Budgets werden jetzt anhand eines vollständigeren Kostenumfangs bewertet. Falls diese Gebühren für Sie nicht gelten, bleibt das Budgetverhalten unverändert.

Wenn Sie die neuen Kosten so filtern möchten, dass das Budget nur anhand der Azure-Verbrauchsgebühren von Erstanbietern ausgewertet wird, fügen Sie Ihrem Budget die folgenden Filter hinzu:

- Herausgebertyp: Azure
- Gebührentyp: Verwendung

Die Budgetkostenauswertung basiert auf den tatsächlichen Kosten. Sie beinhaltet keine Abschreibungen. Weitere Informationen zu den für Sie in Budgets verfügbaren Filteroptionen finden Sie unter [Grundlegendes zu Gruppierungs-und Filteroptionen](group-filter.md).

## <a name="trigger-an-action-group"></a>Auslösen einer Aktionsgruppe

Wenn Sie ein Budget für einen Abonnement- oder Ressourcengruppenbereich erstellen oder bearbeiten, können Sie es so konfigurieren, dass es eine Aktionsgruppe aufruft. Die Aktionsgruppe kann verschiedene Aktionen ausführen, wenn der Budgetschwellenwert erreicht wird. 

Aktionsgruppen werden derzeit nur für Abonnement- und Ressourcengruppenbereiche unterstützt. Weitere Informationen zum Erstellen von Aktionsgruppen finden Sie unter [Konfigurieren grundlegender Aktionsgruppeneinstellungen](../../azure-monitor/alerts/action-groups.md#configure-basic-action-group-settings). 

Weitere Informationen zum Verwenden von budgetbasierter Automatisierung mit Aktionsgruppen finden Sie unter [Verwalten von Kosten mit Azure-Budgets](../manage/cost-management-budget-scenario.md).

Um Aktionsgruppen zu erstellen oder zu aktualisieren, wählen Sie beim Erstellen oder Bearbeiten eines Budgets **Aktionsgruppen verwalten** aus.

:::image type="content" source="./media/tutorial-acm-create-budgets/trigger-action-group.png" alt-text="Screenshot: Erstellen eines Budgets zum Anzeigen von „Aktionsgruppen verwalten“" lightbox="./media/tutorial-acm-create-budgets/trigger-action-group.png" :::

Klicken Sie anschließend auf **Aktionsgruppe hinzufügen**, und erstellen Sie die Aktionsgruppe.

Die Budgetintegration in Aktionsgruppen funktioniert nur bei Aktionsgruppen, für die das allgemeine Warnungsschema deaktiviert ist. Weitere Informationen zum Deaktivieren des Schemas finden Sie unter [Wie kann ich das allgemeine Warnungsschema aktivieren?](../../azure-monitor/alerts/alerts-common-schema.md#how-do-i-enable-the-common-alert-schema)

## <a name="create-and-edit-budgets-with-powershell"></a>Erstellen und Bearbeiten von Budgets mit PowerShell

Als EA-Kunde können Sie Budgets mithilfe des Azure PowerShell-Moduls programmgesteuert erstellen und bearbeiten. Führen Sie zum Herunterladen der neuesten Version von Azure PowerShell den folgenden Befehl aus:

```azurepowershell-interactive
install-module -name Az
```

Mit den folgenden Beispielbefehlen wird ein Budget erstellt:

```azurepowershell-interactive
#Sign into Azure Powershell with your account

Connect-AzAccount

#Select a subscription to to monitor with a budget

select-AzSubscription -Subscription "Your Subscription"

#Create an action group email receiver and corresponding action group

$email1 = New-AzActionGroupReceiver -EmailAddress test@test.com -Name EmailReceiver1
$ActionGroupId = (Set-AzActionGroup -ResourceGroupName YourResourceGroup -Name TestAG -ShortName TestAG -Receiver $email1).Id

#Create a monthly budget that sends an email and triggers an Action Group to send a second email. Make sure the StartDate for your monthly budget is set to the first day of the current month. Note that Action Groups can also be used to trigger automation such as Azure Functions or Webhooks.

New-AzConsumptionBudget -Amount 100 -Name TestPSBudget -Category Cost -StartDate 2020-02-01 -TimeGrain Monthly -EndDate 2022-12-31 -ContactEmail test@test.com -NotificationKey Key1 -NotificationThreshold 0.8 -NotificationEnabled -ContactGroup $ActionGroupId
```

## <a name="create-a-budget-with-an-azure-resource-manager-template"></a>Erstellen eines Budgets mit einer Azure Resource Manager-Vorlage

Sie können mit einer Azure Resource Manager-Vorlage ein Budget erstellen. Informationen zur Verwendung der Vorlage finden Sie unter [Schnellstart: Erstellen eines Budgets mit einer Azure Resource Manager-Vorlage](quick-create-budget-template.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Budget erstellt haben und es nicht länger benötigen, zeigen Sie seine Details an, und löschen Sie es.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Budgets im Azure-Portal
> * Erstellen und Bearbeiten von Budgets mit PowerShell
> * Erstellen eines Budgets mit einer Azure Resource Manager-Vorlage

Fahren Sie mit dem nächsten Tutorial fort, um einen sich wiederholenden Export für Ihre Cost Management-Daten zu erstellen.

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von exportierten Daten](tutorial-export-acm-data.md)
---
title: Aktionsregeln für Azure Monitor-Warnungen
description: In diesem Artikel wird erläutert, was Aktionsregeln in Azure Monitor sind und wie sie konfiguriert und verwaltet werden können.
ms.topic: conceptual
ms.date: 04/25/2019
ms.openlocfilehash: 07d179f557671a515a7933b64a25e6d41f75219b
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102045614"
---
# <a name="action-rules-preview"></a>Aktionsregeln (Vorschau)

Mit Aktionsregeln können Sie Aktionen in jedem Resource Manager-Bereich (Abonnement, Ressourcengruppe und Ressource) definieren oder unterdrücken. Sie verfügen über verschiedene Filter zum Eingrenzen auf eine bestimmte Teilmenge von Warnungsinstanzen, auf die Sie reagieren möchten.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4rBZ2]

## <a name="why-and-when-should-you-use-action-rules"></a>Wann und wieso sollten Sie Aktionsregeln verwenden?

### <a name="suppression-of-alerts"></a>Unterdrückung von Warnungen

Es gibt viele Szenarien, in denen es nützlich ist, die Benachrichtigungen zu unterdrücken, die von Warnungen generiert werden. Diese Szenarien reichen von der Unterdrückung während eines geplanten Wartungsfensters bis hin zur Unterdrückungen außerhalb der Geschäftszeiten. Angenommen, das Team von **ContosoVM** möchte Warnungsbenachrichtigungen für das kommende Wochenende unterdrücken, weil für **ContosoVM** eine Wartung geplant ist.

Obwohl das Team jede Warnungsregel, die für **ContosoVM** manuell konfiguriert ist, deaktivieren kann (und sie nach der Wartung erneut aktivieren kann), ist dies kein einfacher Vorgang. Mit Aktionsregeln können Sie die Unterdrückung von Warnungen nach Maß definieren, sodass Sie den Zeitraum für die Unterdrückung flexibel konfigurieren können. Im vorherigen Beispiel kann das Team nun eine Aktionsregel für **ContosoVM** definieren, die alle Warnungsbenachrichtigungen für das Wochenende unterdrückt.

### <a name="actions-at-scale"></a>Aktionen nach Maß

Zwar können Sie mit Warnungsregeln die Aktionsgruppe definieren, die ausgelöst wird, wenn eine Warnung generiert wird, jedoch verfügen Kunden meist über eine allgemeine Aktionsgruppe für den gesamten Umfang ihrer Vorgänge. Beispielsweise würde ein Team, das für die Ressourcengruppe **ContosoRG** zuständig ist, vermutlich dieselbe Aktionsgruppe für alle innerhalb von **ContosoRG** definierten Warnungsregeln definieren.

Aktionsregeln helfen Ihnen, diesen Vorgang zu vereinfachen. Durch die Definition von Aktionen im richtigen Maßstab kann eine Aktionsgruppe für jede Warnung ausgelöst werden, die für den konfigurierten Bereich generiert wird. Im vorherigen Beispiel kann das Team eine Aktionsregel für **ContosoRG** definieren, die die gleiche Aktionsgruppe für alle darin generierten Warnungen auslöst.

> [!NOTE]
> Aktionsregeln gelten zurzeit nicht für Azure Service Health-Warnungen.

## <a name="configuring-an-action-rule"></a>Konfigurieren einer Aktionsregel

### <a name="portal"></a>[Portal](#tab/portal)

Sie können die Funktion aufrufen, indem Sie in Azure Monitor auf der Startseite **Warnungen** die Option **Aktionen verwalten** auswählen. Wählen Sie anschließend **Aktionsregeln (Vorschau)** aus. Sie können durch Auswählen von **Aktionsregeln (Vorschau)** im Dashboard auf der Startseite für Warnungen auf die Regeln zugreifen.

![Aktionsregeln auf der Azure Monitor-Startseite](media/alerts-action-rules/action-rules-landing-page.png)

Wählen Sie **+ New Action Rule** (+ Neue Aktionsregel) aus.

![Screenshot der Seite „Aktionen verwalten“ mit hervorgehobener Schaltfläche „Neue Aktionsregel“](media/alerts-action-rules/action-rules-new-rule.png)

Sie können eine Aktionsregel auch erstellen, während Sie eine Warnungsregel konfigurieren.

![Screenshot der Seite „Regel erstellen“ mit hervorgehobener Schaltfläche „Aktionsregel erstellen“](media/alerts-action-rules/action-rules-alert-rule.png)

Nun sollte die Flussseite zum Erstellen von Aktionsregeln angezeigt werden. Konfigurieren Sie die folgenden Elemente:

![Erstellungsflow für neue Aktionsregel](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>`Scope`

Wählen Sie zunächst den Bereich aus (Azure-Abonnement, Ressourcengruppe oder Zielressource). Sie können auch eine Mehrfachauswahl durch Kombinieren von Bereichen innerhalb eines einzelnen Abonnements treffen.

![Bereich der Aktionsregel](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Filterkriterien

Sie können außerdem mindestens einen Filter zum Eingrenzen auf eine bestimmte Teilmenge von Warnungen festlegen.

Die folgenden Filter sind verfügbar:

* **Schweregrad**: Wählen Sie mindestens einen Schweregrad für Warnungen aus. **Schweregrad = Sev1** bedeutet, dass die Aktionsregel für alle Warnungen mit dem Schweregrad Sev1 gilt.
* **Überwachungsdienst**: Ein Filter, der auf dem ursprünglichen Überwachungsdienst basiert. Dieser Filter bietet ebenfalls eine Mehrfachauswahl. **Überwachungsdienst = „Application Insights“** bedeutet beispielsweise, dass die Aktionsregel für alle auf „Application Insights“ basierenden Warnungen gilt.
* **Ressourcentyp**:  Ein Filter, der auf einem bestimmten Ressourcentyp basiert. Dieser Filter bietet ebenfalls eine Mehrfachauswahl. **Ressourcentyp = „Virtual Machines“** bedeutet, dass die Aktionsregel für alle virtuellen Computer gilt.
* **Warnungsregel-ID**: Eine Option zum Filtern nach bestimmten Warnungsregeln anhand der Resource Manager-ID der Warnungsregel.
* **Überwachungsbedingung**:  Ein Filter für Warnungsinstanzen mit der Überwachungsbedingung **Ausgelöst** oder **Aufgelöst**.
* **Beschreibung**: Eine Regex-Entsprechung (regulärer Ausdruck), die eine Zeichenfolgenübereinstimmung mit der Beschreibung definiert, die als Teil der Warnungsregel definiert ist. **Description contains 'prod'** findet beispielsweise alle Warnungen, die die Zeichenfolge „prod“ in ihren Beschreibungen enthalten.
* **Warnungskontext (Nutzlast)** : Eine Regex-Übereinstimmung, die eine Zeichenfolgenübereinstimmung mit den Warnungskontextfeldern der Nutzlast einer Warnung definiert. **Alert context (payload) contains 'Computer-01'** findet beispielsweise alle Warnungen, deren Nutzlasten die Zeichenfolge „Computer-01“ enthalten.

Diese Filter werden in Verbindung miteinander angewendet. Wenn Sie beispielsweise **Ressourcentyp' = „Virtual Machines** und **Schweregrad' = Sev0** festlegen, wird nach allen **Sev0**-Warnungen ausschließlich auf Ihren VMs gefiltert.

![Filter für Aktionsregeln](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Konfiguration von Unterdrückung oder Aktionsgruppen

Konfigurieren Sie nun die Aktionsregel für die Unterdrückung von Warnungen oder die Unterstützung für Aktionsgruppen. Es können nicht beide Optionen ausgewählt werden. Die Konfiguration gilt für alle Warnungsinstanzen, die dem zuvor definierten Bereich und den Filtern entsprechen.

#### <a name="suppression"></a>Unterdrückung

Wenn Sie **Suppression** (Unterdrückung) auswählen, konfigurieren Sie den Zeitraum, für den Aktionen und Benachrichtigungen unterdrückt werden sollen. Wählen Sie eine der folgenden Optionen aus:
* **From now (Always)** (Ab jetzt (Immer)): Alle Benachrichtigungen werden unbefristet unterdrückt.
* **At a scheduled time** (Zur geplanten Zeit): Benachrichtigungen werden in einem bestimmten Zeitraum unterdrückt.
* **With a recurrence** (Mit Wiederholung): Unterdrückt Benachrichtigungen bei einem wiederkehrenden täglichen, wöchentlichen oder monatlichen Zeitplan.

![Unterdrückung von Aktionsregeln](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Aktionsgruppe

Fügen Sie bei Auswahl von **Aktionsgruppe** auf der Umschaltfläche eine vorhandene Aktionsgruppe hinzu, oder erstellen Sie eine neue.

> [!NOTE]
> Sie können einer Aktionsregel nur eine Aktionsgruppe zuordnen.

![Hinzufügen oder Erstellen einer neuen Aktionsregel durch Auswählen der Aktionsgruppe](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Details der Aktionsregel

Konfigurieren Sie schließlich folgende Details für die Aktionsregel:
* Name
* Ressourcengruppe, in der sie gespeichert wird
* BESCHREIBUNG

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Sie können Aktionsregeln mit der Azure-Befehlszeilenschnittstelle (Azure CLI) erstellen, indem Sie den Befehl [az monitor action-rule create](/cli/azure/ext/alertsmanagement/monitor/action-rule#ext-alertsmanagement-az-monitor-action-rule-create) verwenden.  Der `az monitor action-rule`-Verweis ist nur einer von vielen [Azure CLI-Verweisen für Azure Monitor](/cli/azure/azure-cli-reference-for-monitor).

### <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

1. [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli)

   Wenn Sie möchten, können Sie auch Azure Cloud Shell verwenden, um die Schritte in diesem Artikel auszuführen.  Azure Cloud Shell ist eine interaktive Shellumgebung, die Sie über Ihren Browser nutzen können.  Starten Sie Cloud Shell mit einer der folgenden Methoden:

   - Öffnen Sie Cloud Shell, indem Sie zu [https://shell.azure.com](https://shell.azure.com) navigieren.

   - Wählen Sie im [Azure-Portal](https://portal.azure.com) rechts oben im Menü die Schaltfläche **Cloud Shell** aus.

1. Melden Sie sich an.

   Melden Sie sich mit dem Befehl [az login](/cli/azure/reference-index#az-login) an, falls Sie eine lokale Installation der Befehlszeilenschnittstelle verwenden.  Führen Sie die in Ihrem Terminal angezeigten Schritte aus, um den Authentifizierungsprozess abzuschließen.

    ```azurecli
    az login
    ```

1. Installieren Sie die `alertsmanagement`-Erweiterung.

   Der Befehl `az monitor action-rule` ist eine experimentelle Erweiterung der eigentlichen Azure-Befehlszeilenschnittstelle. Weitere Informationen zu Erweiterungen finden Sie unter [Verwenden von Erweiterungen mit der Azure CLI](/cli/azure/azure-cli-extensions-overview?).

   ```azurecli
   az extension add --name alertsmanagement
   ```

   Die folgende Warnung wird erwartet.

   ```output
   The installed extension `alertsmanagement` is experimental and not covered by customer support.  Please use with discretion.
   ```

### <a name="create-action-rules-with-the-azure-cli"></a>Erstellen von Aktionsregeln mit der Azure CLI

Informationen zu den erforderlichen und optionalen Parametern finden Sie im Azure CLI-Referenzinhalt für [az monitor action-rule create](/cli/azure/ext/alertsmanagement/monitor/action-rule#ext-alertsmanagement-az-monitor-action-rule-create).

Erstellen Sie eine Aktionsregel, um Benachrichtigungen in einer Ressourcengruppe zu unterdrücken.

```azurecli
az monitor action-rule create --resource-group MyResourceGroupName \
                              --name MyNewActionRuleName \
                              --location Global \
                              --status Enabled \
                              --rule-type Suppression \
                              --scope-type ResourceGroup \
                              --scope /subscriptions/0b1f6471-1bf0-4dda-aec3-cb9272f09590/resourceGroups/MyResourceGroupName \
                              --suppression-recurrence-type Always \
                              --alert-context Contains Computer-01 \
                               --monitor-service Equals "Log Analytics"
```

Erstellen Sie eine Aktionsregel, um Benachrichtigungen für alle Warnungen des Schweregrads Sev4 auf allen VMs im Abonnement an Wochenenden zu unterdrücken.

```azurecli
az monitor action-rule create --resource-group MyResourceGroupName \
                              --name MyNewActionRuleName \
                              --location Global \
                              --status Enabled \
                              --rule-type Suppression \
                              --severity Equals Sev4 \
                              --target-resource-type Equals Microsoft.Compute/VirtualMachines \
                              --suppression-recurrence-type Weekly \
                              --suppression-recurrence 0 6 \
                              --suppression-start-date 12/09/2018 \
                              --suppression-end-date 12/18/2018 \
                              --suppression-start-time 06:00:00 \
                              --suppression-end-time 14:00:00

```

* * *

## <a name="example-scenarios"></a>Beispielszenarien

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Szenario 1: Unterdrückung von Warnungen nach Schweregrad

Contoso möchte Benachrichtigungen für alle Warnungen des Schweregrads Sev4 auf allen VMs im Abonnement **ContosoSub** an Wochenenden unterdrücken.

**Lösung:** Erstellen Sie eine Aktionsregel mit:
* Bereich = **ContosoSub**
* Filter
    * Schweregrad = **Sev4**
    * Ressourcentyp = **Virtual Machines**
* Unterdrückung, deren Wiederholung auf wöchentlich festgelegt ist, und **Samstag** und **Sonntag** sind ausgewählt

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Szenario 2: Unterdrückung von Warnungen nach Warnungskontext (Nutzlast)

Contoso möchte Benachrichtigungen für alle Protokollwarnungen, die für **Computer-01** in **ContosoSub** generiert werden, unbefristet unterdrücken, während Wartungsarbeiten ausgeführt werden.

**Lösung:** Erstellen Sie eine Aktionsregel mit:
* Bereich = **ContosoSub**
* Filter
    * Überwachungsdienst = **Log Analytics**
    * Warnungskontext (Nutzlast) enthält **Computer-01**
* Unterdrückung ist auf **From now (Always)** (Ab jetzt (Immer)) festgelegt

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Szenario 3: In einer Ressourcengruppe definierte Aktionsgruppe

Contoso hat [eine Metrikwarnung auf Abonnementebene](./alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) definiert. Contoso möchte aber die Aktionen definieren, die bei Warnungen speziell ausgelöst werden, die aus der Ressourcengruppe **ContosoRG** generiert wurden.

**Lösung:** Erstellen Sie eine Aktionsregel mit:
* Bereich = **ContosoRG**
* Keine Filter
* Aktionsgruppe auf **ContosoActionGroup** festgelegt

> [!NOTE]
> *In Aktionsregeln definierte Aktionsgruppen und Warnungsregeln arbeiten unabhängig voneinander, und es erfolgt keine Deduplizierung*. Wenn im zuvor beschriebenen Szenario eine Aktionsgruppe für die Warnungsregel definiert wurde, wird sie in Verbindung mit der in der Aktionsregel definierten Aktionsgruppe ausgelöst.

## <a name="managing-your-action-rules"></a>Verwalten Ihrer Aktionsregeln

### <a name="portal"></a>[Portal](#tab/portal)

Sie können Ihre Aktionsregeln in der Listenansicht anzeigen und verwalten:

![Listenansicht für Aktionsregeln](media/alerts-action-rules/action-rules-list-view.png)

Hier können Sie Aktionsregeln je nach Bedarf aktivieren, deaktivieren und löschen, indem Sie das jeweilige Kontrollkästchen daneben aktivieren. Wenn Sie eine Aktionsregel auswählen, wird die zugehörige Konfigurationsseite geöffnet. Auf der Seite können Sie die Definition der Aktionsregel aktualisieren und die Regel aktivieren oder deaktivieren.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Sie können Ihre Aktionsregeln mit dem Befehl [az monitor action-rule](/cli/azure/ext/alertsmanagement/monitor) aus der Azure CLI anzeigen und verwalten.

Bevor Sie Aktionsregeln mit der Azure CLI verwalten, bereiten Sie Ihre Umgebung anhand der Anweisungen in [Konfigurieren einer Aktionsregel](#configuring-an-action-rule) vor.

```azurecli
# List all action rules for a subscription
az monitor action-rule list

# Get details of an action rule
az monitor action-rule show --resource-group MyResourceGroupName --name MyActionRuleName

# Update an action rule.
az monitor action-rule update --resource-group MyResourceGroupName --name MyActionRuleName --status Disabled

# Delete an action rule.
az monitor action-rule delete --resource-group MyResourceGroupName --name MyActionRuleName
```

* * *

## <a name="best-practices"></a>Bewährte Methoden

Protokollwarnungen, die mit der Option [Anzahl von Ergebnissen](./alerts-unified-log.md) erstellt werden, generieren eine einzige Warnungsinstanz anhand sämtlicher Suchergebnisse (die sich über mehrere Computer erstrecken können). Wenn in diesem Szenario eine Aktionsregel den Filter **Warnungskontext (Nutzlast)** verwendet, reagiert sie auf die Warnungsinstanz, sofern eine Übereinstimmung vorhanden ist. Wenn in Szenario 2, wie zuvor beschrieben, die Suchergebnisse für die generierte Protokollwarnung sowohl **Computer-01** als auch **Computer-02** enthalten, wird die gesamte Benachrichtigung unterdrückt. Es wird überhaupt keine Benachrichtigung für **Computer-02** generiert.

![Diagramm der Aktionsregeln und Protokollwarnungen mit hervorgehobener einzelner Warnungsinstanz](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

Um Protokollwarnungen mit Aktionsregeln optimal zu nutzen, empfiehlt es sich, Protokollwarnungen mit der Option [Metrische Maßeinheit](./alerts-unified-log.md) zu erstellen. Separate Benachrichtigungsinstanzen werden von dieser Option basierend auf dem definierten Gruppenfeld generiert. In Szenario 2 werden schließlich separate Warnungsinstanzen für **Computer-01** und **Computer-02** generiert. Aufgrund der Aktionsregel, die im Szenario beschrieben wird, wird nur die Benachrichtigung für **Computer-01** unterdrückt. Die Benachrichtigung für **Computer-02** wird weiterhin normal ausgelöst.

![Aktionsregeln und Protokollwarnungen (Anzahl von Ergebnissen)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="while-im-configuring-an-action-rule-id-like-to-see-all-the-possible-overlapping-action-rules-so-that-i-avoid-duplicate-notifications-is-it-possible-to-do-that"></a>Beim Konfigurieren einer Aktionsregel möchte ich alle möglichen einander überlappenden Aktionsregeln sehen, sodass ich doppelte Benachrichtigungen vermeiden kann. Ist dies möglich?

Nachdem Sie einen Bereich definiert haben, können Sie bei der Konfiguration einer Aktionsregel eine Liste von Aktionsregeln anzeigen, die sich für den gleichen Bereich überschneiden (falls vorhanden). Diese Überlappung kann wie folgt aussehen:

* Genaue Übereinstimmung: Beispielsweise können sich die von Ihnen definierte Aktionsregel und die überlappende Aktionsregel auf dasselbe Abonnement beziehen.
* Teilmenge: Die von Ihnen definierte Aktionsregel bezieht sich z.B. auf ein Abonnement, und die überlappende Aktionsregel bezieht sich auf eine Ressourcengruppe in diesem Abonnement.
* Obermenge: Die von Ihnen definierte Aktionsregel bezieht sich auf eine Ressourcengruppe, und die überlappende Aktionsregel bezieht sich auf das Abonnement, das diese Ressourcengruppe enthält.
* Schnittmenge: Die von Ihnen definierte Aktionsregel bezieht sich z.B. auf **VM1** und **VM2**, und die überlappende Aktionsregel bezieht sich auf **VM2** und **VM3**.

![Screenshot der Seite „Neue Aktionsregel“ mit Anzeige der überlappenden Aktionsregeln im Fenster „Für denselben Bereich definierte Aktionsregeln“](media/alerts-action-rules/action-rules-overlapping.png)

### <a name="while-im-configuring-an-alert-rule-is-it-possible-to-know-if-there-are-already-action-rules-defined-that-might-act-on-the-alert-rule-im-defining"></a>Kann ich beim Konfigurieren einer Warnungsregel feststellen, ob bereits Aktionsregeln definiert sind, die möglicherweise Aktionen für die Warnungsregel ausführen, die ich gerade definiere?

Nachdem Sie die Zielressource für Ihre Warnungsregel definiert haben, können Sie die Liste der Aktionsregeln anzeigen, die für den gleichen Bereich gelten (falls vorhanden), indem Sie **Konfigurierte Aktionen anzeigen** im Abschnitt **Aktionen** auswählen. Diese Liste wird gemäß den folgenden Szenarien für den Bereich mit Daten aufgefüllt:

* Genaue Übereinstimmung: Beispielsweise können sich die von Ihnen definierte Warnungsregel und die Aktionsregel auf dasselbe Abonnement beziehen.
* Teilmenge: Die von Ihnen definierte Warnungsregel bezieht sich auf ein Abonnement, und die Aktionsregel bezieht sich auf eine Ressourcengruppe in diesem Abonnement.
* Obermenge: Die von Ihnen definierte Warnungsregel bezieht sich auf eine Ressourcengruppe, und die Aktionsregel bezieht sich auf das Abonnement, das die Ressourcengruppe enthält.
* Schnittmenge: Die von Ihnen definierte Warnungsregel bezieht sich beispielsweise auf **VM1** und **VM2**, und die Aktionsregel bezieht sich auf **VM2** und **VM3**.

![Überlappende Aktionsregeln](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

### <a name="can-i-see-the-alerts-that-have-been-suppressed-by-an-action-rule"></a>Kann ich die durch eine Aktionsregel unterdrückten Warnungen anzeigen?

Auf der [Seite mit der Liste der Warnungen](./alerts-managing-alert-instances.md) kann eine weitere Spalte mit dem Namen **Suppression Status** (Unterdrückungsstatus) ausgewählt werden. Wenn die Benachrichtigung für eine Warnungsinstanz unterdrückt wurde, wird dieser Status in der Liste angezeigt.

![Unterdrückte Warnungsinstanzen](media/alerts-action-rules/action-rules-suppressed-alerts.png)

### <a name="if-theres-an-action-rule-with-an-action-group-and-another-with-suppression-active-on-the-same-scope-what-happens"></a>Was geschieht, wenn eine Aktionsregel mit einer Aktionsgruppe und eine weitere mit aktiver Unterdrückung im selben Bereich vorhanden sind?

Unterdrückung hat in einem Bereich immer Vorrang.

### <a name="what-happens-if-i-have-a-resource-thats-monitored-in-two-separate-action-rules-do-i-get-one-or-two-notifications-for-example-vm2-in-the-following-scenario"></a>Was geschieht, wenn ich eine Ressource verwende, die von zwei separaten Aktionsregeln überwacht wird? Erhalte ich dann eine oder zwei Benachrichtigungen? Beispielsweise **VM2** im folgenden Szenario:

   `action rule AR1 defined for VM1 and VM2 with action group AG1`

   `action rule AR2 defined for VM2 and VM3 with action group AG1`

Für jede Warnung zu „VM1“ und „VM3“ wird die Aktionsgruppe „AG1“ ein Mal ausgelöst. Für jede Warnung zu **VM2** wird die Aktionsgruppe „AG1“ zwei Mal ausgelöst (Aktionen werden von Aktionsregeln nicht dedupliziert).

### <a name="what-happens-if-i-have-a-resource-monitored-in-two-separate-action-rules-and-one-calls-for-action-while-another-for-suppression-for-example-vm2-in-the-following-scenario"></a>Was geschieht, wenn ich eine Ressource in zwei separaten Aktionsregeln überwachen lasse, von denen eine eine Aktion ausführt und die andere unterdrückt wird? Beispielsweise **VM2** im folgenden Szenario:

   `action rule AR1 defined for VM1 and VM2 with action group AG1`

   `action rule AR2 defined for VM2 and VM3 with suppression`

Für jede Warnung zu „VM1“ wird die Aktionsgruppe „AG1“ ein Mal ausgelöst. Aktionen und Benachrichtigungen für jede Warnung zu „VM2“ und „VM3“ werden unterdrückt.

### <a name="what-happens-if-i-have-an-alert-rule-and-an-action-rule-defined-for-the-same-resource-calling-different-action-groups-for-example-vm1-in-the-following-scenario"></a>Was geschieht, wenn ich eine Warnungsregel und eine Aktionsregel für dieselbe Ressource definiert habe, die unterschiedliche Aktionsgruppen aufrufen? Beispielsweise **VM1** im folgenden Szenario:

   `alert rule rule1 on VM1 with action group AG2`

   `action rule AR1 defined for VM1 with action group AG1`

Für jede Warnung zu „VM1“ wird die Aktionsgruppe „AG1“ ein Mal ausgelöst. Wird Warnungsregel „rule1“ ausgelöst, wird zusätzlich auch „AG2“ ausgelöst. In Aktionsregeln definierte Aktionsgruppen und Warnungsregeln arbeiten unabhängig voneinander, und es erfolgt keine Deduplizierung.

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Warnungen in Azure](./alerts-overview.md)
---
title: Blaupausenbeispiel für den Vergleichstest für die Azure-Sicherheit – Übersicht
description: Übersicht über das Blaupausenbeispiel für den Vergleichstest für die Azure-Sicherheit Dieses Blaupausenbeispiel unterstützt Kunden bei der Bewertung spezifischer Kontrollen.
ms.date: 01/27/2021
ms.topic: sample
ms.openlocfilehash: 38a50970999965b6a86b8ce8882006c169f2dc5a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98919238"
---
# <a name="azure-security-benchmark-blueprint-sample"></a>Blaupausenbeispiel für den Azure-Sicherheitsvergleichstest

Das Blaupausenbeispiel für den Azure-Sicherheitsvergleichstest bietet mithilfe von [Azure Policy](../../policy/overview.md) Schutzmaßnahmen für Governance, die Ihnen beim Bewerten bestimmter Steuerungen des [Azure-Sicherheitsvergleichstest v1](../../../security/benchmarks/overview.md) helfen. Diese Blaupause hilft Kunden bei der Bereitstellung eines Kernsatzes von Richtlinien für jede in Azure bereitgestellte Architektur, in der sie die Implementierung von Steuerungen des Vergleichstests für die Azure-Sicherheit beabsichtigen.

## <a name="control-mapping"></a>Steuerungszuordnung

In der [Azure Policy-Steuerungszuordnung](../../policy/samples/azure-security-benchmark.md) finden Sie Details zu den in dieser Blaupause enthaltenen Richtliniendefinitionen. Darüber hinaus erfahren Sie, wie diese Richtlinien den **Compliancebereichen** und **Steuerungen** im Azure-Sicherheitsvergleichstest entsprechen. Bei der Zuweisung zu einer Architektur werden Ressourcen von Azure Policy auf Nichtkonformität mit den zugewiesenen Richtliniendefinitionen überprüft. Weitere Informationen finden Sie unter [Was ist Azure Policy?](../../policy/overview.md).

## <a name="deploy"></a>Bereitstellen

Zum Bereitstellen des Azure Blueprints-Blaupausenbeispiels für den Azure-Sicherheitsvergleichstest müssen die folgenden Schritte ausgeführt werden:

> [!div class="checklist"]
> - Erstellen einer neuen Blaupause mithilfe des Beispiels
> - Markieren Ihrer Kopie des Beispiels als **Veröffentlicht**
> - Zuweisen Ihrer Kopie der Blaupause zu einem vorhandenen Abonnement

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

### <a name="create-blueprint-from-sample"></a>Erstellen einer Blaupause mithilfe des Beispiels

Implementieren Sie zuerst das Blaupausenbeispiel, indem Sie mithilfe des Beispiels eine neue Blaupause in Ihrer Umgebung erstellen.

1. Wählen Sie **Alle Dienste** im linken Bereich aus. Suchen Sie nach **Blaupausen**, und wählen Sie die Option aus.

1. Klicken Sie links auf der Seite **Erste Schritte** unter _Blaupause erstellen_ auf die Schaltfläche **Erstellen**.

1. Suchen Sie unter _Weitere Beispiele_ nach dem Blaupausenbeispiel **Azure-Sicherheitsvergleichstest v1**, und wählen Sie zum Auswählen dieses Beispiels den Namen aus.

1. Geben Sie die _Grundlagen_ des Blaupausenbeispiels ein:

   - **Name der Blaupause**: Geben Sie einen Namen für Ihre Kopie des Blaupausenbeispiels für den Azure-Sicherheitsvergleichstest an.
   - **Definitionsspeicherort**: Klicken Sie auf die Schaltfläche mit den Auslassungspunkten, und wählen Sie die Verwaltungsgruppe aus, in der Sie Ihre Kopie des Beispiels speichern möchten.

1. Wählen Sie oben auf der Seite die Registerkarte _Artefakte_ oder unten auf der Seite die Option **Weiter: Artefakte** aus.

1. Überprüfen Sie die Liste der Artefakte, die im Blaupausenbeispiel enthalten sind. Viele der Artefakte enthalten Parameter, die Sie später definieren. Wählen Sie **Entwurf speichern** aus, nachdem Sie das Blaupausenbeispiel überprüft haben.

### <a name="publish-the-sample-copy"></a>Veröffentlichen der Kopie des Beispiels

Ihre Kopie des Blaupausenbeispiels wurde jetzt in Ihrer Umgebung erstellt. Sie wird im Modus **Entwurf** erstellt und muss **veröffentlicht** werden, bevor sie zugewiesen und bereitgestellt werden kann. Die Kopie des Blaupausenbeispiels kann Ihrer Umgebung und Ihren Anforderungen angepasst werden. Durch diese Änderungen ist sie aber möglicherweise nicht mehr mit den Empfehlungen für den Azure-Sicherheitsvergleichstest konform.

1. Wählen Sie **Alle Dienste** im linken Bereich aus. Suchen Sie nach **Blaupausen**, und wählen Sie die Option aus.

1. Wählen Sie links die Seite **Blaupausendefinitionen** aus. Verwenden Sie die Filter, um Ihre Kopie des Blaupausenbeispiels zu suchen, und wählen Sie es aus.

1. Wählen Sie oben auf der Seite die Option **Blaupause veröffentlichen** aus. Geben Sie auf der neuen Seite rechts für Ihre Kopie des Blaupausenbeispiels eine **Version** an. Diese Eigenschaft ist hilfreich, wenn Sie später Änderungen vornehmen. Geben Sie **Änderungshinweise** an, etwa „Erste Version aus dem Blaupausenbeispiel für den Azure-Sicherheitsvergleichstest veröffentlicht“. Wählen Sie dann unten auf der Seite **Veröffentlichen** aus.

### <a name="assign-the-sample-copy"></a>Zuweisen der Kopie des Beispiels

Nachdem die Kopie des Blaupausenbeispiels erfolgreich **veröffentlicht** wurde, kann sie einem Abonnement innerhalb der Verwaltungsgruppe, in der sie gespeichert wurde, zugewiesen werden. In diesem Schritt werden Parameter angegeben, damit jede Bereitstellung der Kopie des Blaupausenbeispiels eindeutig ist.

1. Wählen Sie **Alle Dienste** im linken Bereich aus. Suchen Sie nach **Blaupausen**, und wählen Sie die Option aus.

1. Wählen Sie links die Seite **Blaupausendefinitionen** aus. Verwenden Sie die Filter, um Ihre Kopie des Blaupausenbeispiels zu suchen, und wählen Sie es aus.

1. Wählen Sie oben auf der Seite mit der Blaupausendefinition die Option **Blaupause zuweisen** aus.

1. Geben Sie die Parameterwerte für die Blaupausenzuweisung an:

   - Grundlagen

     - **Abonnements**: Wählen Sie mindestens eines der Abonnements in der Verwaltungsgruppe aus, in der Sie die Kopie des Blaupausenbeispiels gespeichert haben. Wenn Sie mehrere Abonnements auswählen, wird für jedes Abonnement eine Zuweisung mit den eingegebenen Parametern erstellt.
     - **Zuweisungsname:** Der Name wird basierend auf dem Namen der Blaupause vorab ausgefüllt.
       Sie können ihn nach Bedarf ändern oder unverändert übernehmen.
     - **Standort**: Wählen Sie eine Region aus, in der die verwaltete Identität erstellt werden soll. Azure Blueprint verwendet diese verwaltete Identität zum Bereitstellen aller Artefakte in der zugewiesenen Blaupause. Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Version der Blaupausendefinition:** Wählen Sie eine **veröffentlichte** Version Ihrer Kopie des Blaupausenbeispiels aus.

   - Zuweisung sperren

     Wählen Sie die Blaupausensperreinstellung für die Umgebung aus. Weitere Informationen finden Sie unter [Grundlegendes zur Ressourcensperre in Azure Blueprint](../concepts/resource-locking.md).

   - Verwaltete Identität

     Behalten Sie die Standardeinstellung _Vom System zugewiesen_ für die verwaltete Identität bei.

   - Artefaktparameter

     Die in diesem Abschnitt definierten Parameter gelten für das Artefakt, unter dem sie definiert werden. Diese Parameter sind [dynamische Parameter](../concepts/parameters.md#dynamic-parameters), da sie während der Zuweisung der Blaupause definiert werden. Eine vollständige Liste der Artefaktparameter und die zugehörigen Beschreibungen finden Sie in der [Tabelle der Artefaktparameter](#artifact-parameters-table).

1. Nachdem Sie alle Parameter eingegeben haben, wählen Sie unten auf der Seite die Option **Zuweisen** aus. Die Blaupausenzuweisung wird erstellt, und die Artefaktbereitstellung wird gestartet. Die Bereitstellung dauert ungefähr eine Stunde. Um den Status der Bereitstellung zu überprüfen, öffnen Sie die Blaupausenzuweisung.

> [!WARNING]
> Der Azure Blueprints-Dienst und die integrierten Blaupausenbeispiele sind **kostenlos**. Azure-Ressourcen werden [nach Produkt abgerechnet](https://azure.microsoft.com/pricing/). Verwenden Sie den [Preisrechner](https://azure.microsoft.com/pricing/calculator/) zum Schätzen der Kosten für die Ausführung der Ressourcen, die mit diesem Blaupausenbeispiel bereitgestellt werden.

### <a name="artifact-parameters-table"></a>Tabelle der Artefaktparameter

Die folgende Tabelle enthält eine Aufstellung der Parameter des Blaupausenartefakts:

|Artefaktname|Artefakttyp|Parametername|BESCHREIBUNG|
|-|-|-|-|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Liste der aus der Windows-VM-Administratorengruppe ausgeschlossenen Benutzer|Eine durch Semikolon getrennte Liste von Mitgliedern, die aus der Gruppe der lokalen Administratoren ausgeschlossen werden sollen. Beispiel: Administrator; myUser1; myUser2|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Liste der Benutzer, die in der Windows-VM-Administratorengruppe enthalten sein müssen|Eine durch Semikolon getrennte Liste von Mitgliedern, die in der Gruppe der lokalen Administratoren enthalten sein sollen. Beispiel: Administrator; myUser1; myUser2|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Liste der Benutzer, die *als einzige* in der Windows-VM-Administratorengruppe enthalten sein dürfen|Eine durch Semikolon getrennte Liste aller erwarteten Mitglieder der Gruppe der lokalen Administratoren. Beispiel: Administrator; myUser1; myUser2|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Liste der Regionen, in denen Network Watcher aktiviert werden soll|Verwenden Sie „Get-AzLocation“, um eine vollständige Liste der Regionen abzurufen.|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Virtuelles Netzwerk, mit dem VMs verbunden werden sollen|Beispiel: /subscriptions/YourSubscriptionId/resourceGroups/YourResourceGroupName/providers/Microsoft.Network/virtualNetworks/Name|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Netzwerkgateway, das von virtuellen Netzwerken verwendet werden soll|Beispiel: /subscriptions/YourSubscriptionId/resourceGroups/YourResourceGroup/providers/Microsoft.Network/virtualNetworkGateways/Name|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Liste der Arbeitsbereichs-IDs, mit denen Log Analytics-Agents eine Verbindung herstellen sollen|Eine durch Semikolons getrennte Liste der Arbeitsbereichs-IDs, mit denen der Log Analytics-Agent verbunden werden soll|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Liste von Ressourcentypen, für die Diagnoseprotokolle aktiviert werden sollen|Hiermit wird die Diagnoseeinstellung für ausgewählte Ressourcentypen überwacht.|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Neueste PHP-Version|Neueste unterstützte PHP-Version für App Services|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Neueste Java-Version|Neueste unterstützte Java-Version für App Services|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Neueste Python-Version für Windows|Neueste unterstützte Python-Version für App Services|
|Azure Security Benchmark-Empfehlungen überwachen und spezifische unterstützende VM-Erweiterungen bereitstellen|Richtlinienzuweisung|Neueste Python-Version für Linux|Neueste unterstützte Python-Version für App Services|

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../how-to/update-existing-assignments.md).
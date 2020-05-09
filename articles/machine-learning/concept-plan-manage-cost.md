---
title: Planen und Verwalten von Kosten
titleSuffix: Azure Machine Learning
description: Planen und verwalten Sie Kosten für Azure Machine Learning mithilfe der Kostenanalyse im Azure-Portal. Lernen Sie beim Erstellen von Machine Learning-Modellen weitere kostensparende Tipps zur Senkung Ihrer Kosten kennen.
author: sdgilley
ms.author: sgilley
ms.custom: subject-cost-optimization
ms.reviewer: nigup
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: 67b703f0079e26d01330d52d170f99699480fad6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195777"
---
# <a name="plan-and-manage-costs-for-azure-machine-learning"></a>Planen und Verwalten von Kosten für Azure Machine Learning

In diesem Artikel wird beschrieben, wie Sie die Kosten für Azure Machine Learning planen und verwalten. Bevor Sie Ressourcen hinzufügen, verwenden Sie zunächst den Azure-Preisrechner, um die Kosten zu planen. Überprüfen Sie dann beim Hinzufügen der Azure-Ressourcen die geschätzten Kosten. Verwenden Sie schließlich kostensparende Tipps beim Trainieren Ihres Modells mit verwalteten Azure Machine Learning Computeclustern.

Nachdem Sie mit der Verwendung der Azure Machine Learning-Ressourcen begonnen haben, können Sie die Features für die Kostenverwaltung verwenden, um Budgets festzulegen und die Kosten zu überwachen. Überprüfen Sie darüber hinaus die vorhergesagten Kosten, und ermitteln Sie Ausgabentrends, um Bereiche zu identifizieren, in denen ggf. Maßnahmen erforderlich sind.

Beachten Sie, dass die Kosten für Azure Machine Learning nur einen Teil der monatlichen Kosten Ihrer Azure-Rechnung darstellen. Bei Nutzung anderer Azure-Dienste werden Ihnen alle Azure-Dienste und -Ressourcen in Rechnung gestellt, die unter Ihrem Azure-Abonnement genutzt werden, einschließlich der Dienste von Drittanbietern. In diesem Artikel wird beschrieben, wie Sie die Kosten für Azure Machine Learning planen und verwalten. Nachdem Sie sich mit der Verwaltung der Kosten für Azure Machine Learning vertraut gemacht haben, können Sie ähnliche Methoden anwenden, um die Kosten für alle Azure-Dienste zu verwalten, die unter Ihrem Abonnement genutzt werden.

Verwenden Sie beim Trainieren Ihrer Machine Learning-Modelle verwaltete Azure Machine Learning-Computecluster, um weitere Tipps zur Kostensenkung zu nutzen:

* Konfigurieren Sie Ihre Trainingscluster für automatische Skalierung
* Legen Sie Kontingente für Ihr Abonnement und Ihre Arbeitsbereiche fest
* Legen Sie Terminierungsrichtlinien für Ihre Trainingsläufe fest
* Verwenden Sie virtuelle Computer (VMs) mit niedriger Priorität
* Verwenden Sie eine reservierte Azure-VM-Instanz

## <a name="prerequisites"></a>Voraussetzungen

Die Kostenanalyse unterstützt verschiedene Arten von Azure-Kontotypen. Die vollständige Liste der unterstützten Kontotypen finden Sie unter [Grundlegendes zu Cost Management-Daten](../cost-management-billing/costs/understand-cost-mgt-data.md). Um Kostendaten anzeigen zu können, müssen Sie mindestens über Lesezugriff auf Ihr Azure-Konto verfügen. 

Informationen zum Zuweisen des Zugriffs auf Azure Cost Management-Daten finden Sie unter [Zuweisen des Zugriffs auf Cost Management-Daten](../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="estimate-costs"></a>Schätzen der Kosten

Verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/) zum Schätzen der Kosten, bevor Sie die Ressourcen in einem Azure Machine Learning-Konto erstellen. Wählen Sie auf der linken Seite **KI und Machine Learning** und dann **Azure Machine Learning** aus, um anzufangen.  

Der folgende Screenshot zeigt die Kostenschätzung unter Verwendung des Preisrechners:

:::image type="content" source="media/concept-plan-manage-cost/capacity-calculator-cost-estimate.png" alt-text="Kostenschätzung im Azure-Rechner":::

Wenn Sie Ihrem Arbeitsbereich neue Ressourcen hinzufügen, kehren Sie zu diesem Preisrechner zurück, und fügen Sie hier die gleiche Ressource hinzu, um Ihre Kostenschätzung zu aktualisieren.

Für die Vorschauversion der Enterprise Edition wird für ML kein Zuschlag in Rechnung gestellt. Mit Erreichen der allgemeinen Verfügbarkeit der Enterprise Edition wird für Machine Learning ein Zuschlag (für Training und Rückschließen) in Rechnung gestellt.  Weitere Details finden Sie unter [Azure Machine Learning-Preise](https://azure.microsoft.com/pricing/details/machine-learning/).

## <a name="get-cost-alerts"></a>Abrufen von Kostenwarnungen

Erstellen Sie [Budgets](../cost-management/tutorial-acm-create-budgets.md), um Kosten zu verwalten, und erstellen Sie [Warnungen](../cost-management/cost-mgt-alerts-monitor-usage-spending.md), die die Projektbeteiligten automatisch über Ausgabenanomalien und Überschreitungsrisiken informieren. Warnungen basieren auf Ausgaben im Vergleich zum Budget und zu Kostenschwellenwerten. Budgets und Warnungen werden für Azure-Abonnements und -Ressourcengruppen erstellt und sind daher im Rahmen einer umfassenden Strategie zur Kostenüberwachung hilfreich. Unter Umständen weisen Budgets und Warnungen aber eine eingeschränkte Funktionalität für die Verwaltung der Kosten einzelner Azure-Dienste auf, weil diese Funktionen für die Nachverfolgung von Kosten auf höherer Ebene ausgelegt sind.

## <a name="monitor-costs"></a>Überwachen der Kosten

Wenn Sie Ressourcen mit Azure Machine Learning verwenden, fallen Kosten an. Die Kosten pro Azure-Ressourcennutzungseinheit variieren nach dem Zeitintervall (Sekunden, Minuten, Stunden und Tage) oder der Nutzung von Anforderungseinheiten. Kosten fallen an, sobald die Nutzung von Azure Machine Learning beginnt. Zeigen Sie diese Kosten im Bereich [Kostenanalyse](../cost-management/quick-acm-cost-analysis.md) im Azure-Portal an.

Zeigen Sie Kosten in Diagrammen und Tabellen für verschiedene Zeitintervalle an. Beispiele hierfür sind „Tag“, „Aktuell“, „Vorheriger Monat“ und „Jahr“. Ferner können Sie Kosten im Vergleich mit Budgets und vorhergesagten Kosten anzeigen. Wenn Sie im Laufe der Zeit zu längeren Zeiträumen wechseln, können Sie Ausgabentrends ermitteln und erkennen, wo es ggf. zu hohen Ausgaben gekommen ist. Wenn Sie Budgets erstellt haben, sehen Sie, wo diese überschritten wurden.  

Es wird kein separater Dienstbereich für Machine Learning angezeigt.  Stattdessen sehen Sie die verschiedenen Ressourcen, die Sie Ihren Machine Learning-Arbeitsbereichen hinzugefügt haben.

## <a name="use-amlcompute"></a>Verwenden von AmlCompute

Bei sich ständig verändernden Daten benötigen Sie ein schnelles und optimiertes Training und Nachtraining von Modellen, um die Genauigkeit der Modelle aufrecht zu erhalten. Fortlaufendes Training hat aber seinen Preis, insbesondere bei Deep Learning-Modellen auf GPUs. 

Azure Machine Learning-Benutzer können den verwalteten Azure Machine Learning-Computecluster verwenden, der auch als „AmlCompute“ bezeichnet wird. AmlCompute unterstützt eine Vielzahl von GPU- und CPU-Optionen. AmlCompute wird intern im Auftrag Ihres Abonnements von Azure Machine Learning gehostet, bietet aber die gleiche Sicherheit, Compliance und Governance auf Konzernniveau wie Azure IaaS im Cloudmaßstab.

Da sich diese Computepools innerhalb der Azure IaaS-Infrastruktur befinden, können Sie Ihr Training mit den gleichen Sicherheits- und Complianceanforderungen bereitstellen, skalieren und verwalten wie den Rest Ihrer Infrastruktur.  Diese Bereitstellungen erfolgen in Ihrem Abonnement und richten sich nach Ihren Governanceregeln. Weitere Informationen zu [Azure Machine Learning Compute](how-to-set-up-training-targets.md#amlcompute).

## <a name="configure-training-clusters-for-autoscaling"></a>Konfigurieren von Trainingsclustern für automatische Skalierung

Cluster mit automatischer Skalierung auf der Grundlage Ihrer Workloadanforderungen helfen Ihnen, Ihre Kosten zu senken, indem Sie nur nutzen, was Sie benötigen. AmlCompute-Cluster sind dafür ausgelegt, ausgehend von den Anforderungen Ihrer Workload automatisch zu skalieren. Der Cluster kann bis zur maximalen Anzahl der bereitgestellten Knoten und im Rahmen des für das Abonnement bestimmten Kontingents hochskaliert werden. Nach dem Abschluss der einzelnen Läufe gibt der Cluster Knoten frei und skaliert automatisch auf die von Ihnen bestimmte minimale Knotenanzahl herunter.

Über das Festlegen der Minimal- und Maximalanzahl der Knoten hinaus können Sie die Leerlaufzeit des Knotens vor dem Herunterskalieren optimieren. Standardmäßig ist die Leerlaufzeit vor dem Herunterskalieren auf 120 Sekunden festgelegt.

+ Wenn Sie weniger iterative Experimente ausführen, setzen Sie diese Zeit herab, um Kosten zu sparen. 
+ Wenn Sie hochgradig iterative Dev/Test-Experimente ausführen, müssen Sie diesen Wert möglicherweise heraufsetzen, damit Sie nicht für ständiges Hoch- und Herunterskalieren nach jeder Änderung Ihres Trainingsskripts oder Ihrer Umgebung zahlen müssen.

AmlCompute-Cluster können im Azure-Portal für Ihre wechselnden Workloadanforderungen konfiguriert werden, mithilfe der [AmlCompute SDK-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py), des [AmlCompute-CLIs](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute) und mit den [REST-APIs](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/machinelearningservices/resource-manager/Microsoft.MachineLearningServices/stable).

```azure cli
az ml computetarget create amlcompute --name testcluster --vm-size Standard_NC6 --min-nodes 0 --max-nodes 5 --idle-seconds-before-scaledown 300
```

## <a name="set-quotas-on-resources"></a>Festlegen von Kontingenten für Ressourcen

Ganz wie andere Azure-Computeressourcen besitzt auch AmlCompute von Haus aus eine inhärente [Kontingentkonfiguration (oder Grenzwertkonfiguration)](how-to-manage-quotas.md#azure-machine-learning-compute). Dieses Kontingent ist für die VM-Familie spezifisch (z. B. Dv2-Serie, NCv3-Serie) und variiert für die einzelnen Abonnements nach Region. Abonnements beginnen mit kleinen Standardwerten für den Einstieg, aber mit dieser Einstellung können Sie die Menge der Amlcompute-Ressourcen bestimmen, die für das Hochfahren in Ihrem Abonnement verfügbar sind. 

Konfigurieren Sie außerdem für jeden Arbeitsbereich innerhalb eines Abonnements das [Kontingent auf Arbeitsbereichsebene pro VM-Familie](/how-to-manage-quotas.md#workspace-level-quota). Dies ermöglicht Ihnen eine präzisere Kontrolle der Kosten, die für die einzelnen Arbeitsbereiche anfallen können, und eine Einschränkung bestimmter VM-Familien. 

Beginnen Sie das Festlegen von Kontingenten auf Arbeitsbereichsebene im [Azure-Portal](https://portal.azure.com).  Wählen Sie einen beliebigen Arbeitsbereich in Ihrem Abonnement aus, und wählen Sie im linken Bereich **Nutzung + Kontingente** aus. Wählen Sie anschließend die Registerkarte **Kontingente konfigurieren** aus, um die Kontingente anzuzeigen. Sie benötigen Berechtigungen auf Abonnementebene, um dieses Kontingent festzulegen, da diese Einstellung mehrere Arbeitsbereiche betrifft.

## <a name="set-run-auto-termination-policies"></a>Festlegen von Richtlinien für die automatische Beendigung von Läufen 

Konfigurieren Sie für Ihre Trainingsläufe eine beschränkte Ausführungsdauer oder eine vorzeitige Beendigung, falls bestimmte Bedingungen erfüllt sind, insbesondere, wenn Sie die integrierte Hyperparameteroptimierung von Azure Machine Learning oder die Funktionen für automatisiertes maschinelles Lernen verwenden. 

Dies sind einige der Optionen, die sich Ihnen bieten:
* Definieren Sie einen Parameter mit dem Namen `max_run_duration_seconds` in ihrer RunConfiguration, um die maximale Dauer zu steuern, auf die ein Lauf auf der von Ihnen gewählten Compute (lokale oder Remote-Cloudcompute) ausgedehnt werden kann.
* Definieren Sie für die [Hyperparameteroptimierung](how-to-tune-hyperparameters.md#early-termination) eine Richtlinie für vorzeitige Beendigung, eine Medianstopprichtlinie oder eine Kürzungsauswahlrichtlinie. Außerdem sollten Sie Parameter wie `max_total_runs` oder `max_duration_minutes` verwenden, um die verschiedenen Löschungen der Hyperparameter feiner zu steuern.
* Legen Sie für [automatisiertes maschinelles Lernen](how-to-configure-auto-train.md#exit) ähnliche Beendigungsrichtlinien mithilfe des Flags `enable_early_stopping` fest. Verwenden Sie darüber hinaus Eigenschaften wie `iteration_timeout_minutes` und `experiment_timeout_minutes`, um die maximale Dauer eines Laufs für das gesamte Experiment zu steuern.

## <a name="use-low-priority-vms"></a>Verwenden von virtuellen Computern mit niedriger Priorität

Azure erlaubt es Ihnen, nicht ausgelastete Überschusskapazität in Form von VMs mit niedriger Priorität für VM-Skalierungsgruppen, Batch und den Machine Learning-Dienst zu verwenden. Für diese Zuordnungen können Bevorrechtigungen festgelegt werden (pre-emptible), sie bieten jedoch den Vorteil eines niedrigeren Preises im Vergleich mit dedizierten VMs. Im Allgemeinen empfiehlt sich der Einsatz von VMs mit niedriger Priorität für Batchworkloads oder in Fällen, in denen eine Wiederherstellung nach Unterbrechungen erfolgen kann, entweder durch erneute Einleitung (bei Batchrückschließen) oder mithilfe eines Neustarts (bei Training mit Prüfpunktausführung in Deep Learning-Szenarien).

VMs mit niedriger Priorität besitzen ein einzelnes Kontingent, das sich nach der VM-Familie richtet. Weitere Informationen über [AmlCompute-Kontingente](how-to-manage-quotas.md).

Legen Sie die Priorität Ihrer VM auf eine der folgenden Weisen fest:

* Wählen Sie in Studio beim Erstellen einer VM **Niedrige Priorität** aus.

* Legen Sie mit dem Python SDK das `vm_priority`-Attribut in Ihrer Bereitstellungskonfiguration fest.  

    ```python
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                               vm_priority='lowpriority',
                                                               max_nodes=4)
    ```

* Legen Sie an der Befehlszeilenschnittstelle die `vm-priority` fest:

    ```azurecli-interactive
    az ml computetarget create amlcompute --name lowpriocluster --vm-size Standard_NC6 --max-nodes 5 --vm-priority lowpriority
    ```

 VMs mit geringer Priorität funktionieren nicht als Computeinstanzen, da sie in der Lage sein müssen, interaktive Notebookumgebungen zu unterstützen. 

## <a name="use-reserved-instances"></a>Verwenden von reservierten Instanzen

Reservierte Azure-VM-Instanzen bieten eine weitere Möglichkeit, große Einsparungen bei Computeressourcen durch eine verbindliche ein- oder dreijährige Laufzeit zu erzielen. Diese Rabatte erreichen bis zu 72 % gegenüber den Preisen für nutzungsbasierte Bezahlung und werden direkt auf Ihre monatliche Azure-Rechnung angewendet.

Azure Machine Learning Compute unterstützt reservierte Instanzen nativ. Wenn Sie also eine reservierte Instanz für ein Jahr oder drei Jahre erworben haben, wenden wir den Rabatt für die reservierte Instanz automatisch auf die verwaltete Compute an, die in Azure Machine Learning genutzt wird, ohne dass weiteres Setup ihrerseits erforderlich ist.


## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die Verwaltung von Kosten mit der [Kostenanalyse](../cost-management-billing/costs/quick-acm-cost-analysis.md).
* Weitere Informationen zu [Azure Machine Learning Compute](how-to-set-up-training-targets.md#amlcompute).
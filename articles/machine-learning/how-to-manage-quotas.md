---
title: Verwalten von Ressourcen und Kontingenten
titleSuffix: Azure Machine Learning
description: Erfahren Sie mehr über Kontingente für Azure Machine Learning-Ressourcen sowie über das Anfordern von zusätzlichem Kontingent.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 10/13/2020
ms.topic: conceptual
ms.custom: troubleshooting,contperfq4, contperfq2
ms.openlocfilehash: 4b072257d49011819fe19d6e2901560df43b26dc
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92275561"
---
# <a name="manage--increase-quotas-for-resources-with-azure-machine-learning"></a>Verwalten und Erhöhen der Kontingente für Ressourcen mit Azure Machine Learning

Azure verwendet Grenzwerte und Kontingente, um Budgetüberschreitungen aufgrund von Betrug zu vermeiden und Kapazitätseinschränkungen bei Azure Rechnung zu tragen. Berücksichtigen Sie diese Grenzwerte bei der Skalierung für Produktionsworkloads. In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> + Standardgrenzwerte für Azure-Ressourcen in Bezug auf [Azure Machine Learning](overview-what-is-azure-ml.md)
> + Anzeigen Ihrer Kontingente und Grenzwerte
> + Erstellen von Kontingenten auf Arbeitsbereichsebene
> + Anfordern von Kontingenterhöhungen
> + Privater Endpunkt und DNS-Kontingente

Neben dem Verwalten von Kontingenten erfahren Sie auch, wie Sie [Kosten für Azure Machine Learning planen und verwalten](concept-plan-manage-cost.md).

## <a name="special-considerations"></a>Besondere Überlegungen

+ Bei einem Kontingent handelt es sich um ein Kreditlimit und keine Kapazitätsgarantie. Wenn Sie einen umfangreichen Kapazitätsbedarf haben, [wenden Sie sich an den Azure-Support, um Ihr Kontingent zu erhöhen](#request-quota-increases).

+ Das Kontingent wird auf alle Dienste in Ihren Abonnements verteilt, einschließlich Azure Machine Learning. Beziehen Sie beim Berechnen der Kapazitätsbewertung alle Dienste mit ein.
    + Azure Machine Learning Compute ist eine Ausnahme und weist ein vom Kerncomputekontingent getrenntes Kontingent auf. 

+ Die Standardgrenzwerte variieren je nach angebotenem Kategorietyp, z. B. kostenlose Testversion, nutzungsbasierte Bezahlung und virtuelle Computerserien (VM) wie Dv2, F und G usw.

## <a name="default-resource-quotas"></a>Standardressourcenkontingente

In diesem Abschnitt erfahren Sie mehr über die standardmäßigen und maximalen Kontingentgrenzwerte für die folgenden Ressourcen:

+ Virtuelle Computer
+ Azure Machine Learning Compute
+ Azure Machine Learning-Pipelines
+ Container Instances
+ Storage

> [!IMPORTANT]
> Änderungen bei Limits bleiben vorbehalten. Die aktuellen Limits finden Sie immer im [Dokument](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits/) zu Kontingenten auf Dienstebene für die meisten Azure-Dienste.

### <a name="virtual-machines"></a>Virtuelle Computer
Jedes Azure-Abonnement verfügt über einen Grenzwert für die Anzahl virtueller Computer für alle Dienste. Die Kerne virtueller Computer haben einen regionalen Gesamtgrenzwert und einen regionalen Grenzwert pro Größenserie (Dv2, F usw.). Beide Grenzwerte werden separat erzwungen.

Angenommen, Sie verwenden ein Abonnement mit einem Kerngesamtgrenzwert von 30 für VMs in der Region „USA, Osten“, einem Kerngrenzwert von 30 für die A-Serie und einem Kerngrenzwert von 30 für die D-Serie. Für dieses Abonnement dürfen dann 30 virtuelle A1-Computer bzw. 30 virtuelle D1-Computer bereitgestellt werden, oder eine Kombination der beiden, bei der die Gesamtzahl von 30 Kernen nicht überschritten wird.

Die Grenzwerte für virtuelle Computer können nicht über den in der folgenden Tabelle angegebenen Wert erhöht werden.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning Compute
[Azure Machine Learning Compute](concept-compute-target.md#azure-machine-learning-compute-managed) verfügt über eine Standardkontingentgrenze für die Anzahl von Kernen und eindeutigen Computeressourcen, die pro Region in einem Abonnement zulässig sind. Dieses Kontingent ist von dem VM-Kernkontingent aus dem vorherigen Abschnitt getrennt.

[Fordern Sie eine Kontingenterhöhung](#request-quota-increases), um die Grenzwerte in diesem Abschnitt bis zu dem in der Tabelle angegebenen **maximalen Grenzwert** zu erhöhen.

Verfügbare Ressourcen:
+ **Dedizierte Kerne pro Region** haben einen Standardgrenzwert von 24 bis 300, je nach Typ Ihres Abonnementangebots.  Die Anzahl dedizierter Kerne pro Abonnement kann für jede VM-Familie erhöht werden. Spezialisierte VM-Familien wie die Serien NCv2, NCv3 oder ND beginnen mit einem Standardwert von null Kernen.

+ **Kerne mit niedriger Priorität pro Region** haben einen Standardgrenzwert von 100 bis 3000, je nach Typ Ihres Abonnementangebots. Die Anzahl der Kerne niedriger Priorität pro Abonnement kann erhöht werden. Es handelt sich bei allen VM-Familien um einen einzelnen Wert.

+ Für **Cluster pro Region** gilt der Standardgrenzwert 200. Diese teilen sich ein Trainingscluster und eine Compute-Instanz (die für Kontingentzwecke als Einzelknotencluster gilt).

Die folgende Tabelle zeigt zusätzliche Grenzwerte, die nicht überschritten werden können.

| **Ressource** | **Maximales Limit** |
| --- | --- |
| Arbeitsbereiche pro Ressourcengruppe | 800 |
| Knoten in einer einzelnen Ressource von Azure Machine Learning Compute (AmlCompute) | 100 Knoten |
| GPU MPI-Prozesse pro Knoten | 1–4 |
| GPU-Worker pro Knoten | 1–4 |
| Lebensdauer von Aufträgen | 21 Tage<sup>1</sup> |
| Lebensdauer von Aufträgen auf einem Knoten niedriger Priorität | 7 Tage<sup>2</sup> |
| Parameterserver pro Knoten | 1 |

<sup>1</sup> Die maximale Lebensdauer bezieht sich auf die Zeitspanne zwischen Start und Ende einer Ausführung. Abgeschlossene Ausführungen bleiben unbegrenzt erhalten. Daten für Ausführungen, die nicht innerhalb der maximalen Lebensdauer abgeschlossen wurden, sind nicht verfügbar.
<sup>2</sup> Aufträge auf einem Knoten niedriger Priorität können jederzeit vorzeitig beendet werden, wenn eine Kapazitätseinschränkung auftritt. Es wird empfohlen, in Ihrem Auftrag Prüfpunkte zu implementieren.

### <a name="azure-machine-learning-pipelines"></a>Azure Machine Learning-Pipelines
[Azure Machine Learning-Pipelines](concept-ml-pipelines.md) weisen die folgenden Grenzwerte auf.

| **Ressource** | **Begrenzung** |
| --- | --- |
| Schritte in einer Pipeline | 30.000 |
| Arbeitsbereiche pro Ressourcengruppe | 800 |

### <a name="container-instances"></a>Containerinstanzen

Weitere Informationen finden Sie unter [Grenzwerte für Container Instances](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#container-instances-limits).

### <a name="storage"></a>Storage
Azure-Speicherkonten haben einen Grenzwert von 250 Speicherkonten pro Region und Abonnement. Dies umfasst sowohl Standard- als auch Storage Premium-Konten.

Um den Grenzwert zu erhöhen, stellen Sie eine Anforderung über den [Azure-Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). Das Azure Storage-Team überprüft Ihren Fall und kann bis zu 250 Speicherkonten für eine Region genehmigen.


## <a name="workspace-level-quota"></a>Kontingent auf Arbeitsbereichsebene

Verwenden Sie Kontingente auf Arbeitsbereichsebene zur Verwaltung der Azure Machine Learning Compute-Zielzuordnung zwischen mehreren [Arbeitsbereichen](concept-workspace.md) in demselben Abonnement.

Standardmäßig nutzen alle Arbeitsbereiche dasselbe Kontingent wie das Kontingent auf Abonnementebene für VM-Familien. Sie können jedoch ein maximales Kontingent für einzelne VM-Familien in Arbeitsbereichen in einem Abonnement festlegen. Auf diese Weise können Sie Kapazitäten freigeben und Probleme aufgrund von Ressourcenkonflikten vermeiden:

1. Navigieren Sie zu einem beliebigen Arbeitsbereich in Ihrem Abonnement.
1. Wählen Sie im linken Bereich **Nutzung + Kontingente** aus.
1. Wählen Sie die Registerkarte **Kontingente konfigurieren** aus, um die Kontingente anzuzeigen.
1. Erweitern Sie eine VM-Familie.
1. Legen Sie eine Kontingentgrenze für jeden unter dieser VM-Familie aufgeführten Arbeitsbereich fest.

Sie können keinen negativen Wert oder einen Wert höher als das Kontingent auf Abonnementebene festlegen.

[![Azure Machine Learning-Kontingent auf Arbeitsbereichsebene](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)

> [!NOTE]
> Sie benötigen Berechtigungen auf Abonnementebene, um das Kontingent auf Arbeitsbereichsebene festzulegen.

## <a name="view-your-usage-and-quotas"></a>Anzeigen von Nutzungsdaten und Kontingenten

Um Ihr Kontingent für verschiedene Azure-Ressourcen wie Virtual Machines, Storage, Netzwerk anzuzeigen, verwenden Sie das Azure-Portal:

1. Wählen Sie im linken Bereich **Alle Dienste** und dann unter der Kategorie „Allgemein“ **Abonnements** aus.

2. Wählen Sie in der Liste der Abonnements das Abonnement aus, dessen Kontingent Sie suchen.

3. Wählen Sie **Nutzung + Kontingente** aus, um Ihre derzeitigen Kontingentgrenzen und Ihre Nutzung anzuzeigen. Verwenden Sie die Filter, um den Anbieter und die Standorte auszuwählen. 

Das Azure Machine Learning Compute-Kontingent für Ihr Abonnement wird getrennt von anderen Azure-Kontingenten verwaltet. 

1. Navigieren Sie im Azure-Portal zu Ihrem **Azure Machine Learning** -Arbeitsbereich.

2. Wählen Sie im linken Bereich unter dem Abschnitt **Support + Problembehandlung** die Option **Nutzung + Kontingente** aus, um Ihre aktuellen Kontingentgrenzen und die Nutzung anzuzeigen.

3. Wählen Sie ein Abonnement aus, um die Kontingentgrenzen anzuzeigen. Denken Sie daran, nach der gewünschten Region zu filtern.

4. Sie können zwischen einer Ansicht auf Abonnementebene und einer Ansicht auf Arbeitsbereichsebene umschalten:

## <a name="request-quota-increases"></a>Anfordern der Kontingenterhöhung

Wenn Sie einen Grenzwert oder ein Kontingent über den Standardgrenzwert anheben möchten, können Sie eine [gebührenfreie Onlinekundensupport-Anforderung öffnen](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/).

Die Grenzwerte können nicht über den maximalen Grenzwert in den obigen Tabellen angehoben werden. Wenn es keinen maximalen Grenzwert gibt, können Sie den Grenzwert für die Ressource nicht anpassen.

Wenn Sie eine Kontingenterhöhung anfordern, wählen Sie den Dienst aus, für den Sie das Kontingent erhöhen möchten. Beispiel: Azure Machine Learning, Container Instances, Storage, usw. Für Azure Machine Learning Compute können Sie die Schaltfläche **Kontingent anfordern** auswählen, während Sie das Kontingent entsprechend den obigen Schritten anzeigen.

> [!NOTE]
> Bei [Abonnements mit einer kostenlosen Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) sind Einschränkungs- oder Kontingenterhöhungen nicht möglich. Wenn Sie über ein [Abonnement mit einer kostenlosen Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) verfügen, können Sie ein Upgrade auf ein Abonnement mit [nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) durchführen. Weitere Informationen finden Sie unter [Upgrade einer kostenlosen Azure-Testversion auf nutzungsbasierte Bezahlung](../billing/billing-upgrade-azure-subscription.md) und [FAQ zum kostenlosen Azure-Konto](https://azure.microsoft.com/free/free-account-faq).

## <a name="private-endpoint-and-private-dns-quota-increases"></a>Erhöhung des Kontingents für private Endpunkte und privates DNS

Es gibt Einschränkungen hinsichtlich der Anzahl der privaten Endpunkte und privaten DNS-Zonen, die in einem Abonnement erstellt werden können.

Obwohl Azure Machine Learning Ressourcen in Ihrem (Kunden-) Abonnement erstellt, gibt es einige Szenarien, bei denen Ressourcen in einem Microsoft-eigenen Abonnement erstellt werden.

 In den folgenden Szenarien müssen Sie möglicherweise eine Kontingentzuteilung im Microsoft-eigenen Abonnement anfordern:

* __Arbeitsbereich mit aktiviertem Private Link mit einem kundenseitig verwalteten Schlüssel (CMK)__
* __Azure Container Registry für den Arbeitsbereich hinter Ihrem virtuellen Netzwerk__
* __Anfügen eines Azure Kubernetes Service-Clusters mit aktiviertem Private Link an Ihren Arbeitsbereich__

Verwenden Sie die folgenden Schritte, um eine Zuteilung für diese Szenarien anzufordern:

1. [Erstellen Sie eine Azure-Supportanfrage](/azure/azure-portal/supportability/how-to-create-azure-support-request#create-a-support-request), und wählen Sie im Abschnitt __Grundlagen__ die folgenden Optionen aus:

    | Feld | Auswahl |
    | ----- | ----- |
    | Typ des Problems | Technisch |
    | Dienst | Eigene Dienste. Wählen Sie in der Dropdownliste __Machine Learning__ aus. |
    | Problemtyp | Konfiguration und Sicherheit von Arbeitsbereichen |
    | Problemuntertyp | Zuteilungsanforderung für private Endpunkte und private DNS-Zonen |

2. Verwenden Sie im Abschnitt __Details__ das Feld __Beschreibung__ , um die zu verwendende Azure-Region und das Szenario anzugeben, dessen Verwendung Sie planen. Wenn Sie Kontingenterhöhungen für mehrere Abonnements anfordern müssen, geben Sie in diesem Feld auch die Abonnement-IDs an.

3. Wählen Sie __Erstellen__ aus, um die Anforderung zu erstellen.

:::image type="content" source="media/how-to-manage-quotas/quota-increase-private-endpoint.png" alt-text="Screenshot: Anfordern einer Kontingenterhöhung für private Endpunkte und privates DNS":::

## <a name="next-steps"></a>Nächste Schritte

+ [Planen und Verwalten von Kosten für Azure Machine Learning](concept-plan-manage-cost.md)

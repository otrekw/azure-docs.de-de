---
title: Planen der Kostenverwaltung für Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Kosten für Azure Cognitive Services mithilfe der Kostenanalyse im Azure-Portal planen und verwalten.
author: erhopf
ms.author: erhopf
ms.custom: subject-cost-optimization
ms.service: cognitive-services
ms.topic: how-to
ms.date: 12/15/2020
ms.openlocfilehash: 48e111ac8b2b4fd3c1e2fee568e20699896dfff3
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2020
ms.locfileid: "97705615"
---
# <a name="plan-and-manage-costs-for-azure-cognitive-services"></a>Planen und Verwalten von Kosten für Azure Cognitive Services

In diesem Artikel erfahren Sie, wie Sie die Kosten für Azure Cognitive Services planen und verwalten. Verwenden Sie zunächst den Azure-Preisrechner, um die Kosten für Cognitive Services zu planen, bevor Sie Ressourcen für den Dienst hinzufügen, um die voraussichtlichen Kosten zu ermitteln. Überprüfen Sie dann beim Hinzufügen von Azure-Ressourcen die voraussichtlichen Kosten. Nachdem Sie mit der Nutzung von Cognitive Services-Ressourcen wie Speech, maschinelles Sehen, LUIS, Textanalyse, Übersetzer und Ähnlichem begonnen haben, können Sie Cost Management-Features verwenden, um Budgets festzulegen und die Kosten zu überwachen. Sie können auch vorhergesagte Kosten überprüfen und Ausgabentrends ermitteln, um die Bereiche zu identifizieren, in denen ggf. Maßnahmen erforderlich sind. Kosten für Cognitive Services stellen nur einen Teil der monatlichen Kosten Ihrer Azure-Rechnung dar. Zwar wird in diesem Artikel das Planen und Verwalten der Kosten für Cognitive Services erläutert, doch werden Ihnen alle Azure-Dienste und -Ressourcen in Rechnung gestellt, die für Ihr Azure-Abonnement verwendet werden, einschließlich der Dienste von Drittanbietern.

## <a name="prerequisites"></a>Voraussetzungen

Die Kostenanalyse in Cost Management unterstützt die meisten Azure-Kontotypen, jedoch nicht alle. Die vollständige Liste der unterstützten Kontotypen finden Sie unter [Grundlegendes zu Cost Management-Daten](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn). Um Kostendaten anzeigen zu können, müssen Sie mindestens über Lesezugriff auf ein Azure-Konto verfügen. Informationen zum Zuweisen des Zugriffs auf Azure Cost Management-Daten finden Sie unter [Zuweisen des Zugriffs auf Cost Management-Daten](https://docs.microsoft.com/azure/cost-management/assign-access-acm-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

<!--Note for Azure service writer: If you have other prerequisites for your service, insert them here -->

## <a name="estimate-costs-before-using-cognitive-services"></a>Schätzen der Kosten vor der Verwendung von Cognitive Services

Verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/), um die Kosten vor dem Hinzufügen von Cognitive Services zu schätzen.

:::image type="content" source="media/cognitive-services-pricing-calculator.png" alt-text="Azure-Preisrechner für Cognitive Services" border="true":::

Wenn Sie Ihrem Arbeitsbereich neue Ressourcen hinzufügen, kehren Sie zu diesem Preisrechner zurück, und fügen Sie hier die gleiche Ressource hinzu, um Ihre Kostenschätzung zu aktualisieren.

Weitere Informationen finden Sie unter [Azure Cognitive Services – Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

## <a name="understand-the-full-billing-model-for-cognitive-services"></a>Grundlegendes zum Gesamtabrechnungsmodell für Cognitive Services

Cognitive Services wird in einer Azure-Infrastruktur ausgeführt, für die [Kosten anfallen](https://azure.microsoft.com/pricing/details/cognitive-services/), wenn Sie die neue Ressource bereitstellen. Es ist wichtig zu verstehen, dass die zusätzliche Infrastruktur Kosten verursachen kann. Sie müssen diese Kosten verwalten, wenn Sie Änderungen an bereitgestellten Ressourcen vornehmen. 

### <a name="costs-that-typically-accrue-with-cognitive-services"></a>Mit Cognitive Services üblicherweise anfallende Kosten

Nach der Bereitstellung einer Azure-Ressource werden die Kosten in der Regel durch Ihren Tarif und durch die API-Aufrufe bestimmt, die Sie an Ihren Endpunkt richten. Wenn der von Ihnen verwendete Dienst über eine Mindestabnahme verfügt, fällt bei Überschreitung der zugewiesenen Aufrufe in Ihrem Tarif ggf. eine entsprechende Zusatzgebühr an.

Bei Verwendung der folgenden Dienste können zusätzliche Kosten anfallen:

#### <a name="qna-maker"></a>QnA Maker

Wenn Sie Ressourcen für QnA Maker erstellen, werden unter Umständen auch Ressourcen für andere Azure-Dienste erstellt. Dazu gehören:

- [Azure App Service (für die Runtime)](https://azure.microsoft.com/pricing/details/app-service/)
- [Azure Cognitive Search (für die Daten)](https://azure.microsoft.com/pricing/details/search/)
 
### <a name="costs-that-might-accrue-after-resource-deletion"></a>Nach dem Löschen von Ressourcen möglicherweise anfallende Kosten

#### <a name="qna-maker"></a>QnA Maker

Nach dem Löschen von QnA Maker-Ressourcen sind die folgenden Ressourcen möglicherweise weiterhin vorhanden. Hierfür fallen weiter Kosten an, bis Sie sie löschen.

- [Azure App Service (für die Runtime)](https://azure.microsoft.com/pricing/details/app-service/)
- [Azure Cognitive Search (für die Daten)](https://azure.microsoft.com/pricing/details/search/)

### <a name="using-monetary-credit-with-cognitive-services"></a>Verwenden von Guthaben mit Cognitive Services

Sie können Cognitive Services-Gebühren mit Ihrem EA-Mindestverbrauchsguthaben bezahlen. Allerdings können Sie mit dem EA-Mindestverbrauchsguthaben keine Gebühren für Produkte und Dienste von Drittanbietern, einschließlich derjenigen aus Azure Marketplace, bezahlen.

## <a name="create-budgets"></a>Erstellen von Budgets

Sie können [Budgets](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) erstellen, um Kosten zu verwalten, und [Warnungen](https://docs.microsoft.com/azure/cost-management/cost-mgt-alerts-monitor-usage-spending?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) erstellen, die die Beteiligten automatisch über Ausgabenanomalien und Überschreitungsrisiken informieren. Warnungen basieren auf Ausgaben im Vergleich zum Budget und zu Kostenschwellenwerten. Budgets und Warnungen werden für Azure-Abonnements und -Ressourcengruppen erstellt und sind daher im Rahmen einer umfassenden Strategie zur Kostenüberwachung hilfreich. 

Sie können ein Budget mit Filtern für bestimmte Ressourcen oder Dienste in Azure erstellen, wenn Sie eine höhere Granularität in Ihrer Überwachung wünschen. Mit Filtern stellen Sie sicher, dass Sie nicht versehentlich neue Ressourcen erstellen, die Ihnen zusätzliche Kosten bereiten. Weitere Informationen zu den Filteroptionen beim Erstellen eines Budgets finden Sie unter [Gruppen- und Filteroptionen](https://docs.microsoft.com/azure/cost-management-billing/costs/group-filter?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## <a name="export-cost-data"></a>Exportieren von Kostendaten

Sie können [Ihre Kostendaten](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) auch in ein Speicherkonto exportieren. Dies ist hilfreich, wenn Sie oder andere Personen zusätzliche Datenanalysen für Kosten ausführen müssen. Beispielsweise können Finanzteams die Daten mithilfe von Excel oder Power BI analysieren. Sie können Ihre Kosten täglich, wöchentlich oder monatlich exportieren und einen benutzerdefinierten Datumsbereich festlegen. Exportieren von Kostendaten ist die empfohlene Abrufmethode für Kostendatasets.

<!--
## Other ways to manage and reduce costs for Cognitive Services

Work with Dean to complete this section in 2021.

-->

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie Ihre [Cloudinvestitionen mit Azure Cost Management optimieren](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Erfahren Sie mehr über die Verwaltung von Kosten mit der [Kostenanalyse](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Erfahren Sie, wie Sie [unerwartete Kosten vermeiden](https://docs.microsoft.com/azure/cost-management-billing/manage/getting-started?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Nehmen Sie an dem angeleiteten Kurs [Cost Management](https://docs.microsoft.com/learn/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) teil.

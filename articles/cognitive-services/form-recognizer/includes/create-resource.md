---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: cedd302453e75c037f17cc47d8e00a06d3f3a8cd
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996793"
---
Navigieren Sie zum Azure-Portal, und <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer" title="Erstellen einer neuen Formularerkennungsressource" target="_blank">erstellen Sie eine neue Formularerkennungsressource<span class="docon docon-navigate-external x-hidden-focus"></span></a>. Geben Sie im Bereich **Erstellen** die folgenden Informationen an:

|    |    |
|--|--|
| **Name** | Einen aussagekräftigen Namen für Ihre Ressource. Es wird empfohlen, einen aussagekräftigen Namen auszuwählen, z. B. *MyNameFormRecognizer*. |
| **Abonnement** | Wählen Sie das Azure-Abonnement aus, dem Zugriff gewährt wurde. |
| **Location** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
| **Preisstufe** | Die Kosten für Ihre Ressource hängen vom ausgewählten Tarif und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Ressourcengruppe** | Die [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group), die Ihre Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |

> [!NOTE]
> Wenn Sie im Azure-Portal eine Cognitive Service-Ressource erstellen, können Sie normalerweise einen Abonnementschlüssel für mehrere Dienste (der für mehrere Cognitive Services verwendet wird) oder einen Abonnementschlüssel für einen einzelnen Dienst (der nur für den angegebenen Cognitive Service verwendet wird) erstellen. Da die Formularerkennung derzeit als Vorschauversion verfügbar ist, ist sie jedoch nicht im Abonnement für mehrere Dienste enthalten. Sie können daher nur dann ein Abonnement für einen einzelnen Dienst erstellen, wenn Sie den Link in der Willkommens-E-Mail verwenden.

Nach Abschluss der Bereitstellung Ihrer Formularerkennungsressource suchen Sie sie im Portal in der Liste **Alle Ressourcen** und wählen sie aus. Ihr Schlüssel und Endpunkt befinden sich auf der entsprechenden Seite der Ressource unter „Ressourcenverwaltung“. Speichern Sie diese beiden Elemente an einem temporären Speicherort, bevor Sie fortfahren.

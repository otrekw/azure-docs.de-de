---
author: laujan
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/25/2021
ms.author: lajanuar
ms.openlocfilehash: f1024edf7d05d47ba14478a1561c29f7859a245c
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122326877"
---
Navigieren Sie zum Azure-Portal, und <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer" title="Erstellen einer neuen Formularerkennungsressource" target="_blank">erstellen Sie eine neue Formularerkennungsressource</a>. Geben Sie im Bereich **Erstellen** die folgenden Informationen an:

| Projektdetails   | BESCHREIBUNG   |
|--|--|
| **Abonnement** | Wählen Sie das Azure-Abonnement aus, dem Zugriff gewährt wurde. |
| **Ressourcengruppe** | Die [Azure-Ressourcengruppe](/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group), die Ihre Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |
| **Region** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
| **Name** | Einen aussagekräftigen Namen für Ihre Ressource. Es wird empfohlen, einen aussagekräftigen Namen auszuwählen, z. B. *MyNameFormRecognizer*. |
| **Preisstufe** | Die Kosten für Ihre Ressource hängen vom ausgewählten Tarif und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

> [!NOTE]
> Wenn Sie im Azure-Portal eine Cognitive Service-Ressource erstellen, können Sie einen Abonnementschlüssel für mehrere Dienste (der für mehrere Cognitive Services-Dienste verwendet wird) oder einen Abonnementschlüssel für einen einzelnen Dienst (der nur für den angegebenen Cognitive Services-Dienst verwendet wird) erstellen. Derzeit ist die Formularerkennung nicht im Abonnement für mehrere Dienste enthalten.

## <a name="retrieve-the-key-and-endpoint"></a>Abrufen des Schlüssels und des Endpunkts

Nach Abschluss der Bereitstellung Ihrer Formularerkennungsressource suchen Sie sie im Portal in der Liste **Alle Ressourcen** und wählen sie aus. **Schlüssel und Endpunkt befinden** sich auf der entsprechenden Seite der Ressource unter **Ressourcenverwaltung**. Speichern Sie diese beiden Elemente an einem temporären Speicherort, bevor Sie fortfahren.


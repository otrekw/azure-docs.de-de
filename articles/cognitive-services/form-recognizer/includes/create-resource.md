---
author: laujan
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 03/15/2021
ms.author: lajanuar
ms.openlocfilehash: 734b548953c4ede5e7d09feb0042509ae5528e82
ms.sourcegitcommit: 2f322df43fb3854d07a69bcdf56c6b1f7e6f3333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108026110"
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

Nach Abschluss der Bereitstellung Ihrer Formularerkennungsressource suchen Sie sie im Portal in der Liste **Alle Ressourcen** und wählen sie aus. Ihr Schlüssel und Endpunkt befinden sich auf der entsprechenden Seite der Ressource unter „Ressourcenverwaltung“. Speichern Sie diese beiden Elemente an einem temporären Speicherort, bevor Sie fortfahren.

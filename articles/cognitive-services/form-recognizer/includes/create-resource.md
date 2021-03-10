---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: edcbeb15b58d59bf080a1acf1d54f1b60e109c8a
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102425736"
---
Navigieren Sie zum Azure-Portal, und <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer" title="Erstellen einer neuen Formularerkennungsressource" target="_blank">erstellen Sie eine neue Formularerkennungsressource</a>. Geben Sie im Bereich **Erstellen** die folgenden Informationen an:

|    |    |
|--|--|
| **Name** | Einen aussagekräftigen Namen für Ihre Ressource. Es wird empfohlen, einen aussagekräftigen Namen auszuwählen, z. B. *MyNameFormRecognizer*. |
| **Abonnement** | Wählen Sie das Azure-Abonnement aus, dem Zugriff gewährt wurde. |
| **Location** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
| **Preisstufe** | Die Kosten für Ihre Ressource hängen vom ausgewählten Tarif und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Ressourcengruppe** | Die [Azure-Ressourcengruppe](/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group), die Ihre Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |

> [!NOTE]
> Wenn Sie im Azure-Portal eine Cognitive Service-Ressource erstellen, können Sie normalerweise einen Abonnementschlüssel für mehrere Dienste (der für mehrere Cognitive Services verwendet wird) oder einen Abonnementschlüssel für einen einzelnen Dienst (der nur für den angegebenen Cognitive Service verwendet wird) erstellen. Derzeit ist die Formularerkennung jedoch nicht im Abonnement für mehrere Dienste enthalten.

Nach Abschluss der Bereitstellung Ihrer Formularerkennungsressource suchen Sie sie im Portal in der Liste **Alle Ressourcen** und wählen sie aus. Ihr Schlüssel und Endpunkt befinden sich auf der entsprechenden Seite der Ressource unter „Ressourcenverwaltung“. Speichern Sie diese beiden Elemente an einem temporären Speicherort, bevor Sie fortfahren.
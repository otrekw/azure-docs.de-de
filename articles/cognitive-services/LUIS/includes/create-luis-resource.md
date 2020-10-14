---
title: Erstellen einer LUIS-Ressource
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/07/2020
ms.author: aahi
ms.openlocfilehash: ebfe7e80577b163218a7bc501fa4e3e9b206fd62
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "80879201"
---
<a name="create-luis-resources"></a>

## <a name="create-luis-resources-in-azure-portal"></a>Erstellen von LUIS-Ressourcen im Azure-Portal

1. Unter [diesem Link](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) können Sie mit der Erstellung von LUIS-Ressourcen im Azure-Portal beginnen.

1. Geben Sie alle erforderlichen Einstellungen ein:

    |Name|Zweck|
    |--|--|
    |Abonnementname| Das Abonnement, unter dem die Ressource abgerechnet wird.|
    |Resource group| Ein benutzerdefinierter Ressourcengruppenname, den Sie auswählen oder erstellen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren.|
    |Name| Ein von Ihnen gewählter benutzerdefinierter Name, der als benutzerdefinierte Unterdomäne für Ihre Erstellungs- und Vorhersageendpunktabfragen verwendet wird|
    |Authoring location (Erstellungsstandort)|Die Region, die Ihrem Modell zugeordnet ist.|
    |Authoring pricing tier (Erstellungstarif)|Der Tarif bestimmt die maximale Anzahl von Transaktionen pro Sekunde und Monat.|
    |Runtime location (Laufzeitstandort)|Die Region, die Ihrer veröffentlichten Vorhersageendpunkt-Laufzeitumgebung zugeordnet ist.|
    |Runtime pricing tier (Laufzeittarif)|Der Tarif bestimmt die maximale Anzahl von Transaktionen pro Sekunde und Monat.|

    > [!div class="mx-imgBorder"]
    > [![Erstellen der Language Understanding-Ressource (LUIS-Ressource)](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png)](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png#lightbox)

1. Klicken Sie auf **Überprüfen + erstellen**, und warten Sie, bis die Ressource erstellt wird.
1. Wählen Sie nach der Erstellung der beiden Ressourcen im Azure-Portal die neue Erstellungsressource und anschließend **Schnellstarts** aus, um die **Endpunkt-URL** und den **Schlüssel** für die programmgesteuerte Erstellung zu erhalten.

> [!TIP]
> Um die Ressourcen verwenden zu können, müssen Sie sie im LUIS-Portal [zuweisen](../luis-how-to-azure-subscription.md#assign-an-authoring-resource-in-the-luis-portal-for-all-apps).
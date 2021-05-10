---
title: include file
description: include file
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 04/23/2021
ms.topic: include
ms.openlocfilehash: 727178c2851e43c787c2b1fcf1e7bb349b3ea4f5
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107948345"
---
## <a name="sign-in-to-luis-portal"></a>Anmelden beim LUIS-Portal

[!INCLUDE [Note about portal deprecation](luis-portal-note.md)]

Als neuer LUIS-Benutzer müssen Sie wie folgt vorgehen:

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, wählen Sie Ihr Land bzw. Ihre Region aus, und stimmen Sie den Nutzungsbedingungen zu. Wenn stattdessen **Meine Apps** angezeigt wird, ist bereits eine LUIS-Ressource vorhanden, und Sie können mit dem Erstellen einer App fortfahren. Verwenden Sie andernfalls für den Einstieg eine Azure-Ressource. Dies ermöglicht es Ihnen, Ihr LUIS-Konto mit einer neuen oder vorhandenen Azure-Erstellungsressource zu verknüpfen. <!---This is equivalent to signing up already migrated. You won't need to go through the [migration process](../luis-migration-authoring.md#what-is-migration) later on.-->
<!---
    * Using a trial key. This allows you to sign in to LUIS with a trial resource that you don't need to set up. If you choose this option, you will eventually be required to [migrate your account](../luis-migration-authoring.md#migration-steps) and link your applications to an authoring resource.
-->

2. Suchen Sie im angezeigten Fenster zum Auswählen einer Erstellungsressource nach Ihrem Azure-Abonnement und der LUIS-Erstellungsressource. Wenn keine Ressource vorhanden ist, können Sie eine neue erstellen.

    <!---:::image type="content" source="../media/luis-how-to-azure-subscription/choose-authoring-resource.png" alt-text="Choose a type of Language Understanding authoring resource.":::
    -->
    ![Typ der Language Understanding-Erstellungsressource auswählen](../media/luis-how-to-azure-subscription/choose-authoring-resource.png)
    
    Geben Sie beim Erstellen einer neuen Erstellungsressource die folgenden Informationen an:
    * **Mandantenname**: Der Mandant, dem Ihr Azure-Abonnement zugeordnet ist.
    * **Azure-Abonnementname**: Das Abonnement, unter dem die Ressource abgerechnet wird.
    * **Name der Azure-Ressourcengruppe**: Ein benutzerdefinierter Ressourcengruppenname, den Sie auswählen oder erstellen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren.
    * **Azure-Ressourcenname**: Ein von Ihnen gewählter benutzerdefinierter Name, der als Teil der URL für Ihre Abfragen für Erstellungs- und Vorhersageendpunkte verwendet wird.
    * **Tarif**: Der Tarif bestimmt die maximale Anzahl von Transaktionen pro Sekunde und Monat.



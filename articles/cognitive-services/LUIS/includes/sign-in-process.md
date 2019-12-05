---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 5e7feac35ddd607d36cb0ddd3bae10cf1b9bc7a7
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74278598"
---
## <a name="sign-in-to-luis-portal"></a>Anmelden beim LUIS-Portal

Als neuer LUIS-Benutzer müssen Sie wie folgt vorgehen:

1. Melden Sie sich beim [LUIS-Portal (Vorschau)](https://preview.luis.ai) an, wählen Sie Ihr Land aus, und stimmen Sie den Nutzungsbedingungen zu. Wenn stattdessen **Meine Apps** angezeigt wird, ist bereits eine LUIS-Ressource vorhanden, und Sie können mit dem Erstellen einer App fortfahren.

1. Wählen Sie **Azure-Ressource erstellen** und anschließend **Create an authoring resource to migrate your apps to** (Erstellen einer Erstellungsressource für die Migration Ihrer Apps) aus.

    ![Auswählen des Typs für eine LUIS-Erstellungsressource](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Füllen Sie die Details für Ihre Ressource aus.

    ![Erstellen einer Erstellungsressource](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    Geben Sie beim **Erstellen einer neuen Erstellungsressource** die folgenden Informationen an: 

    * **Ressourcenname**: ein von Ihnen gewählter benutzerdefinierter Name, der als Teil der URL für Ihre Abfragen für Erstellungs- und Vorhersageendpunkte verwendet wird
    * **Mandant**: der Mandant, dem Ihr Azure-Abonnement zugeordnet ist 
    * **Abonnementname**: das Abonnement, unter dem die Ressource abgerechnet wird.
    * **Ressourcengruppe**: ein benutzerdefinierter Ressourcengruppenname, den Sie auswählen oder erstellen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren. 
    * **Standort**: Die Auswahl des Standorts hängt von der Auswahl der **Ressourcengruppe** ab.
    * **Tarif**: Der Tarif bestimmt die maximale Transaktion pro Sekunde und Monat.

1. Eine Zusammenfassung der zu erstellenden Ressource wird angezeigt. Klicken Sie auf **Weiter**.

    ![Erstellungsressource erstellen](../media/sign-in/sign-in-confirm-key-selection.png)

1. Bestätigen Sie durch Auswahl von **Fortfahren**. 

    ![Erstellungsressource erstellen](../media/sign-in/sign-in-confirm-continue.png)
---
title: include file
description: include file
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 10/30/2020
ms.topic: include
ms.openlocfilehash: e592c11062e81d48014a90895a0e42b460d4b77c
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93128170"
---
## <a name="sign-in-to-luis-portal"></a>Anmelden beim LUIS-Portal

Als neuer LUIS-Benutzer müssen Sie wie folgt vorgehen:

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, wählen Sie Ihr Land bzw. Ihre Region aus, und stimmen Sie den Nutzungsbedingungen zu. Wenn stattdessen **Meine Apps** angezeigt wird, ist bereits eine LUIS-Ressource vorhanden, und Sie können mit dem Erstellen einer App fortfahren. Für die Registrierung haben Sie zwei Möglichkeiten:

    * Verwendung einer Azure-Ressource (empfohlen): Ermöglicht es Ihnen, Ihr LUIS-Konto mit einer neuen oder vorhandenen Azure-Erstellungsressource zu verknüpfen. Dies entspricht einer Registrierung mit bereits enthaltener Migration. Sie müssen den [Migrationsprozess](../luis-migration-authoring.md#what-is-migration) später nicht mehr durchlaufen.

    * Verwendung eines Testschlüssels: Hiermit können Sie sich bei LUIS mit einer Testressource anmelden, die Sie nicht einrichten müssen. Wenn Sie diese Option auswählen, müssen Sie später dann [Ihr Konto migrieren](../luis-migration-authoring.md#migration-steps) und Ihre Anwendungen mit einer Erstellungsressource verknüpfen.

1. Suchen Sie im angezeigten Fenster zum Auswählen einer Erstellungsressource nach Ihrem Azure-Abonnement und der LUIS-Erstellungsressource. Wenn keine Ressource vorhanden ist, können Sie eine neue erstellen.

    :::image type="content" source="../media/luis-how-to-azure-subscription/choose-authoring-resource.png" alt-text="Typ der Language Understanding-Erstellungsressource auswählen":::
    
    Geben Sie beim Erstellen einer neuen Erstellungsressource die folgenden Informationen an:
    * **Mandantenname** : Der Mandant, dem Ihr Azure-Abonnement zugeordnet ist.
    * **Azure-Abonnementname** : Das Abonnement, unter dem die Ressource abgerechnet wird.
    * **Name der Azure-Ressourcengruppe** : Ein benutzerdefinierter Ressourcengruppenname, den Sie auswählen oder erstellen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren.
    * **Azure-Ressourcenname** : Ein von Ihnen gewählter benutzerdefinierter Name, der als Teil der URL für Ihre Abfragen für Erstellungs- und Vorhersageendpunkte verwendet wird.
    * **Tarif** : Der Tarif bestimmt die maximale Anzahl von Transaktionen pro Sekunde und Monat.



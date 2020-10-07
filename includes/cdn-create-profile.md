---
title: include file
description: include file
services: cdn
author: SyntaxC4
ms.service: azure-cdn
ms.topic: include
ms.date: 04/30/2020
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: d73a8d743a948cbd94a3af81fe2e77c45c0eeb67
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "82996313"
---
## <a name="create-a-new-cdn-profile"></a>Erstellen eines neuen CDN-Profils

Ein CDN-Profil ist ein Container für CDN-Endpunkte und gibt einen Tarif an.

1. Wählen Sie im Azure-Portal die Option **Ressource erstellen** aus (links oben). Der Bereich **Neu** wird angezeigt.
   
1. Suchen Sie nach **CDN**, und wählen Sie diese Option und dann **Erstellen** aus:
   
    ![Klicken auf die Ressource „CDN“](./media/cdn-create-profile/cdn-new-resource.png)

    Der Bereich **CDN-Profil** wird angezeigt.

1. Geben Sie die folgenden Werte ein:
   
    | Einstellung  | Wert |
    | -------- | ----- |
    | **Name** | Geben Sie *cdn-profile-123* als Profilnamen ein. Dieser Name muss global eindeutig sein. Wenn er bereits verwendet wird, müssen Sie einen anderen Namen eingeben. |
    | **Abonnement** | Wählen Sie in der Dropdownliste ein Azure-Abonnement aus. |
    | **Ressourcengruppe** | Wählen Sie **Neu erstellen** aus, und geben Sie *CDNQuickstart-rg* als Namen für Ihre Ressourcengruppe ein. Wählen Sie alternativ **Vorhandene verwenden** und dann *CDNQuickstart-rg* aus, falls die Gruppe bereits vorhanden ist. | 
    | **Ressourcengruppenstandort** | Wählen Sie in der Dropdownliste einen Standort in Ihrer Nähe aus. |
    | **Preisstufe** | Wählen Sie in der Dropdownliste eine **Akamai Standard**-Option aus. (Die Bereitstellungsdauer für den Akamai-Tarif beträgt ungefähr eine Minute. Für den Microsoft-Tarif werden ca. zehn Minuten benötigt, und für die Verizon-Tarife ca. 90 Minuten.) |
    | **Jetzt neuen CDN-Endpunkt erstellen** | Lassen Sie das Kontrollkästchen deaktiviert. |  
   
    ![Neues CDN-Profil](./media/cdn-create-profile/cdn-new-profile.png)

1. Wählen Sie **Erstellen**, um das Profil zu erstellen.


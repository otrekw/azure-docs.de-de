---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 7c3887367b6365ea3577bbff6cc19bf34d178ee6
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095308"
---
### <a name="create-a-notification-hub"></a>Erstellen eines Notification Hubs 

In diesem Abschnitt erstellen Sie einen Benachrichtigungshub und konfigurieren die Authentifizierung mit **APNS**. Sie können ein P12-Push-Zertifikat oder eine tokenbasierte Authentifizierung verwenden. Wenn Sie einen bereits erstellten Notification Hub verwenden möchten, können Sie mit Schritt 5 fortfahren.

1. Melden Sie sich bei [Azure](https://portal.azure.com) an.

1. Klicken Sie auf **Ressource erstellen**, suchen und wählen Sie **Benachrichtigungshub** aus, und klicken Sie dann auf **Erstellen**.

1. Aktualisieren Sie die folgenden Felder, und klicken Sie auf **Erstellen**:

    **GRUNDLEGENDE INFORMATIONEN**  

    **Abonnement:** Wählen Sie das **Zielabonnement** aus der Dropdown-Liste aus.  
    **Ressourcengruppe:** Erstellen Sie eine neue **Ressourcengruppe** (oder verwenden Sie eine bereits vorhandene).  

    **DETAILS ZUM NAMESPACE**  

    **Benachrichtigungshub-Namespace:** Geben Sie einen global eindeutigen Namen für den Namespace **Benachrichtigungshub** ein.  

    > [!NOTE]
    > Stellen Sie sicher, dass die Option **Neu erstellen** für dieses Feld ausgewählt ist.

    **DETAILS ZUM BENACHRICHTIGUNGSHUB**  

    **Benachrichtigungshub:** Geben Sie einen Namen für den **Benachrichtigungshub** ein.  
    **Standort:** Wählen Sie einen passenden Standort aus der Dropdownliste aus.  
    **Tarif:** Behalten Sie die Standardoption **Free** bei,  

    > [!NOTE]
    > es sei denn, Sie haben die maximale Anzahl von Hubs im Free-Tarif erreicht.

1. Sobald der **Benachrichtigungshub** bereitgestellt wurde, navigieren Sie zu dieser Ressource.
1. Navigieren Sie zu Ihrem neuen **Benachrichtigungshub**.
1. Wählen Sie die Option **Zugriffsrichtlinien** aus der Liste aus (unter **VERWALTEN**).
1. Notieren Sie sich den Wert von **Richtlinienname** zusammen mit den entsprechenden **Verbindungszeichenfolge**-Werten.

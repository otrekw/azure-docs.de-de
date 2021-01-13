---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 07/21/2020
ms.openlocfilehash: 27df83552b450b7657c0595d1d419001df98148c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87040242"
---
## <a name="prerequisites"></a>Voraussetzungen

* Ein [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3)-Konto 

Bevor Sie Ihr OneDrive-Konto in Logik-Apps verwenden können, müssen Sie den Logik-Apps die Berechtigung erteilen, Verbindungen mit Ihrem OneDrive-Konto im Azure-Portal herzustellen.

Befolgen Sie die nachstehenden Schritte, um Ihren Logik-Apps Verbindungen mit Ihrem OneDrive-Konto zu genehmigen:  

1. Melden Sie sich beim Azure-Portal an. 

1. Wählen Sie unter **Azure-Dienste** die Option **Logik-Apps** aus. Wählen Sie dann den Namen Ihrer Logik-App aus der Liste aus.

1. Wählen Sie im Menü Ihrer Logik-App unter **Entwicklungstools** die Option **Logik-App-Designer** aus.

1. Wählen Sie im Logik-App-Designer in der Dropdownliste **Von Microsoft verwaltete APIs anzeigen** aus, und geben Sie dann *OneDrive* in das Suchfeld ein. Wählen Sie den zu verwendenden Trigger oder die gewünschte Aktion aus:

   ![Screenshot von Logic Apps Designer mit einer Liste hinzuzufügender OneDrive-API-Aktionen.](./media/connectors-create-api-onedrive/onedrive-1.png)

2. Wenn Sie bisher noch keine Verbindung mit OneDrive hergestellt haben, befolgen Sie die Aufforderung, sich mit Ihren OneDrive-Anmeldeinformationen anzumelden:  

   ![Screenshot von Logik-App-Designer mit der Anmeldeaufforderung für die OneDrive-API.](./media/connectors-create-api-onedrive/onedrive-2.png)

3. Wählen Sie **Anmelden**, und geben Sie Ihren Benutzernamen und Ihr Kennwort ein. Wählen Sie **Anmelden**. 

   ![Screenshot der Anmeldeseite des Microsoft-Kontos für die OneDrive-API-Autorisierung.](./media/connectors-create-api-onedrive/onedrive-3.png)   

    Mithilfe dieser Anmeldeinformationen wird Ihre Logik-App dazu autorisiert, auf die Daten in Ihrem OneDrive-Konto zuzugreifen. 

4. Wählen Sie **Ja**, um die Logik-App für die Verwendung Ihres OneDrive-Kontos zu autorisieren:  

   ![Screenshot der Microsoft-Kontoautorisierung für Logic Apps mit Anzeige zulässiger Aktionen.](./media/connectors-create-api-onedrive/onedrive-4.png)   
   
5. Die Verbindung wird nun in dem Schritt aufgeführt. Wählen Sie „Speichern“ aus, und fahren Sie mit dem Erstellen Ihrer Logik-App fort. 

   ![Screenshot von Logic Apps Designer mit dem Aktions-Editor mit einer OneDrive-API-Verbindung.](./media/connectors-create-api-onedrive/onedrive-5.png)

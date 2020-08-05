---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 07/21/2020
ms.openlocfilehash: 06a503e800309d8689735417931820d9f16a6e75
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102681"
---
## <a name="prerequisites"></a>Voraussetzungen

* Ein [OneDrive for Business](https://OneDrive.com)-Konto 

Bevor Sie Ihr OneDrive for Business-Konto in Logik-Apps verwenden können, müssen Sie den Logik-Apps die Berechtigung erteilen, Verbindungen mit Ihrem OneDrive for Business-Konto im Azure-Portal herzustellen.

Befolgen Sie die nachstehenden Schritte, um Ihren Logik-Apps Verbindungen mit Ihrem OneDrive for Business-Konto zu genehmigen:  

1. Melden Sie sich beim Azure-Portal an. 

1. Wählen Sie unter **Azure-Dienste** die Option **Logik-Apps** aus. Wählen Sie dann den Namen Ihrer Logik-App aus der Liste aus.

1. Wählen Sie im Menü Ihrer Logik-App unter **Entwicklungstools** die Option **Logik-App-Designer** aus.

1. Wählen Sie im Logik-App-Designer in der Dropdownliste **Von Microsoft verwaltete APIs anzeigen** aus, und geben Sie dann *OneDrive for Business* in das Suchfeld ein. Wählen Sie den zu verwendenden Trigger oder die gewünschte Aktion aus:  

   ![Screenshot von Logik-App-Designer mit einem Wiederholungstrigger bei OneDrive for Business-API-Aktionen](./media/connectors-create-api-onedriveforbusiness/onedriveforbusiness-1.png)

2. Wenn Sie noch keine Verbindungen mit OneDrive for Business hergestellt haben, werden Sie aufgefordert, Ihre OneDrive for Business-Anmeldeinformationen anzugeben. Mit diesen Anmeldeinformationen wird Ihre Logik-App autorisiert, auf die Daten Ihres OneDrive for Business-Kontos zuzugreifen:  

   ![Screenshot von Logik-App-Designer mit der Anmeldeaufforderung für OneDrive for Business](./media/connectors-create-api-onedriveforbusiness/onedriveforbusiness-2.png)

3. Geben Sie Ihren OneDrive for Business-Benutzernamen und das zugehörige Kennwort zum Autorisieren Ihrer Logik-App an:  

   ![Screenshot der OneDrive for Business-Anmeldeseite mit der Anmeldeaufforderung](./media/connectors-create-api-onedriveforbusiness/onedriveforbusiness-3.png)   

4. Die Verbindung wird nun im Schritt aufgeführt. Wählen Sie „Speichern“ aus, und fahren Sie mit dem Erstellen Ihrer Logik-App fort. 

   ![Screenshot von Logik-App-Designer mit dem Trigger bei der aufgelisteten OneDrive for Business -Verbindung](./media/connectors-create-api-onedriveforbusiness/onedriveforbusiness-4.png)   

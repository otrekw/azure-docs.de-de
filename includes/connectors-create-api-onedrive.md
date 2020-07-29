---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: 8b5f72f18ee5bf9c1e4dbaaa87d6705f55b09bc5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86524205"
---
## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3)-Konto 

Bevor Sie Ihr OneDrive-Konto in einer Logik-App verwenden können, autorisieren Sie die Logik-App für Verbindungen mit Ihrem OneDrive-Konto.  Sie können dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen. 

Führen Sie folgende Schritte aus, um Ihre Logik-App für die Verbindung mit Ihrem OneDrive-Konto zu autorisieren:

1. Erstellen Sie eine Logik-App. Wählen Sie im Logik-App-Designer in der Dropdownliste **Von Microsoft verwaltete APIs anzeigen** aus, und geben Sie dann „OneDrive“ in das Suchfeld ein. Wählen Sie einen Trigger oder eine Aktion aus:  
   ![Ein Dialogfeld mit dem Titel „Von Microsoft verwaltete APIs anzeigen“ weist ein Suchfeld auf, das „onedrive“ enthält. Darunter befindet sich eine Liste mit vier Triggern. Der erste in der Liste ist „OneDrive – Wenn eine Datei erstellt wird“ und bereits ausgewählt.](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Wenn Sie bisher noch keine Verbindung mit OneDrive hergestellt haben, werden Sie aufgefordert, sich mit Ihren OneDrive-Anmeldeinformationen anzumelden:  
   ![Ein Dialogfeld mit dem Namen „OneDrive – Wenn eine Datei erstellt wird“ enthält die Schaltfläche „Anmelden“.](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Wählen Sie **Anmelden**, und geben Sie Ihren Benutzernamen und Ihr Kennwort ein. Wählen Sie **Anmelden**.  
   ![Ein Dialogfeld mit dem Titel „Anmelden“ weist Sie an, Ihr Microsoft-Konto zu verwenden. Es verfügt über zwei Textfelder mit den Bezeichnungen „E-Mail oder Telefon“ und „Kennwort“. Es enthält auch ein Kontrollkästchen mit der Bezeichnung „angemeldet bleiben“ und eine Schaltfläche mit der Bezeichnung „Anmelden“.](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Mithilfe dieser Anmeldeinformationen wird Ihre Logik-App dazu autorisiert, eine Verbindung mit Ihrem OneDrive-Konto herzustellen und auf die darin gespeicherten Daten zuzugreifen. 
4. Wählen Sie **Ja**, um die Logik-App für die Verwendung Ihres OneDrive-Kontos zu autorisieren:  
   ![Sie werden in einem Dialogfeld mit dem Titel „Dieser App den Zugriff auf Ihre Infos erlauben?“ aufgefordert, die Berechtigung für die folgenden vier Schritte zu erteilen: 1) „Automatisch anmelden“, 2) „Anzeigen Ihrer E-Mail-Adressen“, 3) „Auf Ihre Informationen immer zugreifen“ und 4) „Auf OneDrive-Dateien zugreifen“. Über die Schaltfläche „Ja“ können Sie die Berechtigung erteilen, und mit der Schaltfläche „Nein“ verweigern Sie sie. Es gibt einen Link zum Ändern dieser Anwendungsberechtigungen.](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Beachten Sie, dass die Verbindung erstellt wurde. Fahren Sie nun mit den weiteren Schritten in Ihrer Logik-App fort:  
   ![Ein Dialogfeld mit dem Titel „Wenn eine Datei erstellt wird“ verfügt über ein Textfeld namens „FOLDER“ mit einer zugeordneten Schaltfläche zum Durchsuchen.](./media/connectors-create-api-onedrive/onedrive-5.png)


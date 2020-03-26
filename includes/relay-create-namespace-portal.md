---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 11/09/2018
ms.author: clemensv
ms.openlocfilehash: 500db9f6d299ea81b1f1b1be864df5d91ba1eae7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "76020886"
---
1. Melden Sie sich beim [Azure-Portal][Azure portal] an.
1. Wählen Sie **Ressource erstellen**. Wählen Sie dann **Integration** > **Relay** aus. Wenn **Relay** in der Liste nicht angezeigt wird, klicken Sie in der oberen rechten Ecke auf **Alle anzeigen**.
1. Wählen Sie **Erstellen** aus, und geben Sie im Feld **Name** einen Namespacenamen ein. Das Azure-Portal überprüft, ob der Name verfügbar ist.
1. Wählen Sie ein Azure-Abonnement aus, in dem der Namespace erstellt werden soll.
1. Wählen Sie unter [Ressourcengruppe](../articles/azure-resource-manager/management/manage-resource-groups-portal.md) eine vorhandene Ressourcengruppe aus, in der der Namespace platziert werden soll, oder erstellen Sie eine neue Ressourcengruppe.  
1. Wählen Sie das Land oder die Region aus, in dem bzw. in der Ihr Namespace gehostet werden soll.

    ![Namespace erstellen][create-namespace]

1. Klicken Sie auf **Erstellen**. Ihr Namespace wird im Azure-Portal erstellt und aktiviert. Nach einigen Minuten stellt das System Ressourcen für Ihr Konto bereit.

### <a name="get-management-credentials"></a>Abrufen von Anmeldeinformationen für die Verwaltung

1. Klicken Sie auf **Alle Ressourcen**, und wählen Sie dann den neu erstellten Namespacenamen aus.
1. Klicken Sie auf **Freigegebene Zugriffsrichtlinien**.  
1. Klicken Sie unter **Richtlinien für gemeinsamen Zugriff** auf **RootManageSharedAccessKey**.
1. Klicken Sie unter **SAS-Richtlinie: RootManageSharedAccessKey** neben **Primäre Verbindungszeichenfolge** auf die Schaltfläche **Kopieren**. Dadurch wird die Verbindungszeichenfolge zur späteren Verwendung in die Zwischenablage kopiert. Fügen Sie diesen Wert in den Editor oder an einem anderen temporären Speicherort ein.
1. Wiederholen Sie den vorherigen Schritt, um den Wert von **Primärschlüssel** zu kopieren und zur späteren Verwendung an einem temporären Speicherort einzufügen.  

    ![connection-string][connection-string]

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace-vs2019.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string-vs2019.png
[Azure portal]: https://portal.azure.com

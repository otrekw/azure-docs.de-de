---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: 11b9c553573d9e6188ba634b4cb966d6a9b850b4
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944772"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/dotnet/).

## <a name="create-azure-communication-resource"></a>Erstellen von Azure Communication-Ressourcen

Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com) an, um eine Azure Communication Services-Ressource zu erstellen. Wählen Sie links oben auf der Seite die Option **+ Ressource erstellen** aus. 

:::image type="content" source="../media/create-a-communication-resource/create-resource-plus-sign.png" alt-text="Screenshot mit der hervorgehobenen Schaltfläche zum Erstellen einer Ressource im Azure-Portal":::

Geben Sie **Communication** in **Search the Marketplace** (Marketplace durchsuchen) oder in die Suchleiste oben im Portal ein.

:::image type="content" source="../media/create-a-communication-resource/searchbar-communication-portal.png" alt-text="Screenshot, der eine Suche nach Communication Services in der Suchleiste zeigt":::

Wählen Sie in den Ergebnissen **Communication Services** und anschließend **Hinzufügen** aus.

:::image type="content" source="../media/create-a-communication-resource/add-communication-portal.png" alt-text="Screenshot, der das Azure-Panel mit der hervorgehobenen Schaltfläche „Hinzufügen“ zeigt":::

Sie können Ihre Communication Services-Ressource jetzt konfigurieren. Auf der ersten Seite des Erstellungsprozesses werden Sie aufgefordert, Folgendes anzugeben:

* Das Abonnement
* Die Ressourcengruppe (Sie können eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen)
* Den Namen der Communication Services-Ressource
* Die Geografie, der die Ressource zugeordnet wird

Im nächsten Schritt können Sie der Ressource Tags zuweisen. Tags können verwendet werden, um Azure-Ressourcen zu organisieren. Weitere Informationen zu Tags finden Sie in der [Dokumentation zum Anwenden von Tags auf Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources).

Zuletzt können Sie Ihre Konfiguration überprüfen und die Ressource **erstellen**. Beachten Sie, dass die Bereitstellung einige Minuten dauert.

## <a name="manage-your-communication-services-resource"></a>Verwalten Ihrer Communication Services-Ressource

Wechseln Sie zum Verwalten Ihrer Communication Services-Ressourcen zum [Azure-Portal](https://portal.azure.com), und suchen Sie nach **Azure Communication Services**.

Wählen Sie auf der Seite **Communication Services** den Namen Ihrer Ressource aus.

Die Seite **Übersicht** für Ihre Ressource enthält Optionen für die grundlegende Verwaltung, z. B. durchsuchen, beenden, starten, neu starten und löschen. Weitere Konfigurationsoptionen finden Sie im linken Menü Ihrer Ressourcenseite.
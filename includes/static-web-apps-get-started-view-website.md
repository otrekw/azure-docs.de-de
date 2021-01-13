---
author: craigshoemaker
ms.service: static-web-apps
ms.topic: include
ms.date: 05/13/2020
ms.author: cshoe
ms.openlocfilehash: cb8f8388dd164f4fc17b81b9b39c6b5a058ca3ff
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88752429"
---
## <a name="view-the-website"></a>Anzeigen der Website

Für die Bereitstellung einer statischen App gelten zwei Aspekte. Der erste Aspekt ist die Bereitstellung der zugrunde liegenden Azure-Ressourcen, aus denen Ihre App besteht. Der zweite Aspekt ist ein GitHub Actions-Workflow, mit dem Ihre Anwendung erstellt und veröffentlicht wird.

Bevor Sie zu Ihrer neuen statischen Website navigieren können, muss zuerst die Ausführung des Buildvorgangs für die Bereitstellung abgeschlossen sein.

Im Übersichtsfenster von Azure Static Web Apps werden einige Links angezeigt, die Ihnen als Hilfe bei der Interaktion mit Ihrer Web-App dienen.

:::image type="content" source="../articles/static-web-apps/media/getting-started/overview-window.png" alt-text="Übersichtsfenster":::

1. Wenn Sie auf das Banner _Click here to check the status of your GitHub Actions runs_ (Hier klicken, um den Status Ihrer GitHub Actions-Ausführungen zu überprüfen) klicken, gelangen Sie zu den GitHub Actions-Instanzen, die für Ihr Repository ausgeführt werden. Nachdem Sie sich vergewissert haben, dass der Bereitstellungsauftrag abgeschlossen ist, können Sie über die generierte URL zu Ihrer Website navigieren.

2. Nachdem der GitHub Actions-Workflow abgeschlossen ist, können Sie auf den Link _URL_ klicken, um die Website in einer neuen Registerkarte zu öffnen.

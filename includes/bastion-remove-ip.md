---
author: cherylmc
ms.author: cherylmc
ms.date: 06/29/2021
ms.service: bastion
ms.topic: include
ms.openlocfilehash: f0585ca5bc2a76cb7f969f8a599ee98152b872cf
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113108235"
---
Wenn Sie über Azure Bastion eine Verbindung mit einem virtuellen Computer herstellen, benötigen Sie keine öffentliche IP-Adresse für Ihren virtuellen Computer. Wenn Sie die öffentliche IP-Adresse nicht für etwas anderes verwenden, können Sie ihre Zuordnung zu Ihrem virtuellen Computer wieder aufheben. Führen Sie die folgenden Schritte aus, um die Zuordnung einer öffentlichen IP-Adresse zu einem virtuellen Computer aufzuheben:

1. Navigieren Sie zu Ihrem virtuellen Computer, und wählen Sie **Netzwerk** aus. Wählen Sie die **öffentliche IP-Adresse der NIC** aus, um die Seite mit der öffentlichen IP-Adresse zu öffnen.

   :::image type="content" source="./media/bastion-remove-ip/networking.png" alt-text="Screenshot: Seite „Netzwerk“":::

1. Wählen Sie auf der Seite **Öffentliche IP-Adresse** für den virtuellen Computer die Option **Zuordnung aufheben** aus.

   :::image type="content" source="./media/bastion-remove-ip/disassociate.png" alt-text="Screenshot: Öffentliche IP-Adresse für den virtuellen Computer":::

1. Wählen Sie **Ja** aus, um die Zuordnung der IP-Adresse zur Netzwerkschnittstelle aufzuheben.

   :::image type="content" source="./media/bastion-remove-ip/disassociate-yes.png" alt-text="Screenshot: Zuordnung der öffentlichen IP-Adresse aufheben":::

1. Nachdem Sie die Zuordnung der IP-Adresse aufgehoben haben, können Sie die öffentliche IP-Adressressource löschen. Navigieren Sie zum Löschen der öffentlichen IP-Adressressource zur Ressourcengruppe, und suchen Sie die IP-Adressressource, die Sie löschen möchten. Wählen Sie dann **Löschen** aus, um die Ressource zu löschen.

   :::image type="content" source="./media/bastion-remove-ip/delete-resource.png" alt-text="Screenshot: Löschen der öffentlichen IP-Adressressource":::
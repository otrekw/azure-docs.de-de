---
title: Erstellen einer Azure Digital Twins-Instanz
titleSuffix: Azure Digital Twins
description: Informationen zum Einrichten einer Instanz des Azure Digital Twins-Diensts.
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: fecacbd2c7c6549a1321367157bb179321779ca9
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206521"
---
# <a name="set-up-an-azure-digital-twins-instance"></a>Einrichten einer Azure Digital Twins-Instanz

In diesem Artikel werden Sie durch die grundlegenden Schritte zum Einrichten einer neuen Azure Digital Twins-Instanz geleitet. Dazu gehört das Erstellen der Instanz und das Zuweisen von [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)-Berechtigungen (AAD) an die Instanz für Sie selbst.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="set-up-an-azure-digital-twins-instance"></a>Einrichten einer Azure Digital Twins-Instanz

Als Nächstes erstellen Sie eine neue Azure-Ressourcengruppe zur Verwendung in dieser Anleitung. Anschließend können Sie in dieser Ressourcengruppe **eine neue Azure Digital Twins-Instanz** erstellen. 

Sie müssen auch einen Namen für Ihre Instanz angeben und eine Region für die Bereitstellung wählen. Informationen zu Regionen mit Unterstützung von Azure Digital Twins finden Sie unter [Verfügbare Azure-Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).

>[!NOTE]
> Der Name der neuen Instanz muss innerhalb der Region eindeutig sein (d. h. wenn eine andere Azure Digital Twins-Instanz in dieser Region bereits den von Ihnen gewählten Namen hat, müssen Sie einen anderen Namen wählen).

Erstellen Sie die Ressourcengruppe und die Instanz mit den folgenden Befehlen:

```azurecli
az group create --location <region> --name <name-for-your-resource-group>
az dt create --dt-name <name-for-your-Azure-Digital-Twins-instance> -g <your-resource-group> -l <region>
```

Das Ergebnis dieser Befehle sieht in etwa so aus und gibt Informationen zu den von Ihnen erstellten Ressourcen aus:

:::image type="content" source="media/how-to-set-up-instance/create-instance.png" alt-text="Befehlsfenster mit erfolgreicher Erstellung der Ressourcengruppe und der Azure Digital Twins-Instanz":::

Beachten Sie die Angaben für *hostName*, *name* und *resourceGroup* der Azure Digital Twins-Instanz in der Ausgabe. Dies sind alles Schlüsselwerte, die Sie möglicherweise benötigen, wenn Sie mit Ihrer Azure Digital Twins-Instanz weiterarbeiten, um die Authentifizierung und zugehörigen Azure-Ressourcen einzurichten.

> [!TIP]
> Sie können diese Eigenschaften sowie alle Eigenschaften Ihrer Instanz jederzeit durch Ausführen von `az dt show --dt-name <your-Azure-Digital-Twins-instance>` einsehen.

### <a name="assign-azure-active-directory-permissions"></a>Zuweisen von Azure Active Directory-Berechtigungen

In Azure Digital Twins wird [Azure Active Directory (AAD)](../active-directory/fundamentals/active-directory-whatis.md) für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwendet. Das bedeutet, dass Sie sich zuvor eine Rolle mit diesen Berechtigungen zuweisen müssen, bevor Sie Aufrufe auf Datenebene an Ihre Azure Digital Twins-Instanz richten können.

Um Azure Digital Twins mit einer Clientanwendung zu verwenden, müssen Sie auch sicherstellen, dass diese sich bei Azure Digital Twins authentifizieren kann. Dies erfolgt durch Einrichten einer Azure Active Directory-App-Registrierung (AAD). Informationen dazu finden Sie unter [ Authentifizieren einer Clientanwendung](how-to-authenticate-client.md).

#### <a name="assign-yourself-a-role"></a>Sich selbst eine Rolle zuweisen

Erstellen Sie in der Azure Digital Twins-Instanz eine Rollenzuweisung für sich selbst. Verwenden Sie dazu Ihre E-Mail Adresse, die mit dem AAD-Mandanten Ihres Azure-Abonnements verknüpft ist. 

Dazu müssen Sie in Ihrem Azure-Abonnement als Besitzer klassifiziert sein. Sie können dies überprüfen, indem Sie den Befehl `az role assignment list --assignee <your-Azure-email>` ausführen und in der Ausgabe überprüfen, ob der Wert für *roleDefinitionName* gleich *Owner* ist. Wenn als Wert *Contributor* bzw. ein anderer Wert als *Owner*  angegeben ist, wenden Sie sich an den Abonnementadministrator, der Berechtigungen in Ihrem Abonnement gewähren kann. Er kann Ihre Rolle entweder für das gesamte Abonnement heraufstufen, damit Sie den folgenden Befehl ausführen können, oder ein Besitzer kann den folgenden Befehl in Ihrem Auftrag ausführen, um die Azure Digital Twins-Berechtigungen für Sie einzurichten.

Verwenden Sie den folgenden Befehl (muss von einem Besitzer des Azure-Abonnements ausgeführt werden), um dem Benutzer die Berechtigung „Besitzer“ in Ihrer Azure Digital Twins-Instanz zuzuweisen:

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<your-AAD-email>" --role "Azure Digital Twins Owner (Preview)"
```

Die Ausgabe dieses Befehls enthält Informationen zur erstellten Rollenzuweisung.

> [!TIP]
> Wenn Sie stattdessen den Fehler *400: BadRequest* erhalten, führen Sie den folgenden Befehl aus, um die *Objekt-ID* Ihres Benutzers abzurufen:
> ```azurecli
> az ad user show --id <your-AAD-email> --query objectId
> ```
> Wiederholen Sie dann den Befehl zur Rollenzuweisung unter Verwendung der *Objekt-ID* Ihres Benutzers anstelle Ihrer E-Mail-Adresse.

Sie verfügen nun über eine einsatzbereite Azure Digital Twins-Instanz sowie über Berechtigungen zu ihrer Verwaltung.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie eine Client-App für die Zusammenarbeit mit Ihrer Instanz einrichten und authentifizieren:
* [Gewusst wie: Authentifizieren einer Clientanwendung](how-to-authenticate-client.md)

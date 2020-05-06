---
title: Importieren und Veröffentlichen Ihrer ersten API in Azure API Management
description: Es wird beschrieben, wie Sie eine OpenAPI-Spezifikations-API in Azure API Management importieren und diese im Azure-Portal testen.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 04/20/2020
ms.author: apimpm
ms.openlocfilehash: 0b5fbb49e2f60f101f16988538af86c2caf550eb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82202848"
---
# <a name="import-and-publish-your-first-api"></a>Importieren und Veröffentlichen Ihrer ersten API

In diesem Tutorial wird veranschaulicht, wie Sie eine Back-End-API mit OpenAPI-Spezifikation im JSON-Format in Azure API Management importieren. Microsoft stellt die Back-End-API bereit und hostet sie unter [https://conferenceapi.azurewebsites.net?format=json](https://conferenceapi.azurewebsites.net?format=json) in Azure.

Nachdem Sie die Back-End-API in API Management importiert haben, wird Ihre API Management-API zu einer Fassade für die Back-End-API. Sie können die Fassade an Ihre Anforderungen in API Management anpassen, ohne hierfür die Back-End-API bearbeiten zu müssen. Weitere Informationen finden Sie unter [Transformieren und Schützen Ihrer API](transform-api.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Importieren einer API in API Management
> * Testen der API im Azure-Portal

![Neue API](./media/api-management-import-and-publish/created-api.png)

## <a name="prerequisites"></a>Voraussetzungen

- Machen Sie sich mit der [Azure API Management-Terminologie](api-management-terminology.md) vertraut.
- [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-backend-api"></a><a name="create-api"> </a>Importieren und Veröffentlichen einer Back-End-API

In diesem Abschnitt wird gezeigt, wie Sie eine Back-End-API mit OpenAPI-Spezifikation importieren und veröffentlichen.

1. Wählen Sie im linken Navigationsbereich Ihrer API Management-Instanz im Abschnitt **API Management** die Option **APIs** aus.
1. Wählen Sie die Kachel **OpenAPI** und dann auf dem Popupbildschirm die Option **Vollständig** aus.
1. Verwenden Sie auf dem Bildschirm **Create from OpenAPI specification** (Aus OpenAPI-Spezifikation erstellen) die Werte aus der folgenden Tabelle, um Ihre API zu erstellen.

   Ein roter Stern neben einem Feld des Formulars gibt an, dass das Feld erforderlich ist. Sie können API-Werte während der Erstellung oder später über die Registerkarte **Einstellungen** festlegen.

   ![Erstellen einer API](./media/api-management-import-and-publish/create-api.png)

   |Einstellung|Wert|BESCHREIBUNG|
   |-------|-----|-----------|
   |**OpenAPI-Spezifikation**|*https:\//conferenceapi.azurewebsites.net?format=json*|Der Dienst, der die API implementiert. API Management leitet Anfragen an diese Adresse weiter.|
   |**Anzeigename**|Nachdem Sie die vorherige Dienst-URL eingegeben haben, füllt API Management dieses Feld basierend auf dem JSON-Code aus.|Der Name wird im Entwicklerportal angezeigt.|
   |**Name**|Nachdem Sie die vorherige Dienst-URL eingegeben haben, füllt API Management dieses Feld basierend auf dem JSON-Code aus.|Ein eindeutiger Name für die API.|
   |**Beschreibung**|Nachdem Sie die vorherige Dienst-URL eingegeben haben, füllt API Management dieses Feld basierend auf dem JSON-Code aus.|Eine optionale Beschreibung der API.|
   |**URL-Schema**|**HTTPS**|Gibt an, welche Protokolle für den Zugriff auf die API verwendet werden können.|
   |**API-URL-Suffix**|*conference*|Das Suffix wird an die Basis-URL für den API Management-Dienst angehängt. API Management unterscheidet APIs anhand des Suffix. Daher muss jede API eines bestimmten Herausgebers ein eindeutiges Suffix haben.|
   |**Tags**| |Tags zum Organisieren von APIs für die Suche, Gruppierung oder Filterung.|
   |**Produkte**|**Unbegrenzt**|Zuordnung von einer oder mehreren APIs. Jede API Management-Instanz enthält zwei Beispielprodukte: **Starter** und **Unbegrenzt**. Zur Veröffentlichung einer API muss diese einem Produkt zugeordnet werden (in diesem Beispiel **Unbegrenzt**).<br/>Sie können ein Produkt mit mehreren APIs versehen und sie Entwicklern über das Entwicklerportal zur Verfügung stellen. Geben Sie den Produktnamen ein, oder wählen Sie ihn aus, um diese API einem anderen Produkt hinzuzufügen. Wiederholen Sie diesen Schritt, um die API mehreren Produkten hinzuzufügen. Sie können APIs Produkten auch später über die Seite **Einstellungen** hinzufügen.<br/>Entwickler müssen ein Produkt zunächst abonnieren, um Zugriff auf die API zu erhalten. Wenn sie ein Produkt abonnieren, erhalten sie einen Abonnementschlüssel, der für jede API in diesem Produkt gilt. <br/>Wenn Sie die API Management-Instanz erstellt haben, sind Sie bereits Administrator und haben dadurch alle Produkte der Instanz abonniert.|
   |**Gateways**|**Verwaltet**|API-Gateways, die die API verfügbar machen. Dieses Feld ist nur bei Diensten mit **Developer**- und **Premium**-Tarif verfügbar.<br/>**Verwaltet** gibt an, dass das Gateway in den API Management-Dienst integriert ist und von Microsoft in Azure gehostet wird. Andere Gateways sind [selbstgehostete Gateways](self-hosted-gateway-overview.md) und nur bei Diensten mit Premium- und Developer-Tarif verfügbar. Sie können sie lokal oder in anderen Clouds bereitstellen.<br/>Wenn keine Gateways ausgewählt sind, ist die API nicht verfügbar, und Ihre API-Anforderungen können nicht erfolgreich ausgeführt werden.|
   |**Versionsverwaltung für diese API?**|Auswählen bzw. Aufheben der Auswahl|Weitere Informationen zur Versionsverwaltung finden Sie unter [Veröffentlichen mehrerer Versionen Ihrer API](api-management-get-started-publish-versions.md).|

   > [!NOTE]
   > Wenn Sie die API für API-Consumer veröffentlichen möchten, müssen Sie sie einem Produkt zuordnen.

2. Klicken Sie auf **Erstellen**.

Falls beim Importieren einer API-Definition Probleme auftreten, helfen Ihnen die Informationen in der [Liste mit den bekannten Problemen und Einschränkungen](api-management-api-import-restrictions.md) weiter.

## <a name="test-the-new-api-in-the-azure-portal"></a>Testen der neuen API im Azure-Portal

Sie können API-Vorgänge direkt über das Azure-Portal aufrufen. Dies ist ein einfacher Weg, die Vorgänge anzuzeigen und zu testen.

1. Wählen Sie im linken Navigationsbereich Ihrer API Management-Instanz im Abschnitt **API Management** die Option **APIs** und anschließend **Demo Conference API** aus.
1. Wählen Sie die Registerkarte **Test** und dann **GetSpeakers** aus. Auf der Seite werden **Abfrageparameter** und **Header** angezeigt (falls zutreffend). **Ocp-Apim-Subscription-Key** wird automatisch für den Abonnementschlüssel eingefügt, der dieser API zugeordnet ist.
1. Wählen Sie **Senden** aus.

   ![Testen der API](./media/api-management-import-and-publish/01-import-first-api-01.png)

   Das Back-End antwortet mit **200 OK** und einigen Daten.

## <a name="next-steps"></a><a name="next-steps"> </a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Importieren Ihrer ersten API
> * Testen der API im Azure-Portal

Im nächsten Tutorial erfahren Sie, wie Sie ein Produkt erstellen und veröffentlichen:

> [!div class="nextstepaction"]
> [Erstellen und Veröffentlichen eines Produkts](api-management-howto-add-products.md)

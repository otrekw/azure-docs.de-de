---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 01/15/2020
ms.author: diberry
ms.openlocfilehash: 11abd52681d7c9962af4e5bf0728f97b256223c1
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122752"
---
## <a name="create-a-personalizer-azure-resource"></a>Erstellen einer Azure-Ressource für die Personalisierung

Erstellen Sie auf Ihrem lokalen Computer eine Ressource für die Personalisierung (entweder über das [Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli)). Sie können außerdem:

* Rufen Sie einen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services) ab, mit dem Sie sieben Tage lang kostenlos testen können. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, erstellen Sie zwei [Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_RESOURCE_KEY` für den Ressourcenschlüssel.
* `PERSONALIZER_RESOURCE_ENDPOINT` für den Ressourcenendpunkt.

Die Schlüssel- und Endpunktwerte finden Sie im Azure-Portal auf der Seite **Schnellstart**.

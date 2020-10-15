---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 4c5c0197ab6cecbba4bac4c0bff5ef76de24b6de
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78301000"
---
## <a name="prepare-your-web-app"></a>Vorbereiten Ihrer Web-App

Wenn Sie benutzerdefinierte Sicherheitsbindungen erstellen oder Clientzertifikate für Ihre App Service-App aktivieren möchten, müssen Sie über einen [App-Service-Plan](https://azure.microsoft.com/pricing/details/app-service/) im Tarif **Basic**, **Standard**, **Premium** oder **I** verfügen. Stellen Sie in diesem Schritt sicher, dass sich Ihre Web-App im richtigen Tarif befindet.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie das [Azure-Portal](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Navigieren zu Ihrer Web-App

Suchen Sie nach **App Services**, und wählen Sie diese Option aus.

![Auswählen von „App Services“](./media/app-service-ssl-prepare-app/app-services.png)

Wählen Sie auf der Seite **App Services** den Namen Ihrer Web-App aus.

![Portalnavigation zur Azure-App](./media/app-service-ssl-prepare-app/select-app.png)

Sie befinden sich auf der Verwaltungsseite Ihrer Web-App.  

### <a name="check-the-pricing-tier"></a>Überprüfen des Tarifs

Scrollen Sie im linken Navigationsbereich auf der Seite Ihrer Web-App zum Abschnitt **Einstellungen**, und wählen Sie **Hochskalieren (App Service-Plan)** aus.

![Menü „Zentral hochskalieren“](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Vergewissern Sie sich, dass sich Ihre Web-App nicht im Tarif **F1** oder **D1** befindet. Der aktuelle Tarif Ihrer Web-App wird durch einen dunkelblauen Rahmen hervorgehoben.

![Überprüfen des Tarifs](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

Benutzerdefiniertes SSL wird im Tarif **F1** oder **D1** nicht unterstützt. Wenn Sie Ihren App Service-Plan hochskalieren müssen, führen Sie die Schritte im nächsten Abschnitt aus. Schließen Sie andernfalls die Seite **Hochskalieren**, und überspringen Sie den Abschnitt [Hochskalieren Ihres App Service-Plans](#scale-up-your-app-service-plan).

### <a name="scale-up-your-app-service-plan"></a>Hochskalieren Ihres App Service-Plans

Wählen Sie einen der kostenpflichtigen Tarife aus (**B1**, **B2**, **B3** oder einen beliebigen Tarif aus der Kategorie **Produktion**). Klicken Sie auf **Alle Optionen anzeigen**, um weitere Optionen anzuzeigen.

Klicken Sie auf **Anwenden**.

![Auswählen eines Tarifs](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

Wenn die unten angegebene Benachrichtigung angezeigt wird, ist der Skalierungsvorgang abgeschlossen.

![Benachrichtigung zum Hochskalieren](./media/app-service-ssl-prepare-app/scale-notification.png)


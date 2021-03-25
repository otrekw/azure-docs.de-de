---
title: Erstellen einer Front Door-Ressource mit Umleitung von HTTP zu HTTPS über das Azure-Portal
description: Erfahren Sie, wie Sie mithilfe des Azure-Portals eine Front Door-Ressource mit Datenverkehr erstellen, der von HTTP zu HTTPS umgeleitet wird.
services: front-door
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 09/30/2020
ms.author: duau
ms.openlocfilehash: 19908b3cba63bc76a205097ef8d16e612d58503b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91626641"
---
# <a name="create-a-front-door-with-http-to-https-redirection-using-the-azure-portal"></a>Erstellen einer Front Door-Ressource mit Umleitung von HTTP zu HTTPS über das Azure-Portal

Sie können das Azure-Portal verwenden, um eine Instanz von [Front Door](quickstart-create-front-door.md) mit einem Zertifikat für TLS-Terminierung zu erstellen. Eine Routingregel wird verwendet, um HTTP-Datenverkehr an HTTPS umzuleiten.

## <a name="create-a-front-door-with-an-existing-web-app-resource"></a>Erstellen einer Front Door-Ressource mit einer vorhandenen Web-App-Ressource

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

1. Wählen Sie in der linken oberen Ecke des Azure-Portals **Ressource erstellen** aus.

1. Suchen Sie mithilfe der Suchleiste nach **Front Door**. Wählen Sie **Erstellen** aus, wenn Sie den Ressourcentyp gefunden haben.

1. Wählen Sie ein *Abonnement* aus, und verwenden Sie dann eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Wählen Sie **Weiter** aus, um auf die Registerkarte „Konfiguration“ zu gelangen.

    > [!NOTE]
    > Der Speicherort, der in der Benutzeroberfläche abgefragt wird, bezieht sich nur auf die Ressourcengruppe. Die Front Door-Konfiguration wird an allen [POP-Standorten von Azure Front Door](front-door-faq.md#what-are-the-pop-locations-for-azure-front-door) bereitgestellt.

    :::image type="content" source="./media/front-door-url-redirect/front-door-create-basics.png" alt-text="Konfigurieren der Grundlagen für eine neue Front Door-Ressource":::

1. Die Konfiguration für Front Door erfolgt in drei Schritten: Hinzufügen eines Front-End-Standardhosts, Hinzufügen von Back-Ends in einem Back-End-Pool und anschließendes Erstellen von Routingregeln, um das Routingverhalten für den Front-End-Host zuzuordnen. Wählen Sie auf den _Front-End-Hosts_ das Symbol **+** aus, um einen Front-End-Host zu erstellen.

    :::image type="content" source="./media/front-door-url-redirect/front-door-designer.png" alt-text="Front Door-Konfigurations-Designer":::

1. Geben Sie einen global eindeutigen Namen für Ihren Front-End-Standardhost für Front Door an. Wählen Sie **Hinzufügen** aus, um mit dem nächsten Schritt fortzufahren.

    :::image type="content" source="./media/front-door-url-redirect/front-door-create-frontend-host.png" alt-text="Hinzufügen eines Front-End-Hosts":::

### <a name="create-backend-pool"></a>Erstellen eines Back-End-Pools

1. Wählen Sie das Symbol **+** für die _Back-End-Pools_ aus, um einen Back-End-Pool zu erstellen. Geben Sie einen Namen für den Back-End-Pool an, und wählen Sie dann **Back-End hinzufügen** aus.

    :::image type="content" source="./media/front-door-url-redirect/front-door-designer-backend-pool.png" alt-text="Back-End-Pool des Front Door-Konfigurations-Designers":::

1. Wählen Sie den Back-End-Hosttyp als _App Service_ aus. Wählen Sie das Abonnement aus, in dem Ihre Web-App gehostet wird, und wählen Sie dann die entsprechende Web-App aus der Dropdownliste für **Name des Back-End-Hosts** aus.

    :::image type="content" source="./media/front-door-url-redirect/front-door-create-backend-pool.png" alt-text="Hinzufügen eines Back-Ends in einem Back-End-Pool":::

1. Wählen Sie **Hinzufügen** aus, um das Back-End zu speichern, und wählen Sie dann nochmals **Hinzufügen** aus, um die Konfiguration des Back-End-Pools zu speichern. 

## <a name="create-http-to-https-redirect-rule"></a>Erstellen einer Umleitungsregel von HTTP zu HTTPS

1. Wählen Sie das Symbol **+** für die *Routingregeln* aus, um eine Route zu erstellen. Geben Sie einen Namen für die Route an (z.B. „HttpToHttpsRedirect“), und legen Sie dann das Feld *Akzeptiertes Protokoll* auf **Nur HTTP** fest. Stellen Sie sicher, dass das entsprechende *Front-End bzw. die richtigen Domänen* ausgewählt ist/sind.  

    :::image type="content" source="./media/front-door-url-redirect/front-door-designer-routing-rule.png" alt-text="Routingregel des Front Door-Konfigurations-Designers":::

1. Legen Sie im Abschnitt *Routendetails* den *Routentyp* auf **Umleiten** fest. Stellen Sie sicher, dass der *Umleitungstyp* auf **Found (302)** und das *Umleitungsprotokoll* auf **Nur HTTPS** festgelegt ist. 

    :::image type="content" source="./media/front-door-url-redirect/front-door-redirect-config-example.png" alt-text="Hinzufügen einer Umleitungsroute von HTTP zu HTTPS":::

1. Wählen Sie **Hinzufügen** aus, um die Routingregel für die Umleitung von HTTP zu HTTPS zu speichern.

## <a name="create-forwarding-rule"></a>Erstellen einer Weiterleitungsregel

1. Fügen Sie eine weitere Routingregel für die Verarbeitung des HTTPS-Datenverkehrs hinzu. Wählen Sie das Zeichen **+** für die *Routingregeln* aus, und geben Sie einen Namen für die Route an, z. B. “DefaultForwardingRoute“. Legen Sie dann das Feld *Akzeptierte Protokolle* auf **Nur HTTPS** fest. Stellen Sie sicher, dass das entsprechende *Front-End bzw. die richtigen Domänen* ausgewählt ist/sind.

1. Legen Sie im Abschnitt „Routendetails“ den *Routentyp* auf **Umleiten** fest. Stellen Sie sicher, dass der richtige Back-End-Pool ausgewählt wird und das *Weiterleitungsprotokoll* auf **Nur HTTPS** festgelegt ist. 

    :::image type="content" source="./media/front-door-url-redirect/front-door-forward-route-example.png" alt-text="Hinzufügen einer Weiterleitungsroute für HTTPS-Datenverkehr" border="false":::

1. Wählen Sie **Hinzufügen** aus, um die Routingregel für die Anforderungsweiterleitung zu speichern.

1. Wählen Sie **Überprüfen und erstellen** und dann **Erstellen** aus, um Ihr Front Door-Profil zu erstellen. Navigieren Sie zu der Ressource, nachdem diese erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
- Weitere Informationen zur [URL-Umleitung für Front Door](front-door-url-redirect.md).

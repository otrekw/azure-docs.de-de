---
title: Verbinden von Azure Front Door Premium mit einem Web-App-Ursprung mithilfe von Private Link
titleSuffix: Azure Private Link
description: Erfahren Sie, wie Sie Ihr Azure Front Door Premium privat mit einer Web-App verbinden.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: tyao
ms.openlocfilehash: 805c3ba0360fcffe2bfd4217c0ef625fe61e5d64
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102030578"
---
# <a name="connect-azure-front-door-premium-to-a-web-app-origin-with-private-link"></a>Verbinden von Azure Front Door Premium mit einem Web-App-Ursprung mithilfe von Private Link

Dieser Artikel führt Sie durch die Konfiguration der Azure Front Door Premium-SKU für die private Verbindung mit Ihrer Web-App mithilfe des Azure Private Link-Dienstes.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Erstellen Sie einen [Private Link](../../private-link/create-private-link-service-portal.md)-Dienst für Ihre Ursprungswebserver.

> [!Note]
> Der private Endpunkt ist in öffentlichen Regionen für Windows- und Linux-Web-Apps mit den Tarifen PremiumV2 und PremiumV3 sowie dem Premium-Tarif für Azure Functions (auch als Elastic Premium-Tarif bezeichnet) verfügbar.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="enable-private-link-to-a-web-app-in-azure-front-door-premium"></a>Aktivieren von Private Link für eine Web-App in Azure Front Door Premium
 
In diesem Abschnitt ordnen Sie den Private Link-Dienst einem privaten Endpunkt zu, der im privaten Netzwerk von Azure Front Door erstellt wurde. 

1. Wählen Sie in Ihrem Azure Front Door Premium-Profil unter *Einstellungen* die Option **Ursprungsgruppen** aus.

1. Wählen Sie die Ursprungsgruppe aus, die den Web-App-Ursprung enthält, für den Sie Private Link aktivieren möchten.

1. Wählen Sie **+ Ursprung hinzufügen** aus, um einen neuen Web-App-Ursprung hinzuzufügen, oder wählen Sie einen zuvor erstellten Web-App-Ursprung aus der Liste aus.

    :::image type="content" source="../media/how-to-enable-private-link-web-app/private-endpoint-web-app.png" alt-text="Screenshot der Aktivierung von Private Link für eine Web-App.":::

1. Wählen Sie unter **Azure-Ressource auswählen** die Option **In mein Verzeichnis** aus. Wählen Sie die folgenden Einstellungen aus, oder geben Sie sie ein, um die Site zu konfigurieren, mit der Azure Front Door Premium privat verbunden werden soll.

    | Einstellung | Wert |
    | ------- | ----- |
    | Region | Wählen Sie die Region aus, die identisch ist oder Ihrem Ursprung am nächsten ist. |
    | Ressourcentyp | Wählen Sie **Microsoft.Web/sites** aus. |
    | Resource | Wählen Sie **myPrivateLinkService** aus. |
    | Untergeordnete Zielressource | sites |
    | Request-Nachricht | Passen Sie die Nachricht an, oder wählen Sie den Standardwert aus. |

1. Wählen Sie dann **Hinzufügen** aus, um Ihre Konfiguration zu speichern.

## <a name="approve-azure-front-door-premium-private-endpoint-connection-from-web-app"></a>Genehmigen der Azure Front Door Premium-Verbindung mit einem privaten Endpunkt über die Web-App

1. Wechseln Sie zu der Web-App, für die Sie Private Link im letzten Abschnitt konfiguriert haben. Wählen Sie **Netzwerk** unter **Einstellungen** aus.

1. Wählen Sie in **Netzwerk** die Option **Verbindungen mit privaten Endpunkten konfigurieren** aus.

    :::image type="content" source="../media/how-to-enable-private-link-web-app/web-app-configure-endpoint.png" alt-text="Screenshot der Netzwerkeinstellungen in einer Web-App.":::

1. Wählen Sie die *ausstehende* Anforderung für den privaten Endpunkt aus Azure Front Door Premium aus, und wählen Sie dann **Genehmigen** aus.

    :::image type="content" source="../media/how-to-enable-private-link-web-app/private-endpoint-pending-approval.png" alt-text="Screenshot der ausstehenden Anforderung für den privaten Endpunkt.":::

1. Nach der Genehmigung sollte der Screenshot wie der folgende Screenshot aussehen. Es dauert einige Minuten, bis die Verbindung vollständig hergestellt ist. Sie können jetzt über Azure Front Door Premium auf Ihre Web-App zugreifen. Der direkte Zugriff auf die Web-App aus dem öffentlichen Internet wird deaktiviert, nachdem der private Endpunkt aktiviert wurde.

    :::image type="content" source="../media/how-to-enable-private-link-web-app/private-endpoint-approved.png" alt-text="Screenshot der genehmigten Endpunktanforderung.":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [Private Link-Dienst mit der Azure-Web-App](../../app-service/networking/private-endpoint.md).

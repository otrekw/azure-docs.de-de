---
title: Bereitstellen von Azure API Management-Diensten in mehreren Azure-Regionen
titleSuffix: Azure API Management
description: Erfahren Sie, wie Sie eine Azure API Management-Dienstinstanz für mehrere Azure-Regionen bereitstellen.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/20/2020
ms.author: apimpm
ms.openlocfilehash: 17c92558ebef2eee0a4daead45d16a295cedd1bb
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82790478"
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Bereitstellen einer Azure API Management-Dienstinstanz für mehrere Azure-Regionen

Azure API Management unterstützt eine Bereitstellung für mehrere Regionen, sodass API-Herausgeber einen einzelnen API-Verwaltungsdienst in einer beliebigen Anzahl von unterstützten Azure-Regionen zur Verfügung stellen können. Die Bereitstellung in mehreren Regionen trägt dazu bei, die Anforderungslatenz bei geografisch verteilten API-Nutzern zu verringern, und verbessert gleichzeitig die Dienstverfügbarkeit, wenn eine Region offline geht.

Ein neuer Azure API Management-Dienst enthält zunächst nur eine [Einheit][unit] in einer Azure-Region (der primären Region). Der primären oder sekundären Region können zusätzliche Einheiten hinzugefügt werden. Eine API Management-Gatewaykomponente wird in jeder ausgewählten primären und sekundären Region bereitgestellt. Eingehende API-Anforderungen werden automatisch an die nächstgelegene Region weitergeleitet. Wenn eine Region offline geschaltet wird, werden die API-Anforderungen automatisch unter Auslassung der fehlerhaften Region an das nächstgelegene Gateway weitergeleitet.

> [!NOTE]
> Nur die Gatewaykomponente von API Management wird für alle Regionen bereitgestellt. Die Dienstverwaltungskomponente und das Entwicklerportal werden nur in der primären Region gehostet. Daher werden bei einem Ausfall der primären Region der Zugriff auf das Entwicklerportal und die Möglichkeit zum Ändern der Konfiguration (z. B. Hinzufügen von APIs, Anwenden von Richtlinien) beeinträchtigt, bis die primäre Region wieder online ist. Während die primäre Region offline ist, verarbeiten die verfügbaren sekundären Regionen den API-Datenverkehr mithilfe der letzten verfügbaren Konfiguration.

[!INCLUDE [premium.md](../../includes/api-management-availability-premium.md)]

## <a name="deploy-api-management-service-to-a-new-region"></a><a name="add-region"> </a>Bereitstellen des API Management-Diensts in einer neuen Region

> [!NOTE]
> Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen unter [Erstellen einer API Management-Dienstinstanz][create an api management service instance].

1. Navigieren Sie im Azure-Portal zu Ihrem API Management-Dienst, und klicken Sie im Menü auf den Eintrag **Standorte**.
2. Klicken Sie in der oberen Leiste auf **+ Hinzufügen**.
3. Wählen Sie den Standort aus der Dropdownliste aus, und legen Sie die Anzahl der Einheiten mit dem Schieberegler fest.
4. Klicken Sie zur Bestätigung auf die Schaltfläche **Hinzufügen**.
5. Wiederholen Sie diesen Vorgang, bis alle Standorte konfiguriert sind.
6. Klicken Sie in der oberen Leiste auf **Speichern**, um den Bereitstellungsprozess zu starten.

## <a name="delete-an-api-management-service-location"></a><a name="remove-region"> </a>Löschen einer API Management-Dienstidentifizierung

1. Navigieren Sie im Azure-Portal zu Ihrem API Management-Dienst, und klicken Sie im Menü auf den Eintrag **Standorte**.
2. Öffnen Sie für den Standort, den Sie entfernen möchten, das Kontextmenü mit der Schaltfläche **...** ganz rechts in der Tabelle. Wählen Sie die Option **Löschen** aus.
3. Bestätigen Sie den Löschvorgang, und klicken Sie auf **Speichern** um die Änderungen zu übernehmen.

## <a name="route-api-calls-to-regional-backend-services"></a><a name="route-backend"> </a>Weiterleiten von API-Aufrufen an regionale Back-End-Dienste

Azure API Management bietet nur eine Back-End-Dienst-URL. Obwohl in verschiedenen Regionen Azure API Management-Instanzen vorhanden sind, leitet das API-Gateway Anforderungen dennoch an denselben Back-End-Dienst weiter, der nur in einer Region bereitgestellt wird. In diesem Fall wird der Leistungsgewinn nur durch Antworten erzielt, die innerhalb von Azure API Management in einer für die Anforderung spezifischen Region zwischengespeichert werden, aber die Kontaktaufnahme mit dem Back-End weltweit kann dennoch zu einer hohen Latenz führen.

Um die geografische Verteilung Ihres Systems voll auszuschöpfen, sollten Sie Back-End-Dienste in den gleichen Regionen wie die Azure API Management-Instanzen bereitstellen. Anschließend können Sie mithilfe von Richtlinien und der `@(context.Deployment.Region)`-Eigenschaft den Datenverkehr an lokale Instanzen Ihres Back-Ends weiterleiten.

1. Navigieren Sie zu Ihrer Azure API Management-Instanz, und klicken Sie im Menü auf der linken Seite auf **APIs**.
2. Wählen Sie die gewünschte API aus.
3. Klicken Sie in der Pfeildropdownliste in **Verarbeitung von eingehendem Datenverkehr** auf **Code-Editor**.

    ![API-Code-Editor](./media/api-management-howto-deploy-multi-region/api-management-api-code-editor.png)

4. Verwenden Sie `set-backend` in Kombination mit bedingten `choose`-Richtlinien zum Erstellen einer ordnungsgemäßen Routingrichtlinie im Abschnitt `<inbound> </inbound>` der Datei.

    Die unten gezeigte XML-Datei würde z.B. für die Regionen „USA, Westen“ und „Asien, Osten“ funktionieren:

    ```xml
    <policies>
        <inbound>
            <base />
            <choose>
                <when condition="@("West US".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-us.com/" />
                </when>
                <when condition="@("East Asia".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-asia.com/" />
                </when>
                <otherwise>
                    <set-backend-service base-url="http://contoso-other.com/" />
                </otherwise>
            </choose>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>
    ```

> [!TIP]
> Sie können Ihren Back-End-Diensten auch [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) vorschalten, die API-Aufrufe an Traffic Manager weiterleiten und diesem das automatische Routing überlassen.

## <a name="use-custom-routing-to-api-management-regional-gateways"></a><a name="custom-routing"> </a>Verwenden von benutzerdefiniertem Routing an regionale API Management-Gateways

API Management leitet die Anforderungen basierend auf [der geringsten Latenz](../traffic-manager/traffic-manager-routing-methods.md#performance) an ein regionales _Gateway_ weiter. Es ist zwar nicht möglich, diese Einstellung in API Management außer Kraft zu setzen, Sie können jedoch Ihre eigene Traffic Manager-Instanz mit benutzerdefinierten Routingregeln verwenden.

1. Erstellen Sie Ihren eigenen [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/).
1. Bei Verwendung einer benutzerdefinierten Domäne [verwenden Sie sie mit Traffic Manager](../traffic-manager/traffic-manager-point-internet-domain.md) anstelle des API Management-Diensts.
1. [Konfigurieren Sie die regionalen API Management-Endpunkte in Traffic Manager](../traffic-manager/traffic-manager-manage-endpoints.md). Die regionalen Endpunkte folgen dem URL-Muster `https://<service-name>-<region>-01.regional.azure-api.net`, z. B. `https://contoso-westus2-01.regional.azure-api.net`.
1. [Konfigurieren Sie die regionalen API Management-Statusendpunkte in Traffic Manager](../traffic-manager/traffic-manager-monitoring.md). Die regionalen Statusendpunkte folgen dem URL-Muster `https://<service-name>-<region>-01.regional.azure-api.net/status-0123456789abcdef`, z. B. `https://contoso-westus2-01.regional.azure-api.net/status-0123456789abcdef`.
1. Geben Sie die [Routingmethode](../traffic-manager/traffic-manager-routing-methods.md) von Traffic Manager an.

[create an api management service instance]: get-started-create-service-instance.md
[get started with azure api management]: get-started-create-service-instance.md
[deploy an api management service instance to a new region]: #add-region
[delete an api management service instance from a region]: #remove-region
[unit]: https://azure.microsoft.com/pricing/details/api-management/
[premium]: https://azure.microsoft.com/pricing/details/api-management/

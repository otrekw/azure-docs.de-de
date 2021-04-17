---
title: Hinzufügen einer benutzerdefinierten Domäne zur Konfiguration der Azure Front Door Standard/Premium-SKU
description: In diesem Tutorial erfahren Sie, wie Sie eine benutzerdefinierte Domäne in eine Azure Front Door Standard/Premium-SKU integrieren.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: amsriva
ms.openlocfilehash: 36cb5720e409c86fcb4bc1a97863e5d3523cd3bc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104588748"
---
# <a name="create-a-custom-domain-on-azure-front-door-standardpremium-sku-preview-using-the-azure-portal"></a>Erstellen einer benutzerdefinierten Domäne in der Azure Front Door Standard/Premium-SKU (Vorschau) mit dem Azure-Portal

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Wenn Sie Azure Front Door Standard/Premium zur Anwendungsbereitstellung verwenden, ist eine benutzerdefinierte Domäne erforderlich, sofern Ihr eigener Domänenname in den Endbenutzeranforderungen sichtbar sein soll. Die Verwendung eines sichtbaren Domänennamens kann für Ihre Kunden komfortabel und für Branding-Zwecke hilfreich sein.

Nachdem Sie ein Azure Front Door Standard/Premium-Profil erstellt haben, verfügt der Front-End-Standardhost über die Unterdomäne „azurefd.net“. Diese Unterdomäne wird standardmäßig in die URL eingeschlossen, wenn Azure Front Door Standard/Premium Inhalte vom Back-End bereitstellt. Beispielsweise `https://contoso-frontend.azurefd.net/activeusers.htm`. Zur Vereinfachung bietet Azure Front Door Service Ihnen die Möglichkeit, dem Standardhost eine benutzerdefinierte Domäne zuzuordnen. Mit dieser Option enthält die URL bei der Übermittlung der Inhalte eine benutzerdefinierte Domäne anstelle eines Azure Front Door Standard/Premium-Domänennamens. Beispielsweise https://www.contoso.com/photo.png.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie in den [**zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen**](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen
* Bevor Sie die Schritte in diesem Tutorial ausführen können, müssen Sie zunächst eine Azure Front Door Service-Konfiguration erstellen. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Instanz von Front Door Standard/Premium](create-front-door-portal.md).

* Wenn Sie noch nicht über eine benutzerdefinierte Domäne verfügen, müssen Sie zunächst eine bei einem Domänenanbieter erwerben. Informationen hierzu finden Sie beispielsweise unter [Kaufen eines benutzerdefinierten Domänennamens für Azure-Web-Apps](../../app-service/manage-custom-dns-buy-domain.md).

* Wenn Sie Azure zum Hosten Ihrer [DNS-Domänen](../../dns/dns-overview.md) verwenden, müssen Sie das Domain Name System (DNS) des Domänenanbieters an eine Azure DNS-Instanz delegieren. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](../../dns/dns-delegate-domain-azure-dns.md). Wenn Sie einen Anbieter für die Verwaltung Ihrer DNS-Domäne verwenden, müssen Sie die Domäne andernfalls manuell überprüfen, indem Sie die angeforderten DNS-TXT-Einträge eingeben.

## <a name="add-a-new-custom-domain"></a>Hinzufügen einer neuen benutzerdefinierte Domäne

> [!NOTE]
> In der Public Preview wird die Verwendung von Azure DNS zum Erstellen von Apex-Domänen für Azure Front Door Standard/Premium nicht unterstützt. Es gibt außerdem weitere DNS-Anbieter, die das Vereinfachen von CNAME oder das Nachverfolgen von DNS unterstützen und die Verwendung von Apex-Domänen für Azure Front Door Standard/Premium ermöglichen.

Benutzerdefinierte Domänen werden im Portal im Abschnitt „Domänen“ verwaltet. Eine benutzerdefinierte Domäne kann vor der Zuordnung zu einem Endpunkt erstellt und überprüft werden. Eine benutzerdefinierte Domäne mit ihren Unterdomänen kann jeweils nur einem einzelnen Endpunkt zugeordnet werden. Innerhalb einer benutzerdefinierten Domäne können Sie jedoch verschiedene Unterdomänen für unterschiedliche Front Door-Endpunkte verwenden. Außerdem können Sie benutzerdefinierte Domänen mit unterschiedlichen Unterdomänen demselben Front Door-Endpunkt zuordnen.

1. Wählen Sie unter „Einstellungen“ für Ihr Azure Front Door-Profil *Domänen* und dann die Schaltfläche **Domäne hinzufügen** aus.

    :::image type="content" source="../media/how-to-add-custom-domain/add-domain-button.png" alt-text="Screenshot der Schaltfläche „Domäne hinzufügen“ auf der Landing Page „Domänen“":::

1. Die Seite **Domäne hinzufügen** wird angezeigt, auf der Sie Informationen zu der benutzerdefinierten Domäne eingeben können. Sie können „Von Azure verwaltetes DNS (empfohlen)“ oder die Option zum Verwenden eines eigenen DNS-Anbieters auswählen. Wenn Sie „Von Azure verwaltetes DNS (empfohlen)“ auswählen, wählen Sie eine vorhandene DNS-Zone aus, und wählen Sie dann eine benutzerdefinierte Unterdomäne aus, oder erstellen Sie eine neue. Wenn Sie einen anderen DNS-Anbieter verwenden, müssen Sie den Namen der benutzerdefinierten Domäne manuell eingeben. Wählen Sie **Hinzufügen** aus, um die benutzerdefinierte Domäne hinzuzufügen.

    :::image type="content" source="../media/how-to-add-custom-domain/add-domain-page.png" alt-text="Screenshot der Seite „Domäne hinzufügen“":::

    Es wird eine neue benutzerdefinierte Domäne mit dem Überprüfungsstatus **Wird übermittelt** erstellt.

    :::image type="content" source="../media/how-to-add-custom-domain/validation-state-submitting.png" alt-text="Screenshot des Domänenüberprüfungsstatus „Wird übermittelt“":::

    Warten Sie, bis sich der Überprüfungsstatus in **Ausstehend** ändert. Dieser Vorgang kann einige Minuten dauern.

    :::image type="content" source="../media/how-to-add-custom-domain/validation-state-pending.png" alt-text="Screenshot des Domänenüberprüfungsstatus „Ausstehend“":::

1. Wählen Sie den Überprüfungsstatus **Ausstehend** aus. Es wird eine neue Seite mit den Informationen zum DNS-TXT-Eintrag angezeigt, die zum Überprüfen der benutzerdefinierten Domäne erforderlich sind. Der TXT-Eintrag hat das Format `_dnsauth.<your_subdomain>`. Wenn Sie eine Azure DNS-basierte Zone verwenden, wählen Sie die Schaltfläche **Hinzufügen** aus. Anschließend wird in der Azure DNS-Zone ein neuer TXT-Eintrag mit dem angezeigten Eintragswert erstellt. Wenn Sie einen anderen DNS-Anbieter verwenden, erstellen Sie manuell einen neuen TXT-Eintrag mit dem Namen `dnsauth.<your_subdomain>` und dem auf der Seite angezeigten Eintragswert.

    :::image type="content" source="../media/how-to-add-custom-domain/validate-custom-domain.png" alt-text="Screenshot der Seite zum Überprüfen der benutzerdefinierten Domäne":::

1. Klicken Sie auf „Status aktualisieren“. Nachdem die Domäne mit dem DNS-TXT-Eintrag überprüft wurde, ändert sich der Überprüfungsstatus in **Überprüft**. Die Überprüfung kann einige Minuten in Anspruch nehmen.

    :::image type="content" source="../media/how-to-add-custom-domain/domain-status-verified.png" alt-text="Screenshot der überprüften benutzerdefinierten Domäne":::

1. Schließen Sie die Seite, um zur Landing Page mit der Liste der benutzerdefinierten Domänen zurückzukehren. Der Bereitstellungsstatus der benutzerdefinierten Domäne ändert sich in **Bereitgestellt**, und der Überprüfungsstatus ändert sich in **Genehmigt**.

    :::image type="content" source="../media/how-to-add-custom-domain/provisioned-approved-status.png" alt-text="Screenshot der Status „Bereitgestellt“ und „Genehmigt“":::

## <a name="associate-the-custom-domain-with-your-front-door-endpoint"></a>Zuordnen der benutzerdefinierten Domäne zum Front Door-Endpunkt

Nachdem Sie die benutzerdefinierte Domäne überprüft haben, können Sie sie dem Azure Front Door Standard/Premium-Endpunkt hinzufügen.

1. Nachdem die benutzerdefinierte Domäne überprüft wurde, können Sie sie einem vorhandenen Azure Front End Standard/Premium-Endpunkt und einer vorhandenen Azure Front End Standard/Premium-Route zuordnen. Wählen Sie den Link **Endpoint association** (Endpunktzuordnung) aus, um die Seite **Associate endpoint and routes** (Endpunkt und Routen zuordnen) zu öffnen. Wählen Sie einen Endpunkt und Routen aus, die Sie zuordnen möchten. Wählen Sie dann **Zuordnen** aus. Schließen Sie die Seite, sobald der Zuordnungsvorgang abgeschlossen wurde.

    :::image type="content" source="../media/how-to-add-custom-domain/associate-endpoint-routes.png" alt-text="Screenshot der Seite zum Zuordnen von Endpunkt und Routen":::

    Der Status von „Endpoint association“ (Endpunktzuordnung) sollte sich entsprechend dem Endpunkt ändern, dem die benutzerdefinierte Domäne zurzeit zugeordnet ist. 

    :::image type="content" source="../media/how-to-add-custom-domain/endpoint-association-status.png" alt-text="Screenshot des Links „Endpoint association“ (Endpunktzuordnung)":::

1. Wählen Sie den Link „DNS-Zustand“ aus.

    :::image type="content" source="../media/how-to-add-custom-domain/dns-state-link.png" alt-text="Screenshot des Links „DNS-Zustand“":::

1. Die Seite **CNAME-Eintrag hinzufügen oder aktualisieren** wird mit den Informationen zum CNAME-Eintrag angezeigt, die bereitgestellt werden müssen, bevor Datenverkehr gestartet werden kann. Wenn Sie in Azure DNS gehostete Zonen verwenden, können die CNAME-Einträge erstellt werden, indem Sie auf der Seite die Schaltfläche **Hinzufügen** auswählen. Wenn Sie einen anderen DNS-Anbieter verwenden, müssen Sie den auf der Seite angezeigten Namen und Wert des CNAME-Eintrags manuell eingeben.

    :::image type="content" source="../media/how-to-add-custom-domain/add-update-cname-record.png" alt-text="Screenshot der Seite „CNAME-Eintrag hinzufügen oder aktualisieren“":::

1. Sobald der CNAME-Eintrag erstellt und die benutzerdefinierte Domäne dem Azure Front Door-Endpunkt zugeordnet wurde, wird der Datenverkehr gestartet.

    > [!NOTE]
    > Wenn HTTPS aktiviert ist, kann die Zertifikatbereitstellung und -verteilung einige Minuten dauern, da die Verteilung an alle Edgestandorte erfolgt. 

## <a name="verify-the-custom-domain"></a>Überprüfen der benutzerdefinierten Domäne

Nachdem Sie die benutzerdefinierte Domäne überprüft und zugeordnet haben, überprüfen Sie, ob der Endpunkt ordnungsgemäß auf die benutzerdefinierte Domäne verweist.

:::image type="content" source="../media/how-to-add-custom-domain/verify-configuration.png" alt-text="Screenshot der überprüften und zugeordneten benutzerdefinierten Domäne":::

Überprüfen Sie abschließend mit einem Browser, ob die Anwendungsinhalte bereitgestellt werden.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Tutorial erfahren Sie, wie Sie HTTPS für Ihre benutzerdefinierte Domäne aktivieren:

> [!div class="nextstepaction"]
> [Aktivieren von HTTPS für eine benutzerdefinierte Domäne]()

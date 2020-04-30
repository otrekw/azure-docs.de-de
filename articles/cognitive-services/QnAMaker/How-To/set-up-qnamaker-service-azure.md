---
title: Einrichten eines QnA Maker-Diensts – QnA Maker
description: Vor dem Erstellen von QnA Maker-Wissensdatenbanken müssen Sie zuerst einen QnA Maker-Dienst in Azure einrichten. Jeder mit der Berechtigung zum Erstellen neuer Ressourcen in einem Abonnement kann einen QnA Maker-Dienst einrichten.
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 563a56fdb288568e7fe667fa54658400064a560f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81402992"
---
# <a name="manage-qna-maker-resources"></a>QnA Maker-Ressourcen

Vor dem Erstellen von QnA Maker-Wissensdatenbanken müssen Sie zuerst einen QnA Maker-Dienst in Azure einrichten. Jeder mit der Berechtigung zum Erstellen neuer Ressourcen in einem Abonnement kann einen QnA Maker-Dienst einrichten.

Ein fundiertes Verständnis der folgenden Konzepte ist hilfreich, bevor Sie Ihre Ressource erstellen:

* [QnA Maker-Ressourcen](../Concepts/azure-resources.md)
* [Erstellungs- und Veröffentlichungsschlüssel](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Erstellen eines neuen QnA Maker-Diensts

Mit diesem Verfahren werden die Azure-Ressourcen erstellt, die zum Verwalten des Inhalts der Wissensdatenbank benötigt werden. Nachdem Sie diese Schritte ausgeführt haben, finden Sie die _Abonnementschlüssel_ auf der Seite **Schlüssel** für die Ressource im Azure-Portal.

1. Melden Sie sich beim Azure-Portal an, und [erstellen Sie eine QnA Maker-Ressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker).

1. Nachdem Sie die Geschäftsbedingungen gelesen haben, wählen Sie **Erstellen** aus:

    ![Erstellen eines neuen QnA Maker-Diensts](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. Wählen Sie in **QnA Maker** die geeigneten Tarife und Regionen aus:

    ![Erstellen eines neuen QnA Maker-Diensts – Tarif und Regionen](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Geben Sie im Feld **Name** einen eindeutigen Namen zur Identifizierung dieses QnA Maker-Diensts ein. Dieser Name kennzeichnet auch den QnA Maker-Endpunkt, dem Ihre Wissensdatenbanken zugeordnet werden sollen.
    * Wählen Sie das **Abonnement** aus, unter dem die QnA Maker-Ressource bereitgestellt werden soll.
    * Wählen Sie den **Tarif** für die QnA Maker-Verwaltungsdienste (Portal und Verwaltungs-APIs) aus. Lesen Sie [weitere Informationen zu SKU-Preisen](https://aka.ms/qnamaker-pricing).
    * Erstellen Sie eine neue **Ressourcengruppe** (empfohlen), oder verwenden Sie eine vorhandene Ressourcengruppe, in der diese QnA Maker-Ressource bereitgestellt werden soll. QnA Maker erstellt verschiedene Azure-Ressourcen. Wenn Sie eine Ressourcengruppe für diese Ressourcen erstellen, können Sie die Ressourcen anhand des Ressourcengruppennamens leicht finden, verwalten und löschen.
    * Wählen Sie einen **Ressourcengruppenstandort** aus.
    * Wählen Sie den **Tarif für Suche** des Azure Cognitive Search-Diensts aus. Wenn die Option „Free-Tarif“ nicht verfügbar (abgeblendet) ist, bedeutet dies, dass Ihnen schon über Ihr Abonnement ein kostenloser Dienst bereitgestellt wird. In diesem Fall müssen Sie mit dem Basic-Tarif beginnen. Siehe [Azure Cognitive Search – Preise](https://azure.microsoft.com/pricing/details/search/).
    * Wählen Sie den **Suchspeicherort** aus, an dem die Azure Cognitive Search-Indizes bereitgestellt werden sollen. Einschränkungen zu den verpflichtenden Speicherorten für Kundendaten helfen bei der Bestimmung des Speicherorts, den Sie für Azure Cognitive Search wählen.
    * Geben Sie im Feld **App-Name** einen Namen für Ihre Azure App Service-Instanz ein.
    * Standardmäßig wird App Service nach dem Standardtarif (S1) abgerechnet. Sie können den Plan nach der Erstellung ändern. Erfahren Sie mehr über [App Service-Preise](https://azure.microsoft.com/pricing/details/app-service/).
    * Wählen Sie den **Website-Speicherort** aus, an dem App Service bereitgestellt werden soll.

        > [!NOTE]
        > Der **Suchspeicherort** kann vom **Website-Speicherort** abweichen.

    * Wählen Sie, ob Sie **Application Insights** aktivieren möchten. Wenn **Application Insights** aktiviert ist, sammelt QnA Maker Telemetriedaten zu Datenverkehr, Chatprotokollen und Fehlern.
    * Wählen Sie den **App Insights-Speicherort** aus, an dem die Application Insights-Ressource bereitgestellt werden soll.
    * Zur Kosteneinsparung können Sie einige, aber nicht alle Azure-Ressourcen, die für QnA Maker erstellt wurden, [freigeben](#configure-qna-maker-to-use-different-cognitive-search-resource).

1. Nachdem Sie alle Felder überprüft haben, wählen Sie **Erstellen** aus. Der Vorgang kann einige Minuten dauern.

1. Nachdem die Bereitstellung abgeschlossen ist, werden die folgenden neu erstellten Ressourcen in Ihrem Abonnement angezeigt:

   ![Im neuen QnA Maker-Dienst erstellte Ressource](../media/qnamaker-how-to-setup-service/resources-created.png)

    Die Ressource mit dem _Cognitive Services_-Typ enthält Ihre _Abonnementschlüssel_.

## <a name="find-subscription-keys-in-the-azure-portal"></a>Suchen von Abonnementschlüsseln im Azure-Portal

Sie können Ihre Abonnementschlüssel in dem Azure-Portal anzeigen und zurücksetzen, in dem Sie die QnA Maker-Ressource erstellt haben.

1. Wechseln Sie im Azure-Portal zur QnA Maker-Ressource, und wählen Sie die Ressource mit dem _Cognitive Services_-Typ aus:

    ![QnA Maker-Ressourcenliste](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Wechseln Sie zu **Schlüssel**:

    ![Abonnementschlüssel](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="find-endpoint-keys-in-the-qna-maker-portal"></a>Suchen von Endpunktschlüsseln im QnA Maker-Portal

Der Endpunkt befindet sich in demselben Bereich wie die Ressource, da die Endpunktschlüssel verwendet werden, um einen Aufruf an die Wissensdatenbank zu senden.

Endpunktschlüssel können im [QnA Maker-Portal](https://qnamaker.ai) verwaltet werden.

1. Melden Sie sich beim [QnA Maker-Portal](https://qnamaker.ai) an, wechseln Sie zu Ihrem Profil, und wählen Sie **Diensteinstellungen** aus:

    ![Endpunktschlüssel](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Zeigen Sie Ihre Schlüssel an, oder setzen Sie die Schlüssel zurück:

    > [!div class="mx-imgBorder"]
    > ![Endpunktschlüssel-Manager](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Aktualisieren Sie Ihre Schlüssel, wenn Sie denken, dass sie gefährdet sind. Dazu müssen möglicherweise entsprechende Änderungen an Ihrem Clientanwendungs- oder Botcode vorgenommen werden.

## <a name="upgrade-qna-maker-sku"></a>Upgraden der QnA Maker-SKU

Wenn Ihre Wissensdatenbank mehr Fragen und Antworten enthalten soll, die über Ihren aktuellen Tarif hinausgehen, upgraden Sie den Tarif Ihres QnA Maker-Diensts.

So führen Sie ein Upgrade der QnA Maker Management-SKU aus:

1. Wechseln Sie im Azure-Portal zu Ihrer QnA Maker-Ressource, und wählen Sie **Tarif** aus.

    ![QnA Maker-Ressource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Wählen Sie die passende SKU aus, und drücken Sie **Auswählen**.

    ![QnA Maker-Preise](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Upgraden von App Service

 Wenn Ihre Wissensdatenbank mehr Anforderungen aus Ihrer Client-App verarbeiten muss, upgraden Sie Ihren App Service-Tarif.

Sie können den App Service [hochskalieren](https://docs.microsoft.com/azure/app-service/manage-scale-up) oder aufskalieren.

Wechseln Sie im Azure-Portal zur App Service-Ressource, und wählen Sie nach Bedarf die Option **Hochskalieren** oder **Aufskalieren** aus.

![QnA Maker App Service-Staffelung](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-the-azure-cognitive-search-service"></a>Upgrade des Azure Cognitive Search-Diensts

Wenn Sie voraussichtlich eine Vielzahl von Wissensdatenbanken benötigen, upgraden Sie den Tarif Ihres Azure Cognitive Search-Diensts.

Sie können derzeit kein direktes Upgrade der Azure Search-SKU ausführen. Allerdings können Sie eine neue Azure Search-Ressource mit der gewünschten SKU erstellen, die Daten in der neuen Ressource wiederherstellen und anschließend eine Verknüpfung mit dem QnA Maker-Stapel herstellen. Gehen Sie dazu folgendermaßen vor:

1. Erstellen Sie im Azure-Portal eine neue Azure Search-Ressource, und wählen Sie die gewünschte SKU aus.

    ![QnA Maker-Azure Search-Ressource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Stellen Sie die Indizes aus Ihrer ursprünglichen Azure Search-Ressource in der neuen wieder her. Sehen Sie dazu den [Beispielcode für die Wiederherstellung aus einer Sicherung](https://github.com/pchoudhari/QnAMakerBackupRestore).

1. Nachdem die Daten wiederhergestellt wurden, wechseln Sie zu Ihrer neuen Azure Search-Ressource, wählen Sie **Schlüssel** aus, und notieren Sie sich den **Namen** und den **Administratorschlüssel**:

    ![Schlüssel von QnA Maker-Azure Search](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Um die neue Azure Search-Ressource mit dem QnA Maker-Stapel zu verknüpfen, wechseln Sie zur QnA Maker App Service-Instanz.

    ![QnA Maker App Service-Instanz](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. Wählen Sie **Anwendungseinstellungen** aus, und ändern Sie die Einstellungen in den Feldern **AzureSearchName** und **AzureSearchAdminKey** aus Schritt 3.

    ![QnA Maker App Service-Einstellung](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. Starten Sie die App Service-Instanz neu.

    ![Neustarten der QnA Maker App Service-Instanz](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="get-the-latest-runtime-updates"></a>Abrufen der neuesten Runtime-Updates

Die QnA Maker-Runtime ist Teil der Azure App Service-Instanz, die beim [Erstellen eines QnA Maker-Diensts](./set-up-qnamaker-service-azure.md) im Azure-Portal bereitgestellt wird. Updates für die Runtime werden in regelmäßigen Abständen bereitgestellt. Die QnA Maker App Service-Instanz befindet sich nach der Websiteerweiterung vom April 2019 (Version 5+) im automatischen Updatemodus. Dieses Update ist so konzipiert, dass es während Upgrades keine Downtime berücksichtigt.

Sehen Sie sich die aktuelle Version unter https://www.qnamaker.ai/UserSettings an. Wenn Ihre Version älter als Version 5.x ist, müssen Sie App Service neu starten, damit die neuesten Updates angewendet werden:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem QnA Maker-Dienst (Ressourcengruppe).

    > [!div class="mx-imgBorder"]
    > ![Azure-Ressourcengruppe von QnA Maker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Wählen Sie die App Service-Instanz aus, und öffnen Sie den Abschnitt **Übersicht**.

    > [!div class="mx-imgBorder"]
    > ![QnA Maker App Service-Instanz](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)


1. Starten Sie App Service neu. Der Updateprozess sollte innerhalb weniger Sekunden abgeschlossen sein. Abhängige Anwendungen oder Bots, in denen dieser QnA Maker-Dienst verwendet wird, stehen während dieses Neustartzeitraums für Endbenutzer nicht zur Verfügung.

    ![Neustarten der QnA Maker App Service-Instanz](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="configure-qna-maker-to-use-different-cognitive-search-resource"></a>Konfigurieren von QnA Maker zur Verwendung einer anderen Cognitive Search-Ressource

Wenn Sie einen QnA-Dienst und dessen Abhängigkeiten (z.B. Search) über das Portal erstellen, wird automatisch ein Suchdienst erstellt und mit dem QnA Maker-Dienst verknüpft. Nachdem diese Ressourcen erstellt wurden, können Sie die App Service-Einstellung aktualisieren, um einen bereits vorhandenen Suchdienst zu nutzen und den soeben erstellten Suchdienst zu entfernen.

Die **App Service**-Ressource von QnA Maker verwendet die Cognitive Search-Ressource. Um die von QnA Maker verwendete Cognitive Search-Ressource zu ändern, müssen Sie die Einstellung im Azure-Portal ändern.

1. Rufen Sie den **Administratorschlüssel** und den **Namen** der Cognitive Search-Ressource ab, die von QnA Maker verwendet werden soll.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und suchen Sie den **App Service**, der Ihrer QnA Maker-Ressource zugeordnet ist. Beide weisen denselben Namen auf.

1. Wählen Sie **Einstellungen** und dann **Konfiguration** aus. Dadurch werden alle vorhandenen Einstellungen für den App Service des QnA Makers angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Screenshot des Azure-Portals mit dargestellten Konfigurationseinstellungen für den App Service](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-app-service-configuration.png)

1. Ändern Sie die Werte für die folgenden Schlüssel:

    * **AzureSearchAdminKey**
    * **AzureSearchName**

1. Um die neuen Einstellungen zu verwenden, müssen Sie den App Service neu starten. Wählen Sie **Übersicht** und dann **Neu starten** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot des Azure-Portals beim Neustart von App Service nach dem Ändern der Konfigurationseinstellungen](../media/qnamaker-how-to-upgrade-qnamaker/screenshot-azure-portal-restart-app-service.png)

Wenn Sie einen QnA-Dienst über Azure Resource Manager-Vorlagen erstellen, können Sie alle Ressourcen erstellen und die App Service-Erstellung so steuern, dass sie einen vorhandenen Suchdienst nutzt.

Weitere Informationen zum Konfigurieren der App Service-Anwendungseinstellungen finden Sie [hier](../../../app-service/configure-common.md#configure-app-settings).

## <a name="configure-app-service-idle-setting-to-avoid-timeout"></a>Konfigurieren der App Service-Leerlaufeinstellung zur Vermeidung von Timeouts

Die App Service-Instanz, von der die QnA Maker-Vorhersageruntime für eine veröffentlichte Wissensdatenbank bereitgestellt wird, verfügt über eine Leerlauftimeoutkonfiguration, durch die standardmäßig automatisch ein Timeout ausgelöst wird, wenn sich der Dienst im Leerlauf befindet. Dies hat für QnA Maker zur Folge, dass für die generateAnswer-API Ihrer Vorhersageruntime gelegentlich ein Timeout auftritt, wenn eine Weile kein Datenverkehr übermittelt wurde.

Legen Sie den Leerlauf auf „Immer aktiv“ fest, um zu gewährleisten, dass die Vorhersageendpunkt-App geladen bleibt, auch wenn kein Datenverkehr übermittelt wird.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach der App Service-Instanz Ihrer QnA Maker-Ressource, und wählen Sie sie aus. Sie hat den gleichen Namen wie die QnA Maker-Ressource, aber einen anderen **Typ** (App Service).
1. Navigieren Sie zu **Einstellungen**, und wählen Sie **Konfiguration** aus.
1. Wählen Sie im Konfigurationsbereich **Allgemeine Einstellungen** aus, navigieren Sie zu **Immer aktiv**, und wählen Sie **Ein** aus.

    > [!div class="mx-imgBorder"]
    > ![Wählen Sie im Konfigurationsbereich **Allgemeine Einstellungen** aus, navigieren Sie zu **Immer aktiv**, und wählen Sie **Ein** aus.](../media/qnamaker-how-to-upgrade-qnamaker/configure-app-service-idle-timeout.png)

1. Wählen Sie zum Speichern der Konfiguration **Speichern** aus.
1. Sie werden gefragt, ob Sie die App neu starten möchten, um die neue Einstellung zu verwenden. Wählen Sie **Weiter**.

Weitere Informationen zum Konfigurieren der allgemeinen App Service-Einstellungen finden Sie [hier](../../../app-service/configure-common.md#configure-general-settings).

## <a name="delete-azure-resources"></a>Löschen von Azure-Ressourcen

Wenn Sie Azure-Ressourcen löschen, die für Ihre QnA Maker-Wissensdatenbanken verwendet werden, funktionieren diese Wissensdatenbanken nicht mehr. Stellen Sie vor dem Löschen von Ressourcen sicher, dass Sie Ihre Wissensdatenbanken auf der Seite **Einstellungen** exportieren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [App-Dienst](../../../app-service/index.yml) und den [Suchdienst](../../../search/index.yml).

> [!div class="nextstepaction"]
> [Erstellen und Veröffentlichen einer Wissensdatenbank](../Quickstarts/create-publish-knowledge-base.md)

---
title: Einrichten eines QnA Maker-Diensts – QnA Maker
description: Vor dem Erstellen von QnA Maker-Wissensdatenbanken müssen Sie zuerst einen QnA Maker-Dienst in Azure einrichten. Jeder mit der Berechtigung zum Erstellen neuer Ressourcen in einem Abonnement kann einen QnA Maker-Dienst einrichten.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 7137b26dcf951f98473f0fcc139f563438ce8878
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102203469"
---
# <a name="manage-qna-maker-resources"></a>QnA Maker-Ressourcen

Vor dem Erstellen von QnA Maker-Wissensdatenbanken müssen Sie zuerst einen QnA Maker-Dienst in Azure einrichten. Jeder mit der Berechtigung zum Erstellen neuer Ressourcen in einem Abonnement kann einen QnA Maker-Dienst einrichten.

Ein fundiertes Verständnis der folgenden Konzepte ist hilfreich, bevor Sie Ihre Ressource erstellen:

* [QnA Maker-Ressourcen](../Concepts/azure-resources.md)
* [Erstellungs- und Veröffentlichungsschlüssel](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Erstellen eines neuen QnA Maker-Diensts

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/v1)

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

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/v2)

Mit diesem Verfahren werden die Azure-Ressourcen erstellt, die zum Verwalten des Inhalts der Wissensdatenbank benötigt werden. Nachdem Sie diese Schritte ausgeführt haben, finden Sie die *Abonnementschlüssel* auf der Seite **Schlüssel** für die Ressource im Azure-Portal.

1. Melden Sie sich beim Azure-Portal an, und [erstellen Sie eine QnA Maker-Ressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker).

1. Nachdem Sie die Geschäftsbedingungen gelesen haben, wählen Sie **Erstellen** aus:

    ![Erstellen eines neuen QnA Maker-Diensts](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. Aktivieren Sie in **QnA Maker** das Kontrollkästchen „Verwaltet (Vorschau)“, und wählen Sie die geeigneten Tarife und Regionen aus:

    ![Erstellen eines neuen QnA Maker verwaltet-Diensts – Tarif und Regionen](../media/qnamaker-how-to-setup-service/enter-qnamaker-v2-info.png)

    * Wählen Sie das **Abonnement** aus, unter dem die QnA Maker-Ressource bereitgestellt werden soll.
    * Erstellen Sie eine neue **Ressourcengruppe** (empfohlen), oder verwenden Sie eine vorhandene Ressourcengruppe, in der diese QnA Maker verwaltet (Vorschau)-Ressource bereitgestellt werden soll. QnA Maker verwaltet (Vorschau) erstellt ein paar Azure-Ressourcen. Wenn Sie eine Ressourcengruppe für diese Ressourcen erstellen, können Sie die Ressourcen anhand des Ressourcengruppennamens leicht finden, verwalten und löschen.
    * Geben Sie in das Feld **Name** einen eindeutigen Namen ein, um diesen QnA Maker verwaltet (Vorschau)-Dienst zu identifizieren. 
    * Wählen Sie den **Standort** aus, an dem der QnA Maker verwaltet (Vorschau)-Dienst bereitgestellt werden soll. Die Verwaltungs-APIs und der Dienstendpunkt werden an diesem Standort gehostet. 
    * Wählen Sie den **Tarif** für den QnA Maker verwaltet (Vorschau)-Dienst aus (für die Vorschauversion „Free“). Lesen Sie [weitere Informationen zu SKU-Preisen](https://aka.ms/qnamaker-pricing).
    * Wählen Sie den **Suchspeicherort** aus, an dem die Azure Cognitive Search-Indizes bereitgestellt werden sollen. Einschränkungen zu den verpflichtenden Speicherorten für Kundendaten helfen bei der Bestimmung des Speicherorts, den Sie für Azure Cognitive Search wählen.
    * Wählen Sie den **Tarif für Suche** des Azure Cognitive Search-Diensts aus. Wenn die Option „Free-Tarif“ nicht verfügbar (abgeblendet) ist, bedeutet dies, dass Ihnen schon über Ihr Abonnement ein kostenloser Dienst bereitgestellt wird. In diesem Fall müssen Sie mit dem Basic-Tarif beginnen. Siehe [Azure Cognitive Search – Preise](https://azure.microsoft.com/pricing/details/search/).

1. Nachdem Sie alle Felder überprüft haben, wählen Sie **Überprüfen und erstellen** aus. Der Vorgang kann einige Minuten dauern.

1. Nachdem die Bereitstellung abgeschlossen ist, werden die folgenden neu erstellten Ressourcen in Ihrem Abonnement angezeigt:

    ![In einem neuen QnA Maker verwaltet (Vorschau)-Dienst erstellte Ressource](../media/qnamaker-how-to-setup-service/resources-created-v2.png)

    Die Ressource mit dem _Cognitive Services_-Typ enthält Ihre _Abonnementschlüssel_.

---

## <a name="upgrade-azure-resources"></a>Durchführen von Upgrades für Azure-Ressourcen

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/v1)

### <a name="upgrade-qna-maker-sku"></a>Upgraden der QnA Maker-SKU

Wenn Ihre Wissensdatenbank mehr Fragen und Antworten enthalten soll, die über Ihren aktuellen Tarif hinausgehen, upgraden Sie den Tarif Ihres QnA Maker-Diensts.

So führen Sie ein Upgrade der QnA Maker Management-SKU aus:

1. Wechseln Sie im Azure-Portal zu Ihrer QnA Maker-Ressource, und wählen Sie **Tarif** aus.

    ![QnA Maker-Ressource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Wählen Sie die passende SKU aus, und drücken Sie **Auswählen**.

    ![QnA Maker-Preise](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)
    
### <a name="upgrade-app-service"></a>Upgraden von App Service

Wenn Ihre Wissensdatenbank mehr Anforderungen aus Ihrer Client-App verarbeiten muss, upgraden Sie Ihren App Service-Tarif.

Sie können den App Service [hochskalieren](../../../app-service/manage-scale-up.md) oder aufskalieren.

Wechseln Sie im Azure-Portal zur App Service-Ressource, und wählen Sie nach Bedarf die Option **Hochskalieren** oder **Aufskalieren** aus.

![QnA Maker App Service-Staffelung](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Upgrade des Azure Cognitive Search-Diensts

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
    
### <a name="inactivity-policy-for-free-search-resources"></a>Richtlinie zur Inaktivität für kostenlose Search-Ressourcen

Wenn Sie keine QnA Maker-Ressource verwenden, sollten Sie alle Ressourcen entfernen. Wenn Sie die nicht verwendeten Ressourcen nicht entfernen, wird Ihre Wissensdatenbank nicht mehr funktionieren, wenn Sie eine kostenlose Search-Ressource erstellt haben.

Kostenlose Search-Ressourcen werden nach 90 Tagen gelöscht, wenn kein API-Aufruf empfangen wurde.
    
# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/v2)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Upgrade des Azure Cognitive Search-Diensts

Wenn Sie voraussichtlich eine Vielzahl von Wissensdatenbanken benötigen, upgraden Sie den Tarif Ihres Azure Cognitive Search-Diensts.

Sie können derzeit kein direktes Upgrade der Azure Search-SKU ausführen. Allerdings können Sie eine neue Azure Search-Ressource mit der gewünschten SKU erstellen, die Daten in der neuen Ressource wiederherstellen und anschließend eine Verknüpfung mit dem QnA Maker-Stapel herstellen. Gehen Sie dazu folgendermaßen vor:

1. Erstellen Sie im Azure-Portal eine neue Azure Search-Ressource, und wählen Sie die gewünschte SKU aus.

    ![QnA Maker-Azure Search-Ressource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Stellen Sie die Indizes aus Ihrer ursprünglichen Azure Search-Ressource in der neuen wieder her. Sehen Sie dazu den [Beispielcode für die Wiederherstellung aus einer Sicherung](https://github.com/pchoudhari/QnAMakerBackupRestore).

1. Informationen, wie Sie die neue Azure Search-Ressource mit dem QnA Maker verwaltet (Vorschau)-Dienst verknüpfen, finden Sie im folgenden Thema.

### <a name="inactivity-policy-for-free-search-resources"></a>Richtlinie zur Inaktivität für kostenlose Search-Ressourcen

Wenn Sie keine QnA Maker-Ressource verwenden, sollten Sie alle Ressourcen entfernen. Wenn Sie die nicht verwendeten Ressourcen nicht entfernen, wird Ihre Wissensdatenbank nicht mehr funktionieren, wenn Sie eine kostenlose Search-Ressource erstellt haben.

Kostenlose Search-Ressourcen werden nach 90 Tagen gelöscht, wenn kein API-Aufruf empfangen wurde.

---

## <a name="configure-azure-resources"></a>Konfigurieren von Azure-Ressourcen

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/v1)

### <a name="configure-qna-maker-to-use-different-cognitive-search-resource"></a>Konfigurieren von QnA Maker zur Verwendung einer anderen Cognitive Search-Ressource

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

### <a name="get-the-latest-runtime-updates"></a>Abrufen der neuesten Runtime-Updates

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

### <a name="configure-app-service-idle-setting-to-avoid-timeout"></a>Konfigurieren der App Service-Leerlaufeinstellung zur Vermeidung von Timeouts

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

### <a name="business-continuity-with-traffic-manager"></a>Geschäftskontinuität mit Traffic Manager

Das Hauptziel des Geschäftskontinuitätsplans besteht darin, einen ausfallsicheren Wissensdatenbank-Endpunkt zu erstellen, der sicherstellen kann, dass keine Ausfallzeiten für den Bot oder die ihn verwendende Anwendung entstehen.

> [!div class="mx-imgBorder"]
> ![QnA Maker-Geschäftskontinuitätsplan](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Das oben dargestellte allgemeine Konzept lautet wie folgt:

1. Richten Sie zwei parallele [QnA Maker-Dienste](set-up-qnamaker-service-azure.md) in [Azure-Regionspaaren](../../../best-practices-availability-paired-regions.md) ein.

1. [Sichern](../../../app-service/manage-backup.md) Sie Ihren primären QnA Maker-App-Dienst, und [stellen](../../../app-service/web-sites-restore.md) Sie ihn bei der Sekundäreinrichtung wieder her. Dadurch wird sichergestellt, dass beide Setups mit demselben Hostnamen und denselben Schlüsseln arbeiten.

1. Halten Sie die primären und sekundären Azure-Suchindizes synchron. Verwenden Sie das [hier](https://github.com/pchoudhari/QnAMakerBackupRestore) aufgeführte GitHub-Beispiel, um zu erfahren, wie Azure-Indizes gesichert und wiederhergestellt werden.

1. Sichern Sie die Application Insights-Daten mithilfe des [fortlaufenden Exports](../../../azure-monitor/app/export-telemetry.md).

1. Verwenden Sie [Traffic Manager](../../../traffic-manager/traffic-manager-overview.md) nach der Einrichtung der primären und sekundären Stapel, um die zwei Endpunkte zu konfigurieren und eine Routingmethode einzurichten.

1. Sie müssten ein TLS-Zertifikat (Transport Layer Security), früher SSL-Zertifikat (Secure Sockets Layer) genannt, für Ihren Traffic Manager-Endpunkt erstellen. [Binden Sie das TLS/SSL-Zertifikat](../../../app-service/configure-ssl-bindings.md) an Ihre App-Dienste.

1. Verwenden Sie dann den Traffic Manager-Endpunkt in Ihrem Bot oder in Ihrer App.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/v2)

### <a name="configure-qna-maker-managed-preview-service-to-use-different-cognitive-search-resource"></a>Konfigurieren des QnA Maker verwaltet (Vorschau)-Diensts zur Verwendung einer anderen Cognitive Search-Ressource

Wenn Sie einen QnA-Dienst verwaltet (Vorschau) und dessen Abhängigkeiten (z. B. Search) über das Portal erstellen, wird automatisch ein Suchdienst für Sie erstellt und mit dem QnA Maker verwaltet (Vorschau)-Dienst verknüpft. Nachdem diese Ressourcen erstellt wurden, können Sie den Suchdienst auf der Registerkarte **Konfiguration** aktualisieren.

1. Wechseln Sie zu Ihrem QnA Maker verwaltet (Vorschau)-Dienst im Azure-Portal.

1. Wählen Sie **Konfiguration** aus, und wählen Sie den Azure Cognitive Search-Dienst aus, den Sie mit Ihrem QnA Maker verwaltet (Vorschau)-Dienst verknüpfen möchten.

    ![Screenshot der Seite „Konfiguration“ von QnA Maker verwaltet (Vorschau)](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-configuration.png)

1. Klicken Sie auf **Speichern**.

> [!NOTE]
> Wenn Sie den dem QnA Maker zugeordneten Azure Search-Dienst ändern, verlieren Sie den Zugriff auf alle bereits darin vorhandenen Wissensdatenbanken. Stellen Sie sicher, dass Sie die vorhandenen Wissensdatenbanken exportieren, bevor Sie den Azure Search-Dienst ändern.

---

## <a name="delete-azure-resources"></a>Löschen von Azure-Ressourcen

Wenn Sie Azure-Ressourcen löschen, die für Ihre QnA Maker-Wissensdatenbanken verwendet werden, funktionieren diese Wissensdatenbanken nicht mehr. Stellen Sie vor dem Löschen von Ressourcen sicher, dass Sie Ihre Wissensdatenbanken auf der Seite **Einstellungen** exportieren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [App-Dienst](../../../app-service/index.yml) und den [Suchdienst](../../../search/index.yml).

> [!div class="nextstepaction"]
> [Erstellen zusammen mit anderen Personen](../index.yml)

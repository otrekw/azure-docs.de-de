---
title: Hochskalieren von Features und Funktionen
description: Hier erfahren Sie, wie Sie eine App in Azure App Service hochskalieren. Profitieren Sie von mehr CPU, Speicher, Speicherplatz und zusätzlichen Features.
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.topic: article
ms.date: 08/19/2019
ms.custom: seodec18
ms.openlocfilehash: eb0ce5cfadbab247299cd3c22bf6ee618bf19731
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100582481"
---
# <a name="scale-up-an-app-in-azure-app-service"></a>Hochskalieren einer App in Azure App Service

In diesem Artikel wird die Vorgehensweise beim Skalieren Ihrer App in Azure App Service beschrieben. Es gibt zwei Workflows für die Skalierung: Hochskalieren und Aufskalieren. Dieser Artikel befasst sich mit dem Workflow zum Hochskalieren.

* [Hochskalieren:](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling)Bei dieser Methode erhalten Sie mehr CPUs, mehr Arbeitsspeicher und mehr Speicherplatz sowie zusätzliche Features wie dedizierte virtuelle Computer, benutzerdefinierte Domänen und Zertifikate, Stagingslots, automatische Skalierung und mehr. Zum Hochskalieren muss der Tarif des App Service-Plans geändert werden, zu dem die App gehört.
* [Aufskalieren:](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling) Bei dieser Methode wird die Anzahl von VM-Instanzen erhöht, die Ihre App ausführen.
  Die Instanzenanzahl kann je nach Tarif auf bis zu 30 aufskaliert werden. [App Service-Umgebungen](environment/intro.md) im Tarif **Isolated** erhöhen die Anzahl der Instanzen beim horizontalen Hochskalieren weiter auf 100. Weitere Informationen zum Aufskalieren finden Sie unter [Scale instance count manually or automatically](../azure-monitor/autoscale/autoscale-get-started.md) (Manuelles oder automatisches Skalieren der Instanzanzahl). Dort erfahren Sie, wie Sie die automatische Skalierung verwenden, d.h. das automatische Skalieren der Instanzenzahl basierend auf vordefinierten Regeln und Zeitplänen.

Diese Skalierungseinstellungen werden innerhalb von Sekunden angewendet und wirken sich auf alle Apps im [App Service-Plan](../app-service/overview-hosting-plans.md)aus.
Dazu müssen Sie weder Ihren Code ändern noch Ihre Anwendung erneut bereitstellen.

Informationen zur Preisgestaltung und zu den Funktionen der einzelnen App Service-Pläne finden Sie unter [App Service Preise](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Bevor Sie einen App Service-Plan aus dem Tarif **Free** entfernen, müssen Sie zunächst die [Ausgabenlimits](https://azure.microsoft.com/pricing/spending-limits/) für Ihr Azure-Abonnement aufheben. Informationen zum Anzeigen oder Ändern von Optionen für Ihr Microsoft Azure App Service-Abonnement finden Sie unter [Microsoft Azure-Abonnements][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Hochskalieren Ihres Tarifs

> [!NOTE]
> Informationen zum Hochskalieren auf den Tarif **PremiumV3** finden Sie unter [Konfigurieren des PremiumV3-Tarifs für App Service](app-service-configure-premium-tier.md).
>

1. Öffnen Sie das [Azure-Portal][portal] in Ihrem Browser.

1. Wählen Sie auf Seite „App Service“ der App im linken Menü **Hochskalieren (App Service-Plan)** aus.
   
3. Wählen Sie Ihren Tarif, und klicken Sie auf **Übernehmen**. Wählen Sie die verschiedenen Kategorien (z. B. **Produktion**) aus, und **sehen Sie sich auch weitere Optionen an**, um weitere Tarife einzublenden.
   
    ![Navigieren Sie, um Ihre Azure-App hochzuskalieren.][ChooseWHP]

    Nach Abschluss des Vorgangs sehen Sie ein Popupfenster mit einem grünen Erfolgshäkchen.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Skalieren zugehöriger Ressourcen
Wenn Ihre App von anderen Diensten abhängt, beispielsweise von Azure SQL-Datenbank oder Azure Storage, können Sie diese Ressourcen gesondert hochskalieren. Diese Ressourcen werden vom App Service-Plan nicht verwaltet.

1. Klicken Sie auf der Seite **Übersicht** Ihrer App auf den Link **Ressourcengruppe**.
   
    ![Hochskalieren der zugehörigen Ressourcen Ihrer Azure-App](./media/web-sites-scale/RGEssentialsLink.png)

2. Wählen Sie im Bereich **Zusammenfassung** der Seite **Ressourcengruppe** eine Ressource aus, die Sie skalieren möchten. Der folgende Screenshot zeigt eine SQL-Datenbank-Ressource.
   
    ![Navigieren zur Seite „Ressourcengruppe“, um Ihre Azure-App hochzuskalieren](./media/web-sites-scale/ResourceGroup.png)

    Informationen zum Hochskalieren der betreffenden Ressource finden Sie in der Dokumentation zum jeweiligen Ressourcentyp. Informationen zum Hochskalieren einer einzelnen SQL-Datenbank finden Sie unter [Skalieren von Einzeldatenbankressourcen in Azure SQL-Datenbank](../azure-sql/database/single-database-scale.md). Informationen zum Hochskalieren einer Azure Database for MySQL-Ressource finden Sie unter [Skalieren von MySQL-Ressourcen](../mysql/concepts-pricing-tiers.md#scale-resources).

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Vergleichen von Tarifen

Ausführliche Informationen, z.B. VM-Größen für jeden Tarif, finden Sie unter [App Service-Preisdetails](https://azure.microsoft.com/pricing/details/app-service).

Eine Tabelle der Dienstbeschränkungen, Kontingente und Einschränkungen sowie unterstützte Features in jedem Tarif finden Sie unter [App Service-Grenzwerte](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits).

<a name="Next Steps"></a>

## <a name="more-resources"></a>Weitere Ressourcen

[Manuelles oder automatisches Skalieren der Instanzenzahl](../azure-monitor/autoscale/autoscale-get-started.md)  
[Konfigurieren des PremiumV3-Tarifs für App Service](app-service-configure-premium-tier.md)

<!-- LINKS -->
[vmsizes]:https://azure.microsoft.com/pricing/details/app-service/
[SQLaccountsbilling]:https://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:https://account.windowsazure.com/subscriptions
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
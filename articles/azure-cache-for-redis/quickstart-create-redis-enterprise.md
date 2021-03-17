---
title: 'Schnellstart: Erstellen einer Redis Cache-Instanz im Enterprise-Tarif'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie eine Azure Cache for Redis-Instanz mit Enterprise-Tarifen erstellen.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.custom: mvc
ms.topic: quickstart
ms.date: 02/08/2021
ms.openlocfilehash: 63daa7cda2f10448e9a0f8e250412137dc2af892
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102563896"
---
# <a name="quickstart-create-a-redis-enterprise-cache"></a>Schnellstart: Erstellen einer Redis Cache-Instanz im Enterprise-Tarif

Enterprise-Tarife für Azure Cache for Redis bieten vollständig integrierte und verwaltete Instanzen von [Redis Enterprise](https://redislabs.com/redis-enterprise/) in Azure. Sie sind derzeit als Vorschau verfügbar. In dieser Vorschau stehen zwei neue Tarife zur Verfügung:
* Enterprise: Bei diesem Tarif wird flüchtiger Speicher (DRAM) auf einem virtuellen Computer als Datenspeicher verwendet.
* Enterprise Flash: Bei diesem Tarif wird eine Kombination aus flüchtigem und nicht flüchtigem Speicher (Non-Volatile Memory, NVMe oder SSD) als Datenspeicher verwendet.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Azure-Abonnement. Erstellen Sie ein [Konto](https://azure.microsoft.com/), falls Sie noch keines haben. Weitere Informationen finden Sie unter [Spezielle Überlegungen zu Enterprise-Tarifen](cache-overview.md#special-considerations-for-enterprise-tiers).

## <a name="create-a-cache"></a>Erstellen eines Caches
1. Melden Sie sich zum Erstellen eines Caches über den Link in Ihrer Vorschaueinladung beim Azure-Portal an, und wählen Sie **Ressource erstellen** aus.

1. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann **Azure Cache for Redis** aus.
   
   :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Auswählen von Azure Cache for Redis":::
   
1. Konfigurieren Sie auf der Seite **Neuer Redis Cache** die Einstellungen für den neuen Cache.
   
   | Einstellung      | Vorgeschlagener Wert  | Beschreibung |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Abonnement** | Öffnen Sie die Dropdownliste, und wählen Sie Ihr Abonnement aus. | Das Abonnement, unter dem diese neue Azure Cache for Redis-Instanz erstellt wird. | 
   | **Ressourcengruppe** | Öffnen Sie die Dropdownliste, und wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für eine neue Ressourcengruppe ein. | Der Name der Ressourcengruppe, in der Ihr Cache und weitere Ressourcen erstellt werden. Wenn Sie alle Ihre App-Ressourcen in einer Ressourcengruppe zusammenfassen, können Sie sie einfacher gemeinsam verwalten oder löschen. | 
   | **DNS-Name** | Geben Sie einen global eindeutigen Namen ein. | Der Cachename muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und Bindestriche enthalten. Der Name muss mit einer Zahl oder einem Buchstaben beginnen und enden und darf keine aufeinanderfolgenden Bindestriche enthalten. Der *Hostname* Ihrer Cache-Instanz lautet *\<DNS name>.<Azure region>.redisenterprise.cache.azure.net*. | 
   | **Standort** | Öffnen Sie die Dropdownliste, und wählen Sie einen Standort aus. | Enterprise-Tarife sind während der Vorschauphase in eingeschränkten Azure-Regionen verfügbar. |
   | **Cachetyp** | Wählen Sie in der Dropdownliste einen Tarif vom Typ *Enterprise* oder *Enterprise Flash* sowie eine Größe aus. |  Der Tarif bestimmt Größe, Leistung und verfügbare Features für den Cache. |
   
   :::image type="content" source="media/cache-create/enterprise-tier-basics.png" alt-text="Enterprise-Tarif: Registerkarte „Grundlagen“":::

   > [!NOTE] 
   > Aktivieren Sie das Kontrollkästchen unter „Bestimmungen“, bevor Sie fortfahren.
   >

1. Wählen Sie **Weiter: Netzwerk** aus, und überspringen Sie den Schritt.

1. Klicken Sie auf **Weiter: Erweitert**, und legen Sie **Clustering policy** (Clusteringrichtlinie) auf **Enterprise** fest. Aktivieren Sie **Nur Nicht-TLS-Zugriff**, wenn Sie beabsichtigen, eine Verbindung mit dem neuen Cache ohne Verwendung von TLS herzustellen. Dies ist wird jedoch nicht empfohlen.

   :::image type="content" source="media/cache-create/enterprise-tier-advanced.png" alt-text="Enterprise-Tarif: Registerkarte „Erweitert“":::

1. Wählen Sie **Weiter: Tags** aus, und überspringen Sie den Schritt.

1. Klicken Sie auf **Weiter: Überprüfen + erstellen**.

   :::image type="content" source="media/cache-create/enterprise-tier-summary.png" alt-text="Enterprise-Tarif: Registerkarte „Überprüfen und erstellen“":::

1. Überprüfen Sie die Einstellungen, und klicken Sie auf **Erstellen**.
   
   Die Erstellung des Caches dauert eine Weile. Sie können den Fortschritt auf der Seite **Übersicht** von Azure Cache for Redis überwachen. Wenn **Wird ausgeführt** als **Status** angezeigt wird, ist der Cache einsatzbereit.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie eine Azure Cache for Redis-Instanz mit Enterprise-Tarif erstellen.

> [!div class="nextstepaction"]
> [Erstellen einer ASP.NET-Web-App, die eine Azure Cache for Redis-Instanz verwendet](./cache-web-app-howto.md)


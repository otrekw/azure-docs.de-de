---
title: Azure Cache for Redis mit Azure Private Link (Vorschau)
description: Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit dem von Azure Private Link betriebenen Azure Cache for Redis verbindet. In diesem Artikel erfahren Sie, wie Sie einen Azure-Cache, ein virtuelles Azure-Netzwerk und einen privaten Endpunkt im Azure-Portal erstellen.
author: curib
ms.author: cauribeg
ms.service: cache
ms.topic: conceptual
ms.date: 07/21/2020
ms.openlocfilehash: d85fe36bb948ae9a0c81fa25f87450c7f5fe93b7
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337261"
---
# <a name="azure-cache-for-redis-with-azure-private-link-preview"></a>Azure Cache for Redis mit Azure Private Link (Vorschau)
Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit dem von Azure Private Link betriebenen Azure Cache for Redis verbindet. 

In diesem Artikel erfahren Sie, wie Sie einen Azure Cache, ein virtuelles Azure-Netzwerk und einen privaten Endpunkt im Azure-Portal erstellen.  

## <a name="prerequisites"></a>Voraussetzungen
* Azure-Abonnement –  [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)

> [!NOTE]
  > Diese Funktion befindet sich zurzeit in der Vorschauphase. [Wenden Sie sich an uns](mailto:azurecache@microsoft.com), wenn Sie daran interessiert sind.
  >


## <a name="create-a-cache"></a>Erstellen eines Caches
1. Melden Sie sich zum Erstellen eines Caches beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie **Ressource erstellen** aus. 

    :::image type="content" source="media/cache-private-link/1-create-resource.png" alt-text="Auswählen von „Ressource erstellen“":::
   
1. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann **Azure Cache for Redis** aus.

    :::image type="content" source="media/cache-private-link/2-select-cache.png" alt-text="Auswählen von „Azure Cache for Redis“.":::
   
1. Konfigurieren Sie auf der Seite **Neuer Redis Cache** die Einstellungen für den neuen Cache.
   
   | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
   | ------------ |  ------- | -------------------------------------------------- |
   | **DNS-Name** | Geben Sie einen global eindeutigen Namen ein. | Der Cachename muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und Bindestriche enthalten. Der Name muss mit einer Zahl oder einem Buchstaben beginnen und enden und darf keine aufeinanderfolgenden Bindestriche enthalten. Der *Hostname* Ihrer Cache-Instanz lautet *\<DNS name>.redis.cache.windows.net*. | 
   | **Abonnement** | Öffnen Sie die Dropdownliste, und wählen Sie Ihr Abonnement aus. | Das Abonnement, unter dem diese neue Azure Cache for Redis-Instanz erstellt wird. | 
   | **Ressourcengruppe** | Öffnen Sie die Dropdownliste, und wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für eine neue Ressourcengruppe ein. | Der Name der Ressourcengruppe, in der Ihr Cache und weitere Ressourcen erstellt werden. Wenn Sie alle Ihre App-Ressourcen in einer Ressourcengruppe zusammenfassen, können Sie sie einfacher gemeinsam verwalten oder löschen. | 
   | **Location** | Öffnen Sie die Dropdownliste, und wählen Sie einen Standort aus. | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in der Nähe anderer Dienste aus, die Ihren Cache verwenden. |
   | **Preisstufe** | Öffnen Sie die Dropdownliste, und wählen Sie einen [Tarif](https://azure.microsoft.com/pricing/details/cache/) aus. |  Der Tarif bestimmt Größe, Leistung und verfügbare Features für den Cache. Weitere Informationen finden Sie unter [What is Azure Cache for Redis](cache-overview.md) (Was ist Azure Cache for Redis?). |
   
1. Klicken Sie auf **Erstellen**. 
   
    :::image type="content" source="media/cache-private-link/3-new-cache.png" alt-text="Erstellen von Azure Cache for Redis.":::
   
   Es dauert eine Weile, bis der Cache erstellt wird. Sie können den Fortschritt auf der Seite **Übersicht** von Azure Cache for Redis überwachen. Wenn **Wird ausgeführt** als **Status** angezeigt wird, ist der Cache einsatzbereit.
    
    :::image type="content" source="media/cache-private-link/4-status.png" alt-text="Azure Cache for Redis erstellt.":::

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und ein Subnetz.

1. Wählen Sie **Ressource erstellen**.

    :::image type="content" source="media/cache-private-link/1-create-resource.png" alt-text="Auswählen von „Ressource erstellen“.":::

2. Wählen Sie auf der Seite **Neu** zuerst **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.

    :::image type="content" source="media/cache-private-link/5-select-vnet.png" alt-text="Erstellen eines virtuellen Netzwerks.":::

3. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundlegende Einstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | **Einstellung**          | **Wert**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Projektdetails**  |                                                                 |
    | Subscription     | Öffnen Sie die Dropdownliste, und wählen Sie Ihr Abonnement aus.                                  |
    | Ressourcengruppe   | Öffnen Sie die Dropdownliste, und wählen Sie eine Ressourcengruppe aus. |
    | **Instanzendetails** |                                                                 |
    | Name             | Eingeben von **\<virtual-network-name>**                                    |
    | Region           | Auswählen von **\<region-name>** |

4. Wählen Sie die Registerkarte **IP-Adressen** oder die Schaltfläche **Weiter: IP-Adressen** am unteren Seitenrand aus.

5. Geben Sie auf der Registerkarte **IP-Adressen** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | IPv4-Adressraum | Eingeben von **\<IPv4-address-space>** |

6. Wählen Sie unter **Subnetzname** das Wort **Standard** aus.

7. Geben Sie unter **Subnetz bearbeiten** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | Subnetzname | Eingeben von **\<subnet-name>** |
    | Subnetzadressbereich | Eingeben von **\<subnet-address-range>**

8. Wählen Sie **Speichern** aus.

9. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder die Schaltfläche **Überprüfen + erstellen** aus.

10. Klicken Sie auf **Erstellen**.


## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts 

In diesem Abschnitt erstellen Sie einen privaten Endpunkt und verbinden diesen mit dem zuvor erstellten Cache.

1. Suchen Sie nach **Private Link**, und drücken Sie die EINGABETASTE, oder wählen Sie den Eintrag aus den Suchvorschlägen aus.

    :::image type="content" source="media/cache-private-link/7-create-private-link.png" alt-text="Suchen nach einem Private Link.":::

2. Wählen Sie im linken Bildschirmbereich **Private Endpunkte** aus.

    :::image type="content" source="media/cache-private-link/8-select-private-endpoint.png" alt-text="Auswählen von Private Link.":::

3. Wählen Sie die Schaltfläche **+ Hinzufügen** aus, um Ihren privaten Endpunkt zu erstellen. 

    :::image type="content" source="media/cache-private-link/9-add-private-endpoint.png" alt-text="Einen Private Link hinzufügen.":::

4. Konfigurieren Sie auf der Seite **Privaten Endpunkt erstellen** die Einstellungen für Ihren privaten Endpunkt.

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Öffnen Sie die Dropdownliste, und wählen Sie Ihr Abonnement aus. |
    | Resource group | Öffnen Sie die Dropdownliste, und wählen Sie eine Ressourcengruppe aus. |
    | **INSTANZDETAILS** |  |
    | Name |Geben Sie einen Namen für den privaten Endpunkt ein.  |
    | Region |Öffnen Sie die Dropdownliste, und wählen Sie einen Standort aus. |
    |||

5. Wählen Sie unten auf der Seite die Schaltfläche **Next: Ressource** aus.

6. Wählen Sie auf der Registerkarte **Ressource** Ihr Abonnement aus, wählen Sie den Ressourcentyp „Microsoft.Cache/Redis“ aus, und wählen Sie dann den Cache aus, den Sie im vorherigen Abschnitt erstellt haben.

    :::image type="content" source="media/cache-private-link/10-resource-private-endpoint.png" alt-text="Ressourcen für Private Link.":::

7. Wählen Sie unten auf der Seite die Schaltfläche **Next: Konfiguration** aus.

8. Wählen Sie auf der Registerkarte **Konfiguration** das im vorherigen Abschnitt erstellte virtuelle Netzwerk und Subnetz aus.

    :::image type="content" source="media/cache-private-link/11-configuration-private-endpoint.png" alt-text="Konfiguration für Private Link.":::

9. Wählen Sie unten auf der Seite die Schaltfläche **Next: Tags** (Weiter: Tags) aus.

10. Geben Sie auf der Registerkarte **Tags** den Namen und den Wert ein, wenn Sie die Ressource kategorisieren möchten. Dieser Schritt ist optional.

    :::image type="content" source="media/cache-private-link/12-tags-private-endpoint.png" alt-text="Tags für Private Link.":::

11. Wählen Sie **Überprüfen und erstellen** aus. Sie werden zu der Registerkarte  **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

12. Wenn die grüne Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Private Link finden Sie in der [Dokumentation zu Azure Private Link](https://docs.microsoft.com/azure/private-link/private-link-overview). 


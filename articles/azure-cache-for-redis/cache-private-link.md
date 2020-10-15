---
title: Azure Cache for Redis mit Azure Private Link (Vorschau)
description: Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit dem von Azure Private Link betriebenen Azure Cache for Redis verbindet. In diesem Artikel erfahren Sie, wie Sie einen Azure-Cache, eine Azure Virtual Network-Instanz und einen privaten Endpunkt im Azure-Portal erstellen.
author: curib
ms.author: cauribeg
ms.service: cache
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: e2c071ff9cf020f99e990e670cfb29cca3c1ebbc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91838652"
---
# <a name="azure-cache-for-redis-with-azure-private-link-public-preview"></a>Azure Cache for Redis mit Azure Private Link (Public Preview)
In diesem Artikel erfahren Sie, wie Sie im Azure-Portal ein virtuelles Netzwerk und eine Azure Cache for Redis-Instanz mit einem privaten Endpunkt erstellen. Außerdem erfahren Sie, wie Sie einer vorhandenen Azure Cache for Redis-Instanz einen privaten Endpunkt hinzufügen.

Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit dem von Azure Private Link betriebenen Azure Cache for Redis verbindet. 

## <a name="prerequisites"></a>Voraussetzungen
* Azure-Abonnement –  [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)

> [!NOTE]
> Ihre Azure Cache for Redis-Instanz muss nach dem 28. Juli 2020 erstellt worden sein, damit Sie private Endpunkte verwenden können.
>
>

## <a name="create-a-private-endpoint-with-a-new-azure-cache-for-redis-instance"></a>Erstellen eines privaten Endpunkts mit einer neuen Azure Cache for Redis-Instanz 

In diesem Abschnitt erstellen Sie eine neue Azure Cache for Redis-Instanz mit einem privaten Endpunkt.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie dann **Ressource erstellen** aus.

    :::image type="content" source="media/cache-private-link/1-create-resource.png" alt-text="Auswählen von „Ressource erstellen“.":::

2. Wählen Sie auf der Seite **Neu** zuerst **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.

3. Wählen Sie **Hinzufügen** aus, um ein virtuelles Netzwerk zu erstellen.

4. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundlegende Einstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:

   | Einstellung      | Vorgeschlagener Wert  | Beschreibung |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Abonnement** | Öffnen Sie die Dropdownliste, und wählen Sie Ihr Abonnement aus. | Das Abonnement, unter dem dieses virtuelle Netzwerk erstellt werden soll. | 
   | **Ressourcengruppe** | Öffnen Sie die Dropdownliste, und wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für eine neue Ressourcengruppe ein. | Name der Ressourcengruppe, in der Ihr virtuelles Netzwerk und weitere Ressourcen erstellt werden. Wenn Sie alle Ihre App-Ressourcen in einer Ressourcengruppe zusammenfassen, können Sie sie einfacher gemeinsam verwalten oder löschen. | 
   | **Name** | Geben Sie einen Namen für das virtuelle Netzwerk ein. | Der Name muss mit einem Buchstaben oder einer Ziffer beginnen, auf einen Buchstaben, eine Ziffer oder einen Unterstrich enden und darf nur Buchstaben, Ziffern, Unterstriche, Punkte und Bindestriche enthalten. | 
   | **Region** | Öffnen Sie die Dropdownliste, und wählen Sie eine Region aus. | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in der Nähe anderer Dienste aus, die Ihr virtuelles Netzwerk verwenden. |

5. Wählen Sie die Registerkarte **IP-Adressen** aus, oder klicken Sie auf die Schaltfläche **Weiter: IP-Adressen** am unteren Seitenrand aus.

6. Geben Sie auf der Registerkarte **IP-Adressen** den **IPv4-Adressraum** als eine oder mehrere Adresspräfixe in CIDR-Schreibweise an (z. B. 192.168.1.0/24).

7. Klicken Sie unter **Subnetzname** auf **Standard**, um die Eigenschaften des Subnetzes zu bearbeiten.

8. Geben Sie im Bereich **Subnetz bearbeiten** einen **Subnetznamen** sowie den **Subnetzadressbereich** an. Der Adressbereich des Subnetzes in CIDR-Schreibweise (z. B. 192.168.1.0/24). Er muss innerhalb des Adressraums des virtuellen Netzwerks liegen.

9. Wählen Sie **Speichern** aus.

10. Wählen Sie die Registerkarte **Überprüfen + erstellen**, oder klicken Sie auf die Schaltfläche **Überprüfen + erstellen**.

11. Vergewissern Sie sich, dass alle Informationen richtig sind, und klicken Sie auf **Erstellen**, um das virtuelle Netzwerk bereitzustellen.

### <a name="create-an-azure-cache-for-redis-instance-with-a-private-endpoint"></a>Erstellen eine einer Azure Cache for Redis-Instanz mit einem privatem Endpunkt
Führen Sie zum Erstellen einer Cache-Instanz die folgenden Schritte aus:

1. Wechseln Sie zurück zur Homepage des Azure-Portals, oder öffnen Sie das Randleistenmenü, und wählen Sie dann **Ressource erstellen** aus. 
   
1. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann **Azure Cache for Redis** aus.

    :::image type="content" source="media/cache-private-link/2-select-cache.png" alt-text="Auswählen von „Ressource erstellen“.":::
   
1. Konfigurieren Sie auf der Seite **Neuer Redis Cache** die Einstellungen für den neuen Cache.
   
   | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
   | ------------ |  ------- | -------------------------------------------------- |
   | **DNS-Name** | Geben Sie einen global eindeutigen Namen ein. | Der Cachename muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und Bindestriche enthalten. Der Name muss mit einer Zahl oder einem Buchstaben beginnen und enden und darf keine aufeinanderfolgenden Bindestriche enthalten. Der *Hostname* Ihrer Cache-Instanz lautet *\<DNS name>.redis.cache.windows.net*. | 
   | **Abonnement** | Öffnen Sie die Dropdownliste, und wählen Sie Ihr Abonnement aus. | Das Abonnement, unter dem diese neue Azure Cache for Redis-Instanz erstellt wird. | 
   | **Ressourcengruppe** | Öffnen Sie die Dropdownliste, und wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für eine neue Ressourcengruppe ein. | Der Name der Ressourcengruppe, in der Ihr Cache und weitere Ressourcen erstellt werden. Wenn Sie alle Ihre App-Ressourcen in einer Ressourcengruppe zusammenfassen, können Sie sie einfacher gemeinsam verwalten oder löschen. | 
   | **Location** | Öffnen Sie die Dropdownliste, und wählen Sie einen Standort aus. | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in der Nähe anderer Dienste aus, die Ihren Cache verwenden. |
   | **Preisstufe** | Öffnen Sie die Dropdownliste, und wählen Sie einen [Tarif](https://azure.microsoft.com/pricing/details/cache/) aus. |  Der Tarif bestimmt Größe, Leistung und verfügbare Features für den Cache. Weitere Informationen finden Sie unter [What is Azure Cache for Redis](cache-overview.md) (Was ist Azure Cache for Redis?). |

1. Wählen Sie die Registerkarte **Netzwerk** aus, oder klicken Sie unten auf der Seite auf die Schaltfläche **Netzwerk**.

1. Wählen Sie auf der Registerkarte **Netzwerk** die Option **Privater Endpunkt** als Konnektivitätsmethode aus.

1. Klicken Sie auf die Schaltfläche **Hinzufügen**, um Ihren privaten Endpunkt zu erstellen.

    :::image type="content" source="media/cache-private-link/3-add-private-endpoint.png" alt-text="Auswählen von „Ressource erstellen“.":::

1. Konfigurieren Sie auf der Seite **Erstellen eines privaten Endpunkts** die Einstellungen für Ihren privaten Endpunkt mit dem virtuellen Netzwerk und Subnetz, die Sie im letzten Abschnitt erstellt haben, und wählen Sie **OK** aus. 

1. Wählen Sie unten auf der Seite die Registerkarte **Weiter: Erweitert** aus, oder klicken Sie unten auf der Seite auf die Schaltfläche **Weiter: Erweitert**.

1. Aktivieren Sie auf der Registerkarte **Erweitert** für eine Basic- oder Standard-Cache-Instanz die Aktivierungsoption, wenn Sie einen TLS-fremden Port aktivieren möchten.

1. Konfigurieren Sie auf der Registerkarte **Advanced** für eine Premium-Cache-Instanz die Einstellungen für einen TLS-fremden Port, Clustering und Datenpersistenz.


1. Wählen Sie die Registerkarte **Weiter: Tags** aus, oder klicken Sie unten auf der Seite auf die Schaltfläche **Weiter: Tags** (Weiter: Tags) aus.

1. Geben Sie optional auf der Registerkarte **Tags** den Namen und den Wert ein, wenn Sie die Ressource kategorisieren möchten. 

1. Wählen Sie **Überprüfen und erstellen** aus. Sie werden zur Registerkarte „Überprüfen und erstellen“ weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die grüne Meldung „Validierung erfolgreich“ angezeigt wird, wählen Sie **Erstellen** aus.

Es dauert eine Weile, bis der Cache erstellt wird. Sie können den Fortschritt auf der Seite  **Übersicht**  von Azure Cache for Redis überwachen. Wenn  **Wird ausgeführt** als  **Status**  angezeigt wird, ist der Cache einsatzbereit. 
    

## <a name="create-a-private-endpoint-with-an-existing-azure-cache-for-redis-instance"></a>Erstellen eines privaten Endpunkts mit einer vorhandenen Azure Cache for Redis-Instanz 

In diesem Abschnitt fügen Sie einer vorhandenen Azure Cache for Redis-Instanz einen privaten Endpunkt hinzu. 

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks 
Führen Sie die folgenden Schritte aus, um ein virtuelles Netzwerk zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie dann **Ressource erstellen** aus.

2. Wählen Sie auf der Seite **Neu** zuerst **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.

3. Wählen Sie **Hinzufügen** aus, um ein virtuelles Netzwerk zu erstellen.

4. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundlegende Einstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:

   | Einstellung      | Vorgeschlagener Wert  | Beschreibung |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Abonnement** | Öffnen Sie die Dropdownliste, und wählen Sie Ihr Abonnement aus. | Das Abonnement, unter dem dieses virtuelle Netzwerk erstellt werden soll. | 
   | **Ressourcengruppe** | Öffnen Sie die Dropdownliste, und wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für eine neue Ressourcengruppe ein. | Name der Ressourcengruppe, in der Ihr virtuelles Netzwerk und weitere Ressourcen erstellt werden. Wenn Sie alle Ihre App-Ressourcen in einer Ressourcengruppe zusammenfassen, können Sie sie einfacher gemeinsam verwalten oder löschen. | 
   | **Name** | Geben Sie einen Namen für das virtuelle Netzwerk ein. | Der Name muss mit einem Buchstaben oder einer Ziffer beginnen, auf einen Buchstaben, eine Ziffer oder einen Unterstrich enden und darf nur Buchstaben, Ziffern, Unterstriche, Punkte und Bindestriche enthalten. | 
   | **Region** | Öffnen Sie die Dropdownliste, und wählen Sie eine Region aus. | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in der Nähe anderer Dienste aus, die Ihr virtuelles Netzwerk verwenden. |

5. Wählen Sie die Registerkarte **IP-Adressen** aus, oder klicken Sie auf die Schaltfläche **Weiter: IP-Adressen** am unteren Seitenrand aus.

6. Geben Sie auf der Registerkarte **IP-Adressen** den **IPv4-Adressraum** als eine oder mehrere Adresspräfixe in CIDR-Schreibweise an (z. B. 192.168.1.0/24).

7. Klicken Sie unter **Subnetzname** auf **Standard**, um die Eigenschaften des Subnetzes zu bearbeiten.

8. Geben Sie im Bereich **Subnetz bearbeiten** einen **Subnetznamen** sowie den **Subnetzadressbereich** an. Der Adressbereich des Subnetzes in CIDR-Schreibweise (z. B. 192.168.1.0/24). Er muss innerhalb des Adressraums des virtuellen Netzwerks liegen.

9. Wählen Sie **Speichern** aus.

10. Wählen Sie die Registerkarte **Überprüfen + erstellen**, oder klicken Sie auf die Schaltfläche **Überprüfen + erstellen**.

11. Vergewissern Sie sich, dass alle Informationen richtig sind, und klicken Sie auf **Erstellen**, um das virtuelle Netzwerk bereitzustellen.

### <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts 

Führen Sie die folgenden Schritte aus, um einen privaten Endpunkt zu erstellen:

1. Suchen Sie im Azure-Portal nach **Azure Cache for Redis**, und drücken Sie die EINGABETASTE, oder wählen Sie es in den Suchvorschlägen aus.

    :::image type="content" source="media/cache-private-link/4-search-for-cache.png" alt-text="Auswählen von „Ressource erstellen“.":::

2. Wählen Sie die Cache-Instanz aus, der Sie einen privaten Endpunkt hinzufügen möchten.

3. Wählen Sie im linken Bildschirmbereich **(VORSCHAU) Privater Endpunkt** aus.

4. Klicken Sie auf die Schaltfläche **Privater Endpunkt**, um Ihren privaten Endpunkt zu erstellen.

    :::image type="content" source="media/cache-private-link/5-add-private-endpoint.png" alt-text="Auswählen von „Ressource erstellen“.":::

5. Konfigurieren Sie auf der Seite **Privaten Endpunkt erstellen** die Einstellungen für Ihren privaten Endpunkt.

   | Einstellung      | Vorgeschlagener Wert  | Beschreibung |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Abonnement** | Öffnen Sie die Dropdownliste, und wählen Sie Ihr Abonnement aus. | Das Abonnement, unter dem dieser private Endpunkt erstellt werden soll. | 
   | **Ressourcengruppe** | Öffnen Sie die Dropdownliste, und wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für eine neue Ressourcengruppe ein. | Der Name der Ressourcengruppe, in der Ihr privater Endpunkt und weitere Ressourcen erstellt werden. Wenn Sie alle Ihre App-Ressourcen in einer Ressourcengruppe zusammenfassen, können Sie sie einfacher gemeinsam verwalten oder löschen. | 
   | **Name** | Geben Sie einen Namen für den privaten Endpunkt ein. | Der Name muss mit einem Buchstaben oder einer Ziffer beginnen, auf einen Buchstaben, eine Ziffer oder einen Unterstrich enden und darf nur Buchstaben, Ziffern, Unterstriche, Punkte und Bindestriche enthalten. | 
   | **Region** | Öffnen Sie die Dropdownliste, und wählen Sie eine Region aus. | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in der Nähe anderer Dienste aus, die Ihren privaten Endpunkt verwenden. |

6. Klicken Sie auf die Schaltfläche **Weiter: Ressource** aus.

7. Wählen Sie auf der Registerkarte **Ressource** Ihr Abonnement aus, wählen Sie `Microsoft.Cache/Redis` als Ressourcentyp aus, und wählen Sie dann den Cache aus, mit dem Sie den privaten Endpunkt verbinden möchten.

8. Klicken Sie auf die Schaltfläche **Weiter: Konfiguration** aus.

9. Wählen Sie auf der Registerkarte **Konfiguration** das im vorherigen Abschnitt erstellte virtuelle Netzwerk und Subnetz aus.

10. Klicken Sie auf die Schaltfläche **Weiter: Tags** (Weiter: Tags) aus.

11. Geben Sie optional auf der Registerkarte **Tags** den Namen und den Wert ein, wenn Sie die Ressource kategorisieren möchten.

12. Wählen Sie **Überprüfen und erstellen** aus. Sie werden zu der Registerkarte  **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

13. Wenn die grüne Meldung **Validierung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Private Link finden Sie in der [Dokumentation zu Azure Private Link](https://docs.microsoft.com/azure/private-link/private-link-overview). 


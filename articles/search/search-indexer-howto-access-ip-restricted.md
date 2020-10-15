---
title: Gewähren von Zugriff auf die Indexer-IP-Adressbereiche
titleSuffix: Azure Cognitive Search
description: In dieser Anleitung wird das Einrichten von IP-Firewallregeln beschrieben, damit Indexer Zugriff erhalten.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: f485569caef285601d1dce7acd116f13675da83a
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91950192"
---
# <a name="setting-up-ip-firewall-rules-to-enable-indexer-access"></a>Einrichten von IP-Firewallregeln zum Aktivieren des Indexerzugriffs

IP-Firewallregeln für Azure-Ressourcen wie Speicherkonten, Cosmos-DB-Konten und Azure SQL-Server gestatten nur Datenverkehr, der aus bestimmten IP-Bereichen stammt, auf Daten zuzugreifen.

In diesem Artikel wird beschrieben, wie die IP-Regeln über das Azure-Portal für ein Speicherkonto konfiguriert werden können, sodass Indexer von Azure Cognitive Search sicher auf die Daten zugreifen können. Dieser Leitfaden ist zwar speziell auf die Speicherung ausgerichtet, kann aber auch direkt in andere Azure-Ressourcen übersetzt werden, die ebenfalls IP-Firewallregeln zur Sicherung des Datenzugriffs bieten.

> [!NOTE]
> IP-Firewallregeln für Speicherkonten sind nur wirksam, wenn sich das Speicherkonto und der Suchdienst in verschiedenen Regionen befinden. Wenn Ihr Setup dies nicht zulässt, empfehlen wir die Verwendung der [Option „Ausnahme für vertrauenswürdige Dienste“](search-indexer-howto-access-trusted-service-exception.md).

## <a name="get-the-ip-address-of-the-search-service"></a>Abrufen der IP-Adresse des Suchdiensts

Rufen Sie den vollqualifizierten Domänennamen (FQDN) Ihres Suchdiensts ab. Dieser sieht wie `<search-service-name>.search.windows.net` aus. Sie können den FQDN ermitteln, indem Sie Ihren Suchdienst im Azure-Portal suchen.

   ![Abrufen des Dienst-FQDN](media\search-indexer-howto-secure-access\search-service-portal.png "Abrufen des Dienst-FQDN")

Die IP-Adresse des Suchdiensts kann durch Ausführen von `nslookup` (oder von `ping`) des FQDN abgerufen werden. Dies wird eine der IP-Adressen sein, die zu den Firewallregeln hinzugefügt werden müssen.

```azurepowershell

nslookup contoso.search.windows.net
Server:  server.example.org
Address:  10.50.10.50

Non-authoritative answer:
Name:    <name>
Address:  150.0.0.1
Aliases:  contoso.search.windows.net
```

## <a name="get-the-ip-address-ranges-for-azurecognitivesearch-service-tag"></a>Abrufen der IP-Adressbereiche für das Diensttag „AzureCognitiveSearch“

Die IP-Adressbereiche für das Diensttag `AzureCognitiveSearch` können entweder über die [Ermittlungs-API (Vorschau)](../virtual-network/service-tags-overview.md#use-the-service-tag-discovery-api-public-preview) oder die [herunterladbare JSON-Datei](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files) abgerufen werden.

Für diese exemplarische Vorgehensweise sollte unter der Annahme, dass der Suchdienst der öffentliche Azure-Clouddienst ist, die [öffentliche Azure-JSON-Datei](https://www.microsoft.com/download/details.aspx?id=56519) heruntergeladen werden.

   ![Herunterladen der JSON-Datei](media\search-indexer-howto-secure-access\service-tag.png "Herunterladen der JSON-Datei")

Aus der JSON-Datei sind unter der Annahme, dass sich der Suchdienst in „USA, Westen-Mitte“ befindet, die IP-Adressen für die Ausführungsumgebung des Indexers für mehrere Mandanten nachfolgend aufgeführt.

```json
    {
      "name": "AzureCognitiveSearch.WestCentralUS",
      "id": "AzureCognitiveSearch.WestCentralUS",
      "properties": {
        "changeNumber": 1,
        "region": "westcentralus",
        "platform": "Azure",
        "systemService": "AzureCognitiveSearch",
        "addressPrefixes": [
          "52.150.139.0/26",
          "52.253.133.74/32"
        ]
      }
    }
```

Für /32-IP-Adressen lassen Sie das „/32“ weg (52.253.133.74/32 -> 52.253.133.74), andere können unverändert verwendet werden.

## <a name="add-the-ip-address-ranges-to-ip-firewall-rules"></a>Hinzufügen von IP-Adressbereichen zu IP-Firewallregeln

Der einfachste Weg, IP-Adressbereiche zur Firewallregel eines Speicherkontos hinzuzufügen, ist über das Azure-Portal. Suchen Sie das Speicherkonto auf dem Portal, und navigieren Sie zur Registerkarte „**Firewalls und virtuelle Netzwerke**“.

   ![Firewall und virtuelle Netzwerke](media\search-indexer-howto-secure-access\storage-firewall.png "Firewall und virtuelle Netzwerke")

Fügen Sie die drei zuvor erhaltenen IP-Adressen (1 für die IP-Adresse des Suchdiensts, 2 für das `AzureCognitiveSearch`-Diensttag) in den Adressbereich ein, und klicken Sie auf „**Speichern**“.

   ![IP-Regeln der Firewall](media\search-indexer-howto-secure-access\storage-firewall-ip.png "IP-Regeln der Firewall")

Die Firewallregeln benötigen 5-10 Minuten für die Aktualisierung, danach können Indexer auf die Daten im Speicherkonto zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Da Sie jetzt wissen, wie Sie die beiden Sätze von IP-Adressen dazu bringen können, den Zugriff für Indexer zu ermöglichen, verwenden Sie die folgenden Links, um die IP-Firewallregeln für einige gemeinsame Datenquellen zu aktualisieren.

- [Konfigurieren von Azure Storage-Firewalls](../storage/common/storage-network-security.md)
- [Konfigurieren der IP-Firewall für CosmosDB](../cosmos-db/firewall-support.md)
- [Konfigurieren der IP-Firewall für Azure SQL-Server](../azure-sql/database/firewall-configure.md)
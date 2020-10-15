---
title: Azure Blockchain Service-Ledgerversionen, Patches und Upgrades
description: Übersicht über die unterstützten Ledgerversionen in Azure Blockchain Service, einschließlich Richtlinien für das Patchen und Aktualisieren von Systemen.
ms.date: 06/30/2020
ms.topic: conceptual
ms.reviewer: ravastra
ms.openlocfilehash: ea7c54e229178320329204a3199ab2b4c44058fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85807739"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Von Azure Blockchain unterstützte Ledgerversionen

Azure Blockchain verwendet den auf Ethereum basierenden Ledger [Quorum](https://www.goquorum.com/developers), der dafür konzipiert ist, private Transaktionen innerhalb einer Gruppe bekannter Teilnehmer zu verarbeiten. Diese werden in Azure Blockchain als Konsortium bezeichnet.

Momentan unterstützt Azure Blockchain Service [Quorum Version 2.6.0](https://github.com/jpmorganchase/quorum/releases/tag/v2.6.0) und den [Transaktions-Manager Tessera](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades

Für die Versionsverwaltung in Quorum werden eine Hauptversion, eine Nebenversion und Patchreleases verwendet. Wenn die Quorum-Version beispielsweise 2.0.1 lautet, wird der Releasetyp wie folgt kategorisiert:

|Hauptversion | Nebenversion  | Patch  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure Blockchain Service führt die Aktualisierung der Patchreleases von Quorum auf vorhandene ausgeführte Mitglieder innerhalb von 30 Tagen nach der Bereitstellung durch Quorum automatisch durch.

## <a name="availability-of-new-ledger-versions"></a>Verfügbarkeit neuer Ledgerversionen

Azure Blockchain Service macht die aktuellste Haupt- und Nebenversion des Quorum-Ledgers innerhalb von 60 Tagen nach dessen Veröffentlichung durch den Quorum-Hersteller verfügbar. Wenn Sie ein neues Mitglied und Konsortium bereitstellen, stehen maximal vier Nebenversionen für Konsortien zur Auswahl. Upgrades von einer Haupt- auf eine Nebenversion werden derzeit nicht unterstützt. Wenn Sie also beispielsweise die Version 2.x verwenden, wird derzeit kein Upgrade auf die Version 3.x unterstützt. Analog dazu wird bei Verwendung der Version 2.2 derzeit kein Upgrade auf die Version 2.3 unterstützt.

## <a name="how-to-check-quorum-ledger-version"></a>Überprüfen der Quorum-Ledgerversion

Sie können die Quorum-Version im Azure Blockchain Service-Mitglied überprüfen, indem Sie mithilfe von Geth eine Anfügung an den Knoten herstellen oder indem Sie Diagnoseprotokolle anzeigen.

### <a name="using-geth"></a>Verwenden von Geth

Stellen Sie mithilfe von Geth eine Anfügung an den Azure Blockchain Service-Knoten her. Beispiel: `geth attach https://myblockchainmember.blockchain.azure.com:3200/<Access key>`.

Wenn der Knoten verbunden ist, meldet Geth die Quorum-Version ähnlich der folgenden Ausgabe:

``` text
instance: Geth/v1.9.7-stable-9339be03(quorum-v2.6.0)/linux-amd64/go1.13.12
```

Weitere Informationen zur Verwendung von Geth finden Sie unter [Schnellstart: Anfügen an einen Azure Blockchain Service-Transaktionsknoten mithilfe von Geth](connect-geth.md).

### <a name="using-diagnostic-logs"></a>Verwenden von Diagnoseprotokollen

Wenn Sie Diagnoseprotokolle aktivieren, wird die Quorum-Version für Transaktionsknoten gemeldet. Beispielsweise enthält die folgende Informationsprotokollmeldung zu den Knoten die Quorum-Version.

``` text 
{"NodeName":"transaction-node","Message":"INFO [06-22|05:31:45.156] Starting peer-to-peer node instance=Geth/v1.9.7-stable-9339be03(quorum-v2.6.0)/linux-amd64/go1.13.12\n"}
{"NodeName":"transaction-node","Message":"[*] Starting Quorum node with QUORUM_VERSION=2.6.0, TESSERA_VERSION=0.10.5 and PRIVATE_CONFIG=/working-dir/c/tm.ipc\n"}
111
```

Weitere Informationen zu Diagnoseprotokollen finden Sie unter [Überwachen von Azure Blockchain Service über Azure Monitor](monitor-azure-blockchain-service.md#diagnostic-settings).

## <a name="how-to-check-genesis-file-content"></a>Überprüfen des Genesis-Dateiinhalts

Den Genesis-Dateiinhalt des Blockchain-Knotens können Sie mithilfe der folgenden Ethereum-JavaScript-API überprüfen:

``` bash
admin.nodeInfo.protocols
```
Sie können die API über eine Geth-Konsole oder eine web3-Bibliothek aufrufen. Weitere Informationen zur Verwendung von Geth finden Sie unter [Schnellstart: Anfügen an einen Azure Blockchain Service-Transaktionsknoten mithilfe von Geth](connect-geth.md).

## <a name="next-steps"></a>Nächste Schritte

[Einschränkungen des Azure Blockchain-Diensts](limits.md)

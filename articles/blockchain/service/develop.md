---
title: Übersicht über die Entwicklung mit dem Azure Blockchain-Dienst
description: Einführung in die Entwicklung von Lösungen im Azure Blockchain-Dienst
ms.date: 03/26/2020
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: 4a98e5ea025894303bc5c77bba0c6154a08315f2
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348376"
---
# <a name="azure-blockchain-service-development-overview"></a>Übersicht über die Entwicklung mit dem Azure Blockchain-Dienst

Mit dem Azure Blockchain-Dienst können Sie Konsortiumsblockchainnetzwerke erstellen, um unter anderem Unternehmensszenarios für die Assetnachverfolgung, digitale Token, Treue und Belohnung, die Supply-Chain-Finanzierung und die Herkunft zu ermöglichen. In den folgenden Abschnitten wird Azure Blockchain Service-Entwicklung zum Implementieren von Blockchain-Unternehmenslösungen eingeführt.

## <a name="connecting-to-azure-blockchain-service"></a>Verbinden mit Azure Blockchain Service

Es gibt verschiedene Arten von Clients für Blockchainnetzwerke, einschließlich vollständiger Knoten, einfacher Knoten und Remoteclients. Der Azure Blockchain-Dienst erstellt ein Blockchainnetzwerk, das Knoten enthält. Sie können verschiedene Clients als Einstiegspunkt für die Blockchainentwicklung mit dem Azure Blockchain-Dienst verwenden. Der Azure Blockchain-Dienst stellt die Standardauthentifizierung oder den Zugriffsschlüssel als Entwicklungsendpunkt bereit. Im Folgenden werden beliebte Clients zum Herstellen der Verbindung aufgeführt.

### <a name="visual-studio-code"></a>Visual Studio Code

Sie können eine Verbindung mit Konsortiumsmitgliedern herstellen, indem Sie die Visual Studio Code-Erweiterung für das Azure Blockchain Development Kit verwenden. Nachdem die Verbindung mit einem Konsortium hergestellt wurde, können Sie Smart Contracts kompilieren, erstellen und für ein Azure Blockchain Service-Konsortiumsmitglied bereitstellen.

Für die Entwicklung von professionellen Blockchainlösungen für Unternehmen ist ein Entwicklungsframework erforderlich, um Verbindungen mit verschiedenen Blockchainnetzwerken herzustellen und den Lebenszyklus von Smart Contracts zu verwalten. Die meisten Projekte interagieren mit mindestens zwei Blockchainknoten. Entwickler verwenden bei der Entwicklung eine lokale Blockchain. Wenn die Anwendung für Tests oder Release bereit ist, stellt der Entwickler sie in einem Blockchainnetzwerk bereit. Beispiele hierfür sind das öffentliche Ethereum-Hauptnetzwerk oder der Azure Blockchain-Dienst. Das Azure Blockchain Development Kit für die Ethereum-Erweiterung in Visual Studio Code verwendet Truffle. Truffle ist ein beliebtes Blockchainentwicklungsframework zum Schreiben, Kompilieren, Bereitstellen und Testen von dezentralisierten Anwendungen für Ethereum-Blockchains. Sie können sich Truffle als Framework vorstellen, das versucht, die Smart Contract-Entwicklung und die traditionelle Webentwicklung nahtlos zu integrieren.

Weitere Informationen finden Sie unter [Quickstart: Herstellen einer Verbindung mit einem Azure Blockchain Service-Konsortiumsnetzwerk mithilfe von Visual Studio Code](connect-vscode.md) durch.

### <a name="metamask"></a>MetaMask

MetaMask ist ein browserbasierter Wallet (Remoteclient), RPC-Client und einfacher Vertrags-Explorer. Im Gegensatz zu anderen browserbasierten Wallets injiziert MetaMask eine Web3-Instanz in den JavaScript-Kontext des Browsers und dient als RPC-Client, der eine Verbindung mit mehreren Ethereum-Blockchains (*mainnet*, *Ropsten testnet*, *Kovan testnet*, lokaler RPC-Knoten usw.) herstellt. Sie können einen benutzerdefinierten RPC einrichten, um mithilfe von Remix mühelos eine Verbindung mit den Azure Blockchain-Dienst herzustellen und mit der Blockchainentwicklung zu beginnen.

Weitere Informationen finden Sie unter [Quickstart: Verwenden von MetaMask zum Verbinden und Bereitstellen eines Smart Contract](connect-metamask.md)

### <a name="geth"></a>Geth

Geth ist die Befehlszeilenschnittstelle zum Ausführen eines vollständigen Ethereum-Knotens, der in Go implementiert ist. Sie müssen keinen vollständigen Knoten ausführen, jedoch können Sie dessen interaktive Konsole starten, die eine JavaScript-Laufzeitumgebung bereitstellt, die wiederum eine JavaScript-API für die Interaktion mit Azure Blockchain zur Verfügung stellt.

Weitere Informationen finden Sie unter [Quickstart: Anfügen an einen Azure Blockchain Service-Transaktionsknoten mithilfe von Geth](connect-geth.md).

## <a name="ethereum-quorum-private-transactions"></a>Private Ethereum Quorum-Transaktionen

Quorum ist ein auf Ethereum basierendes verteiltes Ledgerprotokoll mit Datenschutz für Transaktionen und Verträge sowie neuen Konsensmechanismen. Die folgenden wichtigen Verbesserungen gegenüber Go-Ethereum sind enthalten:

* **Datenschutz**: Quorum unterstützt private Transaktionen und Verträge über die Aufteilung privater Zustände und nutzt den Peer-to-Peer-Austausch verschlüsselter Nachrichten für die direkte Übertragung privater Daten zwischen Netzwerkteilnehmern.
* **Alternative Konsensmechanismen**: Proof-of-Work oder Proof-of-Stake sind für ein berechtigtes Netzwerk nicht erforderlich. Quorum stellt mehrere Konsensmechanismen bereit, die für Konsortiumblockchains wie RAFT und IBFT entwickelt wurden.  Azure Blockchain Service nutzt den IBFT-Konsensmechanismus.
* **Peer-Berechtigungsvergabe**: Die Berechtigung von Knoten und Peers erfolgt mithilfe von Smart Contracts, wodurch sichergestellt wird, dass nur bekannte Parteien dem Netzwerk beitreten können.
* **Bessere Leistung**: Quorum bietet bessere Leistung als die öffentliche Geth-Schnittstelle.

## <a name="block-explorers"></a>Block-Explorer

Block-Explorer sind Blockchainbrowser, die individuelle Blockinhalte sowie Adress- und Verlaufsdaten zu Transaktionen anzeigen. Grundlegende Block-Informationen sind über Azure Monitor im Azure Blockchain Service verfügbar. Wenn Sie jedoch ausführlichere Informationen während der Entwicklung benötigen, können sich Block-Explorer als nützlich erweisen.  Die folgenden Block-Explorer funktionieren mit dem Azure Blockchain Service:

* [Azure Blockchain Service-Epirus-Explorer](https://azuremarketplace.microsoft.com/marketplace/apps/blk-technologies.azure-blockchain-explorer-template?tab=Overview) von Web3 Labs
* [BlockScout](https://github.com/Azure-Samples/blockchain/blob/master/ledger/template/ethereum-on-azure/technology-samples/blockscout/README.md)

Sie können auch Ihren eigenen Block-Explorer erstellen, indem Sie Blockchain Data Manager und Azure Cosmos DB verwenden. Siehe hierzu das [Tutorial: Verwenden von Blockchain Data Manager zum Senden von Daten an Azure Cosmos DB](data-manager-cosmosdb.md).

## <a name="tps-measurement"></a>TPS-Messung

Mit der steigenden Verwendung von Blockchains in Unternehmensszenarios spielt die Geschwindigkeit der Transaktionen pro Sekunde (TPS) eine größere Rolle bei der Vermeidung von Leistungsengpässen und Ineffizienzen im System. Die Beibehaltung hoher Transaktionsraten kann innerhalb einer dezentralisierten Blockchain schwierig sein. Eine genaue TPS-Messung kann von verschiedenen Faktoren beeinträchtigt werden, z. B. vom Serverthread, von der Größe der Transaktionswarteschlange und von der Sicherheit. Wenn Sie die TPS-Geschwindigkeit während der Entwicklung messen müssen, empfiehlt sich das beliebte Open-Source-Tool [ChainHammer](https://github.com/drandreaskrueger/chainhammer).

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie eine Schnellstartanleitung aus, die das Azure Blockchain Development Kit für Ethereum zum Anfügen an ein Konsortium in Azure Blockchain Service verwendet.

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Azure Blockchain Service mithilfe von Visual Studio Code](connect-vscode.md)
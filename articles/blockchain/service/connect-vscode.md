---
title: Herstellen einer Verbindung mit Azure Blockchain Service mithilfe von Visual Studio Code
description: Herstellen einer Verbindung mit einem Azure Blockchain Service-Konsortiumsnetzwerk mithilfe des Azure Blockchain Development Kit für die Ethereum-Erweiterung in Visual Studio Code
ms.date: 11/19/2019
ms.topic: quickstart
ms.reviewer: chrisseg
ms.openlocfilehash: 4d4625df1cb07818dcadb88e2c98bf9ae1da1b1f
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455985"
---
# <a name="quickstart-use-visual-studio-code-to-connect-to-an-azure-blockchain-service-consortium-network"></a>Schnellstart: Herstellen einer Verbindung mit einem Azure Blockchain Service-Konsortiumsnetzwerk mithilfe von Visual Studio Code

In diesem Schnellstart installieren und verwenden Sie das Azure Blockchain Development Kit für die Ethereum-Erweiterung in Visual Studio Code (VS Code) zum Anfügen an ein Konsortium in Azure Blockchain Service. Das Azure Blockchain Development Kit vereinfacht die Erstellung, Verbindung und Bereitstellung von Smart Contracts in Ethereum-Blockchain-Ledgern.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Quickstart: Create a blockchain member using the Azure portal (Schnellstart: Erstellen eines Blockchainmitglieds über das Azure-Portal)](create-member.md) oder [Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain-Dienst mithilfe der Azure CLI](create-member-cli.md)
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Azure Blockchain Development Kit für Ethereum-Erweiterung](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Node.js 10.15.x oder höher](https://nodejs.org/download)
* [Git 2.10.x oder höher](https://git-scm.com)
* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/). Fügen Sie Ihrem Pfad „python.exe“ hinzu. Python Version 2.7.15 ist in Ihrem Pfad für das Azure Blockchain Development Kit erforderlich.
* [Truffle 5.0.0](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLI 6.0.0](https://github.com/trufflesuite/ganache-cli)

Unter Windows ist ein installierter C++-Compiler für das Modul „node-gyp“ erforderlich. Sie können die MSBuild-Tools verwenden:

* Ist Visual Studio 2017 installiert, konfigurieren Sie npm mit dem Befehl `npm config set msvs_version 2017 -g` für die Verwendung der MSBuild-Tools.
* Ist Visual Studio 2019 installiert, legen Sie den Pfad der MSBuild-Tools für npm fest. Zum Beispiel, `npm config set msbuild_path "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\MSBuild.exe"`
* Installieren Sie andernfalls die eigenständigen VS-Buildtools. Verwenden Sie hierzu `npm install --global windows-build-tools` in einer Befehlsshell mit erhöhten Rechten (*Als Administrator ausführen*).

Weitere Informationen zu node-gyp finden Sie im [node-gyp-Repository auf GitHub](https://github.com/node-gyp).

### <a name="verify-azure-blockchain-development-kit-environment"></a>Überprüfen der Azure Blockchain Development Kit-Umgebung

Mit dem Azure Blockchain Development Kit wird überprüft, ob die Voraussetzungen für Ihre Entwicklungsumgebung erfüllt wurden. Bereiten Sie Ihre Entwicklungsumgebung wie folgt vor:

Wählen Sie in der VS Code-Befehlspalette die Option **Azure Blockchain: Show Welcome Page** (Azure Blockchain: Willkommensseite anzeigen).

Das Azure Blockchain Development Kit führt ein Überprüfungsskript mit einer Dauer von ca. einer Minute aus. Sie können die Ausgabe anzeigen, indem Sie **Terminal > New Terminal** (Terminal > Neues Terminal) wählen. Wählen Sie in der Menüleiste des Terminals die Registerkarte **Output** (Ausgabe) und in der Dropdownliste dann **Azure Blockchain**. Eine erfolgreiche Überprüfung sieht in etwa wie in der folgenden Abbildung aus:

![Gültige Entwicklungsumgebung](./media/connect-vscode/valid-environment.png)

 Falls ein benötigtes Tool fehlt, können Sie eine neue Registerkarte mit dem Namen **Azure Blockchain Development Kit – Preview** (Azure Blockchain Development Kit: Vorschauversion) verwenden. Darauf sind die erforderlichen Tools mit Links zum Herunterladen aufgeführt.

![Development Kit: Erforderliche Apps](./media/connect-vscode/required-apps.png)

Installieren Sie ggf. fehlende Voraussetzungen, bevor Sie mit dem Schnellstart fortfahren.

## <a name="connect-to-consortium-member"></a>Herstellen einer Verbindung mit dem Konsortiumsmitglied

Sie können eine Verbindung mit Konsortiumsmitgliedern herstellen, indem Sie die VS Code-Erweiterung für das Azure Blockchain Development Kit verwenden. Nachdem die Verbindung mit einem Konsortium hergestellt wurde, können Sie Smart Contracts kompilieren, erstellen und für ein Azure Blockchain Service-Konsortiumsmitglied bereitstellen.

Falls Sie keinen Zugriff auf ein Azure Blockchain Service-Konsortiumsmitglied haben, müssen Sie als Voraussetzung [Quickstart: Create a blockchain member using the Azure portal (Schnellstart: Erstellen eines Blockchainmitglieds über das Azure-Portal)](create-member.md) oder [Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain-Dienst mithilfe der Azure CLI](create-member-cli.md) durcharbeiten.

1. Erweitern Sie im VS Code-Explorer-Bereich die Erweiterung **Azure Blockchain**.
1. Wählen Sie die Option **Connect to Consortium** (Verbindung mit Konsortium herstellen).

   ![Herstellen einer Verbindung mit einem Konsortium](./media/connect-vscode/connect-consortium.png)

    Befolgen Sie die Anweisungen zum Authentifizieren per Browser, wenn Sie zum Durchführen der Azure-Authentifizierung aufgefordert werden.
1. Wählen Sie in der Dropdownliste der Befehlspalette die Option **Connect to Azure Blockchain Service consortium** (Verbindung mit Azure Blockchain Service-Konsortium herstellen).
1. Wählen Sie das Abonnement und die Ressourcengruppe aus, das bzw. die Ihrem Azure Blockchain Service-Konsortiumsmitglied zugeordnet ist.
1. Wählen Sie Ihr Konsortium in der Liste aus.

Die Konsortiums- und Blockchainmitglieder sind im VS Code-Explorer auf der Seitenleiste aufgeführt.

![Anzeige des Konsortiums im Explorer](./media/connect-vscode/consortium-node.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie das Azure Blockchain Development Kit für die Ethereum-Erweiterung in VS Code zum Anfügen an ein Konsortium in Azure Blockchain Service verwendet. Im nächsten Tutorial erfahren Sie, wie Sie das Azure Blockchain Development Kit für Ethereum verwenden, um eine Smart Contract-Funktion über eine Transaktion zu erstellen, bereitstellen und auszuführen.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Bereitstellen von Smart Contracts in Azure Blockchain Service](send-transaction.md)
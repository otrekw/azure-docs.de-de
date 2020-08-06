---
title: 'Erstellen eines Azure Blockchain Service-Mitglieds: Azure CLI'
description: Erstellen Sie mithilfe der Azure CLI ein Azure Blockchain Service-Mitglied für ein Blockchainkonsortium.
ms.date: 07/23/2020
ms.topic: quickstart
ms.reviewer: ravastra
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: f97aab59d38e9b15838a78d0227bc2848615cd92
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87504363"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-azure-cli"></a>Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain-Dienst mithilfe der Azure CLI

In dieser Schnellstartanleitung stellen Sie mithilfe der Azure CLI ein neues Blockchainmitglied und -konsortium in Azure Blockchain Service bereit.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Keine.

## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert.

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock einfach die Option **Ausprobieren**. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/bash](https://shell.azure.com/bash) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.

Wenn Sie es vorziehen, die CLI lokal zu installieren und zu verwenden, müssen Sie für diese Schnellstartanleitung mindestens Version 2.0.51 der Azure CLI verwenden. Führen Sie `az --version` aus, um die Version zu ermitteln. Wenn Sie die CLI installieren oder aktualisieren müssen, finden Sie die entsprechende Anleitung unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

1. Melden Sie sich an.

    Melden Sie sich mit dem Befehl [az login](/cli/azure/reference-index#az-login) an, falls Sie eine lokale Installation der Befehlszeilenschnittstelle verwenden.

    ```azurecli
    az login
    ```

    Führen Sie die in Ihrem Terminal angezeigten Schritte aus, um den Authentifizierungsprozess abzuschließen.

1. Installieren Sie die Erweiterung für die Azure-Befehlszeilenschnittstelle (Azure CLI).

    Bei der Verwendung von Erweiterungsverweisen für die Azure CLI müssen Sie die Erweiterung zunächst installieren.  Mit Azure CLI-Erweiterungen erhalten Sie Zugriff auf experimentelle Befehle und Vorabversionen von Befehlen, die noch nicht als Bestandteil der Kern-CLI bereitgestellt wurden.  Weitere Informationen zu Erweiterungen, u. a. zum Aktualisieren und Deinstallieren, finden Sie unter [Verwenden von Erweiterungen mit der Azure CLI](/cli/azure/azure-cli-extensions-overview).

    Installieren Sie die [Erweiterung für Azure Blockchain Service](/cli/azure/ext/blockchain/blockchain), indem Sie den folgenden Befehl ausführen:

    ```azurecli-interactive
    az extension add --name blockchain
    ```

1. Erstellen Sie eine Ressourcengruppe.

    Azure Blockchain Service muss genau wie alle anderen Azure-Ressourcen in einer Ressourcengruppe bereitgestellt werden. Mit Ressourcengruppen können verwandte Azure-Ressourcen organisiert und verwaltet werden.

    Erstellen Sie für diesen Schnellstart mit dem folgenden Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe namens _myResourceGroup_ am Standort _eastus_:

    ```azurecli-interactive
    az group create \
                     --name "myResourceGroup" \
                     --location "eastus"
    ```

## <a name="create-a-blockchain-member"></a>Erstellen eines Blockchainmitglieds

Ein Azure Blockchain Service-Mitglied ist ein Blockchainknoten in einem privaten Konsortium-Blockchainnetzwerk. Wenn Sie ein Mitglied bereitstellen, können Sie ein Konsortiumsnetzwerk erstellen oder einem Konsortiumsnetzwerk beitreten. Für ein Konsortiumsnetzwerk wird mindestens ein Mitglied benötigt. Die Anzahl der von den Teilnehmern benötigten Blockchainmitglieder hängt von Ihrem Szenario ab. Die Konsortiumsteilnehmer können über eines oder mehrere Blockchainmitglieder verfügen oder Mitglieder gemeinsam mit anderen Teilnehmern nutzen. Weitere Informationen zu Konsortien finden Sie unter [Azure Blockchain Service-Konsortium](consortium.md).

Es gibt mehrere Parameter und Eigenschaften, die Sie übergeben müssen. Ersetzen Sie die Beispielparameter durch eigene Werte.

```azurecli-interactive
az blockchain member create \
                            --resource-group "MyResourceGroup" \
                            --name "myblockchainmember" \
                            --location "eastus" \
                            --password "strongMemberAccountPassword@1" \
                            --protocol "Quorum" \
                            --consortium "myconsortium" \
                            --consortium-management-account-password "strongConsortiumManagementPassword@1" \
                            --sku "Basic"
```

| Parameter | BESCHREIBUNG |
|---------|-------------|
| **resource-group** | Name der Ressourcengruppe, in der die Ressourcen des Azure Blockchain-Diensts erstellt werden. Verwenden Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben.
| **name** | Ein eindeutiger Name, der Ihr Blockchainmitglied in Azure Blockchain identifiziert. Der Name wird für die Adresse eines öffentlichen Endpunkts verwendet. Beispiel: `myblockchainmember.blockchain.azure.com`.
| **location** | Die Azure-Region, in der das Blockchainmitglied erstellt wird. Beispiel: `westus2`. Wählen Sie den Standort aus, der Ihren Benutzern oder anderen Azure-Anwendungen am nächsten liegt. Features sind unter Umständen in einigen Regionen nicht verfügbar. Azure Blockchain Data Manager ist in den folgenden Azure-Regionen verfügbar: („USA, Osten“ und „Europa, Westen“) ausgeführt werden.
| **password** | Das Kennwort für den Standardtransaktionsknoten des Mitglieds. Verwenden Sie das Kennwort für die Standardauthentifizierung, wenn Sie eine Verbindung mit dem öffentlichen Endpunkt des Standardtransaktionsknotens des Blockchainmitglieds herstellen.
| **protocol** | Blockchain-Protokoll. Derzeit wird das *Quorum*-Protokoll unterstützt.
| **consortium** | Name des Konsortiums, dem beigetreten oder das erstellt werden soll. Weitere Informationen zu Konsortien finden Sie unter [Azure Blockchain Service-Konsortium](consortium.md).
| **consortium-management-account-password** | Das Kennwort für das Konsortium wird auch als Mitgliedskontokennwort bezeichnet. Das Mitgliedskontokennwort wird zum Verschlüsseln des privaten Schlüssels für das Ethereum-Konto verwendet, das für Ihr Mitglied erstellt wird. Sie verwenden das Mitgliedskonto und das Mitgliedskontokennwort für die Verwaltung des Konsortiums.
| **sku** | Tarifart. *Standard* oder *Basic*. Verwenden Sie *Basic* für die Entwicklung, das Testen und Proof of Concept-Vorgänge. Verwenden Sie *Standard* für Bereitstellungen für die Produktion. Sie sollten den Tarif *Standard* auch wählen, wenn Sie Blockchain Data Manager verwenden oder eine große Menge privater Transaktionen senden. Das Wechseln zwischen den Tarifen „Basic“ und „Standard“ nach der Erstellung eines Mitglieds wird nicht unterstützt.

Es dauert etwa 10 Minuten, das Blockchainmitglied und die zugehörigen Ressourcen zu erstellen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Das von Ihnen erstellte Blockchainmitglied können Sie für die nächste Schnellstartanleitung oder das nächste Tutorial verwenden. Wenn Ressourcen nicht mehr benötigt werden, können Sie diese löschen, indem Sie die für die Schnellstartanleitung erstellte Ressourcengruppe `myResourceGroup` löschen.

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle zugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete \
                 --name myResourceGroup \
                 --yes
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Azure Blockchain Service-Mitglied und ein neues Konsortium bereitgestellt. In der nächsten Schnellstartanleitung erfahren Sie, wie Sie das Azure Blockchain Development Kit für Ethereum zum Anfügen an ein Azure Blockchain Service-Mitglied verwenden.

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Azure Blockchain Service mithilfe von Visual Studio Code](connect-vscode.md)

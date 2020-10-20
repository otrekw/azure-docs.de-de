---
title: 'Schnellstart: Festlegen und Anzeigen von Azure Key Vault-Zertifikaten – Azure CLI'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Azure CLI ein Zertifikat in Azure Key Vault festlegen und daraus abrufen.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-azurecli
ms.date: 09/03/2019
ms.author: mbaldwin
ms.openlocfilehash: e6146809363681ed73f3d63ccf43ba49044ccc28
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940581"
---
# <a name="quickstart-set-and-retrieve-a-certificate-from-azure-key-vault-using-azure-cli"></a>Schnellstart: Festlegen und Abrufen eines Zertifikats aus Azure Key Vault mit der Azure CLI

In dieser Schnellstartanleitung erstellen Sie einen Schlüsseltresor in Azure Key Vault mit der Azure CLI. Azure Key Vault ist ein Clouddienst, der als sicherer Geheimnisspeicher fungiert. Dadurch können Schlüssel, Kennwörter, Zertifikate und andere Geheimnisse sicher gespeichert werden. Weitere Informationen zu Key Vault finden Sie in der [Übersicht](../general/overview.md). Über die Azure-Befehlszeilenschnittstelle können Azure-Ressourcen mithilfe von Befehlen oder Skripts erstellt und verwaltet werden. Nachdem Sie diese Vorgänge ausgeführt haben, speichern Sie ein Zertifikat.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.4 verwenden. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli).

Geben Sie Folgendes ein, um sich über die Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

Weitere Informationen zu den Anmeldeoptionen für die Befehlszeilenschnittstelle finden Sie unter [Anmelden mit Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *ContosoResourceGroup* am Standort *eastus*.

```azurecli
az group create --name "ContosoResourceGroup" --location eastus
```

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Als Nächstes erstellen Sie eine Key Vault-Instanz in der Ressourcengruppe aus dem vorherigen Schritt. Dazu müssen einige Informationen angegeben werden:

- Im Rahmen dieser Schnellstartanleitung verwenden wir **Contoso-vault2**. Geben Sie bei Ihren Tests einen eindeutigen Namen an.
- Der Ressourcengruppenname lautet **ContosoResourceGroup**.
- Als Standort wird **USA, Osten** verwendet.

```azurecli
az keyvault create --name "Contoso-Vault2" --resource-group "ContosoResourceGroup" --location eastus
```

Die Ausgabe dieses Cmdlets zeigt Eigenschaften der neu erstellten Key Vault-Instanz. Beachten Sie die beiden folgenden Eigenschaften:

- **Tresorname**: In diesem Beispiel wird **Contoso-Vault2** verwendet. Sie verwenden diesen Namen für andere Schlüsseltresor-Befehle.
- **Tresor-URI**: In diesem Beispiel https://contoso-vault2.vault.azure.net/. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-certificate-to-key-vault"></a>Hinzufügen eines Zertifikats zu Key Vault

Um dem Tresor ein Zertifikat hinzuzufügen, sind nur einige zusätzliche Schritte erforderlich. Dieses Zertifikat kann dann von einer Anwendung verwendet werden. 

Geben Sie die folgenden Befehle ein, um das selbstsignierte Zertifikat **ExampleCertificate** mit einer Standardrichtlinie zu erstellen:

```azurecli
az keyvault certificate create --vault-name "Contoso-Vault2" -n ExampleCertificate -p "$(az keyvault certificate get-default-policy)"
```

Nun können Sie mit dem dazugehörigen URI auf dieses Zertifikat verweisen, das Sie der Azure Key Vault-Instanz hinzugefügt haben. Verwenden Sie **„https://Contoso-Vault2.vault.azure.net/certificates/ExampleCertificate“** , um die aktuelle Version abzurufen. 

So zeigen Sie ein zuvor gespeichertes Zertifikat an:

```azurecli

az keyvault certificate show --name "ExampleCertificate" --vault-name "Contoso-Vault2"
```

Sie haben nun eine Key Vault-Instanz erstellt sowie ein Zertifikat gespeichert und abgerufen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, sollten Sie die Ressourcen nicht bereinigen.
Wenn Sie die Ressourcen nicht mehr benötigen, führen Sie den Befehl [az group delete](/cli/azure/group) aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu löschen. Die Ressourcen können wie folgt gelöscht werden:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Key Vault-Instanz erstellt und ein Zertifikat darin gespeichert. Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- [Was ist der Azure-Schlüsseltresor?](../general/overview.md)
- Sehen Sie sich die Referenz zu den [Azure CLI-Befehlen vom Typ „az keyvault“](/cli/azure/keyvault?view=azure-cli-latest) an.
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)
